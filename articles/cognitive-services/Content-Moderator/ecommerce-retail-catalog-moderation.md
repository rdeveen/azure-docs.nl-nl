---
title: 'Zelfstudie: E-commerce-productafbeeldingen beheren - Content Moderator'
titlesuffix: Azure Cognitive Services
description: Lees hoe u een toepassing instelt voor het analyseren en classificeren van productafbeeldingen met opgegeven labels (met behulp van Azure Computer Vision en Custom Vision) om zo ongewenste afbeeldingen te taggen voor verdere controle (met behulp van Azure Content Moderator).
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: tutorial
ms.date: 01/10/2019
ms.author: pafarley
ms.openlocfilehash: 41437933f715af092dea563b0b1be4794c71925c
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55863156"
---
# <a name="tutorial-moderate-e-commerce-product-images-with-azure-content-moderator"></a>Zelfstudie: E-commerce-productafbeeldingen beheren met Azure Content Moderator

In deze zelfstudie leert u hoe u met behulp van cognitieve services van Azure, waaronder Content Moderator, effectief productafbeeldingen voor een e-commerce-scenario kunt classificeren en beheren. U gaat eerst Computer Vision en Custom Vision gebruiken om verschillende tags (labels) toe te passen op afbeeldingen. Daarna maakt u een teambeoordeling, waarin de machine learning-technologieën van Content Moderator worden gecombineerd met teams voor menselijke beoordeling om een intelligent controlesysteem in te richten.

In deze zelfstudie ontdekt u hoe u:

> [!div class="checklist"]
> * Aanmelden voor Content Moderator en een beoordelingsteam maken.
> * De Image-API van Content Moderator gebruiken om te scannen op mogelijke ongepaste inhoud of inhoud voor volwassenen.
> * De Computer Vision-service gebruiken om te scannen op beroemdheden (of andere tags die Computer Vision kan detecteren).
> * De Custom Vision service gebruiken om te scannen op de aanwezigheid van vlaggen, speelgoed en pennen (of andere aangepaste tags).
> * De gecombineerde scanresultaten presenteren voor menselijke beoordeling en uiteindelijke besluitvorming.

De volledige voorbeeldcode is beschikbaar in de opslagplaats [Samples eCommerce Catalog Moderation](https://github.com/MicrosoftContentModerator/samples-eCommerceCatalogModeration) op GitHub.

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

- Een abonnementssleutel voor Content Moderator. Volg de instructies in [Een Cognitive Services-account maken](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) om u te abonneren op Content Moderator en een sleutel op te halen.
- Een abonnementssleutel voor Computer Vision (dezelfde instructies als hierboven).
- Een versie van [Visual Studio 2015 of 2017](https://www.visualstudio.com/downloads/).
- Een reeks afbeeldingen voor elk label dat door de Custom Vision-classificatie wordt gebruikt (in dit geval van speelgoed, pennen en vlaggen).

## <a name="create-a-review-team"></a>Een beoordelingsteam maken

Zie de quickstart [Aan de slag met Content Moderator](quick-start.md) om te lezen hoe u zich kunt aanmelden voor het beoordelingsprogramma [Content Moderator](https://contentmoderator.cognitive.microsoft.com/) en een beoordelingsteam kunt samenstellen. Noteer de waarde voor **Team Id** op de pagina **Create review team**.

## <a name="create-custom-moderation-tags"></a>Aangepaste controletags maken

Maak vervolgens aangepaste labels in het beoordelingsprogramma (raadpleeg het artikel [Tags](https://docs.microsoft.com/azure/cognitive-services/content-moderator/review-tool-user-guide/tags) als u hierbij hulp nodig hebt). In dit geval voegen we deze tags toe: **celebrity**, **USA**, **flag**, **toy** en **pen**. Niet alle tags hoeven categorieën te zijn die kunnen worden gedetecteerd in Computer Vision (zoals **celebrity**). U kunt uw eigen aangepaste tags toevoegen, op voorwaarde dat u Custom Vision traint voor het detecteren van deze tags.

![Aangepaste tags configureren](images/tutorial-ecommerce-tags2.PNG)

## <a name="create-visual-studio-project"></a>Een Visual Studio-project maken

1. Open het dialoogvenster Nieuw project in Visual Studio. Vouw **Geïnstalleerd** en vervolgens **Visual C#** uit en selecteer **Console App (.NET Framework)**.
1. Geef de toepassing de naam **EcommerceModeration** en klik op **OK**.
1. Als u dit project aan een bestaande oplossing toevoegt, selecteert u dit project als het enige opstartproject.

In deze zelfstudie wordt aandacht besteed aan de code die essentieel is voor het project, maar niet elke regel met benodigde code wordt besproken. Kopieer de volledige inhoud van _Program.cs_ uit het voorbeeldproject ([Samples eCommerce Catalog Moderation](https://github.com/MicrosoftContentModerator/samples-eCommerceCatalogModeration)) naar het bestand _Program.cs_ van het nieuwe project. Neem vervolgens de onderstaande gedeelten door voor meer informatie over de werking van het project en hoe u dit zelf kunt gebruiken.

## <a name="define-api-keys-and-endpoints"></a>API-sleutels en -eindpunten definiëren

Zoals eerder gezegd, worden in deze zelfstudie drie cognitieve services gebruikt. Daarom zijn er drie bijbehorende sleutels en API-eindpunten vereist. Bekijk de volgende velden in de klasse **Program**: 

[!code-csharp[define API keys and endpoint URIs](~/samples-eCommerceCatalogModeration/Fusion/Program.cs?range=21-29)]

U moet de velden `___Key` bijwerken met de waarde van de abonnementssleutels (de sleutel voor `CustomVisionKey` krijgt u later). Mogelijk moet u de velden `___Uri` zo wijzigen dat deze de juiste regio-id's bevatten. Voer in het gedeelte `YOURTEAMID` van het veld `ReviewUri` de id in van het beoordelingsteam dat u eerder hebt gemaakt. Het laatste gedeelte van het veld `CustomVisionUri` vult u later in.

## <a name="primary-method-calls"></a>Aanroepen van primaire methode

Bekijk de volgende code in de methode **Main**, waarmee een lijst met afbeeldings-URL's wordt doorlopen. De methode analyseert elke afbeelding met behulp van de drie verschillende services, registreert de toegepaste labels in de matrix **ReviewTags** en maakt vervolgens een beoordeling voor menselijke moderators (verzendt de afbeeldingen naar het beoordelingsprogramma Content Moderator). Deze methoden gaan we in de volgende gedeelten bespreken. U kunt hier desgewenst aangeven welke afbeeldingen ter beoordeling worden verzonden. Hiervoor gebruikt u de matrix **ReviewTags** in een voorwaardelijke instructie om te controleren welke tags zijn toegepast.

[!code-csharp[Main: evaluate each image and create review](~/samples-eCommerceCatalogModeration/Fusion/Program.cs?range=53-70)]

## <a name="evaluateadultracy-method"></a>EvaluateAdultRacy-methode

Bekijk de methode **EvaluateAdultRacy** in de klasse **Program**. Deze methode accepteert een afbeeldings-URL en een matrix van sleutel-waardeparen als parameters. De methode roept de Image API van Content Moderator aan (met behulp van REST) om de waarden voor AdultScore en RacyScore op te vragen voor de afbeelding. Als de score voor een van de twee hoger is dan 0,4 het (loopt van 0 tot 1), wordt de bijbehorende waarde in de matrix **ReviewTags** ingesteld op **True**.

[!code-csharp[define EvaluateAdultRacy method](~/samples-eCommerceCatalogModeration/Fusion/Program.cs?range=73-113)]

## <a name="evaluatecustomvisiontags-method"></a>EvaluateComputerVisionTags-methode

De volgende methode verwerkt een afbeeldings-URL en de gegevens van uw Computer Vision-abonnement en analyseert de afbeelding op de aanwezigheid van beroemdheden. Als er een of meer beroemdheden worden gevonden in de afbeelding, wordt de bijbehorende waarde in de matrix **ReviewTags** ingesteld op **True**. 

[!code-csharp[define EvaluateCustomVisionTags method](~/samples-eCommerceCatalogModeration/Fusion/Program.cs?range=115-146)]

## <a name="evaluatecustomvisiontags-method"></a>EvaluateComputerVisionTags-methode

Bekijk nu de methode **EvaluateCustomVisionTags**, waarmee de daadwerkelijke producten worden geclassificeerd&mdash;in dit geval vlaggen, speelgoed en pennen. Volg de instructies in [Een classificatie bouwen](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/getting-started-build-a-classifier) om uw eigen aangepaste afbeeldingsclassificatie te bouwen om te detecteren op de aanwezigheid van vlaggen, speelgoed en pennen (of wat u ook maar hebt gekozen als aangepaste tags) in afbeeldingen.

![Webpagina van Custom Vision met trainingsafbeeldingen van pennen, speelgoed en vlaggen](images/tutorial-ecommerce-custom-vision.PNG)

Als u de classificatie hebt getraind, haalt u de sleutel en de eindpunt-URL van de voorspelling op en wijst u deze toe aan respectievelijk de velden `CustomVisionKey` en `CustomVisionUri`. Zie eventueel [De URL en voorspellingssleutel ophalen](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/use-prediction-api#get-the-url-and-prediction-key) voor instructies. De methode gebruikt deze waarden om query's uit te voeren op de classificatie. Als de classificatie een of meer van de aangepaste tags vindt in de afbeelding, worden de bijbehorende waarden in de matrix **ReviewTags** ingesteld op **True**. 

[!code-csharp[define EvaluateCustomVisionTags method](~/samples-eCommerceCatalogModeration/Fusion/Program.cs?range=148-171)]

## <a name="create-reviews-for-review-tool"></a>Beoordelingen maken voor het beoordelingsprogramma

In de vorige gedeelten hebben we gekeken naar de methoden voor het scannen van binnenkomende afbeeldingen op inhoud voor volwassen en ongepaste inhoud (Content Moderator), beroemdheden (Computer Vision) en verschillende andere objecten (Custom Vision). Hieronder besteden we aandacht aan de methode **CreateReview**. Hiermee worden de afbeeldingen, samen met alle toegepaste tags (doorgegeven als _Metadata_), geüpload naar het beoordelingsprogramma Content Moderator, zodat ze beschikbaar zijn voor menselijke beoordeling. 

[!code-csharp[define CreateReview method](~/samples-eCommerceCatalogModeration/Fusion/Program.cs?range=173-196)]

De afbeeldingen worden weergegeven op het tabblad Review van het [beoordelingsprogramma Content Moderator](https://contentmoderator.cognitive.microsoft.com/).

![Schermafbeelding van het beoordelingsprogramma Content Moderator met verschillende afbeeldingen en de bijbehorende gemarkeerde tags](images/tutorial-ecommerce-content-moderator.PNG)

## <a name="submit-a-list-of-test-images"></a>Een lijst met testafbeeldingen aanbieden

Zoals u kunt zien in de methode **Main**, zoekt dit programma naar een map 'C:Test' met daarin een bestand _Urls.txt_ dat een lijst met afbeeldings-URL's bevat. Maak dit bestand en die map, of pas het pad aan zodat het verwijst naar uw tekstbestand en voeg aan dit bestand de URL's toe van afbeeldingen die u wilt testen.

[!code-csharp[Main: set up test directory, read lines](~/samples-eCommerceCatalogModeration/Fusion/Program.cs?range=38-51)]

## <a name="run-the-program"></a>Het programma uitvoeren

Als u alle bovenstaande stappen hebt gevolgd, verwerkt het programma de verschillende afbeeldingen door een query op de betreffende tags uit te voeren op alle drie de services. Vervolgens worden de afbeeldingen met gegevens van de gevonden tags geüpload naar het beoordelingsprogramma Content Moderator.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een programma gemaakt voor het analyseren van productafbeeldingen om deze te taggen op producttype, waarna een beoordelingsteam kan beslissen of de afbeeldingen al dan niet geschikt zijn. In de volgende zelfstudie wordt meer aandacht besteed aan het beoordelen van gecontroleerde afbeeldingen.

> [!div class="nextstepaction"]
> [Gecontroleerde afbeeldingen beoordelen](./review-tool-user-guide/review-moderated-images.md)

---
title: 'Quickstart: Zoeken naar entiteiten met de Bing Entity Search SDK voor C#'
titleSuffix: Azure Cognitive Services
description: Gebruik deze quickstart om te zoeken naar entiteiten met de Bing Entity Search SDK voor C#.
services: cognitive-services
author: mikedodaro
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: quickstart
ms.date: 02/01/2019
ms.author: v-gedod
ms.openlocfilehash: 6a365ef5421de3ceb31c5cc78a424f786f174ab3
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55861915"
---
# <a name="send-a-search-request-with-the-bing-entity-search-sdk-for-c"></a>Een aanvraag verzenden om te zoeken naar entiteiten met de Bing Entity Search SDK voor C#

Gebruik deze quickstart om te zoeken naar entiteiten met de Bing Entity Search SDK voor C#. Hoewel Bing Entity Search een REST-API heeft die compatibel is met de meeste moderne programmeertalen, biedt de SDK een eenvoudige manier om de service te integreren in uw toepassingen. De broncode voor dit voorbeeld is te vinden op [GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7/BingEntitySearch).


## <a name="prerequisites"></a>Vereisten

* Elke versie van [Visual Studio 2017](https://www.visualstudio.com/downloads/).
* Het [Json.NET](https://www.newtonsoft.com/json)-framework, beschikbaar als NuGet-pakket.
* Als u Linux/MacOS gebruikt, kan deze toepassing worden uitgevoerd met behulp van [Mono](http://www.mono-project.com/).
* Het [NuGet-pakket voor de Bing Nieuws zoeken SDK](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.EntitySearch/1.2.0). Als u dit pakket installeert, worden ook de volgende onderdelen geïnstalleerd:
    * Microsoft.Rest.ClientRuntime
    * Microsoft.Rest.ClientRuntime.Azure
    * Newtonsoft.Json

Als u de Bing Entity Search SDK aan uw project wilt toevoegen, gaat u naar de optie `Manage NuGet Packages` van Solution Explorer in Visual Studio en voegt u pakket `Microsoft.Azure.CognitiveServices.Search.EntitySearch` toe.


[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](../../../../includes/cognitive-services-bing-entity-search-signup-requirements.md)]

## <a name="create-and-initialize-an-application"></a>De toepassing maken en initialiseren

1. Maak een nieuwe C#-console-oplossing in Visual Studio. Voeg vervolgens de volgende code in het hoofdcodebestand in.

    ```csharp
    using System;
    using System.Linq;
    using System.Text;
    using Microsoft.Azure.CognitiveServices.Search.EntitySearch;
    using Microsoft.Azure.CognitiveServices.Search.EntitySearch.Models;
    using Newtonsoft.Json;
    ```

## <a name="create-a-client-and-send-a-search-request"></a>Een client aanmaken en een zoekaanvraag verzenden

2. Maak een nieuwe zoekclient aan. Uw abonnementssleutel toevoegen door het maken van een nieuw `ApiKeyServiceClientCredentials`.

    ```csharp
    var client = new EntitySearchAPI(new ApiKeyServiceClientCredentials("YOUR-ACCESS-KEY"));
    ```

3. Gebruik de functie `Entities.Search()` van de client om te zoeken naar uw query:
    
    ```csharp
    var entityData = client.Entities.Search(query: "Satya Nadella");
    ```

## <a name="get-and-print-an-entity-description"></a>Een beschrijving van een entiteit ophalen en afdrukken

1. Als de API zoekresultaten retourneert, haalt u de belangrijkste entiteit op basis van `entityData` op.

    ```csharp
    var mainEntity = entityData.Entities.Value.Where(thing => thing.EntityPresentationInfo.EntityScenario == EntityScenario.DominantEntity).FirstOrDefault();
    ```

2. De beschrijving van de belangrijkste entiteit afdrukken 

    ```csharp
    Console.WriteLine(mainEntity.Description);
    ```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een web-app met één pagina maken](../tutorial-bing-entities-search-single-page-app.md)

* [Wat is de Bing Entiteiten zoeken-API?](../overview.md )
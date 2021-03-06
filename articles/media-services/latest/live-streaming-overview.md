---
title: Overzicht van Live streamen met Azure Media Services | Microsoft Docs
description: Dit artikel biedt een overzicht van live streamen met Azure Media Services v3.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 02/01/2019
ms.author: juliako
ms.openlocfilehash: e90dd052f6a4af83d2dd794dd405a4700da75bde
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/02/2019
ms.locfileid: "55656331"
---
# <a name="live-streaming-with-azure-media-services-v3"></a>Live streamen met Azure Media Services v3

Azure Media Services kunt u live-evenementen Bied uw klanten op de Azure-cloud. Om te streamen met Media Services uw live-evenementen, moet u het volgende:  

- Een camera die wordt gebruikt om vast te leggen van de live-gebeurtenis.<br/>Bekijk voor setup ideeën [video versnelling met eenvoudige en draagbare gebeurtenisinstelling]( https://link.medium.com/KNTtiN6IeT).
- Een coderingsprogramma voor live video dat signalen vanaf een camera (of een ander apparaat, zoals een laptop) converteert naar een bijdrage feed die wordt verzonden naar Media Services. De feed bijdrage kan signalen met betrekking tot reclame, zoals SCTE 35 markeringen bevatten.<br/>Zie voor een lijst met aanbevolen live streaming-coderingsprogramma's, [live coderingsprogramma's streamen](recommended-on-premises-live-encoders.md). Controleer ook of deze blog bekijken: [Live streaming productie met IB](https://link.medium.com/ttuwHpaJeT).
- Onderdelen in Media Services, waarmee u om op te nemen, preview, inpakken, registreren, versleutelen en uitzenden van de live-gebeurtenis aan uw klanten of aan een CDN voor verdere distributie.

Met Media Services, kunt u profiteren van **dynamische pakketten**, waarmee u preview uit te zenden van uw live streams in [MPEG DASH, HLS en Smooth Streaming-indelingen](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming) van de bijdrage feed te sturen naar de service. De doelgroep kunnen de live stream met een compatibele spelers HLS, DASH of Smooth Streaming worden afgespeeld. U kunt [Azure Media Player](http://amp.azure.net/libs/amp/latest/docs/index.html) in uw web- of mobiele toepassingen voor het leveren van uw stroom in een van deze protocollen.

Media Services kunt u uw inhoud dynamisch wordt versleuteld bezorgen (**dynamische versleuteling**) met Advanced Encryption Standard (AES-128) of een van de drie belangrijkste digital rights management (DRM)-systemen: Microsoft PlayReady, Google Widevine en FairPlay van Apple. Media Services biedt ook een service voor het leveren van AES-sleutels en DRM-licenties naar geautoriseerde clients. Zie voor meer informatie over het versleutelen van uw inhoud met Media Services [overzicht inhoud beveiligen](content-protection-overview.md)

U kunt ook dynamische filteren, die kunnen worden gebruikt voor het beheren van de aantal sporen te wissen, indelingen bitsnelheden, en presentatie tijdvensters die worden verzonden naar de spelers toepassen. Zie voor meer informatie, [Filters en dynamische manifesten](filters-dynamic-manifest-overview.md).

In dit artikel biedt een overzicht en richtlijnen voor live streamen met Media Services.

## <a name="prerequisites"></a>Vereisten

Voor meer informatie over de werkstroom voor live streaming in Media Services v3, die u moet controleren en begrijpen van de volgende concepten: 

- [Streaming-eindpunten](streaming-endpoint-concept.md)
- [Live-evenementen en Live-uitvoer](live-events-outputs-concept.md)

## <a name="live-streaming-workflow"></a>Werkstroom voor live streaming

Hier volgen de stappen voor een live streaming-werkstroom:

1. In Media Services-account, zorg ervoor dat de **Streaming-eindpunt** wordt uitgevoerd. 
2. Maak een [Live evenement](live-events-outputs-concept.md). <br/>Bij het maken van de gebeurtenis, kunt u op automatisch starten het. U kunt ook kun u de gebeurtenis wanneer u klaar bent om te streamen.<br/> Wanneer autostart is ingesteld op true, wordt de Live gebeurtenis wordt gestart rechts nadat is gemaakt. De facturering begint zodra de Live gebeurtenis wordt gestart. U moet stoppen expliciet aanroepen op de bron van de Live gebeurtenis verder facturering is gestopt. Zie voor meer informatie, [Live gebeurtenis Staten en facturering](live-event-states-billing.md).
3. De opname-URL's ophalen en uw on-premises coderingsprogramma voor het gebruik van de URL voor het verzenden van de bijdrage feed configureren.<br/>Zie [aanbevolen live coderingsprogramma's](recommended-on-premises-live-encoders.md).
4. De voorbeeld-URL ophalen en deze gebruiken om te controleren dat de invoer van het coderingsprogramma daadwerkelijk worden ontvangen.
5. Maak een nieuwe **Asset** object.
6. Maak een **uitvoer Live** en gebruikt u de assetnaam van de die u hebt gemaakt.<br/>De **uitvoer Live** worden gearchiveerd de stroom in de **Asset**.
7. Maak een **Streaming-Locator gemaakt** met de ingebouwde **beleid Streaming** typen.<br/>Als u van plan bent uw inhoud coderen, raadpleegt u [Content protection overzicht](content-protection-overview.md).
8. Lijst van de paden op de **Streaming-Locator gemaakt** om terug te gaan de URL's te gebruiken (dit zijn deterministische).
9. Ophalen van de hostnaam voor de **Streaming-eindpunt** u wilt streamen uit.
10. De URL in stap 8 worden gecombineerd met de hostnaam in stap 9 om op te halen van de volledige URL.
11. Als u wilt stoppen met het maken van uw **Live gebeurtenis** zichtbaar, dat u wilt stoppen met het streamen van de gebeurtenis en verwijderen de **Streaming-Locator gemaakt**.

## <a name="other-important-articles"></a>Andere belangrijke artikelen

- [Aanbevolen live coderingsprogramma 's](recommended-on-premises-live-encoders.md)
- [Met behulp van een cloud-DVR](live-event-cloud-dvr.md)
- [Live gebeurtenistypen vergelijking van functies](live-event-types-comparison.md)
- [Statussen en facturering](live-event-states-billing.md)
- [Latentie](live-event-latency.md)

## <a name="next-steps"></a>Volgende stappen

* [Zelfstudie voor live streaming](stream-live-tutorial-with-api.md)
* [Hulp bij de migratie voor het verplaatsen van Media Services v2 naar v3](migrate-from-v2-to-v3.md)

---
title: Filters maken met Azure Media Services REST-API | Microsoft Docs
description: In dit onderwerp wordt beschreven hoe u filters maken, zodat de client voor het specifieke secties van de stroom van een stroom gebruiken kunt. Media Services wordt gemaakt dynamische manifesten voor het bereiken van deze selectief streaming.
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
ms.date: 12/17/2018
ms.author: juliako
ms.openlocfilehash: 32b9664d12d6fe3a44329665c730dbc8709430f2
ms.sourcegitcommit: e68df5b9c04b11c8f24d616f4e687fe4e773253c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/20/2018
ms.locfileid: "53650838"
---
# <a name="creating-filters-with-media-services-rest-api"></a>Filters maken met Media Services REST API

Wanneer uw inhoud levert aan klanten (streaming Live gebeurtenissen of Video on Demand) is de client mogelijk meer flexibiliteit dan wat wordt beschreven in het manifestbestand van de standaard-asset. Azure Media Services kunt u accountfilters en filters actief voor uw inhoud definiëren. Zie voor meer informatie, [Filters en dynamische manifesten](filters-dynamic-manifest-overview.md).

Dit onderwerp wordt beschreven hoe u een filter te definiëren voor een Video op aanvraag asset en REST-API's gebruiken om te maken [Accountfilters](https://docs.microsoft.com/rest/api/media/accountfilters) en [Asset Filters](https://docs.microsoft.com/rest/api/media/assetfilters). 

## <a name="prerequisites"></a>Vereisten 

Als u wilt de in dit onderwerp beschreven stappen hebt voltooid, hebt u naar:

- Beoordeling [Filters en dynamische manifesten](filters-dynamic-manifest-overview.md).
- [Postman configureren voor Azure Media Services REST API-aanroepen](media-rest-apis-with-postman.md).

    Zorg ervoor dat u de laatste stap in het onderwerp [Azure AD Token ophalen](media-rest-apis-with-postman.md#get-azure-ad-token). 

## <a name="define-a-filter"></a>Definieer een filter  

Hieronder volgt de **aanvraagtekst** voorbeeld waarin de track selectie voorwaarden die zijn toegevoegd aan het manifest. Dit filter bevat alle audionummers die EG-3 en de video nummers die bitrate in de 0-1000000 hebben bereik.

```json
{
    "properties": {
        "tracks": [
          {
                "trackSelections": [
                    {
                        "property": "Type",
                        "value": "Audio",
                        "operation": "Equal"
                    },
                    {
                        "property": "FourCC",
                        "value": "EC-3",
                        "operation": "Equal"
                    }
                ]
            },
            {
                "trackSelections": [
                    {
                        "property": "Type",
                        "value": "Video",
                        "operation": "Equal"
                    },
                    {
                        "property": "Bitrate",
                        "value": "0-1000000",
                        "operation": "Equal"
                    }
                ]
            }
        ]
     }
}
```

## <a name="create-account-filters"></a>Accountfilters maken

Selecteer in van de Postman-verzameling die u hebt gedownload, **Accountfilters**->**maken of bijwerken van een Account-Filter**.

De **plaatsen** HTTP-aanvraagmethode is vergelijkbaar met:

```
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Media/mediaServices/{accountName}/accountFilters/{filterName}?api-version=2018-07-01
```

Selecteer de **hoofdtekst** tabblad en plak de json-code u [eerder hebt gedefinieerd](#define-a-filter).

Selecteer **Verzenden**. 

Het filter is gemaakt.

Zie voor meer informatie, [maken of bijwerken](https://docs.microsoft.com/rest/api/media/accountfilters/createorupdate). Zie ook [JSON-voorbeelden voor filters](https://docs.microsoft.com/rest/api/media/accountfilters/createorupdate#create_an_account_filter).

## <a name="create-asset-filters"></a>Asset-filters maken  

Selecteer in de 'mediaservices v3' Postman-verzameling die u hebt gedownload, **activa**->**maken of bijwerken Asset Filter**.

De **plaatsen** HTTP-aanvraagmethode is vergelijkbaar met:

```
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Media/mediaServices/{accountName}/assets/{assetName}/assetFilters/{filterName}?api-version=2018-07-01
```

Selecteer de **hoofdtekst** tabblad en plak de json-code u [eerder hebt gedefinieerd](#define-a-filter).

Selecteer **Verzenden**. 

Het filter asset is gemaakt.

Zie voor meer informatie over het maken of bijwerken van de asset filters [maken of bijwerken](https://docs.microsoft.com/rest/api/media/assetfilters/createorupdate). Zie ook [JSON-voorbeelden voor filters](https://docs.microsoft.com/rest/api/media/assetfilters/createorupdate#create_an_asset_filter). 

## <a name="next-steps"></a>Volgende stappen

[Stream-video 's](stream-files-tutorial-with-rest.md) 

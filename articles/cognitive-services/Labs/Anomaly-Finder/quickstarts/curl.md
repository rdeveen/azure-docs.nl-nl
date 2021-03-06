---
title: De Anomaliedetectie Finder-API gebruiken met cURL - Microsoft Cognitive Services | Microsoft Docs
description: Informatie om u te helpen snel aan de slag met cURL en de Finder-API voor Afwijkingsdetectie in Cognitive Services.
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.subservice: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: chliang
ms.openlocfilehash: e055bff430c0d090b4d527a79a22922da2eb90d0
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55207052"
---
# <a name="use-the-anomaly-finder-api-with-curl"></a>De Anomaliedetectie Finder-API gebruiken met cURL

[!INCLUDE [PrivatePreviewNote](../../../../../includes/cognitive-services-anomaly-finder-private-preview-note.md)]

Dit artikel bevat informatie en voorbeelden van code om u te helpen snel aan de slag met de Finder-API voor Afwijkingsdetectie cURL taak van het resultaat van de anomaliedetectie van time series-gegevens ophalen uit te voeren.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="getting-anomaly-points-with-the-anomaly-finder-api-using-curl"></a>Anomaliedetectie punten ophalen met de API voor Afwijkingsdetectie Finder met cURL 

[!INCLUDE [DataContract](../includes/datacontract.md)]

### <a name="example-of-time-series-data"></a>Voorbeeld van time series-gegevens

Het voorbeeld van de tijd gegevenspunten van reeks is als volgt.

[!INCLUDE [Request](../includes/request.md)]

### <a name="analyze-data-and-get-anomaly-points-curl-example"></a>Gegevens analyseren en anomaliedetectie punten cURL voorbeeld

De stappen van het gebruik van het voorbeeld zijn er als volgt uit.

1. Vervang de waarde `[YOUR_SUBSCRIPTION_KEY]` door de geldige abonnementssleutel.
2. Vervang de `[YOUR_REGION]` gebruik van de locatie waar u de abonnementssleutels van uw hebt verkregen.
3. Vervang de `[REPLACE_WITH_THE_EXAMPLE_OR_YOUR_OWN_DATA_POINTS]` met het voorbeeld of uw eigen gegevenspunten.
4. Uitvoeren en controleren van het antwoord.

```cURL

curl -v -X POST "https://api.labs.cognitive.microsoft.com/anomalyfinder/v1.0/anomalydetection"
-H "Content-Type: application/json"
-H "Ocp-Apim-Subscription-Key: [YOUR_SUBSCRIPTION_KEY]"
--data-ascii "[REPLACE_WITH_THE_EXAMPLE_OR_YOUR_OWN_DATA_POINTS]" 

```

### <a name="example-response"></a>Voorbeeld van een antwoord
Een geslaagd antwoord wordt geretourneerd in JSON-indeling. Voorbeeld van een antwoord is als volgt: [!INCLUDE [Response](../includes/response.md)]

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Naslaginformatie over REST API](https://dev.labs.cognitive.microsoft.com/docs/services/anomaly-detection/operations/post-anomalydetection)

---
title: De ouderdom van vooraf gemaakte entiteiten
titleSuffix: Azure
description: In dit artikel bevat leeftijd vooraf gedefinieerde entiteitgegevens in Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 11/27/2018
ms.author: diberry
ms.openlocfilehash: a6aeffb40092252a4e48a200da640f22a149f089
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55867304"
---
# <a name="age-prebuilt-entity-for-a-luis-app"></a>De ouderdom van vooraf gedefinieerde entiteit voor een LUIS-app
De vooraf gedefinieerde leeftijd entiteit bevat de leeftijdwaarde zowel numeriek en in termen van dagen, weken, maanden en jaren. Omdat deze entiteit wordt al getraind, hoeft u niet om toe te voegen van de voorbeeld-uitingen met leeftijd aan de toepassing intents. Leeftijd entiteit wordt ondersteund in [veel culturen](luis-reference-prebuilt-entities.md). 

## <a name="types-of-age"></a>Typen leeftijd
Leeftijd wordt beheerd via de [kenmerken tekst](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml#L3) GitHub-opslagplaats

## <a name="resolution-for-prebuilt-age-entity"></a>Oplossing voor vooraf gedefinieerde leeftijd entiteit
Het volgende voorbeeld ziet u de resolutie van de **builtin.age** entiteit.

```json
{
  "query": "A 90 day old utilities bill is quite late.",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.8236133
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.8236133
    }
  ],
  "entities": [
    {
      "entity": "90 day old",
      "type": "builtin.age",
      "startIndex": 2,
      "endIndex": 11,
      "resolution": {
        "unit": "Day",
        "value": "90"
      }
    }
  ]
}
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de [valuta](luis-reference-prebuilt-currency.md), [datetimeV2](luis-reference-prebuilt-datetimev2.md), en [dimensie](luis-reference-prebuilt-dimension.md) entiteiten. 

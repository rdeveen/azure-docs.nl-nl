---
title: Wat is de Sprekerherkenning-API?
titleSuffix: Azure Cognitive Services
description: Gebruik geavanceerde algoritmen voor sprekercontrole en sprekeridentificatie met de Sprekerherkenning-API in Cognitive Services.
services: cognitive-services
author: dwlin
manager: nitinme
ms.service: cognitive-services
ms.subservice: speaker-recognition
ms.topic: overview
ms.date: 10/01/2018
ms.author: dwlin
ms.openlocfilehash: 86162b4d4e752bc11c0d739f6e343c7a357ea0af
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55873713"
---
# <a name="speaker-recognition-api"></a>Sprekerherkenning-API

Welkom bij de Sprekerherkenning-API's van Azure Cognitive Services. Sprekerherkenning-API's zijn cloud-gebaseerde API's die de meest geavanceerde algoritmen voor sprekercontrole en sprekeridentificatie bieden. Sprekerherkenning kan in twee categorieën worden onderverdeeld: sprekercontrole en sprekeridentificatie.


## <a name="speaker-verification"></a>Sprekercontrole

Spraak heeft unieke kenmerken die kunnen worden gebruikt om een persoon te identificeren, net zoals een vingerafdruk.  Gebruik van spraak als een signaal voor scenario's voor toegangsbeheer en verificatie is een nieuw innovatief hulpmiddel, dat in wezen een hoger beveiligingsniveau biedt waarmee de verificatie voor klanten wordt vereenvoudigd.

API's voor sprekercontrole kunnen automatisch gebruikers controleren en verifiëren met behulp van hun stem of spraak.

### <a name="enrollment"></a>Registratie

Registratie voor sprekercontrole is tekstafhankelijk, wat betekent dat sprekers een specifieke wachtwoordzin moeten kiezen die wordt gebruikt tijdens de fasen van inschrijving en verificatie.

Bij de registratie wordt de stem vastgelegd door de spreker een specifieke woordgroep uit te laten spreken, waarna een aantal functies worden uitgepakt en de gekozen woordgroep wordt herkend. De uitgepakte functies en de gekozen woordgroep vormen samen een unieke stemhandtekening.

### <a name="verification"></a>Verificatie

Tijdens verificatie worden een ingevoerde stem en woordgroep vergeleken met de stemhandtekening en woordgroep van de registratie om te controleren of ze al dan niet afkomstig zijn van dezelfde persoon en of de juiste woordgroep wordt uitgesproken.

Raadpleeg de API  [Spreker - Verificatie](https://westus.dev.cognitive.microsoft.com/docs/services/563309b6778daf02acc0a508/operations/563309b7778daf06340c9652) voor meer informatie over sprekercontrole.

## <a name="speaker-identification"></a>Sprekeridentificatie

API's voor sprekeridentificatie kunnen automatisch de persoon identificeren die in een audiobestand praat, uitgaande van een groep mogelijke sprekers. De audio-invoer wordt gekoppeld aan een groep sprekers en als er sprake is van een overeenkomst, wordt de identiteit van de spreker als resultaat gegeven.

Alle sprekers moeten eerst een registratieproces doorlopen om hun stem geregistreerd te krijgen in het systeem en om een stemafdruk te genereren.


### <a name="enrollment"></a>Registratie

Registratie voor sprekeridentificatie is tekstonafhankelijk, wat betekent dat er geen beperkingen zijn voor hetgeen de spreker in de audio zegt. De stem van de spreker wordt vastgelegd en er wordt een aantal functies opgehaald om een unieke stemhandtekening te vormen.


### <a name="recognition"></a>Herkenning

De audio van de onbekende spreker wordt samen met de potentiële groep sprekers tijdens de herkenning opgegeven. De invoerstem wordt met alle sprekers vergeleken om te bepalen wiens stem het is en als er een overeenkomst gevonden, wordt de identiteit van de spreker geretourneerd.

Raadpleeg de API  [Spreker - Identificatie](https://westus.dev.cognitive.microsoft.com/docs/services/563309b6778daf02acc0a508/operations/5645c068e597ed22ec38f42e) voor meer informatie over sprekeridentificatie.

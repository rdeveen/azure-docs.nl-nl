---
title: Azure Monitor voor containers Veelgestelde vragen | Microsoft Docs
description: Azure Monitor voor containers is een oplossing die de status van uw AKS-clusters en exemplaren van de Container in Azure controleert. In dit artikel vindt u antwoorden op veelgestelde vragen.
services: azure-monitor
author: mgoedtel
manager: carmonm
editor: tysonn
ms.service: azure-monitor
ms.topic: article
ms.workload: infrastructure-services
ms.date: 02/18/2019
ms.author: magoedte
ms.openlocfilehash: 27a191bb62ae59aa154167a22c99d3e699f3eb5a
ms.sourcegitcommit: 79038221c1d2172c0677e25a1e479e04f470c567
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/19/2019
ms.locfileid: "56418553"
---
# <a name="azure-monitor-for-containers-frequently-asked-questions"></a>Azure Monitor voor containers Veelgestelde vragen
Dit Microsoft-FAQ is een lijst met veelgestelde vragen over Azure Monitor voor containers. Als u aanvullende vragen over de oplossing hebt, gaat u naar de [discussieforum](https://feedback.azure.com/forums/34192--general-feedback) en plaats uw vraag. Wanneer u een vraag is vaak wordt gevraagd, toevoegen we deze aan dit artikel zodat snel en eenvoudig kunnen worden gevonden.

## <a name="why-am-i-not-seeing-data-if-log-analytics-workspace-is-configured-with-the-free-pricing-tier"></a>Waarom zie ik gegevens als Log Analytics-werkruimte is geconfigureerd met de prijscategorie gratis? 

U hebt de 500 MB bereikt of een dagelijkse limiet voor het beheren van de hoeveelheid gegevens voor het verzamelen van dagelijks opgegeven. Als u wilt controleren en beheren van het gegevensgebruik van uw, Zie [gegevensgebruik en-kosten](../platform/manage-cost-storage.md). 

## <a name="what-are-the-states-of-containers-specified-in-the-containerinventory-table"></a>Wat zijn de status van containers die zijn opgegeven in de tabel ContainerInventory?
De tabel ContainerInventory bevat informatie over containers gestopt en wordt uitgevoerd. De tabel wordt gevuld met een werkstroom in de agent die de docker voor alle containers (wordt uitgevoerd en is gestopt) query's en verzendt die gegevens van de Log Analytics-werkruimte.
 
## <a name="how-do-i-resolve-errors-related-to-missing-subscription-registration-for-microsoftoperationsmanagement"></a>Hoe los ik fouten met betrekking tot **ontbrekende abonnementen registreren voor Microsoft.OperationsManagement**?
De resourceprovider registreren om op te lossen de fout, **Microsoft.OperationsManagement** in het abonnement waarin de werkruimte is gedefinieerd. De documentatie voor hoe u dit doet, kan worden gevonden [hier](../../azure-resource-manager/resource-manager-register-provider-errors.md).

## <a name="does-azure-monitor-for-containers-include-support-for-rbac-enabled-aks-clusters"></a>Omvat Azure Monitor voor containers ondersteuning voor RBAC AKS clusters ingeschakeld?
RBAC ingeschakeld AKS clusters worden momenteel niet ondersteund door de oplossing. De pagina met details van de oplossing kan niet de juiste informatie weergegeven op de blades die gegevens voor deze clusters weergeven.

## <a name="how-do-i-enable-log-collection-for-containers-in-the-kube-system-namespace-through-helm"></a>Hoe schakel ik logboekverzameling voor containers in de naamruimte kube-systeem via Helm?
De logboekverzameling van containers in de naamruimte kube-systeem is standaard uitgeschakeld. Logboekverzameling kan worden ingeschakeld door het instellen van een omgevingsvariabele op de omsagent. Zie voor meer informatie de [Azure Monitor voor containers](https://github.com/helm/charts/tree/master/incubator/azuremonitor-containers) GitHub-pagina. 

## <a name="how-do-i-update-the-omsagent-to-the-latest-released-version"></a>Hoe kan ik de omsagent bijwerken naar de meest recente releaseversie?
Als u wilt weten hoe u de agent bij te werken, Zie [agentbeheer](container-insights-manage-agent.md).

## <a name="how-do-i-enable-multi-line-logging"></a>Hoe kan ik meerdere regels logboekregistratie inschakelen?
Op dit moment meerdere regels logboekregistratie biedt geen ondersteuning voor Azure Monitor voor containers, maar er zijn oplossingen beschikbaar. U kunt alle services om te schrijven in JSON-indeling configureren en Docker/Moby wordt te schrijven als een enkele regel.

U kunt bijvoorbeeld uw logboek verpakken als een JSON-object, zoals wordt weergegeven in het voorbeeld hieronder voor een voorbeeld van een node.js-toepassing:

```
console.log(json.stringify({ 
      "Hello": "This example has multiple lines:",
      "Docker/Moby": "will not break this into multiple lines",
      "and you will receive":"all of them in log analytics",
      "as one": "log entry"
      }));
```

Deze gegevens ziet er als volgt uit in Azure Monitor voor Logboeken wanneer u een query uitvoeren voor:

```
LogEntry : ({“Hello": "This example has multiple lines:","Docker/Moby": "will not break this into multiple lines", "and you will receive":"all of them in log analytics", "as one": "log entry"}

```

Voor een gedetailleerde Kijk op het probleem, raadpleegt u de volgende [link naar github](https://github.com/moby/moby/issues/22920).

## <a name="how-do-i-resolve-azure-active-directory-errors-when-i-enable-live-logs"></a>Hoe kan ik Azure Active Directory-fouten oplossen wanneer ik live Logboeken inschakelen? 
U ziet mogelijk de volgende fout: **Het antwoord op de url die is opgegeven in de aanvraag komt niet overeen met de antwoord-URL's geconfigureerd voor de toepassing: '60b4dec7-5a69-4165-a211-12c40b5c0435'**. De oplossing hiervoor kunt u vinden in het artikel [container logboeken realtime met Azure Monitor voor containers weergeven](container-insights-live-logs.md#configure-aks-with-azure-active-directory). 

## <a name="next-steps"></a>Volgende stappen
Om te beginnen met controleren van uw AKS-cluster, Bekijk [hoe zorgen voor onboarding Azure controleren voor containers](container-insights-onboard.md) om te begrijpen van de vereisten en de beschikbare methoden bewaking wilt inschakelen. 
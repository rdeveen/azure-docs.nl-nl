---
title: OS-patches planning voor clusters in HDInsight op basis van Linux - Azure configureren
description: Informatie over het configureren van OS-patches plannen voor Linux gebaseerde HDInsight-clusters.
services: hdinsight
author: omidm1
ms.author: omidm
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/24/2019
ms.openlocfilehash: ef57608d092c05b30be63a54bb41ba87558eabc3
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/04/2019
ms.locfileid: "55694615"
---
# <a name="os-patching-for-hdinsight"></a>OS-patches voor HDInsight 

> [!IMPORTANT]
> Ubuntu-installatiekopieën beschikbaar voor nieuwe HDInsight-cluster maken binnen drie maanden van het worden gepubliceerd. Vanaf januari 2019, actieve clusters zijn **niet** automatisch gevuld. Klanten moeten scriptacties of andere methoden gebruiken voor het vullen van een actief cluster. Nieuwe clusters hebben altijd de meest recente updates, met inbegrip van de meest recente beveiligingspatches.

## <a name="how-to-configure-the-os-patching-schedule-for-linux-based-hdinsight-clusters"></a>Het configureren van de OS-patches plannen voor Linux gebaseerde HDInsight-clusters
De virtuele machines in een HDInsight-cluster moet opnieuw worden opgestart af en toe zodat belangrijke beveiligingspatches kunnen worden geïnstalleerd. 

Met behulp van de scriptactie die wordt beschreven in dit artikel, kunt u de OS-patches schema als volgt wijzigen:
1. In- of uitschakelen van automatisch opnieuw wordt opgestart
2. Stel de frequentie van opnieuw wordt opgestart (dagen tussen het opnieuw opstarten)
3. Stelt u de dag van de week wanneer opnieuw worden opgestart plaatsvindt

> [!NOTE]  
> Deze scriptactie werkt alleen met HDInsight op basis van Linux-clusters die zijn gemaakt na 1 augustus 2016. Patches worden van kracht wanneer virtuele machines opnieuw zijn opgestart. 

## <a name="how-to-use-the-script"></a>Het gebruik van het script 

Met dit script vereist wanneer de volgende gegevens:
1. Locatie van het script: https://hdiconfigactions.blob.core.windows.net/linuxospatchingrebootconfigv01/os-patching-reboot-config.sh.  HDInsight maakt gebruik van deze URI om te zoeken en voer het script uit op alle virtuele machines in het cluster.
  
2. Knooppunt met de clustertypen waarmee het script wordt toegepast op: het hoofdknooppunt, workernode, zookeeper. Met dit script moet worden toegepast op alle typen in het cluster. Als deze niet op een knooppunttype toegepast wordt, wordt de virtuele machines voor dat knooppunttype blijven om de vorige patch schema te gebruiken.


3.  Parameter: Met dit script worden drie parameters die numerieke geaccepteerd:

    | Parameter | Definitie |
    | --- | --- |
    | Automatisch opnieuw wordt opgestart in-of uitschakelen |0 of 1. De waarde 0 wordt automatisch opnieuw wordt opgestart uitgeschakeld terwijl 1 schakelt u automatisch opnieuw wordt opgestart. |
    | Frequentie |7 tot 90 (inclusief). Het aantal dagen moet worden gewacht voordat opnieuw opstarten van de virtuele machines voor patches die worden opgestart moeten. |
    | Dag van week |1 tot en met 7 (inclusief). Een waarde van 1 geeft aan dat het opnieuw opstarten moet worden uitgevoerd op een maandag en 7 geeft een voorbeeld van Sunday.For, met behulp van parameters van 1 60 2 resulteert in een automatisch opnieuw wordt opgestart elke 60 dagen (maximaal) op dinsdag. |
    | Persistentie |Bij het toepassen van een scriptactie aan een bestaand cluster, kunt u het script als persistent markeren. Persistente scripts worden toegepast wanneer nieuwe workernodes worden toegevoegd aan het cluster via het schalen herverdelen. |

> [!NOTE]  
> U kunt dit script moet markeren als behouden bij het toepassen van aan een bestaand cluster. Nieuwe knooppunten die zijn gemaakt via vergroten / verkleinen Gebruik anders de standaardwaarde toepassen van patches planning.  Als u het script als onderdeel van het proces voor het maken van cluster toepast, worden deze automatisch opgeslagen.

## <a name="next-steps"></a>Volgende stappen

Voor specifieke stappen over het gebruik van de scriptactie, Zie de volgende secties in het [aanpassen Linux gebaseerde HDInsight-clusters met script action](hdinsight-hadoop-customize-cluster-linux.md):

* [Een scriptactie tijdens het maken van clusters gebruiken](hdinsight-hadoop-customize-cluster-linux.md#use-a-script-action-during-cluster-creation)
* [Een scriptactie toepassen op een actief cluster](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster)

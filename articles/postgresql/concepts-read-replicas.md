---
title: Leesreplica's in Azure Database for PostgreSQL
description: Dit artikel beschrijft lezen-replica's in Azure Database voor PostgreSQL.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 02/01/2019
ms.openlocfilehash: 270231b2ad7d94789595cfa4e681cf6c2b0f0541
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/02/2019
ms.locfileid: "55657872"
---
# <a name="read-replicas-in-azure-database-for-postgresql"></a>Lezen-replica's in Azure Database for PostgreSQL

> [!IMPORTANT]
> De functie lezen replica is beschikbaar als openbare preview.

De replica voor lezen-functie kunt u voor het repliceren van gegevens uit een Azure Database for PostgreSQL-server (master) op maximaal vijf alleen-lezen-servers (lezen replica's) binnen dezelfde Azure-regio. Lezen-replica's worden asynchroon bijgewerkt met de PostgreSQL-engine systeemeigen Replicatietechnologie.

Replica's zijn nieuwe servers die kunnen worden beheerd op vergelijkbare wijze als normale zelfstandige Azure Database for PostgreSQL-servers. Voor elke leesreplica worden kosten in rekening gebracht voor de ingerichte compute in vCores en ingerichte opslag in GB/maand.

Ga naar de [procedures pagina voor informatie over het maken en beheren van replica's](howto-read-replicas-portal.md).

## <a name="when-to-use-read-replicas"></a>Wanneer u meer replica 's
De functie lezen replica is gericht op helpen verbeteren de prestaties en schaal van lees-intensieve werkbelastingen. Bijvoorbeeld, kunnen de lees-workloads worden geïsoleerd in de replica's, terwijl schrijven workloads kunnen worden omgeleid naar het hoofdniveau.

Een veelvoorkomend scenario is dat BI en analytische werkbelastingen gebruiken de lezen replica als de gegevensbron voor rapportage.

Omdat de replica's zijn alleen-lezen, ze schrijven-capaciteit Bezig te houden in het model niet rechtstreeks verlichten en dus deze functie niet is gericht op schrijven-intensieve workloads.

De functie lezen replica maakt gebruik van de asynchrone replicatie van de PostgreSQL en dus is niet bedoeld voor scenario's met synchrone replicatie. Er is een meetbare vertraging tussen het model en de replica. De gegevens op de replica wordt uiteindelijk consistent zijn met de gegevens op de hoofddoelserver. Gebruik deze functie voor workloads die deze vertraging aankan.

## <a name="creating-a-replica"></a>Het maken van een replica
De hoofd-server moet de **azure.replication_support** ingesteld op de REPLICA. Wijzigen van deze parameter vereist een herstart van de server moet zijn van kracht. (**Azure.replication_support** parameter is geldig voor algemeen gebruik en geoptimaliseerd voor geheugen-lagen).

Wanneer u de werkstroom van de replica maken hebt gestart, wordt een lege Azure Database for PostgreSQL-server gemaakt. De nieuwe server is gevuld met de gegevens op de hoofd-server. De tijd die nodig zijn om de nieuwe replica te maken, is afhankelijk van de hoeveelheid gegevens op de hoofd- en de tijd sinds de laatste wekelijkse volledige back-up. Dit kan variëren van enkele minuten tot enkele uren.

De functie lezen replica maakt gebruik van PostgreSQL fysieke replicatie (geen logische replicatie). Streaming-replicatie via replicatie sleuven is de standaardmodus voor de bewerking. Indien nodig, worden back-upfunctie wordt gebruikt voor bijwerken.

> [!NOTE]
> Als u nog geen een waarschuwing instellen voor opslag van op uw servers die het is raadzaam dat u dit doet, zodat u wanneer er een server op de hoogte nadert de limiet voor opslag, omdat dit heeft invloed op de replicatie.

[Informatie over het maken van een replica lezen in de Azure-portal](howto-read-replicas-portal.md).

## <a name="connecting-to-a-replica"></a>Verbinding maken met een replica
Wanneer u een replica maakt, is het niet overgenomen de firewall-regels of VNet-service-eindpunt van de hoofd-server. Deze regels moeten afzonderlijk worden ingesteld voor de replica.

De replica neemt de beheerdersaccount van de hoofd-server. Alle gebruikersaccounts op de hoofd-server worden gerepliceerd naar de lezen-replica's. U kunt alleen verbinding maken met een lezen-replica met behulp van de gebruikersaccounts die beschikbaar op de hoofd-server.

U kunt verbinding maken met de replica met behulp van de hostnaam en een geldig gebruikersaccount, zoals u zou voor een gewone Azure Database voor PostgreSQL-server doen. Bijvoorbeeld, als de naam van de server myreplica is en de beheerdersgebruikersnaam myadmin is u verbinding mee kan maken van psql als volgt:

```
psql -h myreplica.postgres.database.azure.com -U myadmin@myreplica -d postgres
```
en bij de prompt voert u het wachtwoord voor het gebruikersaccount.

## <a name="monitoring-replication"></a>Controleren van de replicatie
Er is een **Max Lag voor replica's** metrische gegevens beschikbaar zijn in Azure Monitor. Met deze metriek is beschikbaar op de hoofdserver. De metrische gegevens geeft de vertraging in bytes tussen de hoofd- en de meeste achtergebleven replica. 

We bieden ook een **Replica Lag** metrische gegevens in Azure Monitor. Met deze metriek is beschikbaar voor alleen replica's. 

De metrische gegevens wordt berekend op basis van de weergave pg_stat_wal_receiver:

```SQL
EXTRACT (EPOCH FROM now() - pg_last_xact_replay_timestamp())
```
Houd er rekening mee dat de Replica Lag metrische gegevens de tijd geeft sinds de laatste replay transactie. Als er geen transacties plaatsvinden in uw model, geeft de metriek deze vertraging.

Is het raadzaam dat u instelt dat een waarschuwing waarmee u wordt geïnformeerd wanneer de vertraging replica een waarde die niet acceptabel voor uw workload is bereikt. 

Query uitvoeren op de hoofd-server rechtstreeks voor de vertraging van replicatie in bytes op alle replica's voor meer inzicht.
On PG 10:
```SQL
select pg_wal_lsn_diff(pg_current_wal_lsn(), stat.replay_lsn) 
AS total_log_delay_in_bytes from pg_stat_replication;
```

Op de pagina 9.6 en hieronder:
```SQL
select pg_xlog_location_diff(pg_current_xlog_location(), stat.replay_location) 
AS total_log_delay_in_bytes from pg_stat_replication;
```

> [!NOTE]
> Als een master of de replica-server opnieuw is opgestart, wordt de tijd die nodig zijn om te starten en vervolgens achterstand in de Replica Lag metrische gegevens worden weergegeven.

## <a name="stopping-replication-to-a-replica"></a>Replicatie naar een replica stoppen
U kunt kiezen om replicatie tussen een hoofd- en een replica te stoppen. Dit zorgt ervoor dat de replica opnieuw te starten als u wilt verwijderen van de replicatie-instellingen. Wanneer u replicatie tussen een hoofd- en een replica-server is gestopt, wordt de replica-server een zelfstandige server. De gegevens in de zelfstandige server zijn de gegevens die beschikbaar zijn op de replica is op het moment dat de opdracht stop-replicatie is gestart. Deze zelfstandige server met de hoofd-server geen af van.

Deze server kan niet opnieuw worden gemaakt in een replica.

Zorg ervoor dat de replica alle gegevens die u nodig hebt voordat u replicatie beëindigen.

U kunt [meer informatie over het stoppen van een replica in de documentatie met procedures](howto-read-replicas-portal.md).


## <a name="considerations"></a>Overwegingen

### <a name="preparing-for-replica"></a>Voorbereiden op replica
**Azure.replication_support** moet worden ingesteld op REPLICA op de hoofd-server voordat u kunt een replica maken. Wijzigen van deze parameter vereist een herstart van de server moet zijn van kracht. Deze parameter is van toepassing op algemeen gebruik en geoptimaliseerd voor geheugen-lagen.

### <a name="stopped-replicas"></a>Gestopte replica 's
Als u ervoor kiest om replicatie tussen een model en de replica te stoppen, kan de replica wordt opnieuw opgestart als deze wijziging wilt toepassen. De replica wordt dan een lezen / schrijven-server. Daarna wordt niet kan deze worden gemaakt in een replica opnieuw.

### <a name="replicas-are-new-servers"></a>Replica's worden nieuwe servers
Replica's worden gemaakt als nieuwe Azure Database for PostgreSQL-servers. Bestaande servers kunnen niet worden gemaakt in de replica's.

### <a name="replica-server-configuration"></a>Configuratie van de replica-server
Replica-servers worden gemaakt met behulp van de configuraties van dezelfde server als het model, waaronder de volgende configuraties:
- Prijscategorie
- Bewerking voor COMPUTE
- vCores
- Storage
- Bewaarperiode voor back-up
- Optie voor back-redundantie
- PostgreSQL-engine-versie

Nadat een replica is gemaakt, de prijscategorie (met uitzondering van en naar Basic), bewerking voor compute, vCores, opslag en back-up bewaarperiode het kunnen onafhankelijk worden gewijzigd van de hoofd-server.

> [!IMPORTANT]
> Voordat de configuratie van de server van een model is bijgewerkt met nieuwe waarden, moet de configuratie van de replica's worden bijgewerkt naar waarden gelijk zijn aan of groter zijn. Dit zorgt ervoor dat de replica's kunnen blijven van wijzigingen in het model zijn.

In het bijzonder moet Postgres de waarde van de replica voor de parameter max_connections moet groter dan of gelijk zijn aan de waarde van het model anders dat de replica kan niet worden gestart. De max_connections-waarde is ingesteld in Azure Database for PostgreSQL, afhankelijk van de sku. Lees voor meer informatie, [het document limieten](concepts-limits.md). 

Er wordt geprobeerd een update die in strijd is met dit doen zal leiden tot een fout.


### <a name="deleting-the-master"></a>Het model verwijderen
Wanneer een hoofd-server wordt verwijderd, worden alle bijbehorende lezen replica's zelfstandige servers. De replica's zal opnieuw worden opgestart om deze wijziging weer te geven.

### <a name="other"></a>Overige
- Lezen-replica's kunnen alleen worden gemaakt in dezelfde Azure-regio als het model.
- Het maken van een replica van een replica wordt niet ondersteund.

## <a name="next-steps"></a>Volgende stappen
- [Het maken en beheren van replica's in Azure portal lezen](howto-read-replicas-portal.md).

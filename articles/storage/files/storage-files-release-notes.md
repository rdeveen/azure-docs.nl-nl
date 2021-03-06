---
title: Opmerkingen bij de release voor de Azure File Sync-agent | Microsoft Docs
description: Opmerkingen bij de release voor de Azure File Sync-agent.
services: storage
author: wmgries
ms.service: storage
ms.topic: article
ms.date: 2/12/2019
ms.author: wgries
ms.subservice: files
ms.openlocfilehash: 2bbac21b9ac3e07cbb41ea8aa4cf93dcbd636d15
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56181776"
---
# <a name="release-notes-for-the-azure-file-sync-agent"></a>Opmerkingen bij de release voor de Azure File Sync-agent
Met Azure File Sync kunt u bestandsshares van uw organisatie in Azure Files centraliseren zonder in te leveren op de flexibiliteit, prestaties en compatibiliteit van een on-premises bestandsserver. Uw installaties van Windows Server worden getransformeerd in een snelle cache van uw Azure-bestandsshare. U kunt elk protocol dat beschikbaar is in Windows Server gebruiken voor lokale toegang tot uw gegevens (inclusief SMB, NFS en FTPS) en u kunt zoveel caches hebben als u waar ook ter wereld nodig hebt.

Dit artikel bevat de releaseopmerkingen voor de ondersteunde versies van de Azure File Sync-agent.

## <a name="supported-versions"></a>Ondersteunde versies
De volgende versies worden ondersteund voor de Azure File Sync-agent:

| Mijlpaal | Versienummer agent | Releasedatum | Status |
|----|----------------------|--------------|------------------|
| Versie 5 Release - [KB4459989](https://support.microsoft.com/help/4459989)| 5.0.2.0 | 12 februari 2019 | Ondersteunde (aanbevolen versie) |
| Januari 2019 updatepakket - [KB4481059](https://support.microsoft.com/help/4481059)| 4.3.0.0 | 14 januari 2019 | Ondersteund |
| December 2018 update rollup - [KB4459990](https://support.microsoft.com/help/4459990)| 4.2.0.0 | 10 december 2018 | Ondersteund |
| Updatepakket van december 2018 | 4.1.0.0 | 4 december 2018 | Ondersteund |
| V4-Release | 4.0.1.0 | 13 november 2018 | Ondersteund |
| Updatepakket september 2018 | 3.3.0.0 | 24 september 2018 | Ondersteund - verloopt Agent-versie op 19 juli 2019 |
| Updatepakket augustus 2018 | 3.2.0.0 | 15 augustus 2018 | Ondersteund - verloopt Agent-versie op 19 juli 2019 |
| Algemene beschikbaarheid | 3.1.0.0 | 19 juni 2018 | Ondersteund - verloopt Agent-versie op 19 juli 2019 |
| Verlopen agents | 1.1.0.0 - 3.0.13.0 | N/A | Niet ondersteund - Agent-versies is verlopen op 1 oktober 2018 |

### <a name="azure-file-sync-agent-update-policy"></a>Updatebeleid Azure File Sync-agent
[!INCLUDE [storage-sync-files-agent-update-policy](../../../includes/storage-sync-files-agent-update-policy.md)]

## <a name="agent-version-5020"></a>Agentversie 5.0.2.0
De volgende releaseopmerkingen zijn voor versie 5.0.2.0 van de Azure File Sync-agent (vrijgegeven 12 februari 2019).

### <a name="improvements-and-issues-that-are-fixed"></a>Verbeteringen en problemen die zijn opgelost

- Ondersteuning voor Azure Government-cloud
    - Preview-versie is ondersteuning toegevoegd voor de Azure Government-cloud. Hiervoor hebt u een abonnement op technische vermeld en een speciale agent van Microsoft worden gedownload. Voor toegang tot de Preview-versie, stuur een e-mail rechtstreeks [ AzureFiles@microsoft.com ](mailto:AzureFiles@microsoft.com).
- Ondersteuning voor Gegevensontdubbeling
    - Gegevensontdubbeling wordt nu volledig ondersteund met cloud-opslaglagen worden ingeschakeld op Windows Server 2016 en Windows Server 2019. Inschakelen van Ontdubbeling op een volume met cloud-opslaglagen ingeschakeld, kunt u meer bestanden on-premises zonder in te richten meer opslagruimte in de cache.
- Ondersteuning voor de overdracht van offline (bijvoorbeeld via Data Box)
    - Migreer eenvoudig grote hoeveelheden gegevens in Azure File Sync via middelen die u kiest. U kunt Azure Data Box, AzCopy en zelfs de migratieservices van derden. Niet nodig gebruik van zeer grote bandbreedte om uw gegevens in Azure, in het geval van Data Box – gewoon verzenden er! Zie voor meer informatie, [Offline gegevens overdragen Docs](https://aka.ms/AFS/OfflineDataTransfer).
- Verbeterde prestaties
    - Klanten met meerdere servereindpunten op hetzelfde volume mogelijk ondervonden trage prestaties van vóór deze release. Azure File Sync maakt een tijdelijke VSS-momentopname eenmaal per dag op de server om bestanden te synchroniseren met open ingangen. Synchronisatie biedt nu ondersteuning voor meerdere servereindpunten synchroniseren op een volume als een synchronisatiesessie VSS actief is. Niet meer wachten op een VSS synchroniseren sessie om te voltooien, zodat de synchronisatie op de andere servereindpunten op het volume kan worden hervat.
- Verbeterde bewaken in de portal
    - Grafieken zijn toegevoegd in de portal Opslagsynchronisatieservice om weer te geven:
        - Aantal bestanden die zijn gesynchroniseerd
        - Grootte van gegevens die worden overgedragen
        - Aantal bestanden niet synchroniseren
        - Grootte van gegevens die zijn ingetrokken
        - Status van de server-connectiviteit
    - Zie voor meer informatie, [Monitor Azure File Sync](https://docs.microsoft.com/azure/storage/files/storage-sync-files-monitoring).
- Verbeterde schaalbaarheid en betrouwbaarheid
    - Maximum aantal bestandssysteemobjecten (mappen en bestanden) in een map is verhoogd tot 1.000.000. Vorige limiet is 200.000.
    - Synchronisatie wilt hervatten van de overdracht van gegevens in plaats van nieuwe verzenden wanneer een overdracht is onderbroken voor grote bestanden 

### <a name="evaluation-tool"></a>Hulpprogramma voor het evalueren
Voordat u Azure File Sync implementeert, moet u evalueren of het compatibel is met het systeem met behulp van het hulpprogramma voor het evalueren van Azure File Sync. Dit hulpprogramma is een Azure PowerShell-cmdlet waarmee wordt gecontroleerd op mogelijke problemen met het bestandssysteem en de gegevensset, zoals niet-ondersteunde tekens of een niet-ondersteunde versie van het besturingssysteem. Zie voor de installatie en instructies over het gebruik, [evaluatieprogramma](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning#evaluation-tool) sectie in de handleiding voor capaciteitsplanning. 

### <a name="agent-installation-and-server-configuration"></a>Agentinstallatie en serverconfiguratie
Zie voor meer informatie over het installeren en configureren van de Azure File Sync-agent met Windows Server [Planning voor de implementatie van een Azure File Sync](storage-sync-files-planning.md) en [over het implementeren van Azure File Sync](storage-sync-files-deployment-guide.md).

- Het installatiepakket voor de agent moet worden geïnstalleerd met machtigingen van de opdrachtprompt met verhoogde bevoegdheid (beheerder).
- De agent wordt niet ondersteund op Windows Server Core of Nano Server-implementatie-opties.
- De agent wordt alleen ondersteund op Windows Server 2019, Windows Server 2016 en Windows Server 2012 R2.
- De agent vereist ten minste 2 GiB geheugen. Als de server wordt uitgevoerd in een virtuele machine met dynamisch geheugen is ingeschakeld, kan de virtuele machine moet worden geconfigureerd met een minimaal 2048 MiB van het geheugen.
- De service opslag-Sync-Agent (FileSyncSvc) biedt geen ondersteuning voor servereindpunten die zich op een volume dat de map system volume information (SVI is) gecomprimeerd. Deze configuratie zal leiden tot onverwachte resultaten.
- FIPS-modus wordt niet ondersteund en moet worden uitgeschakeld. 

### <a name="interoperability"></a>Interoperabiliteit
- Antivirusprogramma's, back-uptoepassingen en andere toepassingen die toegang hebben tot gelaagde bestanden, kunnen leiden tot ongewenste intrekking tenzij ze het kenmerk offline respecteren en het lezen van de inhoud van die bestanden overslaan. Zie voor meer informatie, [problemen met Azure File Sync](storage-sync-files-troubleshoot.md).
- Bestandsserverbronbeheer (FSRM) bestandscontroles kunnen eindeloze synchronisatiefouten veroorzaken wanneer bestanden worden geblokkeerd vanwege de bestandscontrole.
- Sysprep uitgevoerd op een server met de Azure File Sync-agent is geïnstalleerd, wordt niet ondersteund en kan leiden tot onverwachte resultaten. De Azure File Sync-agent moet worden geïnstalleerd na het implementeren van de server-installatiekopie en sysprep mini-installatie te voltooien.

### <a name="sync-limitations"></a>Synchronisatiebeperkingen
De volgende items worden niet gesynchroniseerd, maar de rest van het systeem blijft normaal functioneren:
- Bestanden met niet-ondersteunde tekens. Zie [Troubleshooting guide](storage-sync-files-troubleshoot.md#handling-unsupported-characters) voor lijst met niet-ondersteunde tekens.
- Bestanden of mappen die eindigen op een punt.
- Paden langer dan 2.048 tekens.
- Het gedeelte met de discretionaire ACL (Access Control List) van een security descriptor als dit groter is dan 2 kB. (Dit probleem geldt alleen wanneer er meer dan ongeveer 40 vermeldingen voor toegangsbeheer (ACE's) bestaan voor één item.)
- Het gedeelte met de SACL (System Access Control List) van een security descriptor die wordt gebruikt voor controle.
- Uitgebreide kenmerken.
- Alternatieve gegevensstromen.
- Reparse-punten.
- Vaste koppelingen.
- Compressie (indien ingesteld op een serverbestand) blijft niet behouden wanneer wijzigingen vanuit andere eindpunten naar dat bestand worden gesynchroniseerd.
- Elk bestand dat is gecodeerd met EFS (of een andere versleuteling in de gebruikersmodus) dat voorkomt dat de service de gegevens leest.

    > [!Note]  
    > Gegevens die onderweg zijn tussen eindpunten worden altijd versleuteld door Azure File Sync. Inactieve gegevens (data-at-rest) worden altijd versleuteld in Azure.
 
### <a name="server-endpoint"></a>Servereindpunt
- Een servereindpunt kan alleen worden gemaakt op een NTFS-volume. ReFS, FAT, FAT32 en andere bestandssystemen worden op dit moment niet ondersteund door Azure File Sync.
- Gelaagde bestanden wordt niet meer toegankelijk als de bestanden niet ingetrokken worden voordat u het servereindpunt verwijdert. Als u toegang tot de bestanden herstellen, maakt u het servereindpunt opnieuw. Als de 30 dagen zijn verstreken sinds het servereindpunt is verwijderd of als het cloudeindpunt is verwijderd, wordt gelaagde bestanden die niet zijn ingetrokken onbruikbaar worden.
- Cloud tiering wordt niet ondersteund op het systeemvolume. Uitschakelen voor het maken van een servereindpunt op het systeemvolume, cloud-opslaglagen bij het maken van het servereindpunt.
- Failoverclustering wordt alleen ondersteund met geclusterde schijven, maar niet met CSV's (Cluster Shared Volume).
- Een servereindpunt kan niet worden genest. Een eindpunt van dit type kan zich samen met een ander eindpunt op hetzelfde volume bevinden.
- Sla geen Besturingssysteembestand of wisselbestand van de toepassing binnen een eindpunt-serverlocatie.
- De naam van de server in de portal is niet bijgewerkt als de naam van de server is gewijzigd.

### <a name="cloud-endpoint"></a>Cloud-eindpunt
- Azure File Sync ondersteunt rechtstreeks aanbrengen van wijzigingen in de Azure-bestandsshare. Alle wijzigingen in de Azure-bestandsshare moeten echter eerst moeten worden gedetecteerd door een Azure File Sync-taak wijzigen detectie. Een wijziging detectie-taak wordt gestart voor een cloudeindpunt om de 24 uur. Bovendien wijzigingen aangebracht in een Azure-bestandsshare via de REST-protocol wordt niet bijgewerkt door de SMB-tijdstip laatst gewijzigd en zal niet worden gezien als een wijziging door synchronisatie.
- De opslagsynchronisatieservice en/of de storage-account kan worden verplaatst naar een andere resourcegroep of abonnement binnen de bestaande Azure AD-tenant. Als het opslagaccount is verplaatst, moet u de hybride File Sync-Service toegang geven tot het opslagaccount (Zie [Zorg ervoor dat Azure File Sync heeft toegang tot het opslagaccount](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cportal#troubleshoot-rbac)).

    > [!Note]  
    > Azure File Sync biedt geen ondersteuning voor het verplaatsen van het abonnement naar een andere Azure AD-tenant.

### <a name="cloud-tiering"></a>Cloudopslaglagen
- Als een gelaagd bestand met behulp van Robocopy naar een andere locatie wordt gekopieerd, wordt het resulterende bestand niet in een laag geplaatst. Het kenmerk 'offline' kan zijn ingesteld omdat Robocopy dat kenmerk onterecht opneemt in kopieerbewerkingen.
- Bij het kopiëren van bestanden met behulp van robocopy, moet u de /MIR-optie gebruiken om het bestand tijdstempels behouden. Dit zorgt ervoor dat oudere bestanden eerder dan recent geopende bestanden worden geschakeld.
- Wanneer u bestandseigenschappen bekijkt vanuit een SMB-client, lijkt het misschien of het kenmerk 'offline' niet goed is ingesteld als gevolg van SMB-caching van bestandsmetagegevens.

## <a name="agent-version-4300"></a>Agentversie 4.3.0.0
De volgende releaseopmerkingen zijn voor versie 4.3.0.0 van de Azure File Sync-agent die zijn uitgebracht op 14 januari 2019. Deze opmerkingen zijn naast de releaseopmerkingen voor versie 4.0.1.0.

Lijst met problemen opgelost in deze release:  
- Bestanden niet worden geschakeld nadat de Azure File Sync-agent is bijgewerkt naar versie 4.x.
- AfsUpdater.exe wordt nu ondersteund op Windows Server 2019.
- Verbeteringen van de diverse betrouwbaarheid voor synchronisatie. 

## <a name="agent-version-4200"></a>Agentversie 4.2.0.0
De volgende releaseopmerkingen zijn voor versie 4.2.0.0 van de Azure File Sync-agent die zijn uitgebracht 10 December 2018. Deze opmerkingen zijn naast de releaseopmerkingen voor versie 4.0.1.0.

Lijst met problemen opgelost in deze release:  
- Een fout stoppen 0x3B of de fout stoppen 0x1E kan optreden wanneer een VSS-momentopname wordt gemaakt.  
- Een geheugenlek optreden wanneer cloud tiering is ingeschakeld  

## <a name="agent-version-4100"></a>Agentversie 4.1.0.0
De volgende releaseopmerkingen zijn voor versie 4.1.0.0 van de Azure File Sync-agent die zijn uitgebracht 4 December 2018. Deze opmerkingen zijn naast de releaseopmerkingen voor versie 4.0.1.0.

Lijst met problemen opgelost in deze release:  
- De server reageert vanwege een geheugenlek cloud-opslaglagen.  
- De installatie van agent mislukt met de volgende fout: Fout bij het 1921. Service-opslag Sync-Agent' (FileSyncSvc) kan niet worden gestopt.  Controleer of u voldoende bevoegdheden voor het stoppen van systeemservices.  
- De service opslag-Sync-Agent (FileSyncSvc) loopt vast bij het geheugengebruik hoog is.  
- Verbeteringen van de diverse betrouwbaarheid voor cloud-opslaglagen en synchroniseren.

## <a name="agent-version-4010"></a>Agentversie 4.0.1.0
De volgende releaseopmerkingen zijn voor versie 4.0.1.0 van de Azure File Sync-agent (vrijgegeven 13 November 2018).

### <a name="evaluation-tool"></a>Hulpprogramma voor het evalueren
Voordat u Azure File Sync implementeert, moet u evalueren of het compatibel is met het systeem met behulp van het hulpprogramma voor het evalueren van Azure File Sync. Dit hulpprogramma is een Azure PowerShell-cmdlet waarmee wordt gecontroleerd op mogelijke problemen met het bestandssysteem en de gegevensset, zoals niet-ondersteunde tekens of een niet-ondersteunde versie van het besturingssysteem. Zie voor de installatie en instructies over het gebruik, [evaluatieprogramma](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning#evaluation-tool) sectie in de handleiding voor capaciteitsplanning. 

### <a name="agent-installation-and-server-configuration"></a>Agentinstallatie en serverconfiguratie
Zie voor meer informatie over het installeren en configureren van de Azure File Sync-agent met Windows Server [Planning voor de implementatie van een Azure File Sync](storage-sync-files-planning.md) en [over het implementeren van Azure File Sync](storage-sync-files-deployment-guide.md).

- Het installatiepakket voor de agent moet worden geïnstalleerd met machtigingen van de opdrachtprompt met verhoogde bevoegdheid (beheerder).
- De agent wordt niet ondersteund op Windows Server Core of Nano Server-implementatie-opties.
- De agent wordt alleen ondersteund op Windows Server 2019, Windows Server 2016 en Windows Server 2012 R2.
- De agent vereist ten minste 2 GiB geheugen. Als de server wordt uitgevoerd in een virtuele machine met dynamisch geheugen is ingeschakeld, kan de virtuele machine moet worden geconfigureerd met een minimaal 2048 MiB van het geheugen.
- De service opslag-Sync-Agent (FileSyncSvc) biedt geen ondersteuning voor servereindpunten die zich op een volume dat de map system volume information (SVI is) gecomprimeerd. Deze configuratie zal leiden tot onverwachte resultaten.
- FIPS-modus wordt niet ondersteund en moet worden uitgeschakeld. 
- Een fout stoppen 0x3B of de fout stoppen 0x1E kan optreden wanneer een VSS-momentopname wordt gemaakt.

### <a name="interoperability"></a>Interoperabiliteit
- Antivirusprogramma's, back-uptoepassingen en andere toepassingen die toegang hebben tot gelaagde bestanden, kunnen leiden tot ongewenste intrekking tenzij ze het kenmerk offline respecteren en het lezen van de inhoud van die bestanden overslaan. Zie voor meer informatie, [problemen met Azure File Sync](storage-sync-files-troubleshoot.md).
- Bestandsserverbronbeheer (FSRM) bestandscontroles kunnen eindeloze synchronisatiefouten veroorzaken wanneer bestanden worden geblokkeerd vanwege de bestandscontrole.
- Sysprep uitgevoerd op een server met de Azure File Sync-agent is geïnstalleerd, wordt niet ondersteund en kan leiden tot onverwachte resultaten. De Azure File Sync-agent moet worden geïnstalleerd na het implementeren van de server-installatiekopie en sysprep mini-installatie te voltooien.
- Gegevensontdubbeling en cloudopslaglagen worden niet ondersteund op hetzelfde volume.

### <a name="sync-limitations"></a>Synchronisatiebeperkingen
De volgende items worden niet gesynchroniseerd, maar de rest van het systeem blijft normaal functioneren:
- Bestanden met niet-ondersteunde tekens. Zie [Troubleshooting guide](storage-sync-files-troubleshoot.md#handling-unsupported-characters) voor lijst met niet-ondersteunde tekens.
- Bestanden of mappen die eindigen op een punt.
- Paden langer dan 2.048 tekens.
- Het gedeelte met de discretionaire ACL (Access Control List) van een security descriptor als dit groter is dan 2 kB. (Dit probleem geldt alleen wanneer er meer dan ongeveer 40 vermeldingen voor toegangsbeheer (ACE's) bestaan voor één item.)
- Het gedeelte met de SACL (System Access Control List) van een security descriptor die wordt gebruikt voor controle.
- Uitgebreide kenmerken.
- Alternatieve gegevensstromen.
- Reparse-punten.
- Vaste koppelingen.
- Compressie (indien ingesteld op een serverbestand) blijft niet behouden wanneer wijzigingen vanuit andere eindpunten naar dat bestand worden gesynchroniseerd.
- Elk bestand dat is gecodeerd met EFS (of een andere versleuteling in de gebruikersmodus) dat voorkomt dat de service de gegevens leest.

    > [!Note]  
    > Gegevens die onderweg zijn tussen eindpunten worden altijd versleuteld door Azure File Sync. Inactieve gegevens (data-at-rest) worden altijd versleuteld in Azure.
 
### <a name="server-endpoint"></a>Servereindpunt
- Een servereindpunt kan alleen worden gemaakt op een NTFS-volume. ReFS, FAT, FAT32 en andere bestandssystemen worden op dit moment niet ondersteund door Azure File Sync.
- Gelaagde bestanden wordt niet meer toegankelijk als de bestanden niet ingetrokken worden voordat u het servereindpunt verwijdert. Als u toegang tot de bestanden herstellen, maakt u het servereindpunt opnieuw. Als de 30 dagen zijn verstreken sinds het servereindpunt is verwijderd of als het cloudeindpunt is verwijderd, wordt gelaagde bestanden die niet zijn ingetrokken onbruikbaar worden.
- Cloud tiering wordt niet ondersteund op het systeemvolume. Uitschakelen voor het maken van een servereindpunt op het systeemvolume, cloud-opslaglagen bij het maken van het servereindpunt.
- Failoverclustering wordt alleen ondersteund met geclusterde schijven, maar niet met CSV's (Cluster Shared Volume).
- Een servereindpunt kan niet worden genest. Een eindpunt van dit type kan zich samen met een ander eindpunt op hetzelfde volume bevinden.
- Sla geen Besturingssysteembestand of wisselbestand van de toepassing binnen een eindpunt-serverlocatie.
- De naam van de server in de portal is niet bijgewerkt als de naam van de server is gewijzigd.

### <a name="cloud-endpoint"></a>Cloud-eindpunt
- Azure File Sync ondersteunt rechtstreeks aanbrengen van wijzigingen in de Azure-bestandsshare. Alle wijzigingen in de Azure-bestandsshare moeten echter eerst moeten worden gedetecteerd door een Azure File Sync-taak wijzigen detectie. Een wijziging detectie-taak wordt gestart voor een cloudeindpunt om de 24 uur. Bovendien wijzigingen aangebracht in een Azure-bestandsshare via de REST-protocol wordt niet bijgewerkt door de SMB-tijdstip laatst gewijzigd en zal niet worden gezien als een wijziging door synchronisatie.
- De opslagsynchronisatieservice en/of de storage-account kan worden verplaatst naar een andere resourcegroep of abonnement binnen de bestaande Azure AD-tenant. Als het opslagaccount is verplaatst, moet u de hybride File Sync-Service toegang geven tot het opslagaccount (Zie [Zorg ervoor dat Azure File Sync heeft toegang tot het opslagaccount](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cportal#troubleshoot-rbac)).

    > [!Note]  
    > Azure File Sync biedt geen ondersteuning voor het verplaatsen van het abonnement naar een andere Azure AD-tenant.

### <a name="cloud-tiering"></a>Cloudopslaglagen
- De datum op basis van cloud-opslaglagen beleidsinstelling wordt gebruikt om de bestanden die moeten worden opgeslagen in de cache als geopend in een opgegeven aantal dagen opgeven. Zie voor meer informatie, [Cloud Tiering overzicht](https://docs.microsoft.com/azure/storage/files/storage-sync-cloud-tiering#afs-force-tiering).
- Als een gelaagd bestand met behulp van Robocopy naar een andere locatie wordt gekopieerd, wordt het resulterende bestand niet in een laag geplaatst. Het kenmerk 'offline' kan zijn ingesteld omdat Robocopy dat kenmerk onterecht opneemt in kopieerbewerkingen.
- Bij het kopiëren van bestanden met behulp van robocopy, moet u de /MIR-optie gebruiken om het bestand tijdstempels behouden. Dit zorgt ervoor dat oudere bestanden eerder dan recent geopende bestanden worden geschakeld.
- Wanneer u bestandseigenschappen bekijkt vanuit een SMB-client, lijkt het misschien of het kenmerk 'offline' niet goed is ingesteld als gevolg van SMB-caching van bestandsmetagegevens.

## <a name="agent-version-3300"></a>Agentversie 3.3.0.0
De volgende releaseopmerkingen zijn voor versie 3.3.0.0 van de Azure File Sync-agent die zijn uitgebracht op 24 September 2018. Deze opmerkingen zijn naast de releaseopmerkingen voor versie 3.1.0.0.

Lijst met problemen opgelost in deze release:
- Status van de geregistreerde server is 'Wordt als offline weergegeven' nadat de Azure File Sync-agent is bijgewerkt naar versie 3.1 of 3.2.
- Storage-Sync-Agent (FileSyncSvc) service vastloopt vanwege bestanden met lange paden.
- Registratie van de server is mislukt met fout: Kan bestand of de assembly Kailani.Afs.StorageSyncProtocol.V3 niet laden.

## <a name="agent-version-3200"></a>Agent-versie 3.2.0.0
De volgende releaseopmerkingen zijn voor versie 3.2.0.0 van de Azure File Sync-agent die zijn uitgebracht 15 augustus 2018. Deze opmerkingen zijn naast de releaseopmerkingen voor versie 3.1.0.0.

Deze release bevat het volgende probleem:
- Synchronisatie mislukt met fout door onvoldoende geheugen (0x8007000e) vanwege een geheugenlek

## <a name="agent-version-3100"></a>Agentversie 3.1.0.0
De volgende releaseopmerkingen zijn voor versie 3.1.0.0 van de Azure File Sync-agent (vrijgegeven op 19 juni 2018).

### <a name="evaluation-tool"></a>Hulpprogramma voor het evalueren
Voordat u Azure File Sync implementeert, moet u evalueren of het compatibel is met het systeem met behulp van het hulpprogramma voor het evalueren van Azure File Sync. Dit hulpprogramma is een Azure PowerShell-cmdlet waarmee wordt gecontroleerd op mogelijke problemen met het bestandssysteem en de gegevensset, zoals niet-ondersteunde tekens of een niet-ondersteunde versie van het besturingssysteem. Zie voor de installatie en instructies over het gebruik, [evaluatieprogramma](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning#evaluation-tool) sectie in de handleiding voor capaciteitsplanning. 

### <a name="agent-installation-and-server-configuration"></a>Agentinstallatie en serverconfiguratie
Zie voor meer informatie over het installeren en configureren van de Azure File Sync-agent met Windows Server [Planning voor de implementatie van een Azure File Sync](storage-sync-files-planning.md) en [over het implementeren van Azure File Sync](storage-sync-files-deployment-guide.md).

- Het installatiepakket voor de agent moet worden geïnstalleerd met machtigingen van de opdrachtprompt met verhoogde bevoegdheid (beheerder).
- De agent wordt niet ondersteund op Windows Server Core of Nano Server-implementatie-opties.
- De agent wordt alleen ondersteund in Windows Server 2016 en Windows Server 2012 R2.
- De agent vereist ten minste 2 GB fysiek geheugen.
- De service opslag-Sync-Agent (FileSyncSvc) biedt geen ondersteuning voor servereindpunten die zich op een volume dat de map system volume information (SVI is) gecomprimeerd. Deze configuratie zal leiden tot onverwachte resultaten.
- FIPS-modus wordt niet ondersteund en moet worden uitgeschakeld. 

### <a name="interoperability"></a>Interoperabiliteit
- Antivirusprogramma's, back-uptoepassingen en andere toepassingen die toegang hebben tot gelaagde bestanden, kunnen leiden tot ongewenste intrekking tenzij ze het kenmerk offline respecteren en het lezen van de inhoud van die bestanden overslaan. Zie voor meer informatie, [problemen met Azure File Sync](storage-sync-files-troubleshoot.md).
- Gebruik geen FSRM-controles (File Server Resource Manager) of andere bestandscontroles. Bestandscontroles kunnen eindeloze synchronisatiefouten veroorzaken wanneer bestanden worden geblokkeerd vanwege de bestandscontrole.
- Sysprep uitgevoerd op een server met de Azure File Sync-agent is geïnstalleerd, wordt niet ondersteund en kan leiden tot onverwachte resultaten. De registratie van de installatie en server van de agent moet worden uitgevoerd na het implementeren van de server-installatiekopie en sysprep mini-installatie te voltooien.
- Gegevensontdubbeling en cloudopslaglagen worden niet ondersteund op hetzelfde volume.

### <a name="sync-limitations"></a>Synchronisatiebeperkingen
De volgende items worden niet gesynchroniseerd, maar de rest van het systeem blijft normaal functioneren:
- Paden langer dan 2.048 tekens.
- Het gedeelte met de discretionaire ACL (Access Control List) van een security descriptor als dit groter is dan 2 kB. (Dit probleem geldt alleen wanneer er meer dan ongeveer 40 vermeldingen voor toegangsbeheer (ACE's) bestaan voor één item.)
- Het gedeelte met de SACL (System Access Control List) van een security descriptor die wordt gebruikt voor controle.
- Uitgebreide kenmerken.
- Alternatieve gegevensstromen.
- Reparse-punten.
- Vaste koppelingen.
- Compressie (indien ingesteld op een serverbestand) blijft niet behouden wanneer wijzigingen vanuit andere eindpunten naar dat bestand worden gesynchroniseerd.
- Elk bestand dat is gecodeerd met EFS (of een andere versleuteling in de gebruikersmodus) dat voorkomt dat de service de gegevens leest.

    > [!Note]  
    > Gegevens die onderweg zijn tussen eindpunten worden altijd versleuteld door Azure File Sync. Inactieve gegevens (data-at-rest) worden altijd versleuteld in Azure.
 
### <a name="server-endpoint"></a>Servereindpunt
- Een servereindpunt kan alleen worden gemaakt op een NTFS-volume. ReFS, FAT, FAT32 en andere bestandssystemen worden op dit moment niet ondersteund door Azure File Sync.
- Gelaagde bestanden onbruikbaar als de bestanden niet ingetrokken worden voordat u het servereindpunt verwijdert.
- Cloud tiering wordt niet ondersteund op het systeemvolume. Uitschakelen voor het maken van een servereindpunt op het systeemvolume, cloud-opslaglagen bij het maken van het servereindpunt.
- Failoverclustering wordt alleen ondersteund met geclusterde schijven, maar niet met CSV's (Cluster Shared Volume).
- Een servereindpunt kan niet worden genest. Een eindpunt van dit type kan zich samen met een ander eindpunt op hetzelfde volume bevinden.
- Sla geen besturingssysteembestand of wisselbestand van de toepassing op dat zich binnen een servereindpunt bevindt.
- De naam van de server in de portal is niet bijgewerkt als de naam van de server is gewijzigd.

### <a name="cloud-endpoint"></a>Cloud-eindpunt
- Azure File Sync ondersteunt rechtstreeks aanbrengen van wijzigingen in de Azure-bestandsshare. Alle wijzigingen in de Azure-bestandsshare moeten echter eerst moeten worden gedetecteerd door een Azure File Sync-taak wijzigen detectie. Een wijziging detectie-taak wordt gestart voor een cloudeindpunt om de 24 uur. Bovendien wijzigingen aangebracht in een Azure-bestandsshare via de REST-protocol wordt niet bijgewerkt door de SMB-tijdstip laatst gewijzigd en zal niet worden gezien als een wijziging door synchronisatie.
- De opslagsynchronisatieservice en/of de storage-account kan worden verplaatst naar een andere resourcegroep of abonnement binnen de bestaande Azure AD-tenant. Als het opslagaccount is verplaatst, moet u de hybride File Sync-Service toegang geven tot het opslagaccount (Zie [Zorg ervoor dat Azure File Sync heeft toegang tot het opslagaccount](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cportal#troubleshoot-rbac)).

    > [!Note]  
    > Azure File Sync biedt geen ondersteuning voor het verplaatsen van het abonnement naar een andere Azure AD-tenant.

### <a name="cloud-tiering"></a>Cloudopslaglagen
- Als een gelaagd bestand met behulp van Robocopy naar een andere locatie wordt gekopieerd, wordt het resulterende bestand niet in een laag geplaatst. Het kenmerk 'offline' kan zijn ingesteld omdat Robocopy dat kenmerk onterecht opneemt in kopieerbewerkingen.
- Wanneer u bestandseigenschappen bekijkt vanuit een SMB-client, lijkt het misschien of het kenmerk 'offline' niet goed is ingesteld als gevolg van SMB-caching van bestandsmetagegevens.

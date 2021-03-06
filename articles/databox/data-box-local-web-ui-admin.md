---
title: Microsoft Azure Data Box beheren via de lokale webinterface | Microsoft Docs in gegevens
description: Beschrijft hoe u de lokale webinterface gebruikt om uw Data Box-apparaat te beheren.
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: overview
ms.date: 12/27/2018
ms.author: alkohli
ms.openlocfilehash: 6df44afee26570dbdc5d64b449c4e33d6a0956ae
ms.sourcegitcommit: b4755b3262c5b7d546e598c0a034a7c0d1e261ec
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54885707"
---
# <a name="use-the-local-web-ui-to-administer-your-data-box"></a>De lokale webinterface gebruiken om uw Data Box beheren

In dit artikel worden enkele van de complexe werkstromen en beheertaken beschreven die kunnen worden uitgevoerd voor de Data Box. U kunt de Data Box beheren via de gebruikersinterface van Azure Portal en de lokale webinterface voor het apparaat. Dit artikel richt zich op de taken die u kunt uitvoeren met behulp van de lokale webinterface.

De lokale webinterface voor de Data Box wordt gebruikt voor de initiële configuratie van het apparaat. U kunt de lokale webinterface ook gebruiken om de Data Box af te sluiten of opnieuw te starten, diagnostische tests uit te voeren, software bij te werken, logboeken te bekijken en een logboekpakket voor Microsoft Ondersteuning te genereren.

Dit artikel bevat de volgende zelfstudies:

- Een ondersteuningspakket genereren
- Apparaat uitschakelen of opnieuw opstarten
- Beschikbare capaciteit van het apparaat weergeven
- De controlesomvalidatie overslaan 

## <a name="generate-support-package"></a>Ondersteuningspakket genereren

Als u problemen met het apparaat ondervindt, kunt u een ondersteuningspakket maken vanuit het systeemlogboek. Microsoft Ondersteuning gebruikt dit pakket om het probleem op te lossen. Ga als volgt te werk om een ondersteuningspakket te genereren:

1. Ga in de lokale webinterface naar **Contact opnemen met de ondersteuning**  en klik op **Ondersteuningspakket maken**.

    ![Ondersteuningspakket maken 1](media/data-box-local-web-ui-admin/create-support-package-1.png)

2. Er wordt een ondersteuningspakket verzameld. Deze bewerking duurt enkele minuten.

    ![Ondersteuningspakket maken 2](media/data-box-local-web-ui-admin/create-support-package-2.png)

3. Nadat het maken van het ondersteuningspakket is voltooid, klikt u op **Ondersteuningspakket downloaden**. 

    ![Ondersteuningspakket maken 4](media/data-box-local-web-ui-admin/create-support-package-4.png)

4. Blader naar en kies de downloadlocatie. Open de map om de inhoud weer te geven.

    ![Ondersteuningspakket maken 5](media/data-box-local-web-ui-admin/create-support-package-5.png)


## <a name="shut-down-or-restart-your-device"></a>Apparaat uitschakelen of opnieuw opstarten

U kunt uw Data Box afsluiten of opnieuw starten met de lokale webinterface. We raden u aan de shares vóór de herstart offline te zetten op de host en vervolgens op het apparaat. Dit minimaliseert de kans op beschadigde gegevens. Zorg ervoor dat er geen gegevens worden gekopieerd wanneer u het apparaat afsluit.

Voer de volgende stappen uit om uw Data Box af te sluiten.

1. Ga in de lokale webinterface naar **Afsluiten of opnieuw opstarten**.
2. Klik op **Afsluiten**.

    ![Data Box afsluiten 1](media/data-box-local-web-ui-admin/shut-down-local-web-ui-1.png)

3. Klik op **OK** als u om bevestiging wordt gevraagd.

    ![Data Box afsluiten 2](media/data-box-local-web-ui-admin/shut-down-local-web-ui-2.png)

Nadat het apparaat is uitgeschakeld, gebruikt u de aan-uitknop op het voorpaneel om het apparaat in te schakelen.

Voer de volgende stappen uit om uw Data Box opnieuw op te starten.

1. Ga in de lokale webinterface naar **Afsluiten of opnieuw opstarten**.
2. Klik op **Opnieuw opstarten**.

    ![Data Box opnieuw opstarten 1](media/data-box-local-web-ui-admin/restart-local-web-ui-1.png)

3. Klik op **OK** als u om bevestiging wordt gevraagd.

   Het apparaat wordt afgesloten en opnieuw opgestart.

## <a name="download-bom-or-manifest-files"></a>Stuklijst- of manifestbestanden downloaden

De stuklijst- of manifestbestanden bevatten de lijst met bestanden die naar de Data Box worden gekopieerd. Deze bestanden worden gegenereerd wanneer u de Data Box voorbereidt op verzending.

Zorg er voordat u begint voor dat uw Data Box de stap **Voorbereiding voor verzending** heeft doorlopen. Volg deze stappen om stuklijst- of manifestbestanden te downloaden:

1. Ga naar de lokale webgebruikersinterface van uw Data Box. U ziet dat de Data Box gegevens het voorbereiden voor verzending heeft voltooid. Nadat de voorbereiding van het apparaat is voltooid, wordt de status van het apparaat bijgewerkt naar **Gereed voor verzending**.

    ![Apparaat gereed voor verzending](media/data-box-portal-admin/ready-to-ship.png)

2. Klik op **Lijst met bestanden downloaden** om de lijst met bestanden die naar uw Data Box zijn gekopieerd te downloaden.

    ![Klik op Lijst met bestanden downloaden](media/data-box-portal-admin/download-list-of-files.png)

3. In Windows Verkenner ziet u dat er afzonderlijke lijsten met bestanden worden gegenereerd, afhankelijk van het protocol dat wordt gebruikt om verbinding te maken met het apparaat en het gebruikte type Azure Storage.

    ![Bestanden voor opslagtype en verbindingsprotocol](media/data-box-portal-admin/files-storage-connection-type.png)

   In de volgende tabel ziet u welke bestandsnamen horen bij welk type Azure Storage en gebruikt verbindingsprotocol.

    |Bestandsnaam  |Azure Storage-type  |Gebruikt verbindingsprotocol |
    |---------|---------|---------|
    |databoxe2etest_BlockBlob.txt     |Blok-blobs         |SMB/NFS         |
    |databoxe2etest_PageBlob.txt     |Pagina-blobs         |SMB/NFS         |
    |databoxe2etest_AzFile-BOM.txt    |Azure Files         |SMB/NFS         |
    |databoxe2etest_PageBlock_Rest-BOM.txt     |Pagina-blobs         |REST        |
    |databoxe2etest_BlockBlock_Rest-BOM.txt    |Blok-blobs         |REST         |

U kunt deze lijst gebruiken om de bestanden die zijn geüpload naar het Azure Storage-account te controleren nadat de Data Box is geretourneerd naar het Azure-datacenter. Hieronder ziet u een voorbeeld van een manifestbestand.

```xml
<file size="52689" crc64="0x95a62e3f2095181e">\databox\media\data-box-deploy-copy-data\prepare-to-ship2.png</file>
<file size="22117" crc64="0x9b160c2c43ab6869">\databox\media\data-box-deploy-copy-data\connect-shares-file-explorer2.png</file>
<file size="57159" crc64="0x1caa82004e0053a4">\databox\media\data-box-deploy-copy-data\verify-used-space-dashboard.png</file>
<file size="24777" crc64="0x3e0db0cd1ad438e0">\databox\media\data-box-deploy-copy-data\prepare-to-ship5.png</file>
<file size="162006" crc64="0x9ceacb612ecb59d6">\databox\media\data-box-cable-options\cabling-dhcp-data-only.png</file>
<file size="155066" crc64="0x051a08d36980f5bc">\databox\media\data-box-cable-options\cabling-2-port-setup.png</file>
<file size="150399" crc64="0x66c5894ff328c0b1">\databox\media\data-box-cable-options\cabling-with-switch-static-ip.png</file>
<file size="158082" crc64="0xbd4b4c5103a783ea">\databox\media\data-box-cable-options\cabling-mgmt-only.png</file>
<file size="148456" crc64="0xa461ad24c8e4344a">\databox\media\data-box-cable-options\cabling-with-static-ip.png</file>
<file size="40417" crc64="0x637f59dd10d032b3">\databox\media\data-box-portal-admin\delete-order1.png</file>
<file size="33704" crc64="0x388546569ea9a29f">\databox\media\data-box-portal-admin\clone-order1.png</file>
<file size="5757" crc64="0x9979df75ee9be91e">\databox\media\data-box-safety\japan.png</file>
<file size="998" crc64="0xc10c5a1863c5f88f">\databox\media\data-box-safety\overload_tip_hazard_icon.png</file>
<file size="5870" crc64="0x4aec2377bb16136d">\databox\media\data-box-safety\south-korea.png</file>
<file size="16572" crc64="0x05b13500a1385a87">\databox\media\data-box-safety\taiwan.png</file>
<file size="999" crc64="0x3f3f1c5c596a4920">\databox\media\data-box-safety\warning_icon.png</file>
<file size="1054" crc64="0x24911140d7487311">\databox\media\data-box-safety\read_safety_and_health_information_icon.png</file>
<file size="1258" crc64="0xc00a2d5480f4fcec">\databox\media\data-box-safety\heavy_weight_hazard_icon.png</file>
<file size="1672" crc64="0x4ae5cfa67c0e895a">\databox\media\data-box-safety\no_user_serviceable_parts_icon.png</file>
<file size="3577" crc64="0x99e3d9df341b62eb">\databox\media\data-box-safety\battery_disposal_icon.png</file>
<file size="993" crc64="0x5a1a78a399840a17">\databox\media\data-box-safety\tip_hazard_icon.png</file>
<file size="1028" crc64="0xffe332400278f013">\databox\media\data-box-safety\electrical_shock_hazard_icon.png</file>
<file size="58699" crc64="0x2c411d5202c78a95">\databox\media\data-box-deploy-ordered\data-box-ordered.png</file>
<file size="46816" crc64="0x31e48aa9ca76bd05">\databox\media\data-box-deploy-ordered\search-azure-data-box1.png</file>
<file size="24160" crc64="0x978fc0c6e0c4c16d">\databox\media\data-box-deploy-ordered\select-data-box-option1.png</file>
<file size="115954" crc64="0x0b42449312086227">\databox\media\data-box-disk-deploy-copy-data\data-box-disk-validation-tool-output.png</file>
<file size="6093" crc64="0xadb61d0d7c6d4deb">\databox\data-box-cable-options.md</file>
<file size="6499" crc64="0x080add29add367d9">\databox\data-box-deploy-copy-data-via-nfs.md</file>
<file size="11089" crc64="0xc3ce6b13a4fe3001">\databox\data-box-deploy-copy-data-via-rest.md</file>
<file size="9126" crc64="0x820856b5a54321ad">\databox\data-box-overview.md</file>
<file size="10963" crc64="0x5e9a14f9f4784fd8">\databox\data-box-safety.md</file>
<file size="5941" crc64="0x8631d62fbc038760">\databox\data-box-security.md</file>
<file size="12536" crc64="0x8c8ff93e73d665ec">\databox\data-box-system-requirements-rest.md</file>
<file size="3220" crc64="0x7257a263c434839a">\databox\data-box-system-requirements.md</file>
<file size="2823" crc64="0x63db1ada6fcdc672">\databox\index.yml</file>
<file size="4364" crc64="0x62b5710f58f00b8b">\databox\data-box-local-web-ui-admin.md</file>
<file size="3603" crc64="0x7e34c25d5606693f">\databox\TOC.yml</file>
```

Dit bestand bevat de lijst van alle bestanden die naar de Data Box zijn gekopieerd. In dit bestand is de *crc64*-waarde gerelateerd aan de controlesom die is gegenereerd voor het bijbehorende bestand.

## <a name="view-available-capacity-of-the-device"></a>Beschikbare capaciteit van het apparaat weergeven

U kunt het apparaatdashboard gebruiken om de beschikbare en gebruikte capaciteit van het apparaat weer te geven. 

1. Ga in de lokale webinterface naar **Dashboard weergeven**.
2. Onder **Verbinding maken en kopiëren** wordt de beschikbare en gebruikte ruimte op het apparaat weergegeven.

    ![Beschikbare capaciteit weergeven](media/data-box-local-web-ui-admin/verify-used-space-dashboard.png)


## <a name="skip-checksum-validation"></a>De controlesomvalidatie overslaan

Er worden standaard controlesommen gegenereerd voor uw gegevens tijdens de voorbereiding voor verzending. In sommige zeldzame gevallen, afhankelijk van het gegevenstype (kleine bestandsgrootten), kan dit lang duren. In dergelijke gevallen kunt u de controlesom overslaan. 

We raden u ten zeerste aan om de controlesom niet uit te schakelen, tenzij de prestaties te veel worden beïnvloed.

1. Ga naar Instellingen in de rechterbovenhoek van de lokale webinterface van uw apparaat.

    ![Controlesom uitschakelen](media/data-box-local-web-ui-admin/disable-checksum.png)

2. Controlesomvalidatie **Uitschakelen**
3. Klik op **Toepassen**.

## <a name="next-steps"></a>Volgende stappen

- Informatie over het [beheren van de Data Box Disk via Azure Portal](data-box-portal-admin.md).


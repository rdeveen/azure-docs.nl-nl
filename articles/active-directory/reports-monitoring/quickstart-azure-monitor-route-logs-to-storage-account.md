---
title: 'Zelfstudie: Azure Active Directory-logboeken archiveren in een opslagaccount (preview) | Microsoft Docs'
description: Informatie over het instellen van Azure Diagnostics voor het pushen van Azure Active Directory-logboeken naar een opslagaccount (preview)
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: ''
ms.assetid: 045f94b3-6f12-407a-8e9c-ed13ae7b43a3
ms.service: active-directory
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: b50b8f6c54cbefde2120dd61643c23f5175927c9
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56211747"
---
# <a name="tutorial-archive-azure-ad-logs-to-an-azure-storage-account-preview"></a>Zelfstudie: Azure AD-logboeken archiveren in een Azure-opslagaccount (preview)

In deze zelfstudie leert u diagnostische instellingen van Azure Monitor instellen voor het routeren van Azure Active Directory (AD)-logboeken naar een Azure-opslagaccount.

## <a name="prerequisites"></a>Vereisten 

U hebt het volgende nodig om deze functie te gebruiken:

* Een abonnement op Azure met een Azure-opslagaccount. Als u nog geen Azure-abonnement hebt, kunt u zich registreren voor een [gratis proefversie](https://azure.microsoft.com/free/).
* Een Azure AD-tenant.
* Een gebruiker die een *globale beheerder* of *beveiligingsbeheerder* voor de Azure-tenant is.

## <a name="archive-logs-to-an-azure-storage-account"></a>Activiteitenlogboeken in een Azure-opslagaccount archiveren

1. Meld u aan bij [Azure Portal](https://portal.azure.com). 

2. Selecteer **Azure Active Directory** > **Activiteit** > **Auditlogboeken**. 

3. Selecteer **Exportinstellingen**. 

4. In het deelvenster **Diagnostische instellingen** voert u een van de volgende handelingen uit:
    * Selecteer **Instelling bewerken** als u bestaande instellingen wilt wijzigen.
    * Selecteer **Diagnostische instelling toevoegen** als u nieuwe instellingen wilt toevoegen.  
      Er zijn maximaal drie instellingen. 

    ![Exportinstellingen](./media/quickstart-azure-monitor-route-logs-to-storage-account/ExportSettings.png)

5. Voer een beschrijvende naam in voor de instelling om het doel ervan makkelijker te kunnen onthouden (bijvoorbeeld *Verzenden naar Azure-opslagaccount*). 

6. Schakel het selectievakje **Archiveren naar een opslagaccount** in en selecteer **Opslagaccount**. 

7. Selecteer het Azure-abonnement en het opslagaccount waarnaar u de logboeken wilt routeren.
 
8. Selecteer **OK** om de configuratie af te sluiten.

9. Voer een van de volgende bewerkingen uit, of beide:
    * Schakel het selectievakje **AuditLogs** in om auditlogboeken naar het opslagaccount te verzenden. 
    * Schakel het selectievakje **SignInLogs** in om aanmeldingslogboeken naar het opslagaccount te verzenden.

10. Gebruik de schuifregelaar om de retentie voor uw logboekgegevens in te stellen. Standaard is deze waarde *0*, wat inhoudt dat de logboeken voor onbepaalde tijd in het opslagaccount worden bewaard. Als u een andere waarde instelt, worden gebeurtenissen die ouder zijn dan het opgegeven aantal dagen, automatisch gewist.

11. Selecteer **Opslaan** om de instelling op te slaan.

    ![Diagnostische instellingen](./media/quickstart-azure-monitor-route-logs-to-storage-account/DiagnosticSettings.png)

12. Na circa vijftien minuten controleert u of de logboeken naar uw opslagaccount worden gepusht. Ga naar de [Azure-portal](https://portal.azure.com), klik op **Opslagaccount**s, kies het opslagaccount dat u eerder hebt gebruikt en selecteer **Blobs**. Voor **Auditlogboeken** selecteert u **insights-log-audit**. Voor **Aanmeldingslogboeken** selecteert u **insights-log-signin**.

    ![Storage-account](./media/quickstart-azure-monitor-route-logs-to-storage-account/StorageAccount.png)

## <a name="next-steps"></a>Volgende stappen

* [Interpret audit logs schema in Azure Monitor](reference-azure-monitor-audit-log-schema.md) (Auditlogboekenschema interpreteren in Azure Monitor)
* [Interpret sign-in logs schema in Azure Monitor](reference-azure-monitor-sign-ins-log-schema.md) (Aanmeldingslogboekenschema interpreteren in Azure Monitor)
* [Veelgestelde vragen en bekende problemen](concept-activity-logs-azure-monitor.md#frequently-asked-questions)

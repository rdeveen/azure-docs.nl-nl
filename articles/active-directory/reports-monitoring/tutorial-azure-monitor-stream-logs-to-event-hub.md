---
title: 'Zelfstudie: Azure Active Directory-logboeken streamen naar een Azure Event Hub (preview) | Microsoft Docs'
description: Informatie over het instellen van Azure Diagnostics voor het pushen van Azure Active Directory-logboeken naar een Event Hub (preview)
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
ms.openlocfilehash: 3d416f61e2752ad2e26bda1276c3c52b36d5754a
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56198249"
---
# <a name="tutorial-stream-azure-active-directory-logs-to-an-azure-event-hub-preview"></a>Zelfstudie: Azure Active Directory-logboeken streamen naar een Azure Event Hub (preview)

In deze zelfstudie leert u hoe u de diagnostische instellingen van Azure Monitor instelt voor het streamen van Azure AD-logboeken (Azure Active Directory) naar een Azure Event Hub. Gebruik dit mechanisme om uw logboeken te integreren met externe SIEM-hulpprogramma's (Security Information and Event Management), zoals Splunk en QRadar.

## <a name="prerequisites"></a>Vereisten 

U hebt het volgende nodig om deze functie te gebruiken:

* Een Azure-abonnement. Als u nog geen Azure-abonnement hebt, kunt u zich registreren voor een [gratis proefversie](https://azure.microsoft.com/free/).
* Een Azure AD-tenant.
* Een gebruiker die een *globale beheerder* of *beveiligingsbeheerder* voor de Azure-tenant is.
* Een naamruimte van Event Hubs en een Event Hub in uw Azure-abonnement. Informatie over het [maken van een Event Hub](https://docs.microsoft.com/azure/event-hubs/event-hubs-create).

## <a name="stream-logs-to-an-event-hub"></a>Logboeken streamen naar een Event Hub

1. Meld u aan bij [Azure Portal](https://portal.azure.com). 

2. Selecteer **Azure Active Directory** > **Activiteit** > **Auditlogboeken**. 

3. Selecteer **Exportinstellingen**.  
    
4. In het deelvenster **Diagnostische instellingen** voert u een van de volgende handelingen uit:
    * Selecteer **Instelling bewerken** als u bestaande instellingen wilt wijzigen.
    * Selecteer **Diagnostische instelling toevoegen** als u nieuwe instellingen wilt toevoegen.  
      Er zijn maximaal drie instellingen.

      ![Exportinstellingen](./media/quickstart-azure-monitor-stream-logs-to-event-hub/ExportSettings.png)

5. Schakel het selectievakje **Streamen naar een Event Hub** in en selecteer **Event Hub/Configureren**.

6. Selecteer het Azure-abonnement en de Event Hubs-naamruimte waarnaar u de logboeken wilt routeren.  
    Het abonnement en de Event Hubs-naamruimte moeten beide worden gekoppeld aan de Azure AD-tenant van waaruit de logboeken worden gestreamd. Ook kunt u een Event Hub opgeven in de Event Hubs-naamruimte waarnaar logboeken moeten worden verzonden. Als geen Event Hub wordt opgegeven, wordt er een in de naamruimte gemaakt. Deze krijgt de standaardnaam **insights-logs-audit**.

7. Selecteer **OK** om de configuratie van de Event Hub af te sluiten.

8. Voer een van de volgende bewerkingen uit, of beide:
    * Schakel het selectievakje **AuditLogs** in om auditlogboeken naar het opslagaccount te verzenden. 
    * Schakel het selectievakje **SignInLogs** in om aanmeldingslogboeken naar het opslagaccount te verzenden.

9. Selecteer **Opslaan** om de instelling op te slaan.

    ![Diagnostische instellingen](./media/quickstart-azure-monitor-stream-logs-to-event-hub/DiagnosticSettings.png)

10. Controleer na circa vijftien minuten of er gebeurtenissen in uw Event Hub worden weergegeven. Ga hiervoor vanuit de portal naar de Event Hub en controleer of het aantal **inkomende berichten** groter is dan nul. 

    ![Controlelogboeken](./media/quickstart-azure-monitor-stream-logs-to-event-hub/InsightsLogsAudit.png)

## <a name="access-data-from-your-event-hub"></a>Gegevens openen vanuit de Event Hub

Nadat gegevens in de Event Hub worden weergegeven, kunt u de gegevens op twee manieren openen en lezen:

* **Een ondersteund SIEM-hulpprogramma configureren**. Als u gegevens vanuit de Event Hub wilt lezen, zijn voor de meeste hulpprogramma's de Event Hub-verbindingsreeks en bepaalde machtigingen voor uw Azure-abonnement vereist. Hulpprogramma's van derden met Azure Monitor-integratie zijn onder meer:
    * **Splunk**: zie [Integrate Azure AD logs with Splunk by using Azure Monitor](tutorial-integrate-activity-logs-with-splunk.md) (Azure AD-logboeken integreren met Splunk met behulp van Azure Monitor) voor meer informatie over het integreren van Azure AD-logboeken met Splunk.
    
    * **IBM QRadar**: de DSM en Azure Event Hub Protocol kunnen worden gedownload op [IBM support](https://www.ibm.com/support) (Ondersteuning van IBM). Ga naar de site [IBM QRadar Security Intelligence Platform 7.3.0](https://www.ibm.com/support/knowledgecenter/SS42VS_DSM/c_dsm_guide_microsoft_azure_overview.html?cp=SS42VS_7.3.0) (Engelstalig) voor meer informatie over integratie met Azure.
    
    * **Sumo Logic**: zie [Install the Azure AD app and view the dashboards](https://help.sumologic.com/Send-Data/Applications-and-Other-Data-Sources/Azure_Active_Directory/Install_the_Azure_Active_Directory_App_and_View_the_Dashboards) (Azure AD-app installeren en dashboards weergeven) als u Sumo Logic wilt instellen om gegevens uit een Event Hub te gebruiken. 

* **Aangepaste tooling instellen**. Als uw huidige SIEM nog niet in Azure Monitor Diagnostics wordt ondersteund, kunt u aangepaste tooling instellen met behulp van de Event Hub-API. Zie [Berichten ontvangen vanuit een Event Hub](https://docs.microsoft.com/azure/event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph) voor meer informatie.


## <a name="next-steps"></a>Volgende stappen

* [Integrate Azure AD logs with Splunk by using Azure Monitor](tutorial-integrate-activity-logs-with-splunk.md) (Azure AD-logboeken integreren met Splunk met behulp van Azure Monitor)
* [Integrate Azure AD logs with SumoLogic by using Azure Monitor](howto-integrate-activity-logs-with-sumologic.md) (Azure AD-logboeken integreren met SumoLogic met behulp van Azure Monitor)
* [Interpret audit logs schema in Azure Monitor](reference-azure-monitor-audit-log-schema.md) (Auditlogboekenschema interpreteren in Azure Monitor)
* [Interpret sign-in logs schema in Azure Monitor](reference-azure-monitor-sign-ins-log-schema.md) (Aanmeldingslogboekenschema interpreteren in Azure Monitor)

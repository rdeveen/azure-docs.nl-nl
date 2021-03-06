---
title: Azure Stack-validatie met PowerShell automatiseren | Microsoft Docs
description: U kunt Azure Stack-validatie met PowerShell automatiseren.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 11/26/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 11/26/2018
ROBOTS: NOINDEX
ms.openlocfilehash: 6287b4f07c99dd45628ed4b9eb1412200cf7cd43
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55244603"
---
# <a name="automate-azure-stack-validation-with-powershell"></a>Azure Stack-validatie met PowerShell automatiseren

Validatie uit als een Service (VaaS) biedt de mogelijkheid voor het automatiseren van het uitvoeren van tests met behulp van de **LaunchVaaSTests.ps1** script.

U kunt PowerShell gebruiken voor de volgende werkstroom:

- Testronde

In deze zelfstudie leert u hoe u het maken van een script dat:

> [!div class="checklist"]
> * Vereiste onderdelen worden geïnstalleerd
> * Installeert en start u de lokale agent
> * Een categorie van tests, waaronder integratie, functionele, betrouwbaarheid wordt gestart
> * Testresultaten van rapporten

## <a name="launch-the-test-pass-workflow"></a>De werkstroom testronde starten

1. Open een verhoogde PowerShell-prompt.

2. Voer het volgende script voor het downloaden van het automatiseringsscript:

    ```PowerShell
    New-Item -ItemType Directory -Path <VaaSLaunchDirectory>
    Set-Location <VaaSLaunchDirectory>
    Invoke-WebRequest -Uri https://storage.azurestackvalidation.com/packages/Microsoft.VaaS.Scripts.latest.nupkg -OutFile "LaunchVaaS.zip"
    Expand-Archive -Path ".\LaunchVaaS.zip" -DestinationPath .\ -Force
    ```

3. Voer het volgende script met de juiste parameterwaarden:

    ```PowerShell
    $VaaSAccountCreds = New-Object System.Management.Automation.PSCredential "<VaaSUserId>", (ConvertTo-SecureString "<VaaSUserPassword>" -AsPlainText -Force)
    .\LaunchVaaSTests.ps1 -VaaSAccountCreds $VaaSAccountCreds `
                          -VaaSAccountTenantId <VaaSAccountTenantId> `
                          -VaaSSolutionName <VaaSSolutionName> `
                          -VaaSTestPassName <VaaSTestPassName> `
                          -VaaSTestCategories Integration,Functional `
                          -MaxScriptWaitTimeInHours 12 `
                          -ServiceAdminUserName <AzSServiceAdminUser> `
                          -ServiceAdminUserPassword <AzSServiceAdminPassword> `
                          -TenantAdminUserName <AzSTenantAdminUser> `
                          -TenantAdminUserPassword <AzSTenantAdminPassword> `
                          -CloudAdminUserName <AzSCloudAdminUser> `
                          -CloudAdminUserPassword <AzSCloudAdminPassword>
    ```

    **Parameters**

    | Parameter | Beschrijving |
    | --- | --- |
    | VaaSUserId | Uw VaaS gebruikers-ID. |
    | VaaSUserPassword | Uw wachtwoord VaaS. |
    | VaaSAccountTenantId | Uw tenant VaaS GUID. |
    | VaaSSolutionName | De naam van de VaaS-oplossing waarmee het doorgeven van de test wordt uitgevoerd. |
    | VaaSTestPassName | De naam van de test VaaS doorgeven werkstroom te maken. |
    | VaaSTestCategories | `Integration`, `Functional`, or. `Reliability`. Als u meerdere waarden gebruikt, scheidt u elke waarde door een komma.  |
    | ServiceAdminUserName | Uw Azure Stack-account voor service-beheerder.  |
    | ServiceAdminPassword | Het wachtwoord van de Azure Stack-service.  |
    | TenantAdminUserName | De beheerder voor de primaire tenant.  |
    | TenantAdminPassword | Het wachtwoord voor de primaire tenant.  |
    | CloudAdminUserName | De gebruikersnaam van de cloud-beheerder.  |
    | CloudAdminPassword | Het wachtwoord voor de beheerder van de cloud.  |

4. Bekijk de resultaten van uw test. Zie voor andere opties [bewaken en beheren van tests in de portal VaaS](azure-stack-vaas-monitor-test.md).

## <a name="next-steps"></a>Volgende stappen

Bekijk de meest recente modules voor meer informatie over PowerShell in Azure Stack.

> [!div class="nextstepaction"]
> [Azure Stack-Module](https://docs.microsoft.com/powershell/azure/azure-stack/overview?view=azurestackps-1.6.0)

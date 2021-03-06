---
title: PowerShell - Remove TDE-beveiliging - Azure SQL Database | Microsoft Docs
description: Handleiding voor het reageren op een potentieel aangetast TDE-beveiliging voor een Azure SQL Database of de Data Warehouse met TDE met Bring YOur Own Key (BYOK)-ondersteuning.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: aliceku
ms.author: aliceku
ms.reviewer: vanto
manager: craigg
ms.date: 10/12/2018
ms.openlocfilehash: 95a86dafc4705d58ac459ff57e4f221d19fb7a37
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "55990288"
---
# <a name="remove-a-transparent-data-encryption-tde-protector-using-powershell"></a>Verwijderen van de beveiliging van een transparante gegevensversleuteling (TDE) met behulp van PowerShell

## <a name="prerequisites"></a>Vereisten

- U moet een Azure-abonnement hebt en een beheerder zijn op dat aan het abonnement
- U moet Azure PowerShell versie 4.2.0 of hoger zijn geïnstalleerd en uitgevoerd. 
- In deze gebruiksaanwijzing wordt ervan uitgegaan dat u al van een sleutel uit Azure Key Vault als de TDE-beveiliging voor een Azure SQL Database of het datawarehouse gebruikmaakt. Zie [Transparent Data Encryption met Azure Key Vault-integratie - BYOK-ondersteuning](transparent-data-encryption-byok-azure-sql.md) voor meer informatie.

## <a name="overview"></a>Overzicht

In deze gebruiksaanwijzing wordt beschreven hoe om te reageren op een potentieel aangetast TDE-beveiliging voor een Azure SQL Database of de Data Warehouse die van TDE met de klant beheerde sleutels in Azure Key Vault - ondersteuning voor Bring Your Own Key (BYOK gebruikmaakt). Zie voor meer informatie over BYOK-ondersteuning voor TDE, de [overzichtspagina](transparent-data-encryption-byok-azure-sql.md). 

De volgende procedures moeten alleen worden uitgevoerd in uitzonderlijke gevallen of in een testomgeving. De handleiding zorgvuldig te controleren, als het verwijderen van actief gebruikte TDE beveiligingstoepassingen uit Azure Key Vault kunnen resulteren in **gegevensverlies**. 

Als een sleutel is ooit mogelijke gevaar lopen, zodat een service of de gebruiker niet-toegang tot de sleutel geautoriseerde heeft, is het aanbevolen om de sleutel te verwijderen.

Houd er rekening mee dat als de TDE-beveiliging wordt verwijderd in Key Vault, **alle verbindingen met de versleutelde databases op de server worden geblokkeerd en deze databases gaan offline en verwijderd binnen 24 uur**. Oude back-ups die zijn versleuteld met de waarmee is geknoeid sleutel zijn niet meer toegankelijk.

In deze gebruiksaanwijzing gaat via twee benaderingen, afhankelijk van het gewenste resultaat na de respons op incidenten:

- Om de Azure SQL-databases / Data Warehouses **toegankelijk is**
- Om te maken van de Azure SQL-databases / Data Warehouses **niet toegankelijk**

## <a name="to-keep-the-encrypted-resources-accessible"></a>De versleutelde resources om beschikbaar te houden

1. Maak een [nieuwe sleutel in Key Vault](https://docs.microsoft.com/powershell/module/azurerm.keyvault/add-azurekeyvaultkey?view=azurermps-4.1.0). Zorg ervoor dat deze nieuwe sleutel gemaakt in een afzonderlijke key vault vanuit de potentieel aangetast TDE-beveiliging, omdat het toegangsbeheer is ingericht op het kluisniveau van een. 
2. Voeg de nieuwe sleutel toe met de server met de [toevoegen AzureRmSqlServerKeyVaultKey](/powershell/module/azurerm.sql/add-azurermsqlserverkeyvaultkey) en [Set-azurermsqlservertransparentdataencryptionprotector geeft](/powershell/module/azurerm.sql/set-azurermsqlservertransparentdataencryptionprotector) cmdlets en deze worden bijgewerkt naarmate nieuwe TDE-beveiliging van de server.

   ```powershell
   # Add the key from Key Vault to the server  
   Add-AzureRmSqlServerKeyVaultKey `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -KeyId <KeyVaultKeyId>

   # Set the key as the TDE protector for all resources under the server
   Set-AzureRmSqlServerTransparentDataEncryptionProtector `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -Type AzureKeyVault -KeyId <KeyVaultKeyId> 
   ```

3. Controleer of de server en alle replica's hebt bijgewerkt naar de nieuwe TDE-beveiliging met behulp van de [Get-azurermsqlservertransparentdataencryptionprotector geeft](/powershell/module/azurerm.sql/get-azurermsqlservertransparentdataencryptionprotector) cmdlet. 

   >[!NOTE]
   > Het duurt een paar minuten voor de nieuwe TDE-beveiliging doorgegeven aan alle databases en secundaire databases op de server.

   ```powershell
   Get-AzureRmSqlServerTransparentDataEncryptionProtector `
   -ServerName <LogicalServerName> `
   -ResourceGroupName <SQLDatabaseResourceGroupName>
   ```

4. Duren voordat een [back-up van de nieuwe sleutel](/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey) in Key Vault.

   ```powershell
   <# -OutputFile parameter is optional; 
   if removed, a file name is automatically generated. #>
   Backup-AzureKeyVaultKey `
   -VaultName <KeyVaultName> `
   -Name <KeyVaultKeyName> `
   -OutputFile <DesiredBackupFilePath>
   ```

5. Verwijderen van de sleutel waarmee is geknoeid vanuit Key Vault met de [Remove-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/remove-azurekeyvaultkey) cmdlet. 

   ```powershell
   Remove-AzureKeyVaultKey `
   -VaultName <KeyVaultName> `
   -Name <KeyVaultKeyName>
   ```

6. Een sleutel herstellen naar Key Vault in de toekomst met behulp van de [Restore-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey) cmdlet:
   ```powershell
   Restore-AzureKeyVaultKey `
   -VaultName <KeyVaultName> `
   -InputFile <BackupFilePath>
   ```

## <a name="to-make-the-encrypted-resources-inaccessible"></a>Om de versleutelde resources niet toegankelijk

1. Verwijderen van de databases die zijn wordt versleuteld door de sleutel mogelijk waarmee is geknoeid.

   De database en logboekbestanden bestanden automatisch back-ups, zodat een point-in-time-terugzetten van de database kan worden uitgevoerd op elk gewenst moment (zolang u de sleutel opgeven). De databases moeten worden verwijderd vóór de verwijdering van een actieve TDE-beveiliging om te voorkomen dat mogelijk gegevensverlies van maximaal tien minuten van de meest recente transacties. 
2. Back-up van het sleutelmateriaal van de TDE-beveiliging in Key Vault.
3. Verwijder de potentieel aangetast sleutel uit Key Vault

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de TDE-beveiliging van een server om te voldoen aan de beveiligingsvereisten draaien: [De Transparent Data Encryption protector met behulp van PowerShell draaien](transparent-data-encryption-byok-azure-sql-key-rotation.md)
- Aan de slag met Bring Your Own Key-ondersteuning voor TDE: [TDE met behulp van uw eigen sleutel uit Key Vault met behulp van PowerShell inschakelen](transparent-data-encryption-byok-azure-sql-configure.md)

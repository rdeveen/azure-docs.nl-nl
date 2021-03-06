---
title: Visual Studio Code gebruiken om Azure Resource Manager-sjablonen te maken | Microsoft Docs
description: Gebruik Visual Studio Code en de Azure Resource Manager-extensie voor hulpprogramma's om te werken met Resource Manager-sjablonen.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 01/11/2019
ms.topic: quickstart
ms.author: jgao
ms.openlocfilehash: c2684c7373d9ee4536b248650c7012c261166b81
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56235390"
---
# <a name="quickstart-create-azure-resource-manager-templates-by-using-visual-studio-code"></a>Quickstart: Azure Resource Manager-sjablonen maken met Visual Studio Code

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Leer hoe u Visual Studio Code en de Azure Resource Manager Tools-extensie gebruikt om Azure Resource Manager-sjablonen te maken en te bewerken. U kunt Resource Manager-sjablonen maken in Visual Studio Code zonder de extensie, maar de extensie biedt opties voor automatisch aanvullen die het ontwikkelen van sjablonen eenvoudiger maken. Zie [Overzicht van Azure Resource Manager](resource-group-overview.md) voor inzicht in de concepten die gerelateerd zijn aan het implementeren en beheren van uw Azure-oplossingen.

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Vereisten

Als u dit artikel wilt voltooien, hebt u het volgende nodig:

- [Visual Studio Code](https://code.visualstudio.com/).
- Extensie voor Azure Resource Manager-hulpprogramma's. Gebruik de volgende stappen om deze te installeren:

    1. Open Visual Studio Code.
    2. Druk op **Ctrl+Shift+X** om het deelvenster Extensies te openen
    3. Zoek **Azure Resource Manager-hulpprogramma’s** en selecteer **Installeren**.
    4. Selecteer **Opnieuw laden** om de installatie van de extensie te voltooien.

## <a name="open-a-quickstart-template"></a>Een snelstartsjabloon openen

In plaats van een sjabloon helemaal opnieuw te maken, opent u een sjabloon in [Azure-snelstartsjablonen](https://azure.microsoft.com/resources/templates/). Azure-snelstartsjablonen is een opslagplaats voor Resource Manager-sjablonen.

De in deze snelstart gebruikte sjabloon wordt [Create a standard storage account](https://azure.microsoft.com/resources/templates/101-storage-account-create/) (Standaardopslagaccount maken) genoemd. De sjabloon definieert een Azure Storage-accountresource.

1. Selecteer in Visual Studio Code **Bestand**>**Bestand openen**.
2. Plak de volgende URL in **Bestandsnaam**:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
    ```
3. Selecteer **Openen** om het bestand te openen.
4. Selecteer **Bestand**>**Opslaan als** om het bestand op uw lokale computer op te slaan als **azuredeploy.json**.

## <a name="edit-the-template"></a>De sjabloon bewerken

Als u wilt ervaren hoe u een sjabloon met behulp van Visual Studio Code bewerkt, voegt u een extra element toe aan de sectie `outputs` om de opslag-URI weer te geven.

1. Voeg één extra uitvoer toe aan de geëxporteerde sjabloon:

    ```json
    "storageUri": {
      "type": "string",
      "value": "[reference(variables('storageAccountName')).primaryEndpoints.blob]"
    }
    ```

    Als u klaar bent, ziet de uitvoersectie er als volgt uit:

    ```json
    "outputs": {
      "storageAccountName": {
        "type": "string",
        "value": "[variables('storageAccountName')]"
      },
      "storageUri": {
        "type": "string",
        "value": "[reference(variables('storageAccountName')).primaryEndpoints.blob]"
      }
    }
    ```

    Als u de code in Visual Studio Code hebt geplakt, kunt u het element **value** opnieuw typen om een idee te krijgen van de intelliSense-mogelijkheden van de extensie voor Resource Manager-hulpprogramma's.

    ![Visual Studio Code-intelliSense van Resource Manager-sjabloon](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/resource-manager-templates-visual-studio-code-intellisense.png)

2. Selecteer **Bestand**>**Opslaan** om het bestand op te slaan.

## <a name="deploy-the-template"></a>De sjabloon implementeren

Er bestaan meerdere methoden voor het implementeren van sjablonen.  In deze snelstart gebruikt u Azure Cloud Shell. De Cloud Shell is een webtoepassing, die geen configuratie vereist. Het ondersteunt zowel Azure CLI als Azure PowerShell.

1. Meld u aan bij [Azure Cloud Shell](https://shell.azure.com)

    ![Cloud Shell CLI in Azure Portal](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-choose-cli.png)
2. Kies uw favoriete omgeving door in de linkerbovenhoek **PowerShell** of **Bash** te selecteren. Als u CLI wilt gebruiken, moet u een Bash-sessie openen. Als u Azure PowerShell wilt uitvoeren, moet u een PowerShell-sessie openen. Selecteer de pijl-omlaag om te schakelen tussen de Bash en PowerShell. Zie de vorige schermafbeelding. U moet de shell opnieuw starten wanneer u overschakelt.
3. Selecteer **Upload/download files** en selecteer **Uploaden**.

    # <a name="clitabcli"></a>[CLI](#tab/CLI)

    ![Bestand uploaden in Cloud Shell in Azure Portal](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-upload-file.png)
   
    # <a name="powershelltabpowershell"></a>[PowerShell](#tab/PowerShell)
    
    ![Bestand uploaden in Cloud Shell in Azure Portal](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-upload-file-powershell.png)
    
    ---

    Selecteer het bestand dat u in de vorige sectie hebt opgeslagen. De standaardnaam is **azuredeploy.json**. Het sjabloonbestand moet toegankelijk zijn vanuit de shell.

    U kunt eventueel de **ls**-opdracht en de **cat**-opdracht uitvoeren om te controleren of het bestand is geüpload. 

    # <a name="clitabcli"></a>[CLI](#tab/CLI)

    ![Cloud Shell-lijstbestand in Azure Portal](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-list-file.png)
   
    # <a name="powershelltabpowershell"></a>[PowerShell](#tab/PowerShell)
    
    ![Cloud Shell-lijstbestand in Azure Portal](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-list-file-powershell.png)
    
    ---
4. Voer vanuit Cloud Shell de volgende opdrachten uit. Selecteer het tabblad om de PowerShell-code of de CLI-code weer te geven.

    # <a name="clitabcli"></a>[CLI](#tab/CLI)
    ```azurecli
    echo "Enter the Resource Group name:" &&
    read resourceGroupName &&
    echo "Enter the location (i.e. centralus):" &&
    read location &&
    az group create --name $resourceGroupName --location $location &&
    az group deployment create --resource-group $resourceGroupName --template-file "azuredeploy.json"
    ```
   
    # <a name="powershelltabpowershell"></a>[PowerShell](#tab/PowerShell)
    
    ```azurepowershell
    $resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
    $location = Read-Host -Prompt "Enter the location (i.e. centralus)"
    
    New-AzResourceGroup -Name $resourceGroupName -Location $location
    New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile "azuredeploy.json"
    ```
    
    ---

    Werk de naam van de sjabloon bij als u het bestand met een andere naam dan **azuredeploy.json** opslaat.

    In de volgende schermafbeelding ziet u een voorbeeldimplementatie:

    # <a name="clitabcli"></a>[CLI](#tab/CLI)

    ![Sjabloon implementeren in Cloud Shell in Azure Portal](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-deploy-template.png)
   
    # <a name="powershelltabpowershell"></a>[PowerShell](#tab/PowerShell)
    
    ![Sjabloon implementeren in Cloud Shell in Azure Portal](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-deploy-template-powershell.png)
    
    ---

    De naam van het opslagaccount en de URL van de opslag in de uitvoersectie zijn gemarkeerd op de schermafbeelding. U hebt de naam van het opslagaccount nodig in de volgende stap.

5. Voer de volgende CLI- of PowerShell-opdracht uit om het nieuwe opslagaccount weer te geven:

    # <a name="clitabcli"></a>[CLI](#tab/CLI)
    ```azurecli
    echo "Enter the Resource Group name:" &&
    read resourceGroupName &&
    echo "Enter the Storage Account name:" &&
    read storageAccountName &&
    az storage account show --resource-group $resourceGroupName --name $storageAccountName
    ```
   
    # <a name="powershelltabpowershell"></a>[PowerShell](#tab/PowerShell)
    
    ```azurepowershell
    $resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
    $storageAccountName = Read-Host -Prompt "Enter the Storage Account name"
    Get-AzStorageAccount -ResourceGroupName $resourceGroupName -Name $storageAccountName
    ```
    
    ---

Meer informatie over Azure-opslagaccounts vindt u in [Quickstart: blobs uploaden, downloaden en vermelden met behulp van Azure Portal](../storage/blobs/storage-quickstart-blobs-portal.md).

## <a name="clean-up-resources"></a>Resources opschonen

Schoon de geïmplementeerd Azure-resources, wanneer u deze niet meer nodig hebt, op door de resourcegroep te verwijderen.

1. Selecteer **Resourcegroep** in het linkermenu van Azure Portal.
2. Voer de naam van de resourcegroep in het veld **Filter by name** in.
3. Selecteer de naam van de resourcegroep.  U ziet in totaal zes resources in de resourcegroep.
4. Selecteer **Resourcegroep verwijderen** in het bovenste menu.

## <a name="next-steps"></a>Volgende stappen

De focus van deze snelstartgids ligt op het gebruik van Visual Studio Code om een bestaande sjabloon van de Azure-snelstartsjablonen te bewerken. U hebt ook geleerd hoe u de sjabloon kunt implementeren met CLI of PowerShell vanuit Azure Cloud Shell. De sjablonen van de Azure-snelstartsjablonen voldoen mogelijk niet volledig aan uw behoeften. In de volgende zelfstudie leert u hoe u de informatie van de sjabloonverwijzing kunt vinden, zodat u een versleuteld Azure-opslagaccount kunt maken.

> [!div class="nextstepaction"]
> [Een versleuteld opslagaccount maken](./resource-manager-tutorial-create-encrypted-storage-accounts.md)

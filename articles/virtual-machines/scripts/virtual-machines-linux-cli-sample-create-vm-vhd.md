---
title: 'Azure CLI-voorbeeldscript: een virtuele machine met een virtuele harde schijf maken | Microsoft Docs'
description: 'Azure CLI-Script voorbeeld: een virtuele machine met een virtuele harde schijf maken'
services: virtual-machines-linux
documentationcenter: virtual-machines
author: allclark
manager: douge
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/09/2017
ms.author: allclark
ms.custom: mvc
ms.openlocfilehash: 331bf57c415922a6686ba733b5fbcee24699a152
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56118255"
---
# <a name="create-a-vm-with-a-virtual-hard-disk"></a>Een virtuele machine met een virtuele harde schijf maken

In dit voorbeeld wordt een virtuele machine met een virtuele harde schijf gemaakt.
Er wordt eerst een resourcegroep, een opslagaccount en een container gemaakt, en vervolgens een virtuele machine door de virtuele harde schijf naar de container te uploaden.
De openbare ssh-sleutel wordt vervangen door uw eigen openbare sleutel zodat u toegang hebt tot de virtuele machine.

U hebt hiervoor een opstartbare virtuele harde schijf nodig. Het script zoekt naar `~/sample.vhd`.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeldscript

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-vhd/create-vm-vhd.sh "Create VM using a VHD")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Gebruik de volgende opdracht om de resourcegroep, VM, en alle gerelateerde resources te verwijderen.

```azurecli-interactive 
az group delete -n az-cli-vhd
```

## <a name="script-explanation"></a>Uitleg van het script

Dit script gebruikt de volgende opdrachten voor het maken van een resourcegroep, een virtuele machine, een beschikbaarheidsset, een load balancer en alle gerelateerde resources. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group) | Hiermee maakt u een resourcegroep waarin alle resources worden opgeslagen. |
| [az storage account list](https://docs.microsoft.com/cli/azure/storage/account) | Hiermee worden opslagaccounts weergegeven |
| [az storage account check-name](https://docs.microsoft.com/cli/azure/storage/account) | Hiermee wordt gecontroleerd of de naam van een opslagaccount geldig is en of de naam nog niet bestaat |
| [az storage account keys list](https://docs.microsoft.com/cli/azure/storage/account/keys) | Hiermee worden de sleutels voor de opslagaccounts weergegeven |
| [az storage blob exists](https://docs.microsoft.com/cli/azure/storage/blob) | Hiermee wordt gecontroleerd of de blob bestaat |
| [az storage container create](https://docs.microsoft.com/cli/azure/storage/container) | Hiermee maakt u een container in een opslagaccount. |
| [az storage blob upload](https://docs.microsoft.com/cli/azure/storage/blob) | Hiermee maakt u een blob in de container door de virtuele harde schijf te uploaden. |
| [az vm list](https://docs.microsoft.com/cli/azure/vm) | Gebruikt met `--query` om te controleren of de naam van de virtuele machine in gebruik is. | 
| [az vm create](https://docs.microsoft.com/cli/azure/vm/availability-set) | Hiermee maakt u de virtuele machines. |
| [az vm list-ip-addresses](https://docs.microsoft.com/cli/azure/vm#az-vm-list-ip-addresses) | Hiermee haalt u het IP-adres van de gemaakte virtuele machine op. |

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure) voor meer informatie over de Azure CLI.

U kunt extra CLI-scriptvoorbeelden voor virtuele machines vinden in de [Azure-documentatie voor Linux-VM's](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

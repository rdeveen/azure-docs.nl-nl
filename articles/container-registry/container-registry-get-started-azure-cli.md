---
title: 'Quickstart: Een privé-Docker-register maken in Azure - Azure CLI'
description: Leer snel hoe u een persoonlijk Docker-containerregister maakt met behulp van Azure CLI.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: quickstart
ms.date: 01/22/2019
ms.author: danlep
ms.custom: seodec18, H1Hack27Feb2017, mvc
ms.openlocfilehash: 37b1c8516268611a1174edfe20fef36dfb6b36c2
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55295828"
---
# <a name="quickstart-create-a-private-container-registry-using-the-azure-cli"></a>Quickstart: Een privé-containerregister maken met Azure CLI

Azure Container Registry is een beheerde service voor Docker-containerregisters die wordt gebruikt voor het opslaan van installatiekopieën van persoonlijke Docker-containers. In deze handleiding vindt u instructies voor het maken van een Azure Container Registry-exemplaar met behulp van Azure CLI. Gebruik vervolgens Docker-opdrachten om een containerinstallatiekopie naar het register pushen, waarna u de installatiekopie ophaalt en uitvoert vanuit het register.

Voor deze snelstart moet u Azure CLI uitvoeren (versie 2.0.55 of later aanbevolen). Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren][azure-cli].

Docker moet ook lokaal zijn geïnstalleerd. Docker biedt pakketten die eenvoudig Docker configureren op elk [Mac][docker-mac]-, [Windows][docker-windows]- of [Linux][docker-linux]-systeem.

Omdat Azure Cloud-Shell niet alle vereiste Docker-onderdelen bevat (de `dockerd`-daemon), kunt u de Cloud Shell niet voor deze quickstart gebruiken.

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Een resourcegroep maken met de opdracht [az group create][az-group-create]. Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd.

In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *VS - oost*.

```azurecli
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-container-registry"></a>Een containerregister maken

In deze quickstart maakt u een *Basic*-register. Dit is een voor kosten geoptimaliseerde optie voor ontwikkelaars die meer willen leren over Azure Container Registry. Zie [Azure Container Registry-SKU's][container-registry-skus] voor meer informatie over de beschikbare servicelagen.

Maak een ACR-exemplaar met behulp van de opdracht [az acr create][az-acr-create]. De registernaam moet uniek zijn binnen Azure en mag 5 tot 50 alfanumerieke tekens bevatten. In het volgende voorbeeld wordt *myContainerRegistry007* gebruikt. Werk deze waarde bij naar een unieke waarde.

```azurecli
az acr create --resource-group myResourceGroup --name myContainerRegistry007 --sku Basic
```

Wanneer het register is gemaakt, is de uitvoer vergelijkbaar met het volgende:

```json
{
  "adminUserEnabled": false,
  "creationDate": "2019-01-08T22:32:13.175925+00:00",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/myContainerRegistry007",
  "location": "eastus",
  "loginServer": "mycontainerregistry007.azurecr.io",
  "name": "myContainerRegistry007",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "status": null,
  "storageAccount": null,
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

Noteer `loginServer` in de uitvoer. Dit is de volledig gekwalificeerde registernaam (in kleine letters). In de rest van deze quickstart wordt `<acrName>` gebruikt als tijdelijke aanduiding voor de naam van het containerregister.

## <a name="log-in-to-registry"></a>Aanmelden bij register

Voordat u installatiekopieën van containers gaat pushen en ophalen, moet u zich aanmelden bij het register. Gebruik hiervoor de opdracht [az acr login][az-acr-login].

```azurecli
az acr login --name <acrName>
```

Met de opdracht wordt een `Login Succeeded`-bericht geretourneerd nadat deze is voltooid.

[!INCLUDE [container-registry-quickstart-docker-push](../../includes/container-registry-quickstart-docker-push.md)]

## <a name="list-container-images"></a>Containerinstallatiekopieën opvragen

In het volgende voorbeeld worden de opslagplaatsen in uw register vermeld:

```azurecli
az acr repository list --name <acrName> --output table
```

Uitvoer:

```
Result
----------------
busybox
```

In het volgende voorbeeld worden de tags in de **busybox**-opslagplaats vermeld.

```azurecli
az acr repository show-tags --name <acrName> --repository busybox --output table
```

Uitvoer:

```
Result
--------
v1
```

[!INCLUDE [container-registry-quickstart-docker-pull](../../includes/container-registry-quickstart-docker-pull.md)]

## <a name="clean-up-resources"></a>Resources opschonen

U kunt de opdracht [az group delete][az-group-delete] gebruiken om de resourcegroep, het containerregister en de daar opgeslagen containerinstallatiekopieën te verwijderen wanneer u ze niet meer nodig hebt.

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

In deze quickstart hebt u een Azure Container Registry gemaakt met de Azure-CLI. U hebt een containerinstallatiekopie naar het register gepusht en de installatiekopie uit het register opgehaald en uitgevoerd. Ga verder met de zelfstudies voor Azure Container Registry om meer te leren over ACR.

> [!div class="nextstepaction"]
> [Azure Container Registry-zelfstudies][container-registry-tutorial-quick-task]

<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-pull]: https://docs.docker.com/engine/reference/commandline/pull/
[docker-rmi]: https://docs.docker.com/engine/reference/commandline/rmi/
[docker-run]: https://docs.docker.com/engine/reference/commandline/run/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- LINKS - internal -->
[az-acr-create]: /cli/azure/acr#az-acr-create
[az-acr-login]: /cli/azure/acr#az-acr-login
[az-group-create]: /cli/azure/group#az-group-create
[az-group-delete]: /cli/azure/group#az-group-delete
[azure-cli]: /cli/azure/install-azure-cli
[container-registry-tutorial-quick-task]: container-registry-tutorial-quick-task.md
[container-registry-skus]: container-registry-skus.md

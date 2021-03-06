---
title: 'Zelfstudie: Een app upgraden die wordt uitgevoerd in Azure Service Fabric Mesh | Microsoft Docs'
description: In deze zelfstudie leert u hoe u een Service Fabric kunt upgraden in een toepassing die wordt uitgevoerd in Service Fabric Mesh.
services: service-fabric-mesh
documentationcenter: .net
author: rwike77
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/11/2019
ms.author: ryanwi
ms.custom: mvc, devcenter
ms.openlocfilehash: d27cc555a8cf96a07818e83b342cf145d9e98a96
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/14/2019
ms.locfileid: "54265904"
---
# <a name="tutorial-upgrade-a-service-fabric-application-running-in-service-fabric-mesh"></a>Zelfstudie: Een Service Fabric-toepassing upgraden die wordt uitgevoerd in Service Fabric Mesh

Deze zelfstudie is deel drie van een serie. U leert hoe u een Service Fabric-toepassing kunt upgraden die [eerder in Service Fabric Mesh was geïmplementeerd](service-fabric-mesh-tutorial-template-deploy-app.md) door de toegewezen CPU-bronnen uit te breiden.  Als u klaar bent, beschikt u over een front-endwebservice die met uitgebreidere CPU-bronnen wordt uitgevoerd.

In deel drie van de serie leert u het volgende:

> [!div class="checklist"]
> * Toepassingsconfiguraties wijzigen
> * Een toepassing upgraden die wordt uitgevoerd in Service Fabric Mesh

In deze zelfstudiereeks leert u het volgende:
> [!div class="checklist"]
> * [Een toepassing implementeren in Service Fabric Mesh met behulp van een sjabloon](service-fabric-mesh-tutorial-template-deploy-app.md)
> * [Een toepassing schalen die wordt uitgevoerd in Service Fabric Mesh](service-fabric-mesh-tutorial-template-scale-services.md)
> * Een toepassing upgraden die wordt uitgevoerd in Service Fabric Mesh
> * [Een app verwijderen](service-fabric-mesh-tutorial-template-remove-app.md)

[!INCLUDE [preview note](./includes/include-preview-note.md)]

## <a name="prerequisites"></a>Vereisten

Voor u met deze zelfstudie begint:

* Als u nog geen abonnement op Azure hebt, kunt u een [gratis account maken](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) voordat u begint.

* Open [Azure Cloud Shell](service-fabric-mesh-howto-setup-cli.md) of [installeer de Azure CLI en Azure Service Fabric Mesh CLI lokaal](service-fabric-mesh-howto-setup-cli.md#install-the-azure-service-fabric-mesh-cli).

## <a name="upgrade-application-configurations"></a>Toepassingsconfiguraties upgraden

Een van de belangrijkste voordelen van het implementeren van toepassingen in Service Fabric Mesh is de mogelijkheid uw toepassingsconfiguratie makkelijk bij te werken.  Bijvoorbeeld de CPU of geheugenbronnen voor uw services.

In deze zelfstudie wordt het voorbeeld To Do List gebruikt. Dit voorbeeld is [eerder geïmplementeerd](service-fabric-mesh-tutorial-template-deploy-app.md) en moet nu worden uitgevoerd. De toepassing biedt twee services: WebFrontEnd en ToDoService. Elke service is in eerste instantie geïmplementeerd met de waarde 0,5 voor de CPU-bronnen.  Als u de CPU-bronnen voor de service WebFrontEnd wilt zien, voert u deze opdracht uit:

```azurecli
az mesh service show --resource-group myResourceGroup --name WebFrontEnd --app-name todolistapp
```

In de implementatiesjabloon voor de toepassingsresource heeft elke service een eigenschap *cpu* die kan worden gebruikt om de aangevraagde CPU-bronnen in te stellen. Een toepassing kan bestaan uit meerdere services, waarbij elke service een unieke *cpu*-instelling heeft, die samen worden geïmplementeerd en beheerd. Als u de CPU-bronnen van de web-front-endservice wilt uitbreiden, wijzigt u de waarde *cpue* in de implementatiesjabloon of het parameterbestand.  Vervolgens voert u een upgrade van de toepassing uit.

### <a name="modify-the-deployment-template-parameters"></a>Parameters van implementatiesjabloon wijzigen

Wanneer de sjabloon waarden bevat die naar verwachting zullen wijzigen nadat de toepassing is geïmplementeerd, of als u de mogelijkheid wilt hebben om de waarden per implementatie aan te passen (als u van plan bent om deze sjabloon opnieuw te gebruiken voor andere implementaties), is de aanbevolen procedure om de waarden door te geven via parameters.

Eerder werd de toepassing geïmplementeerd via de bestanden [mesh_rp.windows.json deployment template](https://github.com/Azure-Samples/service-fabric-mesh/blob/master/templates/todolist/mesh_rp.windows.json) en [mesh_rp.windows.parameter.json parameters](https://github.com/Azure-Samples/service-fabric-mesh/blob/master/templates/todolist/mesh_rp.windows.parameters.json).

Open het bestand [mesh_rp.windows.parameter.json parameters](https://github.com/Azure-Samples/service-fabric-mesh/blob/master/templates/todolist/mesh_rp.windows.parameters.json) lokaal en stel de waarde *frontEndCpu* in op 1:

```json
      "frontEndCpu":{
        "value": "1"
      }
```

Sla de wijzigingen in het parameterbestand op.  

De parameter *frontEndCpu* wordt gedeclareerd in de sectie *parameters* van het bestand [mesh_rp.windows.json deployment template](https://github.com/Azure-Samples/service-fabric-mesh/blob/master/templates/todolist/mesh_rp.windows.json):

```json
"frontEndCpu": {
    "defaultValue": "0.5",
    "type": "string",
    "metadata": {
        "description": "The CPU resources for the front end web service."
    }
}
```

De eigenschap *codePackages->resources->requests->cpu* van de WebFrontEnd-service verwijst naar de parameter *frontEndCpu*:

```json
    "services": [
          {
            "name": "WebFrontEnd",
            "properties": {
              "description": "WebFrontEnd description.",
              "osType": "Windows",
              "codePackages": [
                {
                  "name": "WebFrontEnd",
                  "image": "[parameters('frontEndImage')]",
                  ...
                  "resources": {
                    "requests": {
                      "cpu": "[parameters('frontEndCpu')]",
                      "memoryInGB": "[parameters('frontEndMemory')]"
                    }
                  },
                  "imageRegistryCredential": {
                    "server": "[parameters('registryServer')]",
                    "username": "[parameters('registryUserName')]",
                    "password": "[parameters('registryPassword')]"
                  }
                }
              ],
              ...
```

### <a name="upgrade-your-application"></a>Uw toepassing upgraden

Terwijl de toepassing wordt uitgevoerd, kunt u deze bijwerken door de sjabloon en het bijgewerkte parameterbestand opnieuw te implementeren:

```azurecli
az mesh deployment create --resource-group myResourceGroup --template-file c:\temp\mesh_rp.windows.json --parameters c:\temp\mesh_rp.windows.parameters.json
```

Hiermee start u een rolling upgrade van uw toepassing en als het goed is, ziet u binnen enkele minuten een toename van het aantal CPU-bronnen.  Als u de CPU-bronnen voor de service WebFrontEnd wilt zien, voert u deze opdracht uit:

```azurecli
az mesh service show --resource-group myResourceGroup --name WebFrontEnd --app-name todolistapp
```

## <a name="next-steps"></a>Volgende stappen

In dit deel van de zelfstudie hebt u het volgende geleerd:

> [!div class="checklist"]
> * Toepassingsconfiguraties wijzigen
> * Een toepassing upgraden die wordt uitgevoerd in Service Fabric Mesh

Ga door naar de volgende zelfstudie:
> [!div class="nextstepaction"]
> [Remove a Service Fabric Mesh application](service-fabric-mesh-tutorial-template-remove-app.md) (Een Service Fabric Mesh-toepassing verwijderen)

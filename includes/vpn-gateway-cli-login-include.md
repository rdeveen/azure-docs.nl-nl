---
title: bestand opnemen
description: bestand opnemen
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 74d7bd087df4b00c0bafb5ec33fbbdfa5c57b379
ms.sourcegitcommit: b3d74ce0a4acea922eadd96abfb7710ae79356e0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2019
ms.locfileid: "56246830"
---
Meld u aan bij uw Azure-abonnement met de opdracht [az login](/cli/azure/) en volg de instructies op het scherm. Zie [Aan de slag met Azure CLI](/cli/azure/get-started-with-azure-cli) voor meer informatie over aanmelden.

```azurecli
az login
```

Als u meer dan één Azure-abonnement hebt, worden alle abonnementen voor het account weergegeven.

```azurecli
az account list --all
```

Geef het abonnement op dat u wilt gebruiken.

```azurecli
az account set --subscription <replace_with_your_subscription_id>
```

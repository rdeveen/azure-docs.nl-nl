---
title: Inleiding tot Azure-wachtrijen | Microsoft Docs
description: Inleiding tot Azure-wachtrijen
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 02/06/2019
ms.author: tamram
ms.subservice: queues
ms.openlocfilehash: 2ae0d3993df54e1c9e5a9bf93619e8f9faa8a917
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55873594"
---
# <a name="what-are-azure-queues"></a>Wat zijn Azure-wachtrijen?

Azure Queue Storage is een service voor de opslag van grote aantallen berichten die via HTTP of HTTPS overal vandaan kunnen worden opgevraagd met geverifieerde aanroepen. Een enkel wachtrijbericht mag maximaal 64 KB groot zijn en een wachtrij kan miljoenen berichten bevatten, tot de totale capaciteitslimiet van een opslagaccount.

## <a name="common-uses"></a>Veelvoorkomende toepassingen

Veelvoorkomende toepassingen van Queue Storage zijn onder andere:

* Het maken van een voorraad werk dat asynchroon moet worden verwerkt
* Het doorgeven van berichten van een Azure-webrol aan een Azure-werkrol

## <a name="queue-service-concepts"></a>Concepten van Queue-service

De Queue-service bevat de volgende onderdelen:

![Concepten van Queue](./media/storage-queues-introduction/queue1.png)

* **URL-indeling hebben:** Wachtrijen kunnen worden opgevraagd met behulp van de volgende URL-indeling:   
    https://`<storage account>`.queue.core.windows.net/`<queue>` 
  
    Met de volgende URL wordt een wachtrij in het diagram opgevraagd:  
  
    `https://myaccount.queue.core.windows.net/images-to-download`

* **Storage-account:** Alle toegang tot Azure Storage vindt plaats via een opslagaccount. Zie [Azure Storage Scalability and Performance Targets](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json) (Schaalbaarheids- en prestatiedoelen in Azure Storage) voor meer informatie over opslagaccountcapaciteit.

* **Wachtrij:** Een wachtrij bevat een set berichten. Alle berichten moeten zich in een wachtrij bevinden. De naam van een wachtrij mag alleen kleine letters bevatten. Zie [Naming Queues and Metadata](https://msdn.microsoft.com/library/azure/dd179349.aspx) (Wachtrijen en metagegevens een naam geven) voor informatie over de naamgeving van wachtrijen.

* **Bericht:** Een bericht in een willekeurige indeling, van maximaal 64 KB. De maximale tijd die een bericht in de wachtrij kan blijven is zeven dagen.

## <a name="next-steps"></a>Volgende stappen

* [Een opslagaccount maken](../storage-create-storage-account.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)
* [Aan de slag met wachtrijen met .NET](storage-dotnet-how-to-use-queues.md)

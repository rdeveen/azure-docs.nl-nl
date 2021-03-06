---
title: Maak een Azure Media Services-taak van een lokaal bestand | Microsoft Docs
description: In dit onderwerp laat zien hoe de Taakinvoer van een maken vanuit een lokaal bestand.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 02/18/2019
ms.author: juliako
ms.openlocfilehash: 3eb16034cc6507944ca7bebb59893e0d72a6f4c9
ms.sourcegitcommit: 79038221c1d2172c0677e25a1e479e04f470c567
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/19/2019
ms.locfileid: "56415456"
---
# <a name="create-a-job-input-from-a-local-file"></a>De Taakinvoer van een maken vanuit een lokaal bestand

Wanneer u taken voor het verwerken van uw video's, verzendt in Media Services v3 hebt u Media Services zien waar vind ik de invoervideo. De invoervideo kan worden opgeslagen als een activum van Media Service, in welk geval het maken van een invoer asset op basis van een bestand (lokaal of in Azure Blob-opslag opgeslagen). In dit onderwerp laat zien hoe de Taakinvoer van een maken vanuit een lokaal bestand. Zie voor een compleet voorbeeld [GitHub voorbeeld](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs).

## <a name="net-sample"></a>.NET-voorbeeld

De volgende code laat zien hoe een invoer asset maken en gebruiken als de invoer voor de taak. De functie CreateInputAsset voert de volgende handelingen uit:

* Hiermee maakt u de Asset
* Haalt een beschrijfbare [SAS-URL](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1) voor de [container in opslag](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-dotnet?tabs=windows#upload-blobs-to-the-container) van de asset
* Uploadt het bestand naar de container in opslag met de SAS-URL

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#CreateInputAsset)]

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#SubmitJob)]

## <a name="job-error-codes"></a>Foutcodes voor taak

Zie [foutcodes](https://docs.microsoft.com/rest/api/media/jobs/get#joberrorcode).

## <a name="next-steps"></a>Volgende stappen

[De Taakinvoer van een maken van een HTTPS-URL](job-input-from-http-how-to.md).

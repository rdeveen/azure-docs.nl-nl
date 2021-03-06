---
title: Pakketten in een Jupyter-notebook op Azure installeren
description: Het installeren van Python, R, en F# pakketten uit binnen een Jupyter-notebook op Azure.
services: app-service
documentationcenter: ''
author: kraigb
manager: douge
ms.assetid: 6f089c12-128b-4dbd-96e3-1320d37eeba4
ms.service: notebooks
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/04/2018
ms.author: kraigb
ms.openlocfilehash: 5baa392d098b0b0e40986bc426c88785db025a29
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2018
ms.locfileid: "53255282"
---
# <a name="install-packages-from-within-a-notebook"></a>Installeren van pakketten van binnen een laptop

Hoewel u kunt configureren de [omgeving voor uw laptop op het niveau van het project](configure-manage-azure-notebooks-projects.md#configure-the-project-environment), kunt u rechtstreeks vanuit de laptop van een afzonderlijke-pakketten installeren.

Pakketten geïnstalleerd vanuit het notitieblok gelden alleen voor de huidige sessie. Installatie van het pakket zijn niet persistent gemaakt nadat de server wordt afgesloten.

## <a name="python"></a>Python

Pakketten in Python kunnen worden geïnstalleerd met behulp van pip of met behulp van de opdrachten binnen code cellen conda:

```bash
!pip install <package_name>

!conda install <package_name> -y
```

Als uitvoer van de opdracht geeft aan dat al door de voorwaarde is voldaan, wordt Azure notitieblokken kan het pakket bevatten standaard. Het pakket kan ook worden geïnstalleerd via een [project omgeving instellingsstap](configure-manage-azure-notebooks-projects.md#configure-the-project-environment).

## <a name="r"></a>R

Pakketten in R kunnen worden geïnstalleerd vanuit CRAN of GitHub met de `install.packages` functie in een codecel:

```r
install.packages("package_name")
```

U kunt ook voorlopige versies en andere pakketten ontwikkeling installeren vanuit GitHub met behulp van de bibliotheek devtools:

```r
options(unzip = 'internal')
library(devtools)
install_github('<user>/<repo>')
```

## <a name="f"></a>F#

Pakketten in F# kan worden geïnstalleerd vanaf [nuget.org](https://www.nuget.org) door het aanroepen van de afhankelijkheid Paket manager uit binnen de cellen van code. Laad eerst de Paket manager:

```fsharp
#load "Paket.fsx"
```

Installeer vervolgens de pakketten:

```fsharp
Paket.Package
[ "MathNet.Numerics"
"MathNet.Numerics.FSharp"
]
```

## <a name="next-steps"></a>Volgende stappen

- [Procedures: Configureren en beheren van projecten](configure-manage-azure-notebooks-projects.md)
- [Procedures: Een diavoorstelling](present-jupyter-notebooks-slideshow.md)

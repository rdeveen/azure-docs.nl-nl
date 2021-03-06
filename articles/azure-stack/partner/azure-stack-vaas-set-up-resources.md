---
title: Zelfstudie - resources instellen voor validatie als een Service | Microsoft Docs
description: In deze zelfstudie leert u hoe u resources voor validatie als een Service instelt.
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
ms.openlocfilehash: 460edc7f092764cce3584c81371db4801fbd69bc
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55240009"
---
# <a name="tutorial-set-up-resources-for-validation-as-a-service"></a>Zelfstudie: Resources voor validatie als een Service instellen

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

U moet een oplossing te maken. Een validatie uit als een serviceoplossing (VaaS) vertegenwoordigt een Azure Stack-oplossing met een bepaalde hardware stuklijst. De oplossing gebruikt u om te controleren of uw hardware uitgevoerd Azure Stack ondersteunen. Volg deze zelfstudie om aan de slag met het gebruik van de service met uw oplossing.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Aan de slag met VaaS gebruiken door het instellen van de Azure AD (Azure AD)-exemplaar.
> * Een opslagaccount maken.

## <a name="configure-an-azure-ad-tenant"></a>Een Azure AD-tenant configureren

Een Azure AD-tenant is vereist voor verificatie en het registreren bij VaaS. De functies op basis van de rol beheer (RBAC) van de tenant wordt gebruikt door de partner om te beheren die in de partnerorganisatie VaaS kunt gebruiken.

Registreren van uw organisatie Azure AD tenant-directory (in plaats van de map Azure AD-tenant voor Azure Stack) en een beleid voor het beheren van de gebruikersaccounts in het tot stand brengen. Zie voor meer informatie, [beheren van uw Azure AD-directory](https://docs.microsoft.com/azure/active-directory/active-directory-administer).

### <a name="create-a-tenant"></a>Een tenant maken

Een tenant maken specifiek voor gebruik met VaaS met een beschrijvende naam, bijvoorbeeld `ContosoVaaS@onmicrosoft.com`.

1. Maken van een Azure AD-tenant in de [Azure-portal](https://portal.azure.com), of gebruik een bestaande tenant. <!-- For instructions on creating new Azure AD tenants, see [Get started with Azure AD](https://docs.microsoft.com/azure/active-directory/get-started-azure-ad). -->

2. Leden van uw organisatie toevoegen aan de tenant. Deze gebruikers is verantwoordelijk voor het gebruik van de service om te bekijken of tests plannen. Wanneer u klaar bent met registratie, definieert u toegangsniveaus voor gebruikers.
 
    Toestaan dat gebruikers in uw tenant om uit te voeren van acties in VaaS door toe te wijzen op een van de volgende rollen:

    | Naam van rol | Beschrijving |
    |---------------------|------------------------------------------|
    | Eigenaar | Heeft volledige toegang tot alle resources. |
    | Lezer | Alle resources weergeven, maar niet maken of te beheren. |
    | Test-Inzender | Kan maken en beheren van resources van de test. |

    Het toewijzen van rollen in de **Azure Stack-validatieservice** toepassing:

    1. Meld u aan bij [Azure Portal](https://portal.azure.com).
    2. Selecteer **alle Services** > **Azure Active Directory** onder de **identiteit** sectie.
    3. Selecteer **bedrijfstoepassingen** > **Azure Stack-validatieservice** toepassing.
    4. Selecteer **Gebruikers en groepen**. De **Azure Stack-validatieservice - gebruikers en groepen** blade geeft een lijst van de gebruikers met een machtiging voor het gebruik van de toepassing.
    5. Selecteer **+ gebruiker toevoegen** toevoegen van een gebruiker uit uw tenant en een rol toe te wijzen.
   
    Als u isoleren VaaS resources en acties van verschillende groepen binnen een organisatie wilt, kunt u meerdere Azure AD-tenant-mappen maken.

### <a name="register-your-tenant"></a>Registreren van uw tenant

Dit proces wordt geautoriseerd voor uw tenant met de **Azure Stack-validatieservice** Azure AD-toepassing.

1. De volgende informatie over de tenant verzenden naar Microsoft op [ vaashelp@microsoft.com ](mailto:vaashelp@microsoft.com).

    | Gegevens | Beschrijving |
    |--------------------------------|---------------------------------------------------------------------------------------------|
    | Naam van organisatie | De naam van de officiële organisatie. |
    | De naam van de map Azure AD-Tenant | De naam van de Azure AD-Tenant-map wordt geregistreerd. |
    | Azure AD-Tenant map-ID | De Azure AD-Tenant-Directory GUID die is gekoppeld aan de directory. Zie voor meer informatie over het vinden van uw Azure AD-Tenant-ID van de Directory [tenant-ID ophalen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal#get-tenant-id). |

2. Wacht op bevestiging van het Azure Stack validatie-team om te controleren dat uw tenant de portal VaaS kunt gebruiken.

### <a name="consent-to-the-vaas-application"></a>Instemmen met de toepassing VaaS

Als de Azure AD-beheerder, geven de VaaS Azure AD-toepassing de vereiste machtigingen namens uw tenant:

1. Gebruik de referenties van de globale beheerder voor de tenant aan te melden bij de [VaaS portal](https://azurestackvalidation.com/). 

2. Selecteer **Mijn Account**.

3 accepteer de voorwaarden om door te gaan wanneer u wordt gevraagd om VaaS de vermelde Azure AD-machtigingen te verlenen.

## <a name="create-an-azure-storage-account"></a>Een Azure Storage-account maken

Tijdens het uitvoeren van test voert VaaS diagnostische logboeken naar een Azure Storage-account. Naast het test-Logboeken, kan het storage-account ook worden gebruikt om het uploaden van de OEM-extensiepakketten voor de werkstroom voor validatie van de oplossing.

De Azure Storage-account wordt gehost in de openbare cloud van Azure, niet op uw Azure Stack-omgeving.

1. Selecteer in de Azure portal, **alle services** > **opslag** > **opslagaccounts**. Op de **Opslagaccounts** Selecteer **toevoegen**.

2. Selecteer het abonnement waarin u het opslagaccount wilt maken.

3. Onder **resourcegroep**, selecteer **nieuw**. Voer een naam voor uw nieuwe resourcegroep.

4. Voer een naam in voor het opslagaccount. De naam die u kiest moet zijn:
    - Uniek zijn voor Azure
    - Tussen de 3 en 24 tekens
    - Mag alleen cijfers en kleine letters

5. Selecteer de **VS West** regio voor uw opslagaccount.

    Om ervoor te zorgen dat netwerken kosten zijn niet in rekening gebracht voor het opslaan van Logboeken, het Azure Storage-account kan worden geconfigureerd voor het gebruik van alleen de **VS West** regio. Replicatie van gegevens en de functie voor hot storage-laag zijn niet nodig om deze gegevens. Inschakelen van de functie, wordt uw kosten aanzienlijk verhogen.

6. Laat u de instellingen op de standaardwaarden, behalve voor **soort Account**:

    - De **implementatiemodel** veld is ingesteld op **Resource Manager** standaard.
    - Het veld **Prestaties** is standaard ingesteld op **Standaard**.
    - Selecteer **soort Account** veld **Blob storage**.
    - De **replicatieveld** is ingesteld op **lokaal redundante opslag (LRS)** standaard.
    - De **toegangslaag** is standaard ingesteld op **Dynamisch**.

7. Klik op **Beoordelen en maken** om uw opslagaccountinstellingen te bekijken en het account te maken.

## <a name="next-steps"></a>Volgende stappen

Als uw omgeving geen in-gebonden verbindingen toestaat, volgt u de zelfstudie over het implementeren van de lokale agent voor het uitvoeren van een test op uw hardware.

> [!div class="nextstepaction"]
> [De lokale agent implementeren](azure-stack-vaas-local-agent.md)

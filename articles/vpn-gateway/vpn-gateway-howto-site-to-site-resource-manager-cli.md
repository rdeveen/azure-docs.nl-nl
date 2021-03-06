---
title: 'Uw on-premises netwerk verbinden met een Azure-netwerk: Site-naar-Site-VPN: CLI | Microsoft Docs'
description: Stappen voor het maken van een IPSec-verbinding van uw on-premises netwerk met een virtueel Azure-netwerk via het openbare internet. Deze stappen helpen u een cross-premises site-naar-site-VPN-gatewayverbinding te maken met CLI.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 10/18/2018
ms.author: cherylmc
ms.openlocfilehash: 18834357651e5fb72dd849a8d8e2e7687f0a8141
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/05/2019
ms.locfileid: "55730353"
---
# <a name="create-a-virtual-network-with-a-site-to-site-vpn-connection-using-cli"></a>Een virtueel netwerk maken met een site-naar-site-VPN-verbinding met CLI

In dit artikel leest u hoe u de Azure CLI gebruikt om een site-naar-site-VPN-gatewayverbinding te maken vanaf uw on-premises netwerk naar het VNet. De stappen in dit artikel zijn van toepassing op het Resource Manager-implementatiemodel. U kunt deze configuratie ook maken met een ander implementatiehulpprogramma of een ander implementatiemodel door in de volgende lijst een andere optie te selecteren:<br>

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [CLI](vpn-gateway-howto-site-to-site-resource-manager-cli.md)
> * [Azure Portal (klassiek)](vpn-gateway-howto-site-to-site-classic-portal.md)
> 
>


![Diagram: cross-premises site-naar-site-VPN-gatewayverbinding](./media/vpn-gateway-howto-site-to-site-resource-manager-cli/site-to-site-diagram.png)

Een site-naar-site-VPN-gatewayverbinding wordt gebruikt om een on-premises netwerk via een IPsec-/IKE-VPN-tunnel (IKEv1 of IKEv2) te verbinden met een virtueel Azure-netwerk. Voor dit type verbinding moet er on-premises een VPN-apparaat aanwezig zijn waaraan een extern openbaar IP-adres is toegewezen. Zie [Overzicht van VPN Gateway](vpn-gateway-about-vpngateways.md) voor meer informatie over VPN-gateways.

## <a name="before-you-begin"></a>Voordat u begint

Controleer voordat u met de configuratie begint, of aan de volgende criteria is voldaan:

* U hebt een compatibel VPN-apparaat nodig en iemand die dit kan configureren. Zie [Over VPN-apparaten](vpn-gateway-about-vpn-devices.md) voor meer informatie over compatibele VPN-apparaten en -apparaatconfiguratie.
* Controleer of u een extern gericht openbaar IPv4-adres voor het VPN-apparaat hebt. Dit IP-adres kan zich niet achter een NAT bevinden.
* Als u de IP-adresbereiken in uw on-premises netwerkconfiguratie niet kent, moet u contact opnemen met iemand die u hierbij kan helpen en de benodigde gegevens kan verstrekken. Wanneer u deze configuratie maakt, moet u de IP-adresbereikvoorvoegsels opgeven die Azure naar uw on-premises locatie doorstuurt. Geen van de subnetten van uw on-premises netwerk kan overlappen met de virtuele subnetten waarmee u verbinding wilt maken.
* U kunt Azure Cloud Shell gebruiken om uit te voeren van de CLI-opdrachten (instructies hieronder). Echter, als u liever uw opdrachten lokaal uitvoeren, controleert u of dat u de meest recente versie van de CLI-opdrachten (2.0 of hoger) hebt geïnstalleerd. Zie [Azure CLI installeren](/cli/azure/install-azure-cli) en [Aan de slag met Azure CLI](/cli/azure/get-started-with-azure-cli) voor meer informatie over de CLI-opdrachten. 
 
  [!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

### <a name="example"></a>Voorbeeldwaarden

U kunt de volgende waarden gebruiken om een testomgeving te maken of ze raadplegen om meer inzicht te krijgen in de voorbeelden in dit artikel:

```
#Example values

VnetName                = TestVNet1 
ResourceGroup           = TestRG1 
Location                = eastus 
AddressSpace            = 10.11.0.0/16 
SubnetName              = Subnet1 
Subnet                  = 10.11.0.0/24 
GatewaySubnet           = 10.11.255.0/27 
LocalNetworkGatewayName = Site2 
LNG Public IP           = <VPN device IP address>
LocalAddrPrefix1        = 10.0.0.0/24
LocalAddrPrefix2        = 20.0.0.0/24   
GatewayName             = VNet1GW 
PublicIP                = VNet1GWIP 
VPNType                 = RouteBased 
GatewayType             = Vpn 
ConnectionName          = VNet1toSite2
```

## <a name="Login"></a>1. Verbinding maken met uw abonnement

Als u ervoor kiest de CLI lokaal uitvoert, verbinding maken met uw abonnement. Als u Azure Cloud Shell in de browser gebruikt, moet u geen verbinding maken met uw abonnement. U wordt automatisch verbinding maken in Azure Cloud Shell. Daarom is het raadzaam om te controleren of dat u van het juiste abonnement gebruikmaakt nadat u verbinding hebt gemaakt.

[!INCLUDE [CLI login](../../includes/vpn-gateway-cli-login-include.md)]

## <a name="rg"></a>2. Een resourcegroep maken

In het volgende voorbeeld wordt een resourcegroep met de naam 'TestRG1' gemaakt op de locatie 'eastus'. Als u al een resourcegroep hebt in de regio waarin u uw VNet wilt maken, kunt u desgewenst ook deze gebruiken.

```azurecli-interactive
az group create --name TestRG1 --location eastus
```

## <a name="VNet"></a>3. Een virtueel netwerk maken

Als u nog geen virtueel netwerk hebt, kunt u er een maken met de opdracht [az network vnet create](/cli/azure/network/vnet). Controleer bij het maken van een virtueel netwerk of de adresruimten die u opgeeft, niet overlappen met adresruimten in uw on-premises netwerk.

>[!NOTE]
>U dient eerst met uw on-premises netwerkbeheerder een IP-adresbereik te reserveren dat u specifiek voor dit virtuele netwerk kunt gebruiken, voordat dit VNet verbinding met een on-premises locatie kan maken. Als er een dubbel adresbereik bestaat aan beide zijden van de VPN-verbinding, wordt verkeer mogelijk niet zoals verwacht gerouteerd. Als u dit VNet met een ander VNet wilt verbinden, kan de adresruimte niet met het andere VNet overlappen. Plan daarom uw netwerkconfiguratie zorgvuldig.
>
>

In het volgende voorbeeld worden een virtueel netwerk met de naam 'TestVNet1' en een subnet met de naam 'Subnet1' gemaakt.

```azurecli-interactive
az network vnet create --name TestVNet1 --resource-group TestRG1 --address-prefix 10.11.0.0/16 --location eastus --subnet-name Subnet1 --subnet-prefix 10.11.0.0/24
```

## 4. <a name="gwsub"></a>Het gatewaysubnet maken


[!INCLUDE [About gateway subnets](../../includes/vpn-gateway-about-gwsubnet-include.md)]

Voer de opdracht [az network vnet subnet create](/cli/azure/network/vnet/subnet) uit om een subnet voor de gateway te maken.

```azurecli-interactive
az network vnet subnet create --address-prefix 10.11.255.0/27 --name GatewaySubnet --resource-group TestRG1 --vnet-name TestVNet1
```

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

## <a name="localnet"></a>5. De lokale netwerkgateway maken

De lokale netwerkgateway verwijst doorgaans naar uw on-premises locatie. U geeft de site een naam waarmee Azure hiernaar kan verwijzen en geeft vervolgens het IP-adres op van het on-premises VPN-apparaat waarmee u verbinding maakt. U geeft ook de IP-adresvoorvoegsels op die via de VPN-gateway worden doorgestuurd naar het VPN-apparaat. De adresvoorvoegsels die u opgeeft, zijn de voorvoegsels die zich in uw on-premises netwerk bevinden. Als uw on-premises netwerk verandert, kunt u de voorvoegsels eenvoudig bijwerken.

Gebruik de volgende waarden:

* Het *--gateway-ip-address* is het IP-adres van uw on-premises VPN-apparaat. Het VPN-apparaat kan zich niet achter een NAT bevinden.
* De *--local-address-prefixes* zijn uw on-premises adresruimten.

Gebruik de opdracht [az network local-gateway create](/cli/azure/network/local-gateway) om een gateway voor het lokale netwerk met meerdere adresvoorvoegsels toe te voegen:

```azurecli-interactive
az network local-gateway create --gateway-ip-address 23.99.221.164 --name Site2 --resource-group TestRG1 --local-address-prefixes 10.0.0.0/24 20.0.0.0/24
```

## <a name="PublicIP"></a>6. Een openbaar IP-adres aanvragen

Een VPN Gateway moet een openbaar IP-adres hebben. U vraagt eerst de resource van het IP-adres aan en verwijst hier vervolgens naar bij het maken van uw virtuele netwerkgateway. Het IP-adres wordt dynamisch aan de resource toegewezen wanneer de VPN Gateway wordt gemaakt. VPN Gateway ondersteunt momenteel alleen *dynamische* toewijzing van openbare IP-adressen. U kunt geen toewijzing van een statisch openbaar IP-adres aanvragen. Dit betekent echter niet dat het IP-adres wordt gewijzigd nadat het aan uw VPN Gateway is toegewezen. Het openbare IP-adres verandert alleen wanneer de gateway wordt verwijderd en opnieuw wordt gemaakt. Het verandert niet wanneer de grootte van uw VPN Gateway verandert, wanneer deze gateway opnieuw wordt ingesteld of wanneer andere interne onderhoudswerkzaamheden of upgrades worden uitgevoerd.

Gebruik de opdracht [az network public-ip create](/cli/azure/network/public-ip) om een dynamisch openbaar IP-adres aan te vragen.

```azurecli-interactive
az network public-ip create --name VNet1GWIP --resource-group TestRG1 --allocation-method Dynamic
```

## <a name="CreateGateway"></a>7. De VPN-gateway maken

Maak de VPN-gateway van het virtuele netwerk. Het maken van een VPN-gateway kan 45 minuten of langer duren.

Gebruik de volgende waarden:

* Het *--gateway-type* voor een site-naar-site-configuratie is *VPN*. Het gatewaytype is altijd specifiek voor de configuratie die u implementeert. Zie [Soorten gateways](vpn-gateway-about-vpn-gateway-settings.md#gwtype) voor meer informatie.
* Het *--vpn-type* kan *RouteBased* (in sommige documentatie een 'dynamische gateway' genoemd) of *PolicyBased* (in sommige documentatie een 'statische gateway' genoemd) zijn. De instelling is specifiek voor de vereisten van het apparaat waarmee u verbinding maakt. Zie [Informatie over VPN-gatewayconfiguratie-instellingen](vpn-gateway-about-vpn-gateway-settings.md#vpntype) voor meer informatie over soorten VPN-gateways.
* Selecteer de gateway-SKU die u wilt gebruiken. Voor bepaalde SKU's gelden configuratiebeperkingen. Zie [Gateway-SKU's](vpn-gateway-about-vpn-gateway-settings.md#gwsku) voor meer informatie.

Maak de VPN Gateway met behulp van de opdracht [az network vnet-gateway create](/cli/azure/network/vnet-gateway). Als u deze opdracht uitvoert met de parameter --no-wait, ziet u geen feedback of uitvoer. Deze parameter zorgt ervoor dat de gateway op de achtergrond wordt gemaakt. Het duurt ongeveer 45 minuten om een gateway te maken.

```azurecli-interactive
az network vnet-gateway create --name VNet1GW --public-ip-address VNet1GWIP --resource-group TestRG1 --vnet TestVNet1 --gateway-type Vpn --vpn-type RouteBased --sku VpnGw1 --no-wait 
```

## <a name="VPNDevice"></a>8. Uw VPN-apparaat configureren

Voor site-naar-site-verbindingen met een on-premises netwerk is een VPN-apparaat vereist. In deze stap configureert u het VPN-apparaat. Bij de configuratie van uw VPN-apparaat hebt u het volgende nodig:

- Een gedeelde sleutel. Dit is dezelfde gedeelde sleutel die u opgeeft wanneer u uw site-naar-site-VPN-verbinding maakt. In onze voorbeelden gebruiken we een eenvoudige gedeelde sleutel. We raden u aan een complexere sleutel te genereren.
- Het openbare IP-adres van de gateway van uw virtuele netwerk. U kunt het openbare IP-adres weergeven met behulp van Azure Portal, PowerShell of de CLI. Gebruik de opdracht [az network public-ip list](/cli/azure/network/public-ip) om het openbare IP-adres van uw virtuele netwerkgateway te vinden. De lijst met openbare IP-adressen wordt in een gemakkelijk leesbare tabelindeling weergegeven.

  ```azurecli-interactive
  az network public-ip list --resource-group TestRG1 --output table
  ```


[!INCLUDE [Configure VPN device](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]


## <a name="CreateConnection"></a>9. De VPN-verbinding maken

Maak de site-naar-site-VPN-verbinding tussen de gateway van uw virtuele netwerk en het on-premises VPN-apparaat. Let vooral op de waarde van de gedeelde sleutel. Deze moet overeenkomen met de geconfigureerde waarde van de gedeelde sleutel voor het VPN-apparaat.

Maak de verbinding met behulp van de opdracht [az network vpn-connection create](/cli/azure/network/vpn-connection).

```azurecli-interactive
az network vpn-connection create --name VNet1toSite2 --resource-group TestRG1 --vnet-gateway1 VNet1GW -l eastus --shared-key abc123 --local-gateway2 Site2
```

Na een korte tijd wordt de verbinding tot stand gebracht.

## <a name="toverify"></a>10. De VPN-verbinding controleren

[!INCLUDE [verify connection](../../includes/vpn-gateway-verify-connection-cli-rm-include.md)]

Zie [Een VPN-gatewayverbinding controleren](vpn-gateway-verify-connection-resource-manager.md) als u een andere methode wilt gebruiken om uw verbinding te controleren.

## <a name="connectVM"></a>Verbinding maken met een virtuele machine

[!INCLUDE [Connect to a VM](../../includes/vpn-gateway-connect-vm-s2s-include.md)]

## <a name="tasks"></a>Algemene taken

Deze sectie bevat veelgebruikte opdrachten die nuttig zijn bij het werken met site-naar-siteconfiguraties. Zie [Azure CLI - Networking](/cli/azure/network) (Azure CLI: netwerken) voor een volledige lijst met CLI-netwerkopdrachten.

[!INCLUDE [local network gateway common tasks](../../includes/vpn-gateway-common-tasks-cli-include.md)]

## <a name="next-steps"></a>Volgende stappen

* Wanneer de verbinding is voltooid, kunt u virtuele machines aan uw virtuele netwerken toevoegen. Zie [Virtuele machines](https://docs.microsoft.com/azure/) voor meer informatie.
* Voor meer informatie over BGP raadpleegt u [BGP Overview](vpn-gateway-bgp-overview.md) (BGP-overzicht) en [How to configure BGP](vpn-gateway-bgp-resource-manager-ps.md) (BGP configureren).
* Zie [Informatie over geforceerde tunneling](vpn-gateway-forced-tunneling-rm.md) voor meer informatie over geforceerde tunneling.
* Zie [Maximaal beschikbare cross-premises en VNet-naar-VNet-connectiviteit](vpn-gateway-highlyavailable.md) voor meer informatie over maximaal beschikbare actieve verbindingen.
* Zie [Azure CLI](https://docs.microsoft.com/cli/azure/network) voor een lijst met Azure CLI-opdrachten voor netwerken.
* Zie [Een site-naar-site-VPN-verbinding maken](https://azure.microsoft.com/resources/templates/101-site-to-site-vpn-create/) voor meer informatie over het maken van een site-naar-site-VPN-verbinding met behulp van een Azure Resource Manager-sjabloon.
* Zie [HBase-geo-replicatie implementeren](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-replication-geo/) voor meer informatie over het maken van een vnet-naar-vnet-VPN-verbinding met behulp van een Azure Resource Manager-sjabloon.

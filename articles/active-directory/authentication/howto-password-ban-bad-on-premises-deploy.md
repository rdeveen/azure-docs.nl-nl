---
title: Voorbeeld van Azure AD-wachtwoord beveiliging implementeren
description: De Preview-versie van het Azure AD-wachtwoord Protection als u wilt verbieden onjuiste wachtwoorden on-premises implementeren
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: article
ms.date: 02/01/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jsimmons
ms.collection: M365-identity-device-management
ms.openlocfilehash: e8d39b614c373c63cf1405c5db0f64581c481d1f
ms.sourcegitcommit: 79038221c1d2172c0677e25a1e479e04f470c567
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/19/2019
ms.locfileid: "56417195"
---
# <a name="preview-deploy-azure-ad-password-protection"></a>Preview: Azure AD-wachtwoord beveiliging implementeren

|     |
| --- |
| Beveiliging van Azure AD-wachtwoord is een openbare preview-functie van Azure Active Directory. Zie voor meer informatie over previews [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

Nu dat we een goed begrip van hebben [hoe af te dwingen van Azure AD wachtwoord beveiliging voor Windows Server Active Directory](concept-password-ban-bad-on-premises.md), de volgende stap is het plannen en uitvoeren van de implementatie.

## <a name="deployment-strategy"></a>Implementatiestrategie

Microsoft raadt aan dat elke implementatie start in de controlemodus. De controlemodus is de eerste standaardinstelling waarbij wachtwoorden kunnen blijven worden ingesteld en die zouden worden geblokkeerd-vermeldingen te maken in het gebeurtenislogboek. Zodra de proxy server (s) en DC-agents zijn volledig is geïmplementeerd in de controlemodus, regelmatige controle moet worden uitgevoerd om te bepalen welke gevolgen wachtwoordbeleid afdwingen kan hebben op de gebruikers en de omgeving als het beleid is afgedwongen.

Tijdens de fase audit vinden veel organisaties:

* Ze moeten voor het verbeteren van de bestaande operationele processen voor het gebruik van meer veilige wachtwoorden.
* Gebruikers gewend zijn aan niet-beveiligde wachtwoorden regelmatig te kiezen
* Ze moeten informeren over gebruikers over toekomstige wijzigingen in de afdwinging van apparaatbeveiliging, de gevolgen kan hebben op deze en waarmee ze beter te begrijpen hoe ze beter beveiligde wachtwoorden kunnen kiezen.

Zodra de functie is voor een redelijke termijn in de controlemodus is uitgevoerd, de configuratie voor uitvoering van kan worden gespiegeld **controleren** naar **afdwingen** waarbij vereist meer veilige wachtwoorden. Gerichte bewaking gedurende deze tijd is een goed idee.

## <a name="deployment-requirements"></a>Vereisten voor de implementatie

* Alle domeincontrollers waarop de service Azure AD wachtwoord beveiliging DC-Agent wordt geïnstalleerd moeten worden uitgevoerd worden WindowsServer 2012 of hoger.
* Alle computers waarop de Azure AD-wachtwoord beveiliging Proxy-service wordt geïnstalleerd moeten worden uitgevoerd op Windows Server 2012 R2 of hoger.
* Alle computers waarop wachtwoordbeveiliging van Azure AD-onderdelen zijn geïnstalleerd met inbegrip van domeincontrollers moeten de Universal C-runtime geïnstalleerd hebben.
Dit gebeurt bij voorkeur volledig patches voor de machine via Windows Update. Anders een juiste specifieke update-pakket kan worden geïnstalleerd: Zie [bijwerken voor universele C tijdens Runtime in Windows](https://support.microsoft.com/help/2999226/update-for-universal-c-runtime-in-windows)
* Verbinding met het netwerk moet bestaan tussen ten minste één domeincontroller in elk domein en ten minste één server die als host fungeert voor de Azure AD-wachtwoord beveiliging Proxy-service. Deze connectiviteit moet de domeincontroller voor toegang tot de RPC-eindpunt-mapper-poort (135) en de RPC-server-poort op de proxy-service toestaan. De RPC-server-poort is standaard een dynamische RPC-poort, maar kan worden geconfigureerd (Zie hieronder). Gebruik een statische poort.
* Alle machines die als host fungeert voor de Azure AD-wachtwoord beveiliging Proxy-service moeten netwerktoegang hebben tot de volgende eindpunten:

    |Eindpunt |Doel|
    | --- | --- |
    |`https://login.microsoftonline.com`|Verificatieaanvragen|
    |`https://enterpriseregistration.windows.net`|Functionaliteit van Azure AD-wachtwoordbeveiliging|

* Alle machines die als host fungeert voor de Azure AD-wachtwoord beveiliging Proxy-service moeten worden geconfigureerd voor het toestaan van uitgaande TLS 1.2 HTTP-verkeer.
* Een globale beheerdersaccount voor de Azure AD-wachtwoord beveiliging Proxy-service en -forest registreren bij Azure AD.
* Een account met Active Directory-domein administrator-bevoegdheden in het forest-hoofddomein het Windows Server Active Directory-forest registreren bij Azure AD.
* Een Active Directory-domein met de DC agent-service-software gebruiken DFS-replicatie voor sysvol-replicatie.

## <a name="single-forest-deployment"></a>Implementatie met één forest

Het volgende diagram toont hoe de basisonderdelen van Azure AD-wachtwoord Protection samenwerken in een on-premises Active Directory-omgeving.

![Hoe Azure AD-wachtwoord beveiliging onderdelen samenwerken](./media/concept-password-ban-bad-on-premises/azure-ad-password-protection.png)

Voorafgaand aan de implementatie is het een goed idee om te controleren hoe de software werkt; Raadpleeg [conceptueel overzicht van Azure AD-wachtwoordbeveiliging](concept-password-ban-bad-on-premises.md).

### <a name="download-the-software"></a>De software downloaden

Er zijn twee vereist installatieprogramma's voor Azure AD-wachtwoord beveiliging die kan worden gedownload vanaf de [Microsoft Downloadcentrum](https://www.microsoft.com/download/details.aspx?id=57071)

### <a name="install-and-configure-the-azure-ad-password-protection-proxy-service"></a>Installeer en configureer de proxy-service van Azure AD-wachtwoord beveiliging

1. Kies een of meer servers als host voor de Azure AD-wachtwoord beveiliging Proxy-service.
   * Elke dergelijke service alleen wachtwoordbeleid biedt voor één forest en de hostcomputer moet worden toegevoegd aan een domein-(en onderliggende domeinen worden beide ondersteund) in dat forest. In de volgorde voor de Azure AD-wachtwoord beveiliging Proxy-service om te voldoen aan de missie, moet bestaan netwerkverbinding tussen ten minste één domeincontroller in elk domein van het forest en de Azure AD-wachtwoord beveiliging Proxy-machine.
   * Dit wordt ondersteund voor het installeren en uitvoeren van de Azure AD-wachtwoord beveiliging Proxy-service op een domeincontroller voor testdoeleinden; Het nadeel is dat de domeincontroller vervolgens verbinding met internet heeft dit kan een beveiligingsprobleem is vereist. Microsoft raadt aan dat deze configuratie alleen worden gebruikt voor testdoeleinden.
   * Ten minste twee proxyservers wordt aanbevolen voor redundantie doeleinden. [Zie de hoge beschikbaarheid](howto-password-ban-bad-on-premises-deploy.md#high-availability)

2. Installeer de Azure AD-wachtwoord beveiliging Proxy-service met behulp van het AzureADPasswordProtectionProxySetup.msi MSI-pakket.
   * De software-installatie vereist niet opnieuw worden opgestart. De software-installatie kan worden geautomatiseerd met behulp van de standaardprocedures van het MSI-bestand, bijvoorbeeld: `msiexec.exe /i AzureADPasswordProtectionProxySetup.msi /quiet /qn`

      > [!NOTE]
      > De service Windows Firewall moet worden uitgevoerd voordat u het AzureADPasswordProtectionProxySetup.msi MSI-pakket installeert of anders een fout bij de installatie wordt uitgevoerd. Als de Windows Firewall is geconfigureerd voor het niet wordt uitgevoerd, wordt de tijdelijke oplossing is tijdelijk inschakelen en starten van de Windows Firewall-service tijdens het installatieproces. De Proxy-software heeft geen specifieke afhankelijkheid van de Windows Firewall-software na de installatie. Als u een firewall van derden gebruikt, moet deze nog steeds worden geconfigureerd om te voldoen aan de vereisten voor de implementatie (binnenkomende toegang tot poort 135 en geven de RPC-proxy-serverpoort of dynamisch of statisch). [Zie de vereisten voor de implementatie](howto-password-ban-bad-on-premises-deploy.md#deployment-requirements)

3. Open een PowerShell-venster als beheerder.
   * De software Protection-Proxy voor Azure AD-wachtwoord bevat een nieuwe PowerShell-module met de naam AzureADPasswordProtection. De volgende stappen zijn gebaseerd op het uitvoeren van verschillende cmdlets van deze PowerShell-module en wordt ervan uitgegaan dat u hebt een nieuwe PowerShell-venster geopend en de nieuwe module als volgt hebt geïmporteerd:

      ```PowerShell
      Import-Module AzureADPasswordProtection
      ```

   * Controleer of de service wordt uitgevoerd met behulp van de volgende PowerShell-opdracht: `Get-Service AzureADPasswordProtectionProxy | fl`.
     Het resultaat te produceren van een resultaat met de **Status** retourneert een resultaat 'Running'.

4. Registreer de proxy.
   * Nadat stap 3 is voltooid wordt de Azure AD-wachtwoord beveiliging Proxy-service op de machine wordt uitgevoerd, maar wordt nog niet de vereiste referenties om te communiceren met Azure AD. Registratie bij Azure AD is vereist om in te schakelen die mogelijkheid met behulp van de `Register-AzureADPasswordProtectionProxy` PowerShell-cmdlet. De cmdlet is vereist voor hoofdbeheerder referenties voor uw Azure-tenant, evenals on-premises Active Directory-domein administrator-bevoegdheden in het forest-hoofddomein. Zodra deze is voltooid voor een opgegeven proxy-service, extra aanroepen van `Register-AzureADPasswordProtectionProxy` voortgezet, maar zijn niet nodig.

      De cmdlet Register-AzureADPasswordProtectionProxy ondersteunt drie verschillende verificatiemodi als volgt.

      * Modus voor interactieve verificatie:

         ```PowerShell
         Register-AzureADPasswordProtectionProxy -AccountUpn 'yourglobaladmin@yourtenant.onmicrosoft.com'
         ```
         > [!NOTE]
         > In deze modus werkt niet op Server Core-besturingssystemen. In plaats daarvan gebruik een van de alternatieve verificatiemodi zoals die hieronder worden beschreven.

         > [!NOTE]
         > In deze modus kan mislukken als verbeterde beveiliging van Internet Explorer is ingeschakeld. De tijdelijke oplossing is het uitschakelen van IESC, het registreren van de proxy, IESC vervolgens opnieuw in te schakelen.

      * Apparaat-code-verificatiemodus:

         ```PowerShell
         Register-AzureADPasswordProtectionProxy -AccountUpn 'yourglobaladmin@yourtenant.onmicrosoft.com' -AuthenticateUsingDeviceMode
         To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code XYZABC123 to authenticate.
         ```

         Vervolgens kunt u de verificatie voltooien door de weergegeven instructies te volgen op een ander apparaat.

      * Op de achtergrond (op basis van wachtwoorden)-verificatiemodus:

         ```PowerShell
         $globalAdminCredentials = Get-Credential
         Register-AzureADPasswordProtectionForest -AzureCredential $globalAdminCredentials
         ```

         > [!NOTE]
         > In deze modus mislukt als de verificatie MFA voor een bepaalde reden vereist. Als dit het geval is, moet u een van de vorige twee modi voor u om uit te voeren een MFA-gebaseerde authenticatie.

      Het is momenteel niet vereist om op te geven van de parameter - ForestCredential die is gereserveerd voor toekomstige functionaliteit.

   > [!NOTE]
   > Registratie van de proxy-service van Azure AD-wachtwoord beveiliging wordt verwacht een eenmalige stap in de levensduur van de service. De proxy-service wordt automatisch eventuele andere benodigde onderhoud uitvoeren vanaf dit punt en hoger. Zodra deze is voltooid voor een bepaalde proxy, extra aanroepen van 'Register-AzureADPasswordProtectionProxy' voortgezet, maar zijn niet nodig.

   > [!TIP]
   > Mogelijk zijn er een aanzienlijke vertraging (op hoeveel seconden) de eerste keer dat deze cmdlet wordt uitgevoerd voor een bepaalde Azure-tenant voordat de cmdlet is voltooid. Tenzij een verzendfouten deze vertraging mogen niet worden beschouwd als verontrustend.

5. Registreer het forest.
   * De on-premises Active Directory-forest moet worden geïnitialiseerd met de benodigde gegevens om te communiceren met Azure met de `Register-AzureADPasswordProtectionForest` PowerShell-cmdlet. De cmdlet is vereist voor hoofdbeheerder referenties voor uw Azure-tenant, evenals on-premises Active Directory-domein administrator-bevoegdheden in het forest-hoofddomein. Deze stap wordt één keer per forest uitgevoerd.

      De cmdlet Register-AzureADPasswordProtectionForest ondersteunt drie verschillende verificatiemodi als volgt.

      * Modus voor interactieve verificatie:

         ```PowerShell
         Register-AzureADPasswordProtectionForest -AccountUpn 'yourglobaladmin@yourtenant.onmicrosoft.com'
         ```
         > [!NOTE]
         > In deze modus werkt niet op Server Core-besturingssystemen. In plaats daarvan gebruik een van de alternatieve verificatiemodi zoals die hieronder worden beschreven.

         > [!NOTE]
         > In deze modus kan mislukken als verbeterde beveiliging van Internet Explorer is ingeschakeld. De tijdelijke oplossing is het uitschakelen van IESC, het registreren van de proxy, IESC vervolgens opnieuw in te schakelen.  

      * Apparaat-code-verificatiemodus:

         ```PowerShell
         Register-AzureADPasswordProtectionForest -AccountUpn 'yourglobaladmin@yourtenant.onmicrosoft.com' -AuthenticateUsingDeviceMode
         To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code XYZABC123 to authenticate.
         ```

         Vervolgens kunt u de verificatie voltooien door de weergegeven instructies te volgen op een ander apparaat.

      * Op de achtergrond (op basis van wachtwoorden)-verificatiemodus:
         ```PowerShell
         $globalAdminCredentials = Get-Credential
         Register-AzureADPasswordProtectionForest -AzureCredential $globalAdminCredentials
         ```

         > [!NOTE]
         > In deze modus mislukt als de verificatie is vereist voor MFA. Als dit het geval is, moet u een van de vorige twee modi voor u om uit te voeren een MFA-gebaseerde authenticatie.

      De bovenstaande voorbeelden slaagt alleen als de momenteel aangemelde gebruiker ook een beheerder van Active Directory-domein voor het hoofddomein is. Als dit niet het geval is, kunt u alternatieve domeinreferenties via de parameter - ForestCredential opgeven.

   > [!NOTE]
   > Als meerdere proxy-servers zijn geïnstalleerd in uw omgeving, het maakt niet uit welke proxyserver wordt gebruikt voor het registreren van het forest.

   > [!TIP]
   > Mogelijk zijn er een aanzienlijke vertraging (op hoeveel seconden) de eerste keer dat deze cmdlet wordt uitgevoerd voor een bepaalde Azure-tenant voordat de cmdlet is voltooid. Tenzij een verzendfouten deze vertraging mogen niet worden beschouwd als verontrustend.

   > [!NOTE]
   > Registratie van het Active Directory-forest is een eenmalige stap verwacht in de levensduur van het forest. De domain controller agents die worden uitgevoerd in het forest zal automatisch eventuele andere benodigde maintainenance uitvoeren vanaf dit punt en hoger. Zodra deze is voltooid voor een opgegeven forest, extra aanroepen van `Register-AzureADPasswordProtectionForest` voortgezet, maar zijn niet nodig.

   > [!NOTE]
   > Opdat `Register-AzureADPasswordProtectionForest` te voltooien ten minste één Windows Server 2012 of hoger domain controller moet beschikbaar zijn in het domein van de proxyserver. Er is echter geen vereiste dat de DC-agentsoftware worden geïnstalleerd op een domeincontroller voordat u deze stap.

6. De Azure AD-wachtwoord beveiliging Proxy-service om te communiceren via een HTTP-proxy configureren

   Als uw omgeving het gebruik van een specifieke HTTP-proxy om te communiceren met Azure vereist, kan dit als volgt worden bereikt.

   Maak een bestand met de naam `proxyservice.exe.config` -bestand in de `%ProgramFiles%\Azure AD Password Protection Proxy\Service` map met de volgende inhoud:

      ```xml
      <configuration>
        <system.net>
          <defaultProxy enabled="true">
           <proxy bypassonlocal="true"
               proxyaddress="http://yourhttpproxy.com:8080" />
          </defaultProxy>
        </system.net>
      </configuration>
      ```

   Als uw HTTP-proxy verificatie is vereist, als volgt de useDefaultCredentials-tag toevoegen:

      ```xml
      <configuration>
        <system.net>
          <defaultProxy enabled="true" useDefaultCredentials="true">
           <proxy bypassonlocal="true"
               proxyaddress="http://yourhttpproxy.com:8080" />
          </defaultProxy>
        </system.net>
      </configuration>
      ```

   In beide gevallen zou u vervangen `http://yourhttpproxy.com:8080` met het adres en poort van uw specifieke HTTP-proxyserver.

   Als uw HTTP-proxy is geconfigureerd met een autorisatiebeleid, moet de toegang worden verleend aan het Active Directory-computeraccount van de computer die als host fungeert voor de Azure AD-wachtwoord beveiliging Proxy-service.

   U moet stoppen en opnieuw starten van de Azure AD-wachtwoord beveiliging Proxy-service na het maken of bijwerken van de `proxyservice.exe.config` bestand.

   De Azure AD-wachtwoord beveiliging Proxy-service biedt geen ondersteuning voor het gebruik van specifieke referenties voor het verbinden met een HTTP-proxy.

7. Optioneel: De Azure AD-wachtwoord beveiliging Proxy-service om te luisteren naar een specifieke poort configureren.
   * RPC via TCP wordt gebruikt door de Azure AD-wachtwoord beveiliging DC-agentsoftware op de domeincontrollers om te communiceren met de Azure AD-wachtwoord beveiliging Proxy-service. Standaard luistert de Azure AD-wachtwoord beveiliging Proxy-service op alle beschikbare dynamische RPC-eindpunt. Indien nodig vanwege de netwerktopologie of firewall-vereisten, kan de service in plaats daarvan worden geconfigureerd om te luisteren naar een specifieke TCP-poort.
      * Gebruik voor het configureren van de service moet worden uitgevoerd in een statische poort de `Set-AzureADPasswordProtectionProxyConfiguration` cmdlet.
         ```PowerShell
         Set-AzureADPasswordProtectionProxyConfiguration –StaticPort <portnumber>
         ```

         > [!WARNING]
         > U moet stoppen en opnieuw starten van de service voor deze wijzigingen worden doorgevoerd.

      * Voor het configureren van de service moet worden uitgevoerd in een dynamische poort, gebruik dezelfde procedure, maar StaticPort opnieuw ingesteld op nul, zoals:
         ```PowerShell
         Set-AzureADPasswordProtectionProxyConfiguration –StaticPort 0
         ```

         > [!WARNING]
         > U moet stoppen en opnieuw starten van de service voor deze wijzigingen worden doorgevoerd.

   > [!NOTE]
   > De Azure AD-wachtwoord beveiliging Proxy-service vereist een handmatig opnieuw opstarten na elke wijziging in de poortconfiguratie. Het is niet nodig om opnieuw te starten van de DC agent-service-software uitgevoerd op domeincontrollers na het aanbrengen van wijzigingen in de configuratie van deze aard.

   * De huidige configuratie van de service kan worden opgevraagd met de `Get-AzureADPasswordProtectionProxyConfiguration` cmdlet zoals in het volgende voorbeeld wordt weergegeven:

      ```PowerShell
      Get-AzureADPasswordProtectionProxyConfiguration | fl

      ServiceName : AzureADPasswordProtectionProxy
      DisplayName : Azure AD password protection Proxy
      StaticPort  : 0
      ```

### <a name="install-the-azure-ad-password-protection-dc-agent-service"></a>Installeer de agent-service van Azure AD-wachtwoord beveiliging DC

   Installeer de Azure AD-wachtwoord beveiliging DC agent service software met de `AzureADPasswordProtectionDCAgent.msi` MSI-pakket

   De software-installatie moeten worden opgestart op installeren en verwijderen vanwege de vereiste van het besturingssysteem dat wachtwoord filter dll-bestanden alleen worden geladen of is verwijderd na het opnieuw opstarten.

   Het wordt ondersteund voor het installeren van de DC-agent-service op een computer die nog niet een domeincontroller. In dit geval de service wordt gestart en uitvoeren, maar wordt anders inactief totdat zijn nadat de computer wordt gepromoveerd tot een domeincontroller.

   De software-installatie kan worden geautomatiseerd met behulp van de standaardprocedures van het MSI-bestand, bijvoorbeeld:

   `msiexec.exe /i AzureADPasswordProtectionDCAgent.msi /quiet /qn`

   > [!WARNING]
   > Het bovenstaande voorbeeld msiexec opdracht, leidt dit onmiddellijk opnieuw opstarten; Dit kan worden vermeden door op te geven de `/norestart` vlag.

Eenmaal geïnstalleerd op een domeincontroller en opnieuw opgestart, wordt de installatie van de Azure AD-wachtwoord beveiliging DC-Agent is voltooid. Er is geen andere configuratie vereist of mogelijk.

## <a name="multiple-forest-deployments"></a>Implementaties met meerdere forests

Er zijn geen aanvullende vereisten voor het implementeren van Azure AD-wachtwoord beveiliging in verschillende forests. Elk forest is onafhankelijk van elkaar geconfigureerd zoals beschreven in de sectie met één forest-implementatie. Elke Azure AD-wachtwoord beveiliging Proxy kan alleen ondersteuning voor domeincontrollers in het forest dat deze is gekoppeld aan. De software Protection voor Azure AD-wachtwoord in een opgegeven forest is geen informatie over Azure AD-wachtwoord-virussoftware is geïmplementeerd in een ander forest, ongeacht de Active Directory-vertrouwensrelaties.

## <a name="read-only-domain-controllers"></a>Alleen-lezen domeincontrollers

Wachtwoord changes\sets worden nooit verwerkt en opgeslagen op alleen-lezen domeincontrollers (RODC's); Deze worden in plaats daarvan doorgestuurd naar beschrijfbare domeincontrollers. Er is daarom niet nodig voor het installeren van de DC-agentsoftware op de RODC's.

## <a name="high-availability"></a>Hoge beschikbaarheid

Het belangrijkste aandachtspunt met ervoor te zorgen dat de hoge beschikbaarheid van Azure AD-wachtwoord beveiliging is de beschikbaarheid van de proxy-servers als domeincontrollers in een forest zijn probeert te downloaden van nieuwe beleidsregels of andere gegevens van Azure. Elke DC-agent maakt gebruik van een eenvoudige round robin-stijl-algoritme bij het bepalen van welke proxyserver voor het aanroepen en slaat het over een proxyservers die niet reageren. Voor het merendeel van volledig verbonden Active Directory-implementaties met in orde replicatie (van zowel directory en sysvol-status) moet twee (2) proxy-servers voldoende zijn om ervoor te zorgen voor beschikbaarheid en daarom tijdige downloads van nieuw beleid en andere gegevens. Extra proxy-servers kunnen worden geïmplementeerd als gewenst.

De gebruikelijke problemen in verband met hoge beschikbaarheid worden verholpen door het ontwerp van de DC-agentsoftware. De DC-agent onderhoudt een lokale cache van het meest recent gedownloade wachtwoordbeleid. Zelfs als alle proxy-servers voor een bepaalde reden niet meer beschikbaar zijn geregistreerd, blijven de DC-agent (s) om af te dwingen van hun in de cache wachtwoordbeleid. Een redelijke updatefrequentie voor wachtwoordbeleid in een grote implementatie is meestal op de volgorde van dagen, niet uur of minder. Korte uitval van de proxy-servers veroorzaken daarom geen aanzienlijke gevolgen voor de werking van de beveiligingsfunctie van de Azure AD-wachtwoord of de voordelen van de beveiliging.

## <a name="next-steps"></a>Volgende stappen

Nu dat u de services die zijn vereist voor de beveiliging van Azure AD-wachtwoord op uw on-premises servers hebt geïnstalleerd voltooit de [installeren na configuratie en verzamelen gegevens](howto-password-ban-bad-on-premises-operations.md) om uit te voeren van uw implementatie.

[Conceptueel overzicht van Azure AD-wachtwoord beveiliging](concept-password-ban-bad-on-premises.md)

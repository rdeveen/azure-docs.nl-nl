---
title: 'Zelfstudie:  Een enkele AD-forest integreren met synchronisatie van wachtwoord-hashes (PHS) | Microsoft Docs'
description: In deze zelfstudie ziet u hoe u een omgeving met een hybride identiteit instelt met behulp van synchronisatie van wachtwoord-hashes.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/17/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 86f9d0ab9c8d8e7fde8afeb479546857c732bd40
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56210642"
---
# <a name="tutorial--integrate-a-single-ad-forest-using-password-hash-sync-phs"></a>Zelfstudie:  Een enkele AD-forest integreren met synchronisatie van wachtwoord-hashes (PHS)

![Maken](media/tutorial-password-hash-sync/diagram.png)

De volgende zelfstudie begeleidt u bij het maken van een omgeving met een hybride identiteit met behulp van synchronisatie van wachtwoord-hashes.  Deze omgeving kan vervolgens worden gebruikt voor testdoeleinden of om meer vertrouwd te raken met de werking van een hybride identiteit.

## <a name="prerequisites"></a>Vereisten
Dit zijn de vereisten voor het voltooien van deze zelfstudie
- Een computer waarop [Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-technology-overview) is geïnstalleerd.  Het wordt aangeraden dit te doen op een computer met [Windows 10](https://docs.microsoft.com/virtualization/hyper-v-on-windows/about/supported-guest-os) of [Windows Server 2016](https://docs.microsoft.com/windows-server/virtualization/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows).
- Een [externe netwerkadapter](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/connect-to-network) waarmee de virtuele machine kan communiceren met internet.
- Een [Azure-abonnement](https://azure.microsoft.com/free)
- Een exemplaar van Windows Server 2016

> [!NOTE]
> In deze zelfstudie worden PowerShell-scripts gebruikt, zodat u de omgeving voor de zelfstudie zo snel mogelijk kunt inrichten.  Elk van de scripts maakt gebruik van variabelen die worden gedeclareerd aan het begin van de scripts.  U kunt en moet de variabelen aanpassen aan uw omgeving.
>
>Met de scripts wordt een algemene Active Directory Domain Services-omgeving ingericht voorafgaand aan de installatie van Azure AD Connect.  De scripts zijn relevant voor alle zelfstudies.
>
> Kopieën van de PowerShell-scripts die worden gebruikt in deze zelfstudie zijn [hier](https://github.com/billmath/tutorial-phs) beschikbaar op GitHub.

## <a name="create-a-virtual-machine"></a>Een virtuele machine maken
Het eerste wat we moeten doen om onze omgeving met een hybride identiteit in te richten, is het maken van een virtuele machine die we gaan gebruiken als onze on-premises Active Directory-server.  Ga als volgt te werk:

1. Open de PowerShell ISE als Administrator.
2. Voer het volgende script uit.

```powershell
#Declare variables
$VMName = 'DC1'
$Switch = 'External'
$InstallMedia = 'D:\ISO\en_windows_server_2016_updated_feb_2018_x64_dvd_11636692.iso'
$Path = 'D:\VM'
$VHDPath = 'D:\VM\DC1\DC1.vhdx'
$VHDSize = '64424509440'

#Create New Virtual Machine
New-VM -Name $VMName -MemoryStartupBytes 16GB -BootDevice VHD -Path $Path -NewVHDPath $VHDPath -NewVHDSizeBytes $VHDSize  -Generation 2 -Switch $Switch  

#Set the memory to be non-dynamic
Set-VMMemory $VMName -DynamicMemoryEnabled $false

#Add DVD Drive to Virtual Machine
Add-VMDvdDrive -VMName $VMName -ControllerNumber 0 -ControllerLocation 1 -Path $InstallMedia

#Mount Installation Media
$DVDDrive = Get-VMDvdDrive -VMName $VMName

#Configure Virtual Machine to Boot from DVD
Set-VMFirmware -VMName $VMName -FirstBootDevice $DVDDrive 
```

## <a name="complete-the-operating-system-deployment"></a>Implementatie van het besturingssysteem voltooien
Om het bouwen van de virtuele machine te voltooien, moet u de installatie van het besturingssysteem afronden.

1. Ga naar Hyper-V Manager en dubbelklik op de virtuele machine
2. Klik op de knop Start.
3.  U wordt gevraagd om op een willekeurige toets te drukken om op te starten vanaf cd of dvd. Druk op een willekeurige toets.
4. Selecteer uw taal in het opstartscherm van Windows Server en klik op **Volgende**.
5. Klik op **Nu installeren**.
6. Voer uw licentiecode in en klik op **Volgende**.
7. Geef aan dat u akkoord gaat met de licentievoorwaarden en klik op **Volgende**.
8. Selecteer **Aangepast:  Alleen Windows installeren (geavanceerd)**
9. Klik op **Volgende**
10. Als de installatie is voltooid, start u de virtuele machine opnieuw op, meldt u zich aan en installeert u de beschikbare Windows-updates om er zeker van te zijn dat de VM up-to-date is.  Installeer de laatste updates.

## <a name="install-active-directory-prerequisites"></a>Vereisten voor de installatie van Active Directory Domain Services
De virtuele machine is nu klaar en we gaan verder met enkele voorbereidende handelingen voorafgaand aan de installatie van Active Directory Domain Services.  We moeten de naam van de virtuele machine wijzigen, een statisch IP-adres en DNS-gegevens instellen, en Remote Server Administration Tools installeren.   Ga als volgt te werk:

1. Open de PowerShell ISE als Administrator.
2. Voer het volgende script uit:

```powershell
#Declare variables
$ipaddress = "10.0.1.117" 
$ipprefix = "24" 
$ipgw = "10.0.1.1" 
$ipdns = "10.0.1.117"
$ipdns2 = "8.8.8.8" 
$ipif = (Get-NetAdapter).ifIndex 
$featureLogPath = "c:\poshlog\featurelog.txt" 
$newname = "DC1"
$addsTools = "RSAT-AD-Tools" 

#Set static IP address
New-NetIPAddress -IPAddress $ipaddress -PrefixLength $ipprefix -InterfaceIndex $ipif -DefaultGateway $ipgw 

# Set the DNS servers
Set-DnsClientServerAddress -InterfaceIndex $ipif -ServerAddresses ($ipdns, $ipdns2)

#Rename the computer 
Rename-Computer -NewName $newname -force 

#Install features 
New-Item $featureLogPath -ItemType file -Force 
Add-WindowsFeature $addsTools 
Get-WindowsFeature | Where installed >>$featureLogPath 

#Restart the computer 
Restart-Computer
```

## <a name="create-a-windows-server-ad-environment"></a>Een Windows Server AD-omgeving inrichten
We hebben nu een VM gemaakt, de naam ervan gewijzigd en een statisch IP-adres toegewezen. De volgende stap is het installeren en configureren van Active Directory Domain Services.  Ga als volgt te werk:

1. Open de PowerShell ISE als Administrator.
2. Voer het volgende script uit.

```powershell 
#Declare variables
$DatabasePath = "c:\windows\NTDS"
$DomainMode = "WinThreshold"
$DomainName = "contoso.com"
$DomaninNetBIOSName = "CONTOSO"
$ForestMode = "WinThreshold"
$LogPath = "c:\windows\NTDS"
$SysVolPath = "c:\windows\SYSVOL"
$featureLogPath = "c:\poshlog\featurelog.txt" 
$Password = "Pass1w0rd"

#Install AD DS, DNS and GPMC 
start-job -Name addFeature -ScriptBlock { 
Add-WindowsFeature -Name "ad-domain-services" -IncludeAllSubFeature -IncludeManagementTools 
Add-WindowsFeature -Name "dns" -IncludeAllSubFeature -IncludeManagementTools 
Add-WindowsFeature -Name "gpmc" -IncludeAllSubFeature -IncludeManagementTools } 
Wait-Job -Name addFeature 
Get-WindowsFeature | Where installed >>$featureLogPath

#Create New AD Forest
Install-ADDSForest -CreateDnsDelegation:$false -DatabasePath $DatabasePath -DomainMode $DomainMode -DomainName $DomainName -SafeModeAdministratorPassword $Password -DomainNetbiosName $DomainNetBIOSName -ForestMode $ForestMode -InstallDns:$true -LogPath $LogPath -NoRebootOnCompletion:$false -SysvolPath $SysVolPath -Force:$true
```

## <a name="create-a-windows-server-ad-user"></a>Een Windows Server AD-gebruiker maken
De Active Directory Domain Services-omgeving is klaar. We gaan nu een testaccount maken.  Dit account wordt gemaakt in onze on-premises AD-omgeving en vervolgens gesynchroniseerd met Azure Active Directory.  Ga als volgt te werk:

1. Open de PowerShell ISE als Administrator.
2. Voer het volgende script uit.

```powershell 
#Declare variables
$Givenname = "Allie"
$Surname = "McCray"
$Displayname = "Allie McCray"
$Name = "amccray"
$Password = "Pass1w0rd"
$Identity = "CN=ammccray,CN=Users,DC=contoso,DC=com"
$SecureString = ConvertTo-SecureString $Password -AsPlainText -Force


#Create the user
New-ADUser -Name $Name -GivenName $Givenname -Surname $Surname -DisplayName $Displayname -AccountPassword $SecureString

#Set the password to never expire
Set-ADUser -Identity $Identity -PasswordNeverExpires $true -ChangePasswordAtLogon $false -Enabled $true
```

## <a name="create-an-azure-ad-tenant"></a>Een Azure Active Directory-tenant maken
Nu moeten we een Azure Active Directory-tenant maken, zodat we onze gebruikers kunnen synchroniseren met de cloud.  Ga als volgt te werk om een nieuwe Azure Active Directory-tenant te maken.

1. Meld u bij de [Azure Portal](https://portal.azure.com) aan met een account waaraan een Azure-abonnement is gekoppeld.
2. Selecteer het **plusteken (+)** en zoek naar **Azure Active Directory**.
3. Selecteer **Azure Active Directory** in de zoekresultaten.
4. Selecteer **Maken**.</br>
![Maken](media/tutorial-password-hash-sync/create1.png)</br>
5. Geef een **naam op voor de organisatie**, evenals een **oorspronkelijke domeinnaam**. Selecteer vervolgens **Maken**. Er wordt nu een map aangemaakt.
6. Als die klaar is, klikt u op de koppeling **hier** om de adreslijst te beheren.

## <a name="create-a-global-administrator-in-azure-ad"></a>Een globale beheerder maken in Azure Active Directory
De Microsoft Azure Active Directory-tenant is klaar en we gaan nu een globale beheerdersaccount maken.  Dit account wordt gebruikt voor het maken van het Azure AD-Connector-account tijdens de installatie van Azure Active Directory Connect.  Het Azure AD-Connector-account wordt gebruikt voor het wegschrijven van gegevens naar Microsoft Azure Active Directory.   Ga als volgt te werk om het globale beheerdersaccount te maken.

1.  Onder **Beheren**, selecteer **Gebruikers**.</br>
![Maken](media/tutorial-password-hash-sync/gadmin1.png)</br>
2.  Selecteer **Alle gebruikers** en selecteer vervolgens **+ Nieuwe gebruiker**.
3.  Geef een naam en gebruikersnaam op voor deze gebruiker. Dit is de globale beheerder voor de tenant. U moet ook de **Maprol** wijzigen naar **Globale beheerder.** U kunt ook het tijdelijke wachtwoord weergeven. Als u klaar bent, selecteert u **Maken**.</br>
![Maken](media/tutorial-password-hash-sync/gadmin2.png)</br>
4. Als de bewerkingen zijn voltooid, opent u een nieuwe webbrowser en meldt u zich met het nieuwe globale beheerdersaccount en het tijdelijke wachtwoord aan bij myapps.microsoft.com.
5. Wijzig het wachtwoord voor de globale beheerder in iets dat u makkelijk kunt onthouden.

## <a name="download-and-install-azure-ad-connect"></a>Azure AD Connect downloaden en installeren
We gaan nu Azure AD Connect downloaden en installeren.  Zodra dat is gebeurd, nemen we de snelle installatie even door.  Ga als volgt te werk:

1. Download [Azure AD Connect](https://www.microsoft.com/download/details.aspx?id=47594)
2. Ga naar **AzureADConnect.msi** en dubbelklik erop.
3. Selecteer in het welkomstscherm het vakje waarmee u aangeeft akkoord te gaan met de licentievoorwaarden en klik op **Doorgaan**.  
4. Klik in het scherm Expresinstellingen op **Expresinstellingen gebruiken**.</br>  
![Maken](media/tutorial-password-hash-sync/express1.png)</br>
5. Voer in het scherm Verbinding maken met Azure AD de gebruikersnaam en het wachtwoord in van de globale beheerder voor Azure AD. Klik op **Volgende**.  
6. Voer in het scherm Verbinding maken met Azure AD de gebruikersnaam en het wachtwoord in voor een enterprisebeheerdersaccount. Klik op **Volgende**.  
7. Klik in het venster Gereed om te configureren op **Installeren**.
8. Wanneer de installatie is voltooid, klikt u op **Afsluiten**.
9. Als de installatie is voltooid, meldt u zich af en weer aan voordat u Synchronization Service Manager of Synchronization Rule Editor gaat gebruiken.


## <a name="verify-users-are-created-and-synchronization-is-occurring"></a>Controleer of gebruikers zijn gemaakt en synchronisatie plaatsvindt
We gaan nu controleren of de gebruikers die aanwezig waren in onze on-premises adreslijst, zijn gesynchroniseerd en nu aanwezig zijn in onze Azure Active Directory-tenant.  Dit synchronisatieproces kan enkele uren duren.  Ga als volgt te werk om te controleren of gebruikers zijn gesynchroniseerd.


1. Meld u bij de [Azure Portal](https://portal.azure.com) aan met een account waaraan een Azure-abonnement is gekoppeld.
2. Selecteer links **Azure Active Directory**
3. Onder **Beheren**, selecteer **Gebruikers**.
4. Kijk of u de nieuwe gebruikers ziet in onze tenant.</br>
![Synchroniseren](media/tutorial-password-hash-sync/synch1.png)</br>

## <a name="test-signing-in-with-one-of-our-users"></a>Aanmelden testen met een van onze gebruikers

1.  Ga naar [https://myapps.microsoft.com](httpss://myapps.microsoft.com)
2. Meld u aan met een gebruikersaccount dat is gemaakt in onze nieuwe tenant.  U moet zich aanmelden met de volgende indeling: (user@domain.onmicrosoft.com). Gebruik het wachtwoord waarmee de gebruiker zich on-premises aanmeldt.</br>
![Verifiëren](media/tutorial-password-hash-sync/verify1.png)</br>

U hebt nu een omgeving met een hybride identiteit ingesteld die u kunt gebruiken voor testdoeleinden en om bekend te raken met wat Azure te bieden heeft.

## <a name="next-steps"></a>Volgende stappen


- [Hardware en vereisten](how-to-connect-install-prerequisites.md) 
- [Snelle instellingen](how-to-connect-install-express.md)
- [Synchronisatie van wachtwoord-hashes](how-to-connect-password-hash-synchronization.md)|

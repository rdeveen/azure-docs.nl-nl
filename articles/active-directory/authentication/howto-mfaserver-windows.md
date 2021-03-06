---
title: Windows-verificatie en Azure MFA-server | Microsoft Docs
description: Leer hoe u Windows-verificatie en Azure Multi-Factor Authentication-server implementeert.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 857e256b0fb2cd726e38232c96f7ce0750681245
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56197195"
---
# <a name="windows-authentication-and-azure-multi-factor-authentication-server"></a>Windows-verificatie en Azure Multi-Factor Authentication-server

Gebruik de Windows-verificatiesectie van de Azure Multi-Factor Authentication-server om Windows-verificatie in te schakelen en te configureren voor toepassingen. Raadpleeg de volgende lijst voordat u Windows-verificatie instelt:

* Start na de installatie Azure Multi-Factor Authentication opnieuw op om Terminal Services te activeren.
* Als 'Overeenkomende Azure Multi-Factor Authentication-gebruiker vereisen' is ingeschakeld en u niet voorkomt in de lijst met gebruikers, kunt u zich niet aanmelden bij de computer wanneer deze opnieuw is opgestart.
* Goedgekeurde IP-adressen is afhankelijk van de vraag of de toepassing de client-IP kan aanbieden bij de verificatie. Momenteel wordt alleen Terminal Services ondersteund.  

> [!NOTE]
> Deze functie wordt niet ondersteund om Terminal Services op Windows Server 2012 R2 te beveiligen.

## <a name="to-secure-an-application-with-windows-authentication-use-the-following-procedure"></a>Gebruik de volgende procedure als u een toepassing met Windows-verificatie wilt beveiligen.
1. Klik in de Azure Multi-Factor Authentication-server op het pictogram Windows-authenticatie.
   ![Windows-verificatie](./media/howto-mfaserver-windows/windowsauth.png)
2. Schakel het selectievakje **Windows-verificatie** in. Dit selectievakje is standaard uitgeschakeld.
3. Op het tabblad Toepassingen kan de beheerder een of meer toepassingen configureren voor Windows-verificatie.
4. Selecteer een server of toepassing. Geef op of de server/toepassing is ingeschakeld. Klik op **OK**.
5. Klik op **Toevoegen...**
6. Op het tabblad Goedgekeurde IP-adressen kunt u Azure Multi-Factor Authentication voor Windows-sessies die afkomstig zijn van specifieke IP-adressen overslaan. Als werknemers bijvoorbeeld de toepassing zowel op kantoor als thuis gebruiken, kunt u instellen dat hun telefoons voor Azure Multi-Factor Authentication niet overgaan wanneer ze op kantoor zijn. Hiervoor geeft u het subnet van de werkplek op als een van de goedgekeurde IP-adressen.
7. Klik op **Toevoegen...**
8. Selecteer **Eén IP-adres** als u één IP-adres wilt overslaan.
9. Selecteer **IP-bereik** als u een heel IP-adresbereik wilt overslaan. Voorbeeld 10.63.193.1-10.63.193.100.
10. Selecteer **Subnet** als u een bereik van IP-adressen wilt opgeven met subnetnotatie. Voer het IP-adres in waarop het subnet begint en kies het juiste IP-masker uit de vervolgkeuzelijst.
11. Klik op **OK**.

## <a name="next-steps"></a>Volgende stappen

- [VPN-apparaten van derden configureren voor Azure MFA Server](howto-mfaserver-nps-vpn.md)

- [De bestaande infrastructuur voor verificatie uitbreiden met de NPS-extensie voor Azure MFA](howto-mfa-nps-extension.md)

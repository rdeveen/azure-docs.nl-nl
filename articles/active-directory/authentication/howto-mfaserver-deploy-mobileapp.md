---
title: Webservice voor mobiele apps voor de Azure MFA-server | Microsoft Docs
description: Configureer de MFA-server om pushmeldingen te verzenden aan gebruikers met de Microsoft Authenticator App.
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
ms.openlocfilehash: 9e0b6a6c2789bfcd5fa462c9ea2cda80f9b5144d
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56162897"
---
# <a name="enable-mobile-app-authentication-with-azure-multi-factor-authentication-server"></a>Authenticatie met de mobiele app integreren met Azure Multi-Factor Authentication-server

De Microsoft verificator-app biedt een extra optie voor verificatie buiten de band. In plaats van tijdens het aanmelden een automatisch telefoongesprek met de gebruiker tot stand te brengen of hem een sms-bericht te sturen, verzendt Azure Multi-Factor Authentication een melding naar de Microsoft verificator-app op de smartphone of tablet van de gebruiker. De gebruiker hoeft in de app enkel op **Verifiëren** te tikken (of een pincode in te voeren en op Verifiëren te tikken) om de aanmelding te voltooien.

Het gebruik van een mobiele app voor verificatie in twee stappen wordt aanbevolen wanneer het telefonische bereik onbetrouwbaar is. Als u de app als een OATH-tokengenerator gebruikt, hebt u geen netwerk- of internetverbinding nodig.

> [!IMPORTANT]
> Als u Microsoft Azure Multi-Factor Authentication-serverv8.x of hoger hebt geïnstalleerd, zijn de meeste van de onderstaande stappen niet vereist. Verificatie voor mobiele apps kan worden ingesteld door de stappen onder [De mobiele app configureren](#configure-the-mobile-app-settings-in-the-azure-multi-factor-authentication-server) te volgen.

## <a name="requirements"></a>Vereisten

Voor het gebruik van de Microsoft Authenticator-app moet u Microsoft Azure Multi-Factor Authentication-server 8.x of hoger gebruiken

## <a name="configure-the-mobile-app-settings-in-the-azure-multi-factor-authentication-server"></a>De instellingen voor de webservice voor mobiele apps op de Azure Multi-Factor Authentication-server configureren

1. Klik in de console van de Multi-Factor Authentication-server op het pictogram Gebruikersportal. Als gebruikers zelf een verificatiemethode mogen kiezen, schakelt u op het tabblad Instellingen onder **Toestaan dat gebruikers de methode selecteren** de optie **Mobiele app** in. Als deze functie niet is ingeschakeld, moeten eindgebruikers contact opnemen met uw helpdesk om de activering van de mobiele app te voltooien.
2. Schakel het selectievakje **Toestaan dat gebruikers de mobiele app activeren** in.
3. Schakel het selectievakje **Registreren van gebruikers toestaan** in.
4. Klik op het pictogram **Mobiele app**.
5. Voer in het veld **Accountnaam** de naam van het bedrijf of de organisatie in die moet worden weergegeven in de mobiele toepassing voor dit account.
   ![Configuratie van MFA-server - instellingen voor mobiele app](./media/howto-mfaserver-deploy-mobileapp/mobile.png)

## <a name="next-steps"></a>Volgende stappen

- [Geavanceerde scenario's met Azure Multi-Factor Authentication en VPN's van derden](howto-mfaserver-nps-vpn.md).

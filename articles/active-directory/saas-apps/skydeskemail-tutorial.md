---
title: 'Zelfstudie: Azure Active Directory-integratie met SkyDesk e-mailadres | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en SkyDesk e-mailadres.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: a9d0bbcb-ddb5-473f-a4aa-028ae88ced1a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8caad0a3c75ab269aa2be93e3b338695dd29caa6
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56209367"
---
# <a name="tutorial-azure-active-directory-integration-with-skydesk-email"></a>Zelfstudie: Azure Active Directory-integratie met SkyDesk e-mailadres

In deze zelfstudie leert u hoe u e-mailadres SkyDesk integreren met Azure Active Directory (Azure AD).

E-mailadres SkyDesk integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot SkyDesk e-mailadres heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij SkyDesk e-mailadres (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met SkyDesk e-mailadres, moet u de volgende items:

- Een Azure AD-abonnement
- Een e-mailadres SkyDesk eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, krijgt u een proefversie van één maand hier [proefversie aanbieding](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. SkyDesk e-mailadres uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-skydesk-email-from-the-gallery"></a>SkyDesk e-mailadres uit de galerie toe te voegen
Voor het configureren van de integratie van SkyDesk e-mailbericht in Azure AD, moet u SkyDesk e-mailadres van de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt SkyDesk e-mailadres van de galerie toevoegen, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **SkyDesk e**.

    ![Het maken van een Azure AD-testgebruiker](./media/skydeskemail-tutorial/tutorial_skydeskemail_search.png)

1. Selecteer in het deelvenster resultaten **SkyDesk e**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/skydeskemail-tutorial/tutorial_skydeskemail_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie configureert u en test Azure AD eenmalige aanmelding met SkyDesk e-mailadres op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in SkyDesk e-mailbericht is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in SkyDesk e-mailbericht tot stand worden gebracht.

In SkyDesk e-mailbericht, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met SkyDesk e-mailadres, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker SkyDesk e](#creating-a-skydesk-email-test-user)**  : als u wilt een equivalent van Britta Simon in SkyDesk e-mailadres dat is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing SkyDesk e-mailadres.

**Voor het configureren van Azure AD eenmalige aanmelding met SkyDesk e-mailadres, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **SkyDesk e** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/skydeskemail-tutorial/tutorial_skydeskemail_samlbase.png)

1. Op de **SkyDesk e-maildomein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/skydeskemail-tutorial/tutorial_skydeskemail_url.png)

    Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://mail.skydesk.jp/portal/<companyname>`

    > [!NOTE] 
    > De waarde is niet echt. Werk de waarde bij met de werkelijke aanmeldings-URL. Neem contact op met [SkyDesk e-mailclient ondersteuningsteam](https://www.skydesk.jp/apps/support/) om de waarde. 
 
1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Base64)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/skydeskemail-tutorial/tutorial_skydeskemail_certificate.png) 

1. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/skydeskemail-tutorial/tutorial_general_400.png)

1. Op de **SkyDesk e-mailconfiguratie** sectie, klikt u op **configureren SkyDesk e** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/skydeskemail-tutorial/tutorial_skydeskemail_configure.png) 

1. Inschakelen van eenmalige aanmelding in **SkyDesk e**, voer de volgende stappen uit:

    a. Aanmelding bij uw SkyDesk e-mailaccount als administrator.

    b. Klik in het menu aan de bovenkant op **Setup**, en selecteer **Org**. 
    
      ![Eenmalige aanmelding configureren](./media/skydeskemail-tutorial/tutorial_skydeskemail_51.png)
  
    c. Klik op **domeinen** in het linkerpaneel.
    
      ![Eenmalige aanmelding configureren](./media/skydeskemail-tutorial/tutorial_skydeskemail_53.png)

    d. Klik op **domein toevoegen**.
    
      ![Eenmalige aanmelding configureren](./media/skydeskemail-tutorial/tutorial_skydeskemail_54.png)

    e. Voer uw domeinnaam en controleer vervolgens of het domein.
    
      ![Eenmalige aanmelding configureren](./media/skydeskemail-tutorial/tutorial_skydeskemail_55.png)

    f. Klik op **SAML-verificatie** in het linkerpaneel.
    
      ![Eenmalige aanmelding configureren](./media/skydeskemail-tutorial/tutorial_skydeskemail_52.png)

1. Op de **SAML-verificatie** dialoogvenster pagina, voert u de volgende stappen uit:
   
      ![Eenmalige aanmelding configureren](./media/skydeskemail-tutorial/tutorial_skydeskemail_56.png)
   
    >[!NOTE]
    >Als u wilt gebruiken op basis van SAML-verificatie, moet u ofwel hebben **geverifieerd domein** of **portal URL** setup. U kunt instellen dat de portal-URL met de unieke naam.
    > 
    > 
   
    ![Eenmalige aanmelding configureren](./media/skydeskemail-tutorial/tutorial_skydeskemail_57.png)

    a. In de **aanmeldings-URL** tekstvak, plak de waarde van **Single Sign-On Service URL voor SAML**, die u hebt gekopieerd vanuit Azure portal.
   
    b. In de **Afmelden** URL-tekstvak, plak de waarde van **afmelding URL**, die u hebt gekopieerd vanuit Azure portal.

    c. **De URL van het wachtwoord wijzigen** is optioneel dus laat dit veld leeg.

    d. Klik op **ophalen uit het bestand** uw gedownloade certificaat selecteert in Azure portal en klik vervolgens op **Open** om het certificaat te uploaden.

    e. Selecteer **RSA** als **algoritme**.

    f. Klik op **Ok** de wijzigingen op te slaan.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/skydeskemail-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/skydeskemail-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/skydeskemail-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/skydeskemail-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-skydesk-email-test-user"></a>Het maken van een testgebruiker SkyDesk e-mailadres

In deze sectie maakt u een gebruiker met de naam van Britta Simon in SkyDesk e-mailbericht.

1. Klik op **gebruikerstoegang** vanaf de linkerkant van het deelvenster in SkyDesk e-mailbericht en voer uw gebruikersnaam. 

    ![Eenmalige aanmelding configureren](./media/skydeskemail-tutorial/tutorial_skydeskemail_58.png)

>[!NOTE] 
>Als u nodig hebt om bulksgewijs gebruikers te maken, moet u contact opnemen met de [SkyDesk e-mailclient ondersteuningsteam](https://www.skydesk.jp/apps/support/).


### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen tot SkyDesk E-mail.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon SkyDesk e-mailberichten, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **SkyDesk e**.

    ![Eenmalige aanmelding configureren](./media/skydeskemail-tutorial/tutorial_skydeskemail_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

Het doel van deze sectie is het testen van de configuratie van uw Azure AD-eenmalige aanmelding via het toegangsvenster.

Wanneer u op de tegel SkyDesk e-mailadres in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing SkyDesk e-mailadres.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/skydeskemail-tutorial/tutorial_general_01.png
[2]: ./media/skydeskemail-tutorial/tutorial_general_02.png
[3]: ./media/skydeskemail-tutorial/tutorial_general_03.png
[4]: ./media/skydeskemail-tutorial/tutorial_general_04.png

[100]: ./media/skydeskemail-tutorial/tutorial_general_100.png

[200]: ./media/skydeskemail-tutorial/tutorial_general_200.png
[201]: ./media/skydeskemail-tutorial/tutorial_general_201.png
[202]: ./media/skydeskemail-tutorial/tutorial_general_202.png
[203]: ./media/skydeskemail-tutorial/tutorial_general_203.png


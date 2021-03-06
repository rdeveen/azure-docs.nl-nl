---
title: 'Zelfstudie: Azure Active Directory-integratie met Kantega SSO voor samenloop | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Kantega SSO voor samenloop.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: d0d99c14-a6ca-45f2-bb84-633126095e7a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 210d69256f2e7f4727ee866af71dd72e765fb0b6
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56205763"
---
# <a name="tutorial-azure-active-directory-integration-with-kantega-sso-for-confluence"></a>Zelfstudie: Azure Active Directory-integratie met Kantega SSO voor samenloop

In deze zelfstudie leert u hoe u Kantega SSO voor samenloop integreren met Azure Active Directory (Azure AD).

Kantega SSO voor samenloop integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Kantega SSO voor samenloop heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Kantega SSO voor samenloop (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Kantega SSO voor samenloop, moet u de volgende items:

- Een Azure AD-abonnement
- Een SSO Kantega voor eenmalige aanmelding samenloop ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u nog geen proefversie van Azure AD hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand aanvragen.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Kantega SSO voor samenloop uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-kantega-sso-for-confluence-from-the-gallery"></a>Kantega SSO voor samenloop uit de galerie toe te voegen
Voor het configureren van de integratie van Kantega SSO voor samenloop in Azure AD, moet u Kantega SSO voor samenloop uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Kantega SSO voor samenloop uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **Kantega SSO voor samenloop**.

    ![Het maken van een Azure AD-testgebruiker](./media/kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_search.png)

1. Selecteer in het deelvenster resultaten **Kantega SSO voor samenloop**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Kantega SSO voor samenloop op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Kantega SSO voor samenloop is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Kantega SSO voor samenloop tot stand worden gebracht.

In Kantega SSO voor samenloop, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met Kantega SSO voor samenloop, u nodig hebt voor de volgende bouwstenen:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een SSO Kantega voor de testgebruiker samenloop](#creating-a-kantega-sso-for-confluence-test-user)**  : als u wilt een equivalent van Britta Simon in Kantega SSO voor samenloop die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en eenmalige aanmelding in uw Kantega SSO voor samenloop toepassing configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Kantega SSO voor samenloop, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Kantega SSO voor samenloop** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_samlbase.png)

1. In **IDP** modus gestart op de **Kantega SSO voor samenloop domein en URL's** sectie de volgende stap uitvoeren:

    ![Eenmalige aanmelding configureren](./media/kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_url1.png)

    a. Typ in het tekstvak **Id** een URL met het volgende patroon: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

    b. In het tekstvak **Antwoord-URL** typt u een URL met behulp van het volgende patroon: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

1. In **SP** gestart modus selectievakje **geavanceerde URL-instellingen weergeven** en voer de volgende stap:

    ![Eenmalige aanmelding configureren](./media/kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_url2.png)

    Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

    > [!NOTE] 
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke id, antwoord-URL en aanmeldings-URL. Deze waarden krijgt u tijdens de configuratie van Confluence, wat later in de zelfstudie wordt uitgelegd.

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![Eenmalige aanmelding configureren](./media/kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_certificate.png) 

1. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/kantegassoforconfluence-tutorial/tutorial_general_400.png)
    
1. In een ander browservenster aanmelden bij uw **samenloop-beheerportal** als beheerder.

1. Wijs het tandwiel aan met de muisaanwijzer en klik op **Add-ons**.
    
    ![Eenmalige aanmelding configureren](./media/kantegassoforconfluence-tutorial/addon1.png)

1. Op het tabblad **ATLASSIAN MARKETPLACE** klikt u op **Nieuwe invoegtoepassingen zoeken**. 

    ![Eenmalige aanmelding configureren](./media/kantegassoforconfluence-tutorial/addon.png)

1. Search **Kantega SSO voor samenloop SAML Kerberos** en klikt u op **installeren** knop voor het installeren van de nieuwe SAML-invoegtoepassing.

    ![Eenmalige aanmelding configureren](./media/kantegassoforconfluence-tutorial/addon2.png)

1. De installatie van de invoegtoepassing wordt gestart.

    ![Eenmalige aanmelding configureren](./media/kantegassoforconfluence-tutorial/addon3.png)

1. Nadat de installatie voltooid is. Klik op **Sluiten**.

    ![Eenmalige aanmelding configureren](./media/kantegassoforconfluence-tutorial/addon33.png)

1.  Klik op **Beheren**.

    ![Eenmalige aanmelding configureren](./media/kantegassoforconfluence-tutorial/addon34.png)
    
1. Klik op **Configure** om de nieuwe invoegtoepassing te configureren.

    ![Eenmalige aanmelding configureren](./media/kantegassoforconfluence-tutorial/addon35.png)

1. Deze nieuwe invoegtoepassing is ook terug te vinden op het tabblad **GEBRUIKERS EN BEVEILIGING**.

    ![Eenmalige aanmelding configureren](./media/kantegassoforconfluence-tutorial/addon36.png)
    
1. In de **SAML** sectie. Selecteer **Azure Active Directory (Azure AD)** uit de **id-provider toevoegen** vervolgkeuzelijst.

    ![Eenmalige aanmelding configureren](./media/kantegassoforconfluence-tutorial/addon4.png)

1. Selecteer het abonnementsniveau van als **Basic**.

    ![Eenmalige aanmelding configureren](./media/kantegassoforconfluence-tutorial/addon5.png)     

1. Op de **eigenschappen van de App** sectie, voert u de volgende stappen uit: 

    ![Eenmalige aanmelding configureren](./media/kantegassoforconfluence-tutorial/addon6.png)

    a. Kopieer de **App ID URI** waarde en deze gebruiken als **-id, de antwoord-URL en de aanmeldings-URL** op de **Kantega SSO voor samenloop domein en URL's** sectie in Azure portal.

    b. Klik op **volgende**.

1. Op de **metagegevens importeren** sectie, voert u de volgende stappen uit: 

    ![Eenmalige aanmelding configureren](./media/kantegassoforconfluence-tutorial/addon7.png)

    a. Selecteer **metagegevensbestand op mijn computer**, en het bestand met metagegevens uploaden, die u hebt gedownload vanuit Azure portal.

    b. Klik op **volgende**.

1. Op de **naam en de SSO-locatie** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/kantegassoforconfluence-tutorial/addon8.png)
    
    a. Naam van de id-Provider in **identiteit providernaam** tekstvak (bijvoorbeeld Azure AD).

    b. Klik op **volgende**.

1. Controleer of het certificaat voor ondertekening en klikt u op **volgende**.

    ![Eenmalige aanmelding configureren](./media/kantegassoforconfluence-tutorial/addon9.png)

1. Op de **samenloop gebruikersaccounts** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/kantegassoforconfluence-tutorial/addon10.png)

    a. Selecteer **gebruikers in samenloop van interne Directory maken, indien nodig** en voer de juiste naam van de groep voor gebruikers (kan meerdere Nee. van groepen met door komma's gescheiden).

    b. Klik op **volgende**.

1. Klik op **Voltooien**.    

    ![Eenmalige aanmelding configureren](./media/kantegassoforconfluence-tutorial/addon11.png)

1. Op de **bekend domeinen voor Azure AD** sectie, voert u de volgende stappen uit: 

    ![Eenmalige aanmelding configureren](./media/kantegassoforconfluence-tutorial/addon12.png)

    a. Selecteer **bekend domeinen** in het linkerdeelvenster van de pagina.

    b. Voer de domeinnaam van het in de **bekend domeinen** tekstvak.

    c. Klik op **Opslaan**. 

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/kantegassoforconfluence-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/kantegassoforconfluence-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/kantegassoforconfluence-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/kantegassoforconfluence-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-kantega-sso-for-confluence-test-user"></a>Het maken van een SSO Kantega voor samenloop testgebruiker

Als u wilt dat Azure AD-gebruikers zich aanmelden bij samenloop, moeten ze worden ingericht voor samenloop. In het geval van Kantega SSO voor samenloop is inrichten een handmatige taak.

**Als u een gebruikersaccount wilt inrichten, voert u de volgende stappen uit:**

1. Meld u aan bij uw Kantega SSO voor samenloop bedrijf site als beheerder.

1. Wijs het tandwiel aan met de muisaanwijzer en klik op **User management**.

    ![Werknemer toevoegen](./media/kantegassoforconfluence-tutorial/user1.png) 

1. Klik onder de sectie gebruikers op **gebruikers toevoegen** tabblad. Voer de volgende stappen uit in het dialoogvenster **Add a User**:

    ![Werknemer toevoegen](./media/kantegassoforconfluence-tutorial/user2.png) 

    a. In het tekstvak **Gebruikersnaam** typt u het e-mailadres van de gebruiker, bijvoorbeeld Brittasimon@contoso.com.

    b. Typ in het tekstvak **Full Name** de volledige naam van de gebruiker, zoals Britta Simon.

    c. Typ in het tekstvak **Email** het e-mailadres van de gebruiker, bijvoorbeeld Brittasimon@contoso.com.

    d. In de **wachtwoord** tekstvak typt u het wachtwoord voor de gebruiker.

    e. Typ het wachtwoord ter bevestiging in het vak **Confirm Password**.
    
    f. Klik op de knop **Add**.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Kantega SSO voor samenloop.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan Kantega SSO voor samenloop toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Kantega SSO voor samenloop**.

    ![Eenmalige aanmelding configureren](./media/kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de Kantega SSO voor samenloop tegel in het toegangsvenster, u moet u automatisch aangemeld bij uw Kantega SSO voor samenloop toepassing.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/kantegassoforconfluence-tutorial/tutorial_general_01.png
[2]: ./media/kantegassoforconfluence-tutorial/tutorial_general_02.png
[3]: ./media/kantegassoforconfluence-tutorial/tutorial_general_03.png
[4]: ./media/kantegassoforconfluence-tutorial/tutorial_general_04.png

[100]: ./media/kantegassoforconfluence-tutorial/tutorial_general_100.png

[200]: ./media/kantegassoforconfluence-tutorial/tutorial_general_200.png
[201]: ./media/kantegassoforconfluence-tutorial/tutorial_general_201.png
[202]: ./media/kantegassoforconfluence-tutorial/tutorial_general_202.png
[203]: ./media/kantegassoforconfluence-tutorial/tutorial_general_203.png


---
title: 'Zelfstudie: Azure Active Directory-integratie met SumoLogic | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en SumoLogic.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: fbb76765-92d7-4801-9833-573b11b4d910
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1fee91b857d9fd127839baaf7a70199c25cfab33
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56191636"
---
# <a name="tutorial-azure-active-directory-integration-with-sumologic"></a>Zelfstudie: Azure Active Directory-integratie met SumoLogic

In deze zelfstudie leert u hoe u SumoLogic integreren met Azure Active Directory (Azure AD).

SumoLogic integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot SumoLogic heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij SumoLogic (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met SumoLogic, moet u de volgende items:

- Een Azure AD-abonnement
- Een SumoLogic eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u nog geen proefversie van Azure AD hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand aanvragen.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. SumoLogic uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-sumologic-from-the-gallery"></a>SumoLogic uit de galerie toe te voegen
Voor het configureren van de integratie van SumoLogic in Azure AD, moet u SumoLogic uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen SumoLogic uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **SumoLogic**.

    ![Het maken van een Azure AD-testgebruiker](./media/sumologic-tutorial/tutorial_sumologic_search.png)

1. Selecteer in het deelvenster resultaten **SumoLogic**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/sumologic-tutorial/tutorial_sumologic_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met SumoLogic op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in SumoLogic is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in SumoLogic tot stand worden gebracht.

In SumoLogic, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met SumoLogic, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker SumoLogic](#creating-a-sumologic-test-user)**  : als u wilt een equivalent van Britta Simon in SumoLogic die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing SumoLogic.

**Voor het configureren van Azure AD eenmalige aanmelding met SumoLogic, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **SumoLogic** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/sumologic-tutorial/tutorial_sumologic_samlbase.png)

1. Op de **SumoLogic domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/sumologic-tutorial/tutorial_sumologic_url.png)

    a. Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://<tenantname>.SumoLogic.com`

    b. In het tekstvak **Id** typt u een URL met het volgende patroon:
    | |
    |--|
    | `https://<tenantname>.us2.sumologic.com` |
    | `https://<tenantname>.sumologic.com` |
    | `https://<tenantname>.us4.sumologic.com` |
    | `https://<tenantname>.eu.sumologic.com` |
    | `https://<tenantname>.au.sumologic.com` |

    > [!NOTE] 
    > Dit zijn geen echte waarden. Werk deze waarden bij met de daadwerkelijke aanmeldings-URL en id. Neem contact op met [SumoLogic Client ondersteuningsteam](https://www.sumologic.com/contact-us/) om deze waarden te verkrijgen. 
 
1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Base64)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/sumologic-tutorial/tutorial_sumologic_certificate.png) 

1. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/sumologic-tutorial/tutorial_general_400.png)

1. Op de **SumoLogic configuratie** sectie, klikt u op **configureren SumoLogic** openen **aanmelding configureren** venster. Kopiëren de **SAML entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/sumologic-tutorial/tutorial_sumologic_configure.png) 

1. In een ander browservenster aanmelden bij uw bedrijf SumoLogic site als beheerder.

1. Ga naar **beheren \> Security**.
   
    ![Manage](./media/sumologic-tutorial/ic778556.png "Manage")

1. Klik op **SAML**.
   
    ![Globale beveiligingsinstellingen](./media/sumologic-tutorial/ic778557.png "globale beveiligingsinstellingen")

1. Uit de **selecteert u een configuratie of maak een nieuwe** Selecteer **Azure AD**, en klik vervolgens op **configureren**.
   
    ![Configureren van SAML 2.0](./media/sumologic-tutorial/ic778558.png "SAML 2.0 configureren")

1. Op de **configureren SAML 2.0** dialoogvenster, voer de volgende stappen uit:
   
    ![Configureren van SAML 2.0](./media/sumologic-tutorial/ic778559.png "SAML 2.0 configureren")
   
    a. In de **configuratienaam** tekstvak, type **Azure AD**. 

    b. Selecteer **foutopsporingsmodus**.

    c. In de **verlener** tekstvak, plak de waarde van **SAML entiteit-ID**, die u hebt gekopieerd vanuit Azure portal. 

    d. In de **Authn aanvraag-URL** tekstvak, plak de waarde van **Single Sign-On Service URL voor SAML**, die u hebt gekopieerd vanuit Azure portal.

    e. Open uw base-64 gecodeerde certificaat in Kladblok, Kopieer de inhoud ervan in het Klembord en plakt u het gehele certificaat in **X.509-certificaat** tekstvak.

    f. Als **e kenmerk**, selecteer **gebruik SAML onderwerp**.  

    g. Selecteer **SP geïnitieerde configuratie van aanmelding**.

    h. In de **aanmelding pad** tekstvak, type **Azure** en klikt u op **opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/sumologic-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/sumologic-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/sumologic-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/sumologic-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-sumologic-test-user"></a>Het maken van een testgebruiker SumoLogic

Als u wilt dat Azure AD-gebruikers zich aanmelden bij SumoLogic, moeten ze worden ingericht op SumoLogic.  

* In het geval van SumoLogic is inrichten een handmatige taak.

**Als u een gebruikersaccount wilt inrichten, voert u de volgende stappen uit:**

1. Meld u aan bij uw **SumoLogic** tenant.

1. Ga naar **beheren \> gebruikers**.
   
    ![Gebruikers](./media/sumologic-tutorial/ic778561.png "Gebruikers")

1. Klik op **Add**.
   
    ![Gebruikers](./media/sumologic-tutorial/ic778562.png "Gebruikers")

1. Op de **nieuwe gebruiker** dialoogvenster, voer de volgende stappen uit:
   
    ![New User](./media/sumologic-tutorial/ic778563.png "New User") 
 
    a. Typ de verwante gegevens van de Azure AD-account dat u wilt inrichten in de **voornaam**, **achternaam**, en **e** tekstvakken.
  
    b. Selecteer een rol.
  
    c. Als **Status**, selecteer **Active**.
  
    d. Klik op **Opslaan**.

>[!NOTE]
>U kunt alle andere SumoLogic gebruiker-account maken van hulpprogramma's of API's geleverd door SumoLogic aan inrichten AAD-gebruikersaccounts. 
> 

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan SumoLogic.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan SumoLogic toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **SumoLogic**.

    ![Eenmalige aanmelding configureren](./media/sumologic-tutorial/tutorial_sumologic_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

Het doel van deze sectie is het testen van uw Azure AD eenmalige aanmelding configuratie via het toegangsvenster.

Wanneer u op de tegel SumoLogic in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing SumoLogic.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/sumologic-tutorial/tutorial_general_01.png
[2]: ./media/sumologic-tutorial/tutorial_general_02.png
[3]: ./media/sumologic-tutorial/tutorial_general_03.png
[4]: ./media/sumologic-tutorial/tutorial_general_04.png

[100]: ./media/sumologic-tutorial/tutorial_general_100.png

[200]: ./media/sumologic-tutorial/tutorial_general_200.png
[201]: ./media/sumologic-tutorial/tutorial_general_201.png
[202]: ./media/sumologic-tutorial/tutorial_general_202.png
[203]: ./media/sumologic-tutorial/tutorial_general_203.png


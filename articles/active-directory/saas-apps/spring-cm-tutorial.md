---
title: 'Zelfstudie: Azure Active Directory-integratie met SpringCM | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en SpringCM.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 4a42f797-ac58-4aca-a8e6-53bfe5529083
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2187d0e986a173e1b8dc37fd0abdfa3bfd9e9505
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56168984"
---
# <a name="tutorial-azure-active-directory-integration-with-springcm"></a>Zelfstudie: Azure Active Directory-integratie met SpringCM

In deze zelfstudie leert u hoe u SpringCM integreren met Azure Active Directory (Azure AD).

SpringCM integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot SpringCM heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij SpringCM (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met SpringCM, moet u de volgende items:

- Een Azure AD-abonnement
- Een SpringCM eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u nog geen proefversie van Azure AD hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand aanvragen.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. SpringCM uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-springcm-from-the-gallery"></a>SpringCM uit de galerie toe te voegen
Voor het configureren van de integratie van SpringCM in Azure AD, moet u SpringCM uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen SpringCM uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **SpringCM**.

    ![Het maken van een Azure AD-testgebruiker](./media/spring-cm-tutorial/tutorial_springcm_search.png)

1. Selecteer in het deelvenster resultaten **SpringCM**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/spring-cm-tutorial/tutorial_springcm_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met SpringCM op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in SpringCM is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in SpringCM tot stand worden gebracht.

In SpringCM, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met SpringCM, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker SpringCM](#creating-a-springcm-test-user)**  : als u wilt een equivalent van Britta Simon in SpringCM die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing SpringCM.

**Voor het configureren van Azure AD eenmalige aanmelding met SpringCM, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **SpringCM** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/spring-cm-tutorial/tutorial_springcm_samlbase.png)

1. Op de **SpringCM domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/spring-cm-tutorial/tutorial_springcm_url.png)

    Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://na11.springcm.com/atlas/SSO/SSOEndpoint.ashx?aid=<identifier>`

    > [!NOTE] 
    > Deze waarde is niet echt. Werk deze waarde bij met de werkelijke aanmeldings-URL. Neem contact op met [SpringCM Client ondersteuningsteam](https://knowledge.springcm.com/support) deze waarde op te halen. 
 
1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Raw)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/spring-cm-tutorial/tutorial_springcm_certificate.png) 

1. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/spring-cm-tutorial/tutorial_general_400.png)

1. Op de **SpringCM configuratie** sectie, klikt u op **configureren SpringCM** openen **aanmelding configureren** venster. Kopiëren de **SAML entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/spring-cm-tutorial/tutorial_springcm_configure.png)     

1. In een ander browservenster aanmelden bij uw **SpringCM** bedrijf site als administrator.

1. Klik in het menu aan de bovenkant op **Ga naar**, klikt u op **voorkeuren**, en klik vervolgens op de **accountvoorkeuren** sectie, klikt u op **SAML SSO**.
   
    ![SAML SSO](./media/spring-cm-tutorial/ic797051.png "SAML SSO")

1. In de sectie configuratie van de id-Provider, moet u de volgende stappen uitvoeren:
   
    ![ID-providerconfiguratie](./media/spring-cm-tutorial/ic797052.png "Identity Provider configureren")
    
    a. Als u wilt uw gedownloade Azure Active Directory-certificaat uploaden, klikt u op **uitgeverscertificaat Selecteer** of **wijzigen-uitgeverscertificaat**.
    
    b. Plakken **SAML entiteit-ID** waarde die u hebt gekopieerd vanuit Azure portal in de **verlener** tekstvak.
    
    c. Plakken **Single Sign-On Service URL voor SAML** waarde die u hebt gekopieerd vanuit de Azure portal in de **Service Provider (SP) geïnitieerd eindpunt** tekstvak.
            
    d. Selecteer **SAML ingeschakeld** als **inschakelen**.

    e. Klik op **Opslaan**.
 
> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/spring-cm-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/spring-cm-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/spring-cm-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/spring-cm-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-springcm-test-user"></a>Het maken van een testgebruiker SpringCM

Als u wilt dat Azure Active Directory-gebruikers zich aanmelden bij SpringCM, moeten ze worden ingericht voor SpringCM. In het geval van SpringCM is inrichten een handmatige taak.

>[!NOTE]
>Zie voor meer informatie, [maken en bewerken van een gebruiker SpringCM](https://knowledge.springcm.com/create-and-edit-a-springcm-user). 

**Voor het inrichten van een gebruikersaccount SpringCM, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw **SpringCM** bedrijf site als administrator.

1. Klik op **GOTO**, en klik vervolgens op **ADRESBOEK**.
   
    ![Create User](./media/spring-cm-tutorial/ic797054.png "Create User")

1. Klik op **gebruiker maken**.

1. Selecteer een **gebruikersrol**.

1. Selecteer **activeringse-mail verzenden**.

1. Typ de voornaam, achternaam en e-mailadres van een geldige Azure Active Directory-gebruikersaccount die u inrichten in de bijbehorende tekstvakken wilt.

1. De gebruiker toevoegen aan een **beveiligingsgroep**.

1. Klik op **Opslaan**.

  >[!NOTE]
  >U kunt alle andere SpringCM gebruiker-account maken van hulpprogramma's of API's geleverd door SpringCM aan inrichten AAD-gebruikersaccounts.  
  > 

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan SpringCM.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan SpringCM toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **SpringCM**.

    ![Eenmalige aanmelding configureren](./media/spring-cm-tutorial/tutorial_springcm_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.
 
Wanneer u op de tegel SpringCM in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing SpringCM.

Zie [Introduction to the Access Panel](../user-help/active-directory-saas-access-panel-introduction.md) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster. 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/spring-cm-tutorial/tutorial_general_01.png
[2]: ./media/spring-cm-tutorial/tutorial_general_02.png
[3]: ./media/spring-cm-tutorial/tutorial_general_03.png
[4]: ./media/spring-cm-tutorial/tutorial_general_04.png

[100]: ./media/spring-cm-tutorial/tutorial_general_100.png

[200]: ./media/spring-cm-tutorial/tutorial_general_200.png
[201]: ./media/spring-cm-tutorial/tutorial_general_201.png
[202]: ./media/spring-cm-tutorial/tutorial_general_202.png
[203]: ./media/spring-cm-tutorial/tutorial_general_203.png


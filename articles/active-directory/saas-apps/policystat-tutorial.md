---
title: 'Zelfstudie: Azure Active Directory-integratie met PolicyStat | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en PolicyStat.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: af5eb0f1-1c8e-4809-b0c4-8ccfb915ca42
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 43ddf56e4c72e7e59778fc43a808b9800bc3b9b3
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56187709"
---
# <a name="tutorial-azure-active-directory-integration-with-policystat"></a>Zelfstudie: Azure Active Directory-integratie met PolicyStat

In deze zelfstudie leert u hoe u PolicyStat integreren met Azure Active Directory (Azure AD).

PolicyStat integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot PolicyStat heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij PolicyStat (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met PolicyStat, moet u de volgende items:

- Een Azure AD-abonnement
- Een PolicyStat eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u nog geen proefversie van Azure AD hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand aanvragen.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. PolicyStat uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-policystat-from-the-gallery"></a>PolicyStat uit de galerie toe te voegen
Voor het configureren van de integratie van PolicyStat in Azure AD, moet u PolicyStat uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen PolicyStat uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **PolicyStat**.

    ![Het maken van een Azure AD-testgebruiker](./media/policystat-tutorial/tutorial_policystat_search.png)

1. Selecteer in het deelvenster resultaten **PolicyStat**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/policystat-tutorial/tutorial_policystat_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met PolicyStat op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in PolicyStat is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in PolicyStat tot stand worden gebracht.

In PolicyStat, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met PolicyStat, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker PolicyStat](#creating-a-policystat-test-user)**  : als u wilt een equivalent van Britta Simon in PolicyStat die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing PolicyStat.

**Voor het configureren van Azure AD eenmalige aanmelding met PolicyStat, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **PolicyStat** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/policystat-tutorial/tutorial_policystat_samlbase.png)

1. Op de **PolicyStat domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/policystat-tutorial/tutorial_policystat_url.png)

    a. Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://<companyname>.policystat.com`

    b. Typ in het tekstvak **Id** een URL met het volgende patroon: `https://<companyname>.policystat.com/saml2/metadata/`

    > [!NOTE] 
    > Dit zijn geen echte waarden. Werk deze waarden bij met de daadwerkelijke aanmeldings-URL en id. Neem contact op met [PolicyStat Client ondersteuningsteam](http://www.policystat.com/support/) om deze waarden te verkrijgen. 
 
1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![Eenmalige aanmelding configureren](./media/policystat-tutorial/tutorial_policystat_certificate.png) 

1. Het doel van deze sectie is om een overzicht van hoe u gebruikers wilt verifiëren naar PolicyStat met hun account in Azure AD gebruikmaakt van Federatie op basis van het SAML-protocol te.

    De toepassing PolicyStat wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling, die vereist dat u om toe te voegen van aangepast kenmerktoewijzingen aan uw **SAML-Token kenmerken** configuratie.  

     De volgende schermafbeelding ziet u een voorbeeld hiervan.

     ![Kenmerken](./media/policystat-tutorial/tutorial_policystat_attribute.png "Kenmerken")

1. Als u wilt toevoegen de vereiste kenmerktoewijzingen, moet u de volgende stappen uitvoeren:

    | Naam kenmerk    |   Waarde kenmerk |
    |------------------- | -------------------- |
    | uid | ExtractMailPrefix([mail]) |
    
    a. Klik op **kenmerk toevoegen** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/policystat-tutorial/tutorial_policystat_04.png)

    ![Eenmalige aanmelding configureren](./media/policystat-tutorial/tutorial_policystat_addatribute.png)
    
    b. In de **kenmerknaam** tekstvak, type **uid**.

    c. In de **kenmerkwaarde** tekstvak, selecteer **ExtractMailPrefix()**.    
   
    d. Uit de **e-Mail** in de lijst met **User.mail**.
    
    e. Klik op **OK**.

1. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/policystat-tutorial/tutorial_general_400.png)

1. Meld u in een ander browservenster in uw bedrijf PolicyStat site als beheerder.

1. Klik op de **Admin** tabblad en klik vervolgens op **configuratie voor eenmalige aanmelding** in het navigatiedeelvenster links.
   
    ![Menu beheerder](./media/policystat-tutorial/ic808633.png "Menu beheerder")

1. In de **Setup** sectie, selecteer **inschakelen Single Sign-on integratie**.
   
    ![Single Sign-On configuratie](./media/policystat-tutorial/ic808634.png "Single Sign-On-configuratie")

1. Klik op **kenmerken configureren**, en klik vervolgens op de **kenmerken configureren** sectie, voert u de volgende stappen uit:
   
    ![Single Sign-On configuratie](./media/policystat-tutorial/ic808635.png "Single Sign-On-configuratie")
   
    a. In de **kenmerk Username** tekstvak, type **uid**.

    b. In de **voornaam kenmerk** tekstvak, type **firstname** van gebruiker **Julia**.

    c. In de **laatste naamkenmerk** tekstvak, type **lastname** van gebruiker **Simon**.

    d. In de **e kenmerk** tekstvak, type **emailaddress** van gebruiker **BrittaSimon@contoso.com**.

    e. Klik op **Wijzigingen opslaan**.

1. Klik op **metagegevens van uw id-provider**, en klik vervolgens op de **uw IDP metagegevens** sectie, voert u de volgende stappen uit:
   
    ![Single Sign-On configuratie](./media/policystat-tutorial/ic808636.png "Single Sign-On-configuratie")
   
    a. Open het metagegevensbestand gedownload, Kopieer de inhoud en plak deze in de **metagegevens van uw id-Provider** tekstvak.

    b. Klik op **Wijzigingen opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/policystat-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/policystat-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/policystat-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/policystat-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-policystat-test-user"></a>Het maken van een testgebruiker PolicyStat

Als u wilt inschakelen in Azure AD-gebruikers zich aanmelden bij PolicyStat, moeten ze worden ingericht voor PolicyStat.  

PolicyStat biedt ondersteuning voor just-in-tijd van gebruikersinrichting. Dit houdt in dat u niet hoeft de gebruikers handmatig toevoegen aan PolicyStat. De gebruikers worden automatisch worden toegevoegd op de eerste keer aanmelden via eenmalige aanmelding.

>[!NOTE]
>U kunt alle andere PolicyStat gebruiker-account maken van hulpprogramma's of API's geleverd door PolicyStat voor het inrichten van gebruikersaccounts van de Azure AD.
> 

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan PolicyStat.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan PolicyStat toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **PolicyStat**.

    ![Eenmalige aanmelding configureren](./media/policystat-tutorial/tutorial_policystat_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel PolicyStat in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing PolicyStat.
Zie [Introduction to the Access Panel](../user-help/active-directory-saas-access-panel-introduction.md) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/policystat-tutorial/tutorial_general_01.png
[2]: ./media/policystat-tutorial/tutorial_general_02.png
[3]: ./media/policystat-tutorial/tutorial_general_03.png
[4]: ./media/policystat-tutorial/tutorial_general_04.png

[100]: ./media/policystat-tutorial/tutorial_general_100.png

[200]: ./media/policystat-tutorial/tutorial_general_200.png
[201]: ./media/policystat-tutorial/tutorial_general_201.png
[202]: ./media/policystat-tutorial/tutorial_general_202.png
[203]: ./media/policystat-tutorial/tutorial_general_203.png


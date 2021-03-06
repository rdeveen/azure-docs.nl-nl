---
title: 'Zelfstudie: Azure Active Directory-integratie met BeeLine | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en BeeLine.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 0726859d-1dac-44a0-810b-da56d89039ee
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2f9e80cd682b87e4503ca61b43208e5e5705f953
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56182031"
---
# <a name="tutorial-azure-active-directory-integration-with-beeline"></a>Zelfstudie: Azure Active Directory-integratie met BeeLine

In deze zelfstudie leert u hoe u BeeLine integreren met Azure Active Directory (Azure AD).

BeeLine integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot BeeLine heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij BeeLine (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met BeeLine, moet u de volgende items:

- Een Azure AD-abonnement
- Een BeeLine eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u nog geen proefversie van Azure AD hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand aanvragen.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. BeeLine uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-beeline-from-the-gallery"></a>BeeLine uit de galerie toe te voegen
Voor het configureren van de integratie van BeeLine in Azure AD, moet u BeeLine uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen BeeLine uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **BeeLine**.

    ![Het maken van een Azure AD-testgebruiker](./media/beeline-tutorial/tutorial_beeline_search.png)

1. Selecteer in het deelvenster resultaten **BeeLine**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/beeline-tutorial/tutorial_beeline_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met BeeLine op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in BeeLine is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in BeeLine tot stand worden gebracht.

In BeeLine, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met BeeLine, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker BeeLine](#creating-a-beeline-test-user)**  : als u wilt een equivalent van Britta Simon in BeeLine die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing BeeLine.

**Voor het configureren van Azure AD eenmalige aanmelding met BeeLine, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **BeeLine** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/beeline-tutorial/tutorial_beeline_samlbase.png)

1. Op de **BeeLine domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/beeline-tutorial/tutorial_beeline_url.png)

    a. Typ in het tekstvak **Id** een URL met het volgende patroon: `https://projects.beeline.net/<instancename>`

    b. In het tekstvak **Antwoord-URL** typt u een URL met de volgende notatie:
    | |
    |--|
    | `https://projects.beeline.net/<instancename>/SSO_External.ashx`|
    | `https://projects.beeline.net/<companyname>/SSO_External.ashx` |

    > [!NOTE] 
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke id en antwoord-URL. Neem contact op met [BeeLine ondersteuningsteam](https://www.beeline.com/contact-us/) om deze waarden te verkrijgen.
 
1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![Eenmalige aanmelding configureren](./media/beeline-tutorial/tutorial_beeline_certificate.png) 

1. Uw toepassing Beeline wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling. Neem contact op met [BeeLine ondersteuningsteam](https://www.beeline.com/contact-us/) eerst om te identificeren van de juiste gebruikers-id die in de toepassing worden toegewezen. Neem even ook de richtlijnen van [BeeLine ondersteuningsteam](https://www.beeline.com/contact-us/) over het kenmerk dat moet worden gebruikt voor deze toewijzing. U kunt de waarde van dit kenmerk van beheren de **gebruikerskenmerken** tabblad van de toepassing. In de volgende schermopname ziet u een voorbeeld hiervan. We hebben hier toegewezen de **gebruikers-id** claim met de **userprincipalname** kenmerk, waarmee u unieke gebruikers-ID, die wordt verzonden naar de toepassing Beeline in het elke geslaagde SAML-antwoord.

    ![Eenmalige aanmelding configureren](./media/beeline-tutorial/tutorial_attribute.png)    

1. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/beeline-tutorial/tutorial_general_400.png)

1. Op de **BeeLine configuratie** sectie, klikt u op **configureren BeeLine** openen **aanmelding configureren** venster. Kopiëren de **afmelding URL** en **SAML entiteit-ID** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/beeline-tutorial/tutorial_beeline_configure.png) 

1. Het configureren van eenmalige aanmelding op **BeeLine** zijde, moet u voor het verzenden van de gedownloade **Metadata XML** en **SAML entiteit-ID**, **afmelding URL** naar [BeeLine ondersteuningsteam](https://www.beeline.com/contact-us/).

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/beeline-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/beeline-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/beeline-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/beeline-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-beeline-test-user"></a>Het maken van een testgebruiker BeeLine

In deze sectie maakt u een gebruiker met de naam van Britta Simon in Beeline. Beeline toepassing moet alle gebruikers in te richten in de toepassing voordat u eenmalige aanmelding. Zo werken met de [BeeLine ondersteuningsteam](https://www.beeline.com/contact-us/) voor het inrichten van al deze gebruikers in de toepassing. 

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan BeeLine.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan BeeLine toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **BeeLine**.

    ![Eenmalige aanmelding configureren](./media/beeline-tutorial/tutorial_beeline_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster. Wanneer u op de tegel Beeline in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Beeline.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/beeline-tutorial/tutorial_general_01.png
[2]: ./media/beeline-tutorial/tutorial_general_02.png
[3]: ./media/beeline-tutorial/tutorial_general_03.png
[4]: ./media/beeline-tutorial/tutorial_general_04.png

[100]: ./media/beeline-tutorial/tutorial_general_100.png

[200]: ./media/beeline-tutorial/tutorial_general_200.png
[201]: ./media/beeline-tutorial/tutorial_general_201.png
[202]: ./media/beeline-tutorial/tutorial_general_202.png
[203]: ./media/beeline-tutorial/tutorial_general_203.png


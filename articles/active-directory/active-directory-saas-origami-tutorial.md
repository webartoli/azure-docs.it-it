---
title: 'Esercitazione: Integrazione di Azure Active Directory con Origami | Microsoft Docs'
description: Informazioni su come configurare l'accesso Single Sign-On tra Azure Active Directory e Origami
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a28bb0ba-b564-46ba-accc-e587699295d4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.translationtype: Human Translation
ms.sourcegitcommit: 6dbb88577733d5ec0dc17acf7243b2ba7b829b38
ms.openlocfilehash: 3420409b72ff032e64ac59365083dd141dfc3c1b
ms.contentlocale: it-it
ms.lasthandoff: 07/04/2017

---
# <a name="tutorial-azure-active-directory-integration-with-origami"></a>Esercitazione: Integrazione di Azure Active Directory con Origami

Questa esercitazione descrive come integrare Origami con Azure Active Directory (Azure AD).

L'integrazione di Origami con Azure AD offre i vantaggi seguenti:

- È possibile controllare in Azure AD chi può accedere a Origami
- È possibile abilitare gli utenti perché possano accedere automaticamente a Origami (Single Sign-On) con i propri account Azure AD
- È possibile gestire gli account in un'unica posizione centrale: il portale di Azure.

Per altre informazioni sull'integrazione di app SaaS con Azure AD, vedere [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Prerequisiti

Per configurare l'integrazione di Azure AD con Origami, sono necessari gli elementi seguenti:

- Sottoscrizione di Azure AD.
- Sottoscrizione di Origami abilitata per l'accesso Single Sign-On

> [!NOTE]
> Non è consigliabile usare un ambiente di produzione per testare i passaggi di questa esercitazione.

A questo scopo, è consigliabile seguire le indicazioni seguenti:

- Non usare l'ambiente di produzione a meno che non sia necessario.
- Se non si dispone di un ambiente di prova di Azure AD, è possibile ottenere una versione di valutazione di un mese [qui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrizione dello scenario
In questa esercitazione viene eseguito il test dell'accesso Single Sign-On di Azure AD in un ambiente di test. Lo scenario descritto in questa esercitazione prevede i due blocchi predefiniti seguenti:

1. Aggiunta di Origami dalla raccolta
2. Configurazione e test dell'accesso Single Sign-On di Azure AD

## <a name="adding-origami-from-the-gallery"></a>Aggiunta di Origami dalla raccolta
Per configurare l'integrazione di Origami in Azure AD, è necessario aggiungere Origami dalla raccolta al proprio elenco di app SaaS gestite.

**Per aggiungere Origami dalla raccolta, seguire questa procedura:**

1. Nel **[portale di Azure](https://portal.azure.com)** fare clic sull'icona di **Azure Active Directory** nel riquadro di spostamento sinistro. 

    ![Active Directory][1]

2. Passare ad **Applicazioni aziendali**. Andare quindi a **Tutte le applicazioni**.

    ![Applicazioni][2]
    
3. Fare clic sul pulsante **Nuova applicazione** nella parte superiore della finestra di dialogo per aggiungere una nuova applicazione.

    ![Applicazioni][3]

4. Nella casella di ricerca digitare **Origami**.

    ![Creazione di un utente test di Azure AD](./media/active-directory-saas-origami-tutorial/tutorial_origami_search.png)

5. Nel pannello dei risultati selezionare **Origami** e quindi fare clic sul pulsante **Aggiungi** per aggiungere l'applicazione.

    ![Creazione di un utente test di Azure AD](./media/active-directory-saas-origami-tutorial/tutorial_origami_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configurazione e test dell'accesso Single Sign-On di Azure AD
In questa sezione viene configurato e testato l'accesso Single Sign-On di Azure AD con Origami con un utente test di nome "Britta Simon".

Per il funzionamento dell'accesso Single Sign-On, Azure AD deve conoscere qual è l'utente controparte di Origami che corrisponde a un utente di Azure AD. In altre parole, deve essere stabilita una relazione di collegamento tra un utente di Azure AD e l'utente correlato in Origami.

Per stabilire la relazione di collegamento, in Origami assegnare il valore di **nome utente** in Azure AD come valore di **Username** (Nome utente).

Per configurare e testare l'accesso Single Sign-On di Azure AD con Origami, è necessario completare i blocchi predefiniti seguenti:

1. **[Configurazione dell'accesso Single Sign-On di Azure AD](#configuring-azure-ad-single-sign-on)** : per abilitare gli utenti all'uso di questa funzionalità.
2. **[Creazione di un utente test di Azure AD](#creating-an-azure-ad-test-user)** : per testare l'accesso Single Sign-On di Azure AD con l'utente Britta Simon.
3. **[Creazione di un utente test di Origami](#creating-an-origami-test-user)**: per avere una controparte di Britta Simon in Origami collegata alla rappresentazione dell'utente in Azure AD.
4. **[Assegnazione dell'utente test di Azure AD](#assigning-the-azure-ad-test-user)** : per abilitare Britta Simon all'uso dell'accesso Single Sign-On di Azure AD.
5. **[Testing Single Sign-On](#testing-single-sign-on)** : per verificare se la configurazione funziona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configurazione dell'accesso Single Sign-On di Azure AD

In questa sezione viene abilitato l'accesso Single Sign-On di Azure AD nel portale di Azure e viene configurato l'accesso Single Sign-On nell'applicazione Origami.

**Per configurare l'accesso Single Sign-On di Azure AD con Origami, seguire questa procedura:**

1. Nella pagina di integrazione dell'applicazione **Origami** del portale di Azure fare clic su **Single Sign-On**.

    ![Configura accesso Single Sign-On][4]

2. Nella finestra di dialogo **Single Sign-On** selezionare **Accesso basato su SAML** per **Modalità** per abilitare l'accesso Single Sign-On.
 
    ![Configura accesso Single Sign-On](./media/active-directory-saas-origami-tutorial/tutorial_origami_samlbase.png)

3. Nella sezione **URL e dominio Origami** seguire questa procedura:

    ![Configura accesso Single Sign-On](./media/active-directory-saas-origami-tutorial/tutorial_origami_url.png)

    Nella casella di testo **URL di accesso** digitare l'URL usando il modello seguente: `https://live.origamirisk.com/origami/account/login?account=<companyname>`

    > [!NOTE] 
    > Poiché non è reale, è necessario aggiornare questo valore con l'URL di accesso effettivo. Per ottenere tale valore, contattare il [team di supporto clienti di Origami](https://wordpress.org/support/theme/origami). 
 
4. Nella sezione **Certificato di firma SAML** fare clic su **Certificato (Base64)** e quindi salvare il file del certificato nel computer.

    ![Configura accesso Single Sign-On](./media/active-directory-saas-origami-tutorial/tutorial_origami_certificate.png) 

5. Fare clic sul pulsante **Salva** .

    ![Configura accesso Single Sign-On](./media/active-directory-saas-origami-tutorial/tutorial_general_400.png)

6. Nella sezione **Configurazione di Origami** fare clic su **Configura Origami** per aprire la finestra **Configura accesso**. Copiare i valori **Sign-Out URL (URL di disconnessione) e SAML Single Sign-On Service URL (URL servizio Single Sign-On SAML)** dalla sezione **Quick Reference** (Riferimento rapido).

    ![Configura accesso Single Sign-On](./media/active-directory-saas-origami-tutorial/tutorial_origami_configure.png) 

7. Accedere all'account Origami con diritti di amministratore.

8. Nel menu in alto fare clic su **Admin**.
   
    ![Configura accesso Single Sign-On](./media/active-directory-saas-origami-tutorial/tutorial_origami_51.png)

9. Nella pagina della finestra di dialogo Single Sign On Setup (Imposta accesso Single Sign-On) eseguire la procedura seguente:
   
    ![Configura accesso Single Sign-On](./media/active-directory-saas-origami-tutorial/tutorial_origami_531.png)

    a. Selezionare **Abilita Single Sign-On**.

    b. Nella casella di testo **URL della pagina di accesso del provider di identità** incollare il valore di **URL servizio Single Sign-On SAML** copiato dal portale di Azure.

    c. Nella casella di testo **URL della pagina di disconnessione del provider di identità** incollare il valore di **URL disconnessione** copiato dal portale di Azure.

    d. Fare clic su **Sfoglia** per caricare il certificato scaricato dal portale di Azure.

    e. Fare clic su **Salva modifiche**.

> [!TIP]
> Un riepilogo delle istruzioni è disponibile all'interno del [portale di Azure](https://portal.azure.com) durante la configurazione dell'app.  Dopo aver aggiunto l'app dalla sezione **Active Directory > Applicazioni aziendali** è sufficiente fare clic sulla scheda **Single Sign-On** e accedere alla documentazione incorporata tramite la sezione **Configurazione** nella parte inferiore. Altre informazioni sulla funzione di documentazione incorporata sono disponibili in [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985) (Documentazione incorporata di Azure AD).
> 

### <a name="creating-an-azure-ad-test-user"></a>Creazione di un utente test di Azure AD
Questa sezione descrive come creare un utente test denominato Britta Simon nel portale di Azure.

![Creare un utente di Azure AD][100]

**Per creare un utente test in Azure AD, eseguire la procedura seguente:**

1. Nel **portale di Azure** fare clic sull'icona di **Azure Active Directory** nel riquadro di spostamento sinistro.

    ![Creazione di un utente test di Azure AD](./media/active-directory-saas-origami-tutorial/create_aaduser_01.png) 

2. Passare a **Utenti e gruppi** e fare clic su **Tutti gli utenti** per visualizzare l'elenco di utenti.
    
    ![Creazione di un utente test di Azure AD](./media/active-directory-saas-origami-tutorial/create_aaduser_02.png) 

3. Nella parte superiore della finestra di dialogo fare clic su **Aggiungi** per aprire la finestra di dialogo **Utente**.
 
    ![Creazione di un utente test di Azure AD](./media/active-directory-saas-origami-tutorial/create_aaduser_03.png) 

4. Nella pagina della finestra di dialogo **Utente** seguire questa procedura:
 
    ![Creazione di un utente test di Azure AD](./media/active-directory-saas-origami-tutorial/create_aaduser_04.png) 

    a. Nella casella di testo **Nome** digitare **BrittaSimon**.

    b. Nella casella di testo **Nome utente** digitare l'**indirizzo di posta elettronica** di BrittaSimon.

    c. Selezionare **Mostra password** e prendere nota del valore della **Password**.

    d. Fare clic su **Crea**.
 
### <a name="creating-an-origami-test-user"></a>Creazione di un utente test in Origami

In questa sezione viene creato un utente di nome Britta Simon in Origami. 

1. Accedere all'account Origami con diritti di amministratore.

2. Nel menu in alto fare clic su **Admin**.
   
    ![Configura accesso Single Sign-On](./media/active-directory-saas-origami-tutorial/tutorial_origami_51.png)

3. Nella finestra di dialogo **Users and Security** (Utenti e sicurezza) fare clic su **Utenti**.
   
    ![Configura accesso Single Sign-On](./media/active-directory-saas-origami-tutorial/tutorial_origami_54.png)

4. Fare clic su **Add new user**.
   
    ![Configura accesso Single Sign-On](./media/active-directory-saas-origami-tutorial/tutorial_origami_55.png)

5. Nella finestra di dialogo Add New User seguire questa procedura:
   
    ![Configura accesso Single Sign-On](./media/active-directory-saas-origami-tutorial/tutorial_origami_56.png)

    a. Nella casella di testo **Username** (Nome utente) immettere l'indirizzo di posta elettronica dell'utente, ad esempio **brittasimon@contoso.com**.

    b. Nella casella di testo **Password** digitare una password.

    c. Nella casella di testo **Conferma password** digitare di nuovo la password.

    d. Nella casella di testo **First Name** (Nome) immettere il nome dell'utente, ad esempio **Britta**.

    e. Nella casella di testo **Last Name** (Cognome) immettere il cognome dell'utente, ad esempio **Simon**.

    f. Fare clic su **Save**.
   
    ![Configura accesso Single Sign-On](./media/active-directory-saas-origami-tutorial/tutorial_origami_57.png)

6. Assegnare i **ruoli utente** e l'**accesso client** all'utente. 
   
    ![Configura accesso Single Sign-On](./media/active-directory-saas-origami-tutorial/tutorial_origami_58.png)

### <a name="assigning-the-azure-ad-test-user"></a>Assegnazione dell'utente test di Azure AD

In questa sezione Britta Simon viene abilitata per l'uso dell'accesso Single Sign-On di Azure concedendo l'accesso a Origami.

![Assegna utente][200] 

**Per assegnare Britta Simon a Origami, seguire questa procedura:**

1. Nel portale di Azure aprire la visualizzazione delle applicazioni e quindi la visualizzazione delle directory e passare ad **Applicazioni aziendali**, quindi fare clic su **Tutte le applicazioni**.

    ![Assegna utente][201] 

2. Nell'elenco di applicazioni, selezionare **Origami**.

    ![Configura accesso Single Sign-On](./media/active-directory-saas-origami-tutorial/tutorial_origami_app.png) 

3. Scegliere **Utenti e gruppi** dal menu a sinistra.

    ![Assegna utente][202] 

4. Fare clic sul pulsante **Aggiungi**. Selezionare quindi **Utenti e gruppi** nella finestra di dialogo **Aggiungi assegnazione**.

    ![Assegna utente][203]

5. Nella finestra di dialogo **Utenti e gruppi** selezionare **Britta Simon** nell'elenco Utenti.

6. Fare clic sul pulsante **Seleziona** nella finestra di dialogo **Utenti e gruppi**.

7. Fare clic sul pulsante **Assegna** nella finestra di dialogo **Aggiungi assegnazione**.
    
### <a name="testing-single-sign-on"></a>Test dell'accesso Single Sign-On

In questa sezione viene testata la configurazione dell'accesso Single Sign-On di Azure AD usando il pannello di accesso.

Quando si fa clic sul riquadro Origami nel pannello di accesso, si dovrebbe accedere automaticamente all'applicazione Origami.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Elenco di esercitazioni sulla procedura di integrazione delle app SaaS con Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-origami-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-origami-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-origami-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-origami-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-origami-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-origami-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-origami-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-origami-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-origami-tutorial/tutorial_general_203.png



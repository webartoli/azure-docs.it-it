---
title: 'Esercitazione: Integrazione di Azure Active Directory con Peoplecart | Microsoft Docs'
description: Informazioni su come configurare l'accesso Single Sign-On tra Azure Active Directory e Peoplecart.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: c83b5d9d-2638-4689-b9f0-f56a9159e7a0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.translationtype: HT
ms.sourcegitcommit: bfd49ea68c597b109a2c6823b7a8115608fa26c3
ms.openlocfilehash: b83a1621263cac0b23bbd35a49fda213d2e4271a
ms.contentlocale: it-it
ms.lasthandoff: 07/25/2017

---
# <a name="tutorial-azure-active-directory-integration-with-peoplecart"></a>Esercitazione: integrazione di Azure Active Directory con Peoplecart

Questa esercitazione descrive come integrare Peoplecart con Azure Active Directory (Azure AD).

L'integrazione di Peoplecart con Azure AD offre i vantaggi seguenti:

- È possibile controllare in Azure AD chi può accedere a Peoplecart
- È possibile abilitare gli utenti per l'accesso automatico a Peoplecart (Single Sign-On) con i propri account Azure AD
- È possibile gestire gli account in un'unica posizione centrale: il portale di Azure.

Per altre informazioni sull'integrazione di app SaaS con Azure AD, vedere [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Prerequisiti

Per configurare l'integrazione di Azure AD con Peoplecart, sono necessari gli elementi seguenti:

- Sottoscrizione di Azure AD.
- Sottoscrizione di Peoplecart abilitata per l'accesso Single Sign-On

> [!NOTE]
> Non è consigliabile usare un ambiente di produzione per testare i passaggi di questa esercitazione.

A questo scopo, è consigliabile seguire le indicazioni seguenti:

- Non usare l'ambiente di produzione a meno che non sia necessario.
- Se non è disponibile un ambiente di valutazione di Azure AD, è possibile [ottenere una versione di valutazione di un mese](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrizione dello scenario
In questa esercitazione viene eseguito il test dell'accesso Single Sign-On di Azure AD in un ambiente di test. Lo scenario descritto in questa esercitazione prevede i due blocchi predefiniti seguenti:

1. Aggiunta di Peoplecart dalla raccolta
2. Configurazione e test dell'accesso Single Sign-On di Azure AD

## <a name="adding-peoplecart-from-the-gallery"></a>Aggiunta di Peoplecart dalla raccolta
Per configurare l'integrazione di Peoplecart in Azure AD, è necessario aggiungere Peoplecart dalla raccolta al proprio elenco di app SaaS gestite.

**Per aggiungere Peoplecart dalla raccolta, seguire questa procedura:**

1. Nel **[portale di Azure](https://portal.azure.com)** fare clic sull'icona di **Azure Active Directory** nel riquadro di spostamento sinistro. 

    ![Pulsante Azure Active Directory][1]

2. Passare ad **Applicazioni aziendali**. Andare quindi a **Tutte le applicazioni**.

    ![Pannello Applicazioni aziendali][2]
    
3. Fare clic sul pulsante **Nuova applicazione** nella parte superiore della finestra di dialogo per aggiungere una nuova applicazione.

    ![Pulsante Nuova applicazione][3]

4. Nella casella di ricerca digitare **Peoplecart**, selezionare **Peoplecart** nel pannello dei risultati e quindi fare clic sul pulsante **Aggiungi** per aggiungere l'applicazione.

    ![Peoplecart nell'elenco risultati](./media/active-directory-saas-peoplecart-tutorial/tutorial_peoplecart_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurare e testare l'accesso Single Sign-On di Azure AD
In questa sezione viene configurato e testato l'accesso Single Sign-On di Azure AD con Peoplecart in base a un utente test di nome "Britta Simon".

Per il funzionamento dell'accesso Single Sign-On, Azure AD deve conoscere qual è l'utente di Peoplecart che corrisponde a un utente di Azure AD. In altre parole, deve essere stabilita una relazione di collegamento tra un utente di Azure AD e l'utente correlato in Peoplecart.

Per stabilire la relazione di collegamento, in Peoplecart assegnare il valore di **nome utente** in Azure AD come valore di **Username** (Nome utente).

Per configurare e testare l'accesso Single Sign-On di Azure AD con Peoplecart, è necessario completare i blocchi predefiniti seguenti:

1. **[Configurare l'accesso Single Sign-On di Azure AD](#configure-azure-ad-single-sign-on)**: per consentire agli utenti di usare questa funzionalità.
2. **[Creare un utente di test di Azure AD](#create-an-azure-ad-test-user)**: per testare l'accesso Single Sign-On di Azure AD con l'utente Britta Simon.
3. **[Creare un utente test di Peoplecart](#create-a-peoplecart-test-user)**: per avere una controparte di Britta Simon in Peoplecart collegata alla rappresentazione dell'utente in Azure AD.
4. **[Assegnare l'utente test di Azure AD](#assign-the-azure-ad-test-user)**: per abilitare Britta Simon all'uso dell'accesso Single Sign-On di Azure AD.
5. **[Testare l'accesso Single Sign-On](#test-single-sign-on)**: per verificare se la configurazione funziona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurare l'accesso Single Sign-On di Azure AD

In questa sezione viene abilitato l'accesso Single Sign-On di Azure AD nel portale di Azure e viene configurato l'accesso Single Sign-On nell'applicazione Peoplecart.

**Per configurare Single Sign-On di Azure AD con Peoplecart, seguire questa procedura:**

1. Nella pagina di integrazione dell'applicazione **Peoplecart** del portale di Azure fare clic su **Single Sign-On**.

    ![Configura accesso Single Sign-On][4]

2. Nella finestra di dialogo **Single Sign-On** selezionare **Accesso basato su SAML** per **Modalità** per abilitare l'accesso Single Sign-On.
 
    ![Configura accesso Single Sign-On](./media/active-directory-saas-peoplecart-tutorial/tutorial_peoplecart_samlbase.png)

3. Nella sezione **URL e dominio Peoplecart** seguire questa procedura:

    ![Informazioni su URL e dominio per Single Sign-On di Peoplecart](./media/active-directory-saas-peoplecart-tutorial/tutorial_peoplecart_url.png)

    a. Nella casella di testo **URL di accesso** digitare l'URL usando il modello seguente: `https://<tenantname>.peoplecart.com/SignIn.aspx`.

    b. Nella casella di testo **Identificatore** digitare l'URL adottando il modello seguente: `https://<tenantname>.peoplecart.com`

    > [!NOTE] 
    > Poiché questi non sono i valori reali, Aggiornare questi valori con l'identificatore e l'URL di accesso effettivi. Per ottenere questi valori, contattare il [team di supporto clienti di Peoplecart](https://peoplecart.com/ContactUs.aspx). 
 
4. Nella sezione **Certificato di firma SAML** fare clic su **XML di metadati** e quindi salvare il file dei metadati nel computer.

    ![Collegamento di download del certificato](./media/active-directory-saas-peoplecart-tutorial/tutorial_peoplecart_certificate.png) 

5. Fare clic sul pulsante **Salva** .

    ![Pulsante Salva di Configura accesso Single Sign-On](./media/active-directory-saas-peoplecart-tutorial/tutorial_general_400.png)

6. Nella sezione **Configurazione di Peoplecart** fare clic su **Configura Peoplecart** per aprire la finestra **Configura accesso**. Copiare l'**URL servizio Single Sign-On SAML** dalla **sezione Riferimento rapido.**

    ![Configurazione di Peoplecart](./media/active-directory-saas-peoplecart-tutorial/tutorial_peoplecart_configure.png) 

7. Per configurare l'accesso Single Sign-On sul lato **Peoplecart**, è necessario inviare il file **XML metadati** scaricato e il valore dell'**URL del servizio Single Sign-On SAML** al [team di supporto di Peoplecart](https://peoplecart.com/ContactUs.aspx). La configurazione viene eseguita in modo che la connessione SSO SAML sia impostata correttamente su entrambi i lati.

> [!TIP]
> Un riepilogo delle istruzioni è disponibile all'interno del [portale di Azure](https://portal.azure.com) durante la configurazione dell'app.  Dopo aver aggiunto l'app dalla sezione **Active Directory > Applicazioni aziendali** è sufficiente fare clic sulla scheda **Single Sign-On** e accedere alla documentazione incorporata tramite la sezione **Configurazione** nella parte inferiore. Altre informazioni sulla funzione di documentazione incorporata sono disponibili in [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985) (Documentazione incorporata di Azure AD).
> 

### <a name="create-an-azure-ad-test-user"></a>Creare un utente test di Azure AD
Questa sezione descrive come creare un utente test denominato Britta Simon nel portale di Azure.

![Creare un utente test di Azure AD][100]

**Per creare un utente test in Azure AD, eseguire la procedura seguente:**

1. Nel **portale di Azure** fare clic sull'icona di **Azure Active Directory** nel riquadro di spostamento sinistro.

    ![Pulsante Azure Active Directory](./media/active-directory-saas-peoplecart-tutorial/create_aaduser_01.png) 

2. Passare a **Utenti e gruppi** e fare clic su **Tutti gli utenti** per visualizzare l'elenco di utenti.
    
    ![Collegamenti "Utenti e gruppi" e "Tutti gli utenti"](./media/active-directory-saas-peoplecart-tutorial/create_aaduser_02.png) 

3. Nella parte superiore della finestra di dialogo fare clic su **Aggiungi** per aprire la finestra di dialogo **Utente**.
 
    ![Pulsante Aggiungi](./media/active-directory-saas-peoplecart-tutorial/create_aaduser_03.png) 

4. Nella pagina della finestra di dialogo **Utente** seguire questa procedura:
 
    ![Finestra di dialogo Utente](./media/active-directory-saas-peoplecart-tutorial/create_aaduser_04.png) 

    a. Nella casella di testo **Nome** digitare **BrittaSimon**.

    b. Nella casella di testo **Nome utente** digitare l'**indirizzo di posta elettronica** di BrittaSimon.

    c. Selezionare **Mostra password** e prendere nota del valore della **Password**.

    d. Fare clic su **Crea**.
 
### <a name="create-a-peoplecart-test-user"></a>Creare un utente test di Peoplecart

In questa sezione viene creato un utente chiamato Britta Simon in Peoplecart. Collaborare con il [team di supporto di Peoplecart](https://peoplecart.com/ContactUs.aspx) per aggiungere gli utenti alla piattaforma Peoplecart. Gli utenti devono essere creati e attivati prima di usare l'accesso Single Sign-On. 

### <a name="assign-the-azure-ad-test-user"></a>Assegnare l'utente test di Azure AD

In questa sezione Britta Simon viene abilitata per l'uso dell'accesso Single Sign-On di Azure concedendole l'accesso a Peoplecart.

![Assegnare il ruolo utente][200] 

**Per assegnare Britta Simon a Peoplecart, seguire questa procedura:**

1. Nel portale di Azure aprire la visualizzazione delle applicazioni e quindi la visualizzazione delle directory e passare ad **Applicazioni aziendali**, quindi fare clic su **Tutte le applicazioni**.

    ![Assegna utente][201] 

2. Nell'elenco delle applicazioni, selezionare **Peoplecart**.

    ![Collegamento Peoplecart nell'elenco Applicazioni](./media/active-directory-saas-peoplecart-tutorial/tutorial_peoplecart_app.png) 

3. Scegliere **Utenti e gruppi** dal menu a sinistra.

    ![Collegamento "Utenti e gruppi"][202] 

4. Fare clic sul pulsante **Aggiungi**. Selezionare quindi **Utenti e gruppi** nella finestra di dialogo **Aggiungi assegnazione**.

    ![Riquadro Aggiungi assegnazione][203]

5. Nella finestra di dialogo **Utenti e gruppi** selezionare **Britta Simon** nell'elenco Utenti.

6. Fare clic sul pulsante **Seleziona** nella finestra di dialogo **Utenti e gruppi**.

7. Fare clic sul pulsante **Assegna** nella finestra di dialogo **Aggiungi assegnazione**.
    
### <a name="test-single-sign-on"></a>Testare l'accesso Single Sign-On

In questa sezione viene testata la configurazione dell'accesso Single Sign-On di Azure AD usando il pannello di accesso.

Quando si fa clic sul riquadro Peoplecart nel pannello di accesso, dovrebbe essere visualizzata la pagina di accesso dell'applicazione Peoplecart.
Per altre informazioni sul pannello di accesso, vedere [Introduzione al Pannello di accesso](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Risorse aggiuntive

* [Elenco di esercitazioni sulla procedura di integrazione delle app SaaS con Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-peoplecart-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-peoplecart-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-peoplecart-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-peoplecart-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-peoplecart-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-peoplecart-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-peoplecart-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-peoplecart-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-peoplecart-tutorial/tutorial_general_203.png



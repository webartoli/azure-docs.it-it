---
title: "Usare un'Identità del servizio gestito per una macchina virtuale Windows per accedere ad Azure Key Vault"
description: "Esercitazione che illustra come usare un'Identità del servizio gestito di una macchina virtuale Windows per accedere ad Azure Key Vault."
services: active-directory
documentationcenter: 
author: elkuzmen
manager: mbaldwin
editor: bryanla
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/14/2017
ms.author: elkuzmen
ms.translationtype: HT
ms.sourcegitcommit: 47ba7c7004ecf68f4a112ddf391eb645851ca1fb
ms.openlocfilehash: 1785b4da8c54354dbc48c514dbb8f969a1f997ca
ms.contentlocale: it-it
ms.lasthandoff: 09/14/2017

---

# <a name="use-managed-service-identity-msi-with-a-windows-vm-to-access-azure-key-vault"></a>Usare un'Identità del servizio gestito con una macchina virtuale Windows per accedere ad Azure Key Vault 

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]

Questa esercitazione illustra come abilitare l'Identità del servizio gestito (MSI, Managed Service Identity) per una macchina virtuale Windows e quindi usarla per accedere ad Azure Key Vault. Le Identità del servizio gestito vengono gestite automaticamente da Azure e consentono di eseguire l'autenticazione ai servizi che supportano l'autenticazione di Azure AD senza la necessità di inserire le credenziali nel codice. Si apprenderà come:


> [!div class="checklist"]
> * Abilitare l'Identità del servizio gestito in una macchina virtuale Windows 
> * Concedere alla macchina virtuale l'accesso a un segreto archiviato in Key Vault 
> * Ottenere un token di accesso usando l'identità della macchina virtuale e usarlo per recuperare il segreto da Key Vault 


Se non si ha una sottoscrizione di Azure, creare un [account gratuito](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) prima di iniziare.

## <a name="sign-in-to-azure"></a>Accedere ad Azure

Accedere al portale di Azure all'indirizzo [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-windows-virtual-machine-in-a-new-resource-group"></a>Creare una macchina virtuale Windows in un nuovo gruppo di risorse

Per questa esercitazione si creerà una nuova macchina virtuale Windows, ma è anche possibile abilitare l'Identità del servizio gestito in una macchina virtuale esistente.

1.  Fare clic sul pulsante **Nuovo** nell'angolo superiore sinistro del portale di Azure.
2.  Selezionare **Calcolo** e quindi **Windows Server 2016 Datacenter**. 
3.  Immettere le informazioni relative alla macchina virtuale. I valori di **Nome utente** e **Password** creati in questa schermata sono le credenziali usate per accedere alla macchina virtuale.
4.  Dall'elenco a discesa **Sottoscrizione** selezionare la sottoscrizione corretta per la macchina virtuale.
5.  Per selezionare un nuovo **Gruppo di risorse** in cui creare la macchina virtuale, scegliere **Crea nuovo**. Al termine fare clic su **OK**.
6.  Selezionare la dimensione della macchina virtuale. Per visualizzare altre dimensioni, selezionare **Visualizza tutto** o modificare il filtro **Supported disk type** (Tipo di disco supportato). Nel pannello delle impostazioni mantenere le impostazioni predefinite e fare clic su **OK**.

    ![Testo immagine alt](media/msi-tutorial-windows-vm-access-arm/msi-windows-vm.png)

## <a name="enable-msi-on-your-vm"></a>Abilitare Identità del servizio gestito nella macchina virtuale 

Un'Identità del servizio gestito per una macchina virtuale consente di ottenere i token di accesso da Azure AD senza dover inserire le credenziali nel codice. L'abilitazione dell'Identità del servizio gestito indica ad Azure di creare un'identità gestita per la macchina virtuale. A livello sottostante, quando si abilita Identità del servizio gestito, si eseguono due operazioni, ovvero si installa l'estensione della macchina virtuale per l'Identità del servizio gestito nella macchina virtuale e si abilita l'Identità del servizio gestito in Azure Resource Manager.

1.  Selezionare la **macchina virtuale** in cui si vuole abilitare Identità del servizio gestito.  
2.  Nella barra di spostamento a sinistra fare clic su **Configurazione**. 
3.  Viene visualizzato **Managed Service Identity** (Identità del servizio gestito). Per registrare e abilitare Identità del servizio gestita, scegliere **Sì**. Se si vuole disabilitare questa funzionalità, scegliere No. 
4.  Assicurarsi di fare clic su **Salva** per salvare la configurazione.  

    ![Testo immagine alt](media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

5. Per verificare le estensioni installate nella macchina virtuale, fare clic su **Estensioni**. Se Identità del servizio gestito è abilitata, nell'elenco sarà inclusa la voce **ManagedIdentityExtensionforWindows**.

    ![Testo immagine alt](media/msi-tutorial-windows-vm-access-arm/msi-windows-extension.png)

## <a name="grant-your-vm-access-to-a-secret-stored-in-a-key-vault"></a>Concedere alla macchina virtuale l'accesso a un segreto archiviato in Key Vault 
 
Usando Identità del servizio gestito, il codice può ottenere i token di accesso per autenticarsi alle risorse che supportano l'autenticazione di Azure AD.  Tuttavia, non tutti i servizi di Azure supportano l'autenticazione di Azure AD. Per usare Identità del servizio gestito con i servizi che non supportano l'autenticazione di Azure AD, è possibile archiviare le credenziali necessarie per tali servizi in Azure Key Vault e usare Identità del servizio gestito per l'autenticazione in Key Vault al fine di recuperare le credenziali. 

In primo luogo, è necessario creare un insieme di credenziali delle chiavi e concedere all'Identità della macchina virtuale l'accesso a Key Vault.   

1. Nella parte superiore della barra di spostamento a sinistra selezionare **+ Nuovo**, quindi **Sicurezza e identità** quindi **Key Vault**.  
2. Specificare un **nome** per il nuovo insieme di credenziali delle chiavi. 
3. Individuare l'insieme di credenziali delle chiavi nella stessa sottoscrizione e nello stesso gruppo di risorse della macchina virtuale creata in precedenza. 
4. Selezionare **Criteri di accesso** condiviso e fare clic su **Aggiungi nuovo**. 
5. In Configura dal modello selezionare **Gestione dei segreti**. 
6. Scegliere **Selezionare un'entità** e nel campo di ricerca immettere il nome della macchina virtuale creata in precedenza.  Selezionare la macchina virtuale nell'elenco dei risultati e fare clic su **Seleziona**. 
7. Fare clic su **OK** per terminare l'aggiunta dei nuovi criteri di accesso e su **OK** per completare la selezione dei criteri di accesso. 
8. Fare clic su **Crea** per completare la creazione dell'insieme di credenziali delle chiavi. 

    ![Testo immagine alt](media/msi-tutorial-windows-vm-access-nonaad/msi-blade.png)


Successivamente, aggiungere un segreto a Key Vault, in modo che in un secondo momento sia possibile recuperare il segreto mediante il codice in esecuzione nella macchina virtuale: 

1. Selezionare **Tutte le risorse**, quindi individuare e selezionare l'insieme di credenziali delle chiavi appena creato. 
2. Selezionare **Segreti** e fare clic su **Aggiungi**. 
3. Selezionare **Manuale** dalle **Opzioni di caricamento**. 
4. Specificare un nome e il valore del segreto.  Il valore può essere qualsiasi. 
5. Lasciare vuoti i campi della data di attivazione e della data di scadenza e lasciare **Abilitato** su **Sì**. 
6. Fare clic su **Crea** per creare il segreto. 
 
## <a name="get-an-access-token-using-the-vm-identity-and-use-it-retrieve-the-secret-from-the-key-vault"></a>Ottenere un token di accesso usando l'identità della macchina virtuale e usarlo per recuperare il segreto da Key Vault  

Dopo aver creato un segreto, averlo archiviato in un insieme di credenziali delle chiavi e aver concesso all'Identità del servizio gestito della macchina virtuale l'accesso all'insieme di credenziali delle chiavi, è possibile scrivere il codice per recuperare il segreto in fase di esecuzione.  Per semplificare questo esempio verranno usate semplici chiamate REST tramite PowerShell.  Se PowerShell non è installato, scaricarlo da [qui](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1).

In primo luogo, si userà l'Identità del servizio gestito della macchina virtuale per ottenere un token di accesso per l'autenticazione in Key Vault:
 
1. Nel portale passare a **Macchine virtuali**, selezionare la macchina virtuale Windows e in **Panoramica** fare clic su **Connetti**.
2. Inserire il **Nome utente** e la **Password** aggiunti al momento della creazione della **macchina virtuale Windows**.  
3. Ora che si è creata una **connessione Desktop remoto** con la macchina virtuale, aprire PowerShell nella sessione remota.  
4. In PowerShell, richiamare la richiesta Web nel tenant per ottenere il token per l'host locale in una porta specifica della macchina virtuale.  

    Richiesta di PowerShell:
    
    ```powershell
    PS C:\> $response = Invoke-WebRequest -Uri http://localhost:50342/oauth2/token -Method GET -Body @{resource="https://vault.azure.net"} -Headers @{Metadata="true"} 
    ```
    
    Estrarre quindi la risposta completa, archiviata come stringa in formato JSON (JavaScript Object Notation) nell'oggetto $response.  
    
    ```powershell
    PS C:\> $content = $response.Content | ConvertFrom-Json 
    ```
    
    Estrarre poi il token di accesso dalla risposta.  
    
    ```powershell
    PS C:\> $KeyVaultToken = $content.access_token 
    ```
    
    Infine, usare il comando Invoke-WebRequest di PowerShell per recuperare il segreto creato in precedenza nell'insieme di credenziali delle chiavi, passando il token di accesso nell'intestazione dell'autorizzazione.  È necessario l'URL dell'insieme di credenziali delle chiavi che si trova nella sezione **Informazioni di base** della pagina **Panoramica** dell'insieme di credenziali delle chiavi.  
    
    ```powershell
    PS C:\> (Invoke-WebRequest -Uri https://<your-key-vault-URL>/secrets/<secret-name>?api-version=2016-10-01 -Method GET -Headers @{Authorization="Bearer $KeyVaultToken"}).content 
    ```
    
    La risposta avrà l'aspetto seguente: 
    
    ```powershell
    {"value":"p@ssw0rd!","id":"https://mytestkeyvault.vault.azure.net/secrets/MyTestSecret/7c2204c6093c4d859bc5b9eff8f29050","attributes":{"enabled":true,"created":1505088747,"updated":1505088747,"recoveryLevel":"Purgeable"}} 
    ```
    
Dopo aver recuperato il segreto dall'insieme di credenziali delle chiavi, è possibile usarlo per eseguire l'autenticazione a un servizio che richiede un nome e una password. 

## <a name="related-content"></a>Contenuti correlati

- Per una panoramica dell'Identità di servizio gestito, vedere [Panoramica dell'Identità di servizio gestito](../active-directory/msi-overview.md).

Usare la sezione dei commenti seguente per fornire commenti e suggerimenti utili per migliorare e organizzare i contenuti disponibili.


---
title: Risolvere i problemi di Sincronizzazione File di Azure (anteprima) | Microsoft Docs
description: Risolvere i problemi comuni di Sincronizzazione File di Azure
services: storage
documentationcenter: 
author: wmgries
manager: klaasl
editor: jgerend
ms.assetid: 297f3a14-6b3a-48b0-9da4-db5907827fb5
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/19/2017
ms.author: wgries
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: cf3f3cf63cafc3b883d26144a53066ee421eb2a6
ms.contentlocale: it-it
ms.lasthandoff: 09/25/2017

---

# <a name="troubleshoot-azure-file-sync-preview"></a>Risolvere i problemi di Sincronizzazione File di Azure (anteprima)
Con Sincronizzazione file di Azure (anteprima), è possibile replicare le condivisioni in Windows Server in locale o in Azure. Gli utenti possono quindi accedere alla condivisione di file da Windows Server, ad esempio attraverso una condivisione SMB o NFS. Questo è particolarmente utile per gli scenari in cui si accede e si modificano i dati lontano da un data center di Azure, ad esempio in una succursale. I dati possono essere replicati tra più endpoint di Windows Server, ad esempio tra più succursali.

L'obiettivo di questo articolo è aiutare l'utente a individuare e risolvere i problemi riscontrati con la distribuzione di Sincronizzazione file di Azure. Se i problemi non vengono risolti, la guida spiega come raccogliere i log importanti dal sistema per effettuare un'analisi più approfondita dei problemi. Sono disponibili le opzioni seguenti per ottenere assistenza per Sincronizzazione file di Azure:

- Supporto tecnico Microsoft: per creare un nuovo caso di supporto, passare alla scheda "Guida e supporto tecnico" nel portale di Azure e fare clic su "Nuova richiesta di supporto".
- [Forum di Archiviazione di Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=windowsazuredata)

## <a name="how-to-troubleshoot-agent-installation-failures"></a>Come risolvere gli errori di installazione dell'agente
Se l'installazione dell'agente Sincronizzazione file di Azure non riesce, eseguire il comando seguente da un prompt dei comandi con privilegi elevati per abilitare la registrazione durante l'installazione dell'agente:

```
StorageSyncAgent.msi /l*v Installer.log
```

Quando l'installazione non riesce, esaminare il file installer.log per determinare la causa.

## <a name="server-is-not-listed-under-registered-servers-in-the-azure-portal"></a>Il server non è elencato in Server registrati nel portale di Azure
Se un server non è elencato tra i server registrati per un servizio di sincronizzazione archiviazione, eseguire i passaggi seguenti:
1. Accedere al server da registrare.
2. Aprire Esplora file e passare alla directory di installazione dell'agente di sincronizzazione archiviazione (il percorso predefinito è `C:\Program Files\Azure\StorageSyncAgent`). 
3. Eseguire ServerRegistration.exe e seguire la procedura guidata per registrare il server con un servizio di sincronizzazione archiviazione.

## <a name="server-registration-displays-the-following-message-after-installing-the-azure-file-sync-agent-this-server-is-already-registered"></a>Al termine dell'installazione dell'agente Sincronizzazione file di Azure, Registrazione server visualizza un messaggio per indicare che il server è già registrato
![Schermata della finestra di dialogo Registrazione server con messaggio di errore che indica che il server è già registrato](media/storage-sync-files-troubleshoot/server-registration-1.png)

Questo messaggio viene visualizzato se il server è stato registrato in precedenza con un servizio di sincronizzazione archiviazione. Per annullare la registrazione del server con il servizio di sincronizzazione archiviazione corrente e registrare il server con un nuovo servizio di sincronizzazione archiviazione, seguire la procedura per [annullare la registrazione di un server con Sincronizzazione file di Azure](storage-sync-files-server-registration.md#unregister-the-server-with-storage-sync-service).

## <a name="how-to-troubleshoot-sync-not-working-on-a-server"></a>Come risolvere i problemi di sincronizzazione in un server
Se la sincronizzazione non riesce in un server, eseguire queste operazioni:
- Verificare che esista un endpoint server nel portale di Azure per la directory che si vuole sincronizzare con una condivisione file di Azure:
    
    ![Schermata di un gruppo di sincronizzazione con un endpoint server e un endpoint cloud nel portale di Azure](media/storage-sync-files-troubleshoot/sync-troubleshoot-1.png)

- Esaminare i log eventi operativi e di diagnostica nel Visualizzatore eventi, sotto `Applications and Services\Microsoft\FileSync\Agent`.
- Verificare la connettività Internet nel server.
- Verificare che il servizio Sincronizzazione file di Azure sia in esecuzione nel server aprendo lo snap-in Servizi di MMC e che l'agente di sincronizzazione archiviazione (FileSyncSvc) sia in esecuzione.

## <a name="how-to-troubleshoot-individual-files-failing-to-sync"></a>Come risolvere i problemi di sincronizzazione dei singoli file 
Se non è possibile sincronizzare singoli file, eseguire queste operazioni:
- Esaminare i log eventi operativi e di diagnostica sotto `Applications and Services\Microsoft\FileSync\Agent` nel Visualizzatore eventi
- Verificare che non esistano handle aperti per il file
    - Nota: Sincronizzazione file di Azure crea periodicamente snapshot Servizio Copia Shadow per sincronizzare i file con handle aperti

## <a name="how-to-troubleshoot-files-that-fail-to-tier"></a>Come risolvere i problemi dei file che non è possibile archiviare a livelli
Se non è possibile archiviare i file a livelli in File di Azure, procedere come segue:

- Verificare che i file esistano nella condivisione di file di Azure
    - Nota: un file deve essere sincronizzato con una condivisione di file di Azure per poter essere archiviato a livelli
- Esaminare i log eventi operativi e di diagnostica situati sotto `Applications and Services\Microsoft\FileSync\Agent` nel Visualizzatore eventi
- Verificare la connettività Internet nel server 
- Verificare che i driver di filtro di Sincronizzazione file di Azure (StorageSync.sys e StorageSyncGuard.sys) siano in esecuzione
    - Aprire un prompt dei comandi con privilegi elevati, eseguire `fltmc` e verificare che siano elencati i driver di filtro StorageSync.sys e StorageSyncGuard.sys del file system

## <a name="how-to-troubleshoot-files-that-fail-to-be-recalled"></a>Come risolvere i problemi dei file che non è possibile richiamare
Se non è possibile richiamare i file, procedere come segue:
- Esaminare i log eventi operativi e di diagnostica situati sotto `Applications and Services\Microsoft\FileSync\Agent` nel Visualizzatore eventi
- Verificare che i file esistano nella condivisione di file di Azure
- Verificare la connettività Internet nel server 
- Verificare che i driver di filtro di Sincronizzazione file di Azure (StorageSync.sys e StorageSyncGuard.sys) siano in esecuzione
    - Aprire un prompt dei comandi con privilegi elevati, eseguire `fltmc` e verificare che siano elencati i driver di filtro StorageSync.sys e StorageSyncGuard.sys del file system

## <a name="how-to-troubleshoot-files-being-unexpectedly-recalled-on-a-server"></a>Come risolvere i problemi dei file richiamati in modo imprevisto su un server
Antivirus, backup e altre applicazioni che leggono quantità elevate di file causano richiami indesiderati a meno che rispettino l'attributo offline e ignorino il contenuto dei file. Ignorare i file offline per i prodotti che supportano questa opzione consente di evitare richiami indesiderati quando si eseguono operazioni come scansioni antivirus o processi di backup.

Rivolgersi al fornitore di software per sapere come configurare la soluzione in modo che non legga i file offline.

I richiami indesiderati possono verificarsi anche in altri scenari, ad esempio l'esplorazione di file in Esplora File. Aprire una cartella che contiene file archiviati a livelli nel cloud in Esplora File nel server può causare richiami indesiderati, soprattutto se nel server è abilitato un antivirus.

## <a name="general-troubleshooting"></a>Risoluzione dei problemi generali
Se si verificano problemi con Sincronizzazione file di Azure in un server, per prima cosa eseguire queste operazioni:
- Esaminare i log eventi operativi e di diagnostica nel Visualizzatore eventi
    - I problemi di sincronizzazione, archiviazione a livelli e richiamo vengono registrati nei log eventi operativi e di diagnostica in `Applications and Services\Microsoft\FileSync\Agent`
    - I problemi di gestione di un server (ad esempio, le impostazioni di configurazione) vengono registrati nei log eventi operativi e di diagnostica in `Applications and Services\Microsoft\FileSync\Management`
- Verificare che il servizio Sincronizzazione file di Azure sia in esecuzione nel server
    - Aprire lo snap-in Servizi di MMC e verificare se l'agente di sincronizzazione archiviazione (FileSyncSvc) è in esecuzione
- Verificare che i driver di filtro di Sincronizzazione file di Azure (StorageSync.sys e StorageSyncGuard.sys) siano in esecuzione
    - Aprire un prompt dei comandi con privilegi elevati, eseguire fltmc e verificare che siano elencati i driver di filtro StorageSync.sys e StorageSyncGuard.sys del file system

Se il problema non viene risolto usando i passaggi precedenti, attenersi a questa procedura per eseguire lo strumento AFSDiag:
1. Creare una directory che verrà usata per salvare l'output di AFSDiag, ad esempio, c:\output.
2. Aprire una finestra di PowerShell con privilegi elevati ed eseguire i comandi seguenti (premere INVIO dopo ogni comando):

    ```PowerShell
    cd "c:\Program Files\Azure\StorageSyncAgent"
    Import-Module .\afsdiag.ps1
    Debug-Afs c:\output # Note: use the path created in step 1
    ```

3. Per il livello di traccia nella modalità kernel di Sincronizzazione file di Azure, immettere 1 (a meno che non sia richiesta in modo specifico la creazione di tracce più dettagliate) e premere INVIO.
4. Per il livello di traccia nella modalità utente di Sincronizzazione file di Azure, immettere 1 (a meno che non sia richiesta in modo specifico la creazione di tracce più dettagliate) e premere INVIO.
5. Riprodurre il problema e al termine premere D.
6. Un file ZIP contenente i file di traccia e log sarà presente nella directory di output specificata.

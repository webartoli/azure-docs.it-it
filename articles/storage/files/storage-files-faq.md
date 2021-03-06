---
title: Domande frequenti su File di Azure | Microsoft Docs
description: Risposte ad alcune domande frequenti sul servizio File di Azure.
services: storage
documentationcenter: 
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 09/19/2017
ms.author: renash
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: eb0b4d7cde568ab1809daa8025120828e75f5d76
ms.contentlocale: it-it
ms.lasthandoff: 09/25/2017

---
# <a name="frequently-asked-questions-about-azure-files"></a>Domande frequenti su File di Azure

## <a name="general"></a>Generale
* **D. Che cos'è File di Azure?**  
   
    File di Azure è un file system distribuito in Azure. Fornisce un'interfaccia del protocollo SMB che consente agli utenti di installare lo spazio di archiviazione come condivisione nativa sul computer locale o su macchine virtuali di Azure supportate.

* **D. Perché File di Azure è utile?**  
   
    File di Azure offre l'accesso ai dati condivisi tra più macchine virtuali e piattaforme. Fare riferimento a [Vantaggi offerti da File di Azure](storage-files-introduction.md#why-azure-files-is-useful).

* **D. Quando usare File di Azure e BLOB di Azure invece di Dischi di Azure**  
   
    Microsoft Azure offre diversi modi per archiviare e accedere ai dati nel cloud.  
   
    File di Azure - Specifica un'interfaccia SMB, librerie client e un'interfaccia REST che consente l'accesso facile ai file archiviati ovunque ci si trovi.  
   
    BLOB di Azure - Include librerie client e un'interfaccia REST che consente di archiviare i dati non strutturati e di accedervi su larga scala in BLOB in blocchi.  
   
    Azure Data Disks - Include librerie client e un'interfaccia REST che consente di archiviare in modo permanente i dati e di accedervi da un disco rigido virtuale collegato.  
   
    Altre informazioni si [Quando usare BLOB di Azure, File di Azure o Dischi dati di Azure](../common/storage-decide-blobs-files-disks.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)

* **D. Come iniziare a usare File di Azure?**  
   
    Iniziare creando una condivisione di file di Azure. È possibile creare condivisioni file di Azure usando il portale di Azure, i cmdlet di PowerShell per Archiviazione di Azure, le librerie client di Archiviazione di Azure o l'API REST di Archiviazione di Azure. In questa esercitazione verrà illustrato:

    * [Informazioni su come creare una condivisione file di Azure usando il portale di Azure](storage-how-to-use-files-portal.md)
    * [Informazioni su come creare una condivisione file di Azure usando Powershell](storage-how-to-create-file-share.md#create-file-share-through-powershell)
    * [Informazioni su come creare una condivisione file di Azure usando l'interfaccia della riga di comando](storage-how-to-create-file-share.md#create-file-share-through-command-line-interface-cli)

* **D. Quali repliche sono supportate da File di Azure?**  
   
    Attualmente, File di Azure supporta solo l'archiviazione con ridondanza locale o con ridondanza geografica. È previsto il supporto per l'archiviazione con ridondanza geografica e accesso in lettura, ma non sono ancora state definite le date.
    
## <a name="scale-targetslimits"></a>Obiettivi e limiti di scalabilità

* **D. Quanti client possono accedere simultaneamente allo stesso file?**

    C'è una quota pari a 2000 handle aperti su un singolo file. Al raggiungimento di 2000 handle aperti, viene restituito un errore che indica che la quota è stata raggiunta.

## <a name="security-authentication-and-access-control"></a>Sicurezza, autenticazione e controllo di accesso

* **D. Quali sono i vari modi per accedere ai file in File di Azure?**
    
    È possibile montare la condivisione file nel computer locale usando il protocollo SMB 3.0 o usare strumenti come [Storage Explorer](http://storageexplorer.com/) per accedere ai file nella condivisione file. Dall'applicazione è possibile usare le librerie client di archiviazione, l'API REST o PowerShell per accedere ai file nella condivisione file di Azure.

* **D. Come è possibile fornire accesso a un file specifico tramite un Web browser?**
    
    Le firme di accesso condiviso consentono di generare token con autorizzazioni specifiche che restano valide per un periodo di tempo definito. Ad esempio, è possibile generare un token con accesso in lettura a un determinato file per un periodo di tempo specifico. Tutti gli utenti che hanno l'URL in corso di validità possono accedere al file direttamente da qualsiasi Web browser. È possibile generare facilmente le chiavi di firma di accesso condiviso dall'interfaccia utente, ad esempio con Storage Explorer.

* **D. È possibile specificare autorizzazioni di sola lettura o di sola scrittura per le cartelle all'interno della condivisione?**
    
    Se la condivisione file viene montata tramite SMB, non è disponibile questo livello di controllo sulle autorizzazioni. È tuttavia possibile ottenere lo stesso risultato creando una firma di accesso condiviso tramite l'API REST o le librerie client.  

* **D. Come è possibile abilitare la crittografia lato server per File di Azure?**

    La [crittografia lato server](../common/storage-service-encryption.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) per File di Azure è in genere disponibile in tutte le aree e nei cloud pubblici e nazionali. Per abilitare la crittografia lato server per File di Azure, è possibile usare il [portale di Azure](https://portal.azure.com/), l'[API del provider di risorse di Archiviazione di Microsoft Azure](/rest/api/storagerp/storageaccounts), [Azure PowerShell](https://msdn.microsoft.com/library/azure/mt607151.aspx) o l'[interfaccia della riga di comando di Azure](../common/storage-azure-cli.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).
    
    Dopo aver abilitato la crittografia lato server in File di Azure, i nuovi dati scritti nell'archiviazione file nell'account di archiviazione vengono crittografati automaticamente. Questa funzionalità è disponibile per tutti i nuovi dati scritti in condivisioni nuove o esistenti in un account di archiviazione nuovo o esistente. Non sono previsti costi aggiuntivi per questa funzionalità. Per altre informazioni, vedere [Come abilitare la crittografia lato server in File di Azure](../common/storage-service-encryption.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

* **D. L'autenticazione basata su Active Directory è supportata da File di Azure?**
   
    Al momento non è supportata l'autenticazione basata su Active Directory o sugli elenchi di controllo di accesso (ACL), ma è inclusa nell'elenco delle richieste di funzionalità. Per ora, per fornire l'autenticazione alla condivisione file vengono usate le chiavi dell'account di archiviazione di Azure. È disponibile una soluzione alternativa che prevede l'uso di firme di accesso condiviso tramite l'API REST o le librerie client. Le firme di accesso condiviso consentono di generare token con autorizzazioni specifiche che restano valide per un periodo di tempo definito. Ad esempio, è possibile generare un token con accesso di sola lettura a un determinato file con scadenza di 10 minuti. Tutti coloro che possiedono il token nel periodo in cui è valido hanno l'accesso in sola lettura a tale file nell'intervallo di 10 minuti.
   
    La firma di accesso condiviso è supportata solo tramite l'API REST o le librerie client. Quando si monta la condivisione file tramite il protocollo SMB, non è possibile usare una firma di accesso condiviso per delegare l'accesso al contenuto. 
    
* **D. Quali sono i criteri di conformità dei dati supportati per File di Azure?**

   L'esecuzione di File di Azure si basa sulla stessa architettura di archiviazione di altri servizi di Archiviazione di Azure e applica gli stessi criteri di conformità dei dati. Altre informazioni sulla conformità dei dati di archiviazione di Azure sono disponibili per il download; fare riferimento al [documento di Protezione dati di Microsoft Azure](http://go.microsoft.com/fwlink/?LinkID=398382&clcid=0x409).

## <a name="on-premises-access"></a>Accesso locale

* **D. È necessario usare Azure ExpressRoute per connettersi a File di Azure da una macchina virtuale locale?**
   
    No. Se non si ha ExpressRoute è comunque possibile accedere alla condivisione file dall'ambiente locale, purché la porta 445 (TCP in uscita) sia aperta per l'accesso a Internet. Tuttavia, è possibile usare ExpressRoute con File di Azure, se si vuole.

* **D. Come è possibile montare la condivisione file di Azure nel computer locale?** 
    
    Se la porta 445 (TCP in uscita) è aperta e il client supporta il protocollo SMB 3.0, ad esempio su Windows 10 o Windows Server 2012, è possibile montare la condivisione file usando il protocollo SMB. Contattare il provider di servizi Internet locale per sbloccare la porta. Nel frattempo è possibile visualizzare i file usando [Esplora archivi](../../vs-azure-tools-storage-explorer-files.md#view-a-file-shares-contents).


## <a name="billing-and-pricing"></a>Prezzi e fatturazione
* **D. Il traffico di rete tra una macchina virtuale di Azure e una condivisione file viene conteggiato come larghezza di banda esterna e addebitato alla sottoscrizione?**
   
    Se la condivisione file e la macchina virtuale si trovano nella stessa area di Azure, il traffico tra di esse è disponibile. Se si trovano in aree diverse, il traffico che viene scambiato viene addebitato come larghezza di banda esterna.

## <a name="backup"></a>Backup

* **D. Come eseguire il backup nella condivisione file di Azure**
    
    È possibile usare AzCopy, RoboCopy o uno strumento di backup di terze parti per eseguire il backup di una condivisione di file montati. È probabile che sia possibile acquisire snapshot delle condivisioni file in un prossimo futuro; sarà possibile usare questa funzionalità per eseguire il backup della condivisione file di Azure.

## <a name="performance"></a>Prestazioni

* **D. Quali sono i limiti di scalabilità di File di Azure?**
    Per informazioni sugli obiettivi di scalabilità e prestazioni di File di Azure, vedere [Obiettivi di scalabilità e prestazioni](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#scalability-targets-for-blobs-queues-tables-and-files).

* **D. Le prestazioni sono ridotte quando si prova a decomprimere i file in File di Azure. Cosa devo fare?**
    
    Per trasferire grandi quantità di file in File di Azure, è consigliabile usare AzCopy (Windows, anteprima per Linux/Unix) o Azure PowerShell che sono strumenti ottimizzati per il trasferimento in rete.

* **D. Quali patch sono state rilasciate per risolvere il problema delle prestazioni ridotte con File di Azure?**
    
    Il team di Windows ha recentemente rilasciato una patch per risolvere un problema di prestazioni ridotte quando il cliente accede a File di Azure da Windows 8.1 o Windows Server 2012 R2. Per altre informazioni, vedere l'articolo della Knowledge Base associato, [Rallentamento delle prestazioni quando si accede a File di Azure da Windows 8.1 o Windows Server 2012 R2](https://support.microsoft.com/kb/3114025).

## <a name="features-and-interoperability-with-other-services"></a>Funzionalità e interoperabilità con altri servizi
* **D. Il controllo di condivisione file per un cluster di failover è uno dei casi d'uso per File di Azure?**
   
    Questa opzione non è attualmente supportata.

* **D. Esiste un'operazione di ridenominazione nell'API REST?**
   
    Attualmente non è possibile.

* **D. È possibile usare condivisioni annidate, ovvero una condivisione all'interno di un'altra condivisione?**
    
    No. La condivisione file è il driver virtuale che è possibile montare, quindi le condivisioni annidate non sono supportate.

* **D. Uso di File di Azure con IBM MQ**
    
    IBM ha rilasciato un documento per guidare i clienti di IBM MQ nella configurazione di File di Azure con tale servizio. Per altre informazioni, vedere la pagina relativa alla [How to setup IBM MQ Multi instance queue manager with Microsoft Azure File Service](https://github.com/ibm-messaging/mq-azure/wiki/How-to-setup-IBM-MQ-Multi-instance-queue-manager-with-Microsoft-Azure-File-Service)(Configurazione di IBM MQ Multi Instance Queue Manager (MIQM) con il servizio file di Microsoft Azure).


## <a name="troubleshooting"></a>Risoluzione dei problemi
* **D. Come risolvere i problemi causati dagli errori di File di Azure?**
    
    È anche possibile fare riferimento all'articolo sulla [risoluzione dei problemi di archiviazione file di Azure](storage-troubleshoot-windows-file-connection-problems.md) per indicazioni sulla risoluzione dei problemi end-to-end. 


## <a name="see-also"></a>Vedere anche
Per altre informazioni su File di Azure, vedere i collegamenti seguenti.

### <a name="conceptual-articles-and-videos"></a>Articoli concettuali e video
* [Azure Files: a frictionless cloud SMB file system for Windows and Linux](https://azure.microsoft.com/documentation/videos/azurecon-2015-azure-files-storage-a-frictionless-cloud-smb-file-system-for-windows-and-linux/) (File di Azure: un file system SMB nel cloud senza conflitti per Windows e Linux)
* [Come usare File di Azure con Linux](storage-how-to-use-files-linux.md)

### <a name="tooling-support-for-file-storage"></a>Supporto degli strumenti per Archiviazione file
* [Come usare AzCopy con Archiviazione di Microsoft Azure](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)
* [Utilizzo dell'interfaccia della riga di comando di Azure con archiviazione di Azure](../common/storage-azure-cli.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)
* [Risoluzione dei problemi di File di Azure](storage-troubleshoot-linux-file-connection-problems.md)

### <a name="blog-posts"></a>Post di BLOG
* [Azure Files is now generally available](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/) (File di Azure è ora disponibile a livello generale)
* [Inside Azure Files](https://azure.microsoft.com/blog/inside-azure-file-storage/) (Analisi di File di Azure)
* [Introduzione al servizio File di Microsoft Azure](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx)
* [Migrating data to Azure Files](https://azure.microsoft.com/blog/migrating-data-to-microsoft-azure-files/) (Migrazione dei dati in File di Azure)

### <a name="reference"></a>riferimento
* [Informazioni di riferimento sulla libreria client di archiviazione per .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [Riferimento API REST del servizio File](http://msdn.microsoft.com/library/azure/dn167006.aspx)


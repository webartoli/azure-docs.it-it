---
title: Caricare un file VHD in Azure DevTest Labs usando AzCopy | Microsoft Docs
description: Caricare un file VHD nell'account di archiviazione del lab usando AzCopy
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2017
ms.author: tarcher
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: a4f43354740d9f17570932b0b9c753f46d67dc33
ms.contentlocale: it-it
ms.lasthandoff: 08/21/2017

---

# <a name="upload-vhd-file-to-labs-storage-account-using-azcopy"></a>Caricare un file VHD nell'account di archiviazione del lab usando AzCopy

[!INCLUDE [devtest-lab-upload-vhd-selector](../../includes/devtest-lab-upload-vhd-selector.md)]

In Azure DevTest Labs è possibile usare i file VHD per creare immagini personalizzate da utilizzare per il provisioning di macchine virtuali. La procedura seguente illustra come usare l'utilità della riga di comando AzCopy per caricare un file VHD nell'account di archiviazione di un lab. Dopo avere caricato il file VHD, vedere la [sezione Passaggi successivi](#next-steps) per un elenco di articoli che illustrano come creare un'immagine personalizzata dal file VHD caricato. Per altre informazioni sui dischi e sui dischi rigidi virtuali in Azure, vedere [Informazioni sui dischi e sui dischi rigidi virtuali per le macchine virtuali](../virtual-machines/linux/about-disks-and-vhds.md)

> [!NOTE] 
>  
> AzCopy è un'utilità della riga di comando disponibile solo in Windows.

## <a name="step-by-step-instructions"></a>Istruzioni dettagliate

La procedura seguente illustra come caricare un file VHD in Azure DevTest Labs usando [AzCopy](http://aka.ms/downloadazcopy). 

1. Ottenere il nome dell'account di archiviazione del lab tramite il portale di Azure:

1. Accedere al [portale di Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Selezionare **Altri servizi** e quindi **DevTest Labs** dall'elenco.

1. Nell'elenco dei lab selezionare il lab desiderato.  

1. Nel pannello del lab selezionare **Configurazione**. 

1. Nel pannello **Configurazione** del lab selezionare **Immagini personalizzate (dischi rigidi virtuali)**.

1. Nel pannello **Immagini personalizzate** selezionare **+Aggiungi**. 

1. Nel pannello **Immagine personalizzata** selezionare **VHD**.

1. Nel pannello **VHD** selezionare l'opzione **Carica un file VHD con PowerShell**.

    ![Carica un file VHD con PowerShell](./media/devtest-lab-upload-vhd-using-azcopy/upload-image-using-psh.png)

1. Nel pannello **Carica un'immagine con PowerShell** è visualizzata una chiamata al cmdlet **Add-AzureVhd**. Il primo parametro (*Destination*) contiene l'URI per un contenitore BLOB (*uploads*) nel formato seguente:

    ```
    https://<STORAGE-ACCOUNT-NAME>.blob.core.windows.net/uploads/...
    ``` 

1. Prendere nota dell'URI completo in quanto verrà usato nei passaggi successivi.

1. Caricare il file VHD usando AzCopy:
 
1. [Scaricare e installare la versione più recente di AzCopy](http://aka.ms/downloadazcopy).

1. Aprire una finestra di comando e passare alla directory di installazione di AzCopy. Se lo si desidera, è possibile aggiungere il percorso di installazione di AzCopy al percorso di sistema. Per impostazione predefinita, AzCopy viene installato nella directory seguente:

    ```command-line
    %ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy
    ```

1. Usando la chiave dell'account di archiviazione e l'URI del contenitore BLOB, eseguire il comando seguente nel prompt dei comandi. Il valore *vhdFileName* deve essere racchiuso tra virgolette. Il processo di caricamento di un file VHD può richiedere molto tempo a seconda delle dimensioni del file VHD e della velocità della connessione.   

    ```command-line
    AzCopy /Source:<sourceDirectory> /Dest:<blobContainerUri> /DestKey:<storageAccountKey> /Pattern:"<vhdFileName>" /BlobType:page
    ```

## <a name="next-steps"></a>Passaggi successivi

- [Creare un'immagine personalizzata in Azure DevTest Labs da un file VHD usando il portale di Azure](devtest-lab-create-template.md)
- [Creare un'immagine personalizzata in Azure DevTest Labs da un file VHD usando PowerShell](devtest-lab-create-custom-image-from-vhd-using-powershell.md)

---
title: Creare una macchina virtuale nel portale di Azure | Documentazione Microsoft
description: Creare una macchina virtuale di Windows nel portale di Azure.
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 1871f823-ebd7-4eff-9a22-8e2411555595
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: cynthn
ms.translationtype: Human Translation
ms.sourcegitcommit: 07584294e4ae592a026c0d5890686eaf0b99431f
ms.openlocfilehash: 0981872ff819fdf49a9cc97afce3c212013ce76b
ms.contentlocale: it-it
ms.lasthandoff: 06/01/2017

---
# <a name="create-a-virtual-machine-running-windows-in-the-azure-portal"></a>Creare una macchina virtuale con Windows nel portale di Azure
> [!div class="op_single_selector"]
> * [Portale di Azure](tutorial.md)
> * [PowerShell: distribuzione classica](create-powershell.md)
>
>

<br>

> [!IMPORTANT]
> Azure offre due diversi modelli di distribuzione per creare e usare le risorse: [Gestione risorse e la distribuzione classica](../../../resource-manager-deployment-model.md). Questo articolo illustra l'uso del modello di distribuzione classica. Microsoft consiglia di usare il modello di Gestione risorse per le distribuzioni più recenti. Informazioni su come [eseguire questa procedura con il modello di distribuzione Resource Manager](../../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) tramite il **portale di Azure**.

Questa esercitazione illustra come creare una macchina virtuale (VM) di Azure che esegue Windows nel portale di Azure. Si utilizzerà un'immagine di Windows Server come esempio, ma questa è solo una delle molte immagini disponibili in Azure. Si noti che le opzioni dell’immagine dipendono dalla sottoscrizione. Ad esempio, i sottoscrittori MSDN possono usufruire di immagini desktop Windows.

Questa sezione illustra come usare il **dashboard** nel portale di Azure per selezionare e quindi creare la macchina virtuale.

È inoltre possibile creare VM usando le [proprie immagini](createupload-vhd.md). Per altre informazioni su questo e altri metodi, vedere [Diversi modi per creare una macchina virtuale Windows](../../virtual-machines-windows-creation-choices.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

<!-- 02/27/2017 Video removed as it was based on the classic portal. -->

## <a id="createvirtualmachine"> </a>Creare la macchina virtuale
[!INCLUDE [virtual-machines-create-WindowsVM](../../../../includes/virtual-machines-create-windowsvm.md)]

## <a name="next-steps"></a>Passaggi successivi
* Informazioni su come [creare una VM con il modello di distribuzione Resource Manager](../../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) nel portale di Azure.
* Accedere alla macchina virtuale. Per istruzioni, vedere [Accedere a una macchina virtuale di Windows tramite il portale di Azure classico](connect-logon.md).
* Collegare un disco per archiviare i dati. È possibile collegare sia dischi vuoti sia dischi contenenti dati. Per istruzioni, vedere [Collegare un disco dati da una macchina virtuale di Windows creata con il modello di distribuzione classico](attach-disk.md).


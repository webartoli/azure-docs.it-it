---
title: "Come assegnare all'Identità del servizio gestito l'accesso a una risorsa di Azure tramite il portale di Azure"
description: "Istruzioni dettagliate per assegnare a un'Identità del servizio gestito in una risorsa l'accesso a un'altra risorsa, tramite il portale di Azure."
services: active-directory
documentationcenter: 
author: bryanla
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/14/2017
ms.author: bryanla
ms.translationtype: HT
ms.sourcegitcommit: 47ba7c7004ecf68f4a112ddf391eb645851ca1fb
ms.openlocfilehash: 9dd02c8d7580cd9233e192f807686b7857ccf696
ms.contentlocale: it-it
ms.lasthandoff: 09/14/2017

---

# <a name="assign-a-managed-service-identity-msi-access-to-a-resource-using-the-azure-portal"></a>Assegnare a un'Identità del servizio gestito, ovvero MSI, l'accesso a una risorsa tramite il portale di Azure

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]

Dopo aver configurato una risorsa di Azure con un'Identità del servizio gestito, è possibile concedere a un'Identità del servizio gestito l'accesso a un'altra risorsa, analogamente a qualsiasi entità di sicurezza. In questo esempio viene illustrato come concedere a un'Identità del servizio gestito della macchina virtuale di Azure l'accesso a un account di archiviazione di Azure, mediante il portale di Azure.

## <a name="prerequisites"></a>Prerequisiti

[!INCLUDE [msi-qs-configure-prereqs](../../includes/msi-qs-configure-prereqs.md)]

## <a name="use-rbac-to-assign-the-msi-access-to-another-resource"></a>Usare il controllo degli accessi in base al ruolo per assegnare a un'Identità del servizio gestito l'accesso a un'altra risorsa

Dopo aver abilitato l'Identità del servizio gestito in una risorsa di Azure, [ad esempio in una macchina virtuale di Azure](msi-qs-configure-portal-windows-vm.md):

1. Accedere al [portale di Azure](https://portal.azure.com) usando un account associato alla sottoscrizione di Azure in cui si desidera configurare l'Identità del servizio gestito.

2. Passare alla risorsa desiderata in cui si desidera modificare il controllo di accesso. In questo esempio, viene concesso l'accesso alla macchina virtuale di Azure in un account di archiviazione. Quindi si passa all'account di archiviazione.

3. Fare clic sulla pagina "Controllo di accesso (IAM)" della risorsa e quindi su "+ Aggiungi". Specificare quindi il ruolo, assegnare l'accesso a una "macchina virtuale" e specificare il gruppo di risorse e la sottoscrizione corrispondente in cui risiede la risorsa. Nell'area per i criteri di ricerca, verrà visualizzata la risorsa. Selezionare la risorsa e premere "Salva": 

   ![Schermata del Controllo di accesso (IAM)](./media/msi-howto-assign-access-portal/assign-access-control-iam-blade-before.png)  

4. Si apre di nuovo la pagina principale "Controllo di accesso (IAM)", in cui viene visualizzata una nuova voce per l'Identità del servizio gestito della risorsa. In questo esempio, alla macchina virtuale "SimpleWinVM" del gruppo di risorse di dimostrazione è stato concesso l'accesso come "Collaboratore" all'account di archiviazione:

   ![Schermata del Controllo di accesso (IAM)](./media/msi-howto-assign-access-portal/assign-access-control-iam-blade-after.png)

## <a name="troubleshooting"></a>Risoluzione dei problemi

Se l'Identità del servizio gestito per la risorsa non viene visualizzata nell'elenco delle identità disponibili, verificare che sia stata abilitata correttamente. In questo caso, si torna alla macchina virtuale di Azure e:

- Si esamina la pagina "Configurazione" e si verifica che l'Identità del servizio gestito sia attivata = "Sì".
- Si esamina la pagina "Estensioni" e si verifica che l'estensione dell'Identità del servizio gestito sia stata distribuita correttamente.

Se una delle due opzioni è errata, è necessario ridistribuire l'Identità del servizio gestito nella risorsa o risolvere il problema di distribuzione.

## <a name="related-content"></a>Contenuti correlati

- Per una panoramica dell'Identità del servizio gestito, vedere [Panoramica dell'Identità del servizio gestito](msi-overview.md).
- Per abilitare l'Identità del servizio gestito in una macchina virtuale di Azure, vedere [Configurare un'Identità del servizio gestito della macchina virtuale di Azure mediante il portale di Azure](msi-qs-configure-portal-windows-vm.md).




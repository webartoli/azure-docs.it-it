---
title: Connettersi ad Azure Analysis Services con Excel | Microsoft Docs
description: Informazioni su come connettersi a un server di Azure Analysis Services usando Excel.
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/15/2017
ms.author: owend
ms.translationtype: Human Translation
ms.sourcegitcommit: 43aab8d52e854636f7ea2ff3aae50d7827735cc7
ms.openlocfilehash: ff96602642c56a3cd02aeada056c059573707731
ms.contentlocale: it-it
ms.lasthandoff: 06/03/2017

---
# <a name="connect-with-excel"></a>Connettersi con Excel

Dopo aver creato un server in Azure e aver distribuito un modello tabulare nel server, è possibile connettersi e iniziare l'esplorazione dei dati.


## <a name="connect-in-excel"></a>Connettersi in Excel

La connessione a un server in Excel è supportata tramite Dati in Excel 2016. La connessione tramite l'Importazione guidata tabella in Power Pivot non è supportata. 

**Per connettersi in Excel 2016**

1. In Excel 2016, sulla barra multifunzione **Dati**, fare clic su **Recupera dati esterni** > **Da altre origini** > **Da Analysis Services**.

2. Nella Connessione guidata dati, in **Nome Server**, immettere il nome del server, inclusi il protocollo e l'URI. In **Credenziali di accesso** selezionare **Usa nome utente e password seguenti** e quindi digitare il nome utente dell'organizzazione, ad esempio nancy@adventureworks.com, e la password.

    ![Connettersi dall'accesso a Excel](./media/analysis-services-connect-excel/aas-connect-excel-logon.png)

3. In **Seleziona database e tabella** selezionare il database e il modello o la prospettiva e quindi fare clic su **Fine**.
   
    ![Connettersi dal modello di selezione in Excel](./media/analysis-services-connect-excel/aas-connect-excel-select.png)


## <a name="see-also"></a>Vedere anche
[Librerie client](analysis-services-data-providers.md)   
[Gestire il server](analysis-services-manage.md)     




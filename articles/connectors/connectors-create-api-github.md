---
title: Connettore GitHub per App per la logica di Azure | Microsoft Docs
description: "Creare app per la logica in Servizio app di Azure. GitHub è un servizio di hosting di repository Git basato sul Web. Offre tutte le funzionalità di Git per il controllo delle revisioni distribuite e la gestione del codice sorgente , oltre a diverse funzionalità aggiuntive."
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 8f873e6c-f4c0-4c2e-a5bd-2e953efe5e2b
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 08/18/2016
ms.author: mandia; ladocs
ms.translationtype: Human Translation
ms.sourcegitcommit: c785ad8dbfa427d69501f5f142ef40a2d3530f9e
ms.openlocfilehash: d4614b0ceff0ec0d36dbb1a136551f985f2fc1a1
ms.contentlocale: it-it
ms.lasthandoff: 05/26/2017

---
# <a name="get-started-with-the-github-connector"></a>Introduzione al connettore GitHub
GitHub è un servizio di hosting di repository Git basato sul Web. Offre tutte le funzionalità di Git per il controllo delle revisioni distribuite e la gestione del codice sorgente , oltre a diverse funzionalità aggiuntive.

Per iniziare subito a creare un'app per la logica, vedere [Creare un'app per la logica](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="create-a-connection-to-github"></a>Creare una connessione a GitHub
Per creare app per la logica con GitHub, è prima necessario creare una **connessione** e quindi indicare i dettagli per le proprietà seguenti: 

| Proprietà | Obbligatorio | Descrizione |
| --- | --- | --- |
| Token |Sì |Fornisce le credenziali per GitHub |

Dopo aver creato la connessione, è possibile usarla per eseguire le azioni e restare in ascolto dei trigger descritti in questo articolo. 

> [!INCLUDE [Steps to create a connection to GitHub](../../includes/connectors-create-api-github.md)]
> 

## <a name="connector-specific-details"></a>Dettagli specifici del connettore

Per visualizzare eventuali azioni e trigger definiti in Swagger ed eventuali limiti, vedere i [dettagli del connettore](/connectors/github/).

## <a name="more-connectors"></a>Altri connettori
Tornare all' [elenco di API](apis-list.md).

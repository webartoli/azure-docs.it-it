---
title: Concetti relativi a Griglia di eventi di Azure
description: Vengono descritti il servizio Griglia di eventi di Azure e i concetti correlati. Vengono definiti diversi componenti chiave di Griglia di eventi.
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 09/18/2017
ms.author: babanisa
ms.translationtype: HT
ms.sourcegitcommit: 1868e5fd0427a5e1b1eeed244c80a570a39eb6a9
ms.openlocfilehash: 5b69478bf00284594b984fde452f6bed4e73859b
ms.contentlocale: it-it
ms.lasthandoff: 09/19/2017

---

# <a name="concepts-in-azure-event-grid"></a>Concetti di Griglia di eventi di Azure

I concetti principali di Griglia di eventi di Azure sono i seguenti:

## <a name="events"></a>Events

Un evento è la quantità minima di informazioni che descrive in modo completo qualcosa che si è verificato nel sistema.  Ogni evento ha informazioni comuni, come: l'origine dell'evento, l'ora in cui l'evento si è verificato e un identificatore univoco.  Ogni evento ha anche informazioni specifiche rilevanti solo per l'evento specifico. Un evento di creazione di un nuovo file in Archiviazione di Azure contiene ad esempio i dettagli sul file, come il valore lastTimeModified. Un evento di riavvio di una macchina virtuale contiene invece il nome della macchina virtuale e il motivo del riavvio. Ogni evento è limitato a 64 KB di dati.

## <a name="event-sourcespublishers"></a>Origini/autori di eventi

Un'origine evento è la posizione in cui si verifica l'evento. Archiviazione di Azure è ad esempio l'origine degli eventi di creazione di BLOB. L'infrastruttura di VM di Azure è l'origine degli eventi relativi alle macchine virtuali. Le origini evento sono responsabili della pubblicazione degli eventi in Griglia di eventi.

## <a name="topics"></a>Argomenti

Gli autori classificano gli eventi in argomenti. L'argomento include un endpoint in cui l'autore invia gli eventi. Per rispondere a determinati tipi di eventi, i sottoscrittori scelgono gli argomenti da sottoscrivere. Gli argomenti forniscono anche uno schema di eventi, in modo che i sottoscrittori possano capire come utilizzare gli eventi in modo appropriato.

Gli argomenti di sistema sono argomenti predefiniti forniti dai servizi di Azure. Gli argomenti personalizzati sono argomenti di applicazioni e di terze parti.

## <a name="event-subscriptions"></a>Sottoscrizioni di eventi

Una sottoscrizione indica a Griglia di eventi gli eventi relativi a un argomento che il sottoscrittore è interessato a ricevere.  Una sottoscrizione contiene anche informazioni su come recapitare gli eventi al sottoscrittore.

## <a name="event-handlers"></a>Gestori eventi

Dal punto di vista di Griglia di eventi, un gestore eventi è la posizione in cui l'evento viene inviato. Il gestore esegue altre azioni per elaborare l'evento.  Griglia di eventi supporta diversi tipi di sottoscrittori. A seconda del tipo di sottoscrittore, Griglia di eventi segue meccanismi diversi per garantire il recapito dell'evento.  Per i gestori eventi webhook HTTP, l'evento viene ripetuto fino a quando il gestore non restituisce un codice di stato `200 – OK`. Per la coda di Archiviazione di Azure, gli eventi vengono ripetuti fino a quando il servizio di accodamento non riesce a elaborare correttamente il push del messaggio nella coda.

## <a name="filters"></a>Filtri

Quando si sottoscrive un argomento, è possibile filtrare gli eventi inviati all'endpoint. È possibile filtrare in base a tipo di evento o criterio di oggetto. Per altre informazioni, vedere [Schema di sottoscrizione per Griglia di eventi](subscription-creation-schema.md).

## <a name="security"></a>Sicurezza

L'evento fornisce la sicurezza per la sottoscrizione degli argomenti e la pubblicazione degli argomenti. Quando si esegue la sottoscrizione, è necessario avere le autorizzazioni appropriate per la risorsa o l'argomento. Quando si esegue la pubblicazione, è necessario avere un token di firma di accesso condiviso o l'autenticazione con chiave per l'argomento. Per altre informazioni, vedere [Event Grid security and authentication](security-authentication.md) (Sicurezza e autenticazione di Griglia di eventi).

## <a name="failed-delivery"></a>Recapito non riuscito

Quando Griglia di eventi non è in grado di confermare che un evento è stato ricevuto dall'endpoint del sottoscrittore, esegue di nuovo il recapito dell'evento. Per altre informazioni, vedere [Recapito di messaggi di Griglia di eventi e nuovi tentativi](delivery-and-retry.md).

## <a name="next-steps"></a>Passaggi successivi

* Per un'introduzione a Griglia di eventi, vedere [Informazioni su Griglia di eventi](overview.md).
* Per iniziare rapidamente a usare Griglia di eventi, vedere [Creare e instradare eventi personalizzati con Griglia di eventi di Azure](custom-event-quickstart.md).

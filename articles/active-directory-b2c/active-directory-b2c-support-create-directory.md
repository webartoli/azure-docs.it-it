---
title: 'Azure Active Directory B2C: Risoluzione dei problemi per la creazione di tenant | Documentazione Microsoft'
description: Problemi e soluzioni per la creazione di un tenant di Azure Active Directory o di un tenant di Azure Active Directory B2C.
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: 7ba4c6b2-161b-45b5-b3bd-ccb662f5d7a0
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.translationtype: Human Translation
ms.sourcegitcommit: 857267f46f6a2d545fc402ebf3a12f21c62ecd21
ms.openlocfilehash: 81af4536fc223319369aff262d42149cfbf1a1e9
ms.contentlocale: it-it
ms.lasthandoff: 06/28/2017

---
# <a name="troubleshoot-creating-an-azure-active-directory-or-azure-active-directory-b2c-tenant"></a>Risoluzione dei problemi per la creazione di un tenant di Azure Active Directory o di un tenant di Azure Active Directory B2C 

## <a name="create-an-azure-ad-tenant"></a>Come creare un tenant di Azure Active Directory
Se non è possibile creare un tenant Azure Active Directory (Azure AD) al primo tentativo, riprovare. Se il problema persiste, contattare il supporto di Azure.

## <a name="create-an-azure-ad-b2c-tenant"></a>Creare un tenant di Azure AD B2C
Se si verificano problemi quando si [crea un tenant di Azure Active Directory B2C (Azure AD B2C)](active-directory-b2c-get-started.md), provare le seguenti opzioni:

* Se il tenant di Azure AD B2C non viene visualizzato nell'elenco di tenant, riprovare a creare il tenant.
* Se il tenant di Azure Active Directory B2C viene visualizzato nell'elenco di tenant e compare il seguente messaggio di errore, eliminare il tenant e crearlo di nuovo:

    "Impossibile completare la creazione del tenant B2C 'contosob2c'. Fare clic su questo [collegamento](http://go.microsoft.com/fwlink/?LinkID=624192&clcid=0x409) per maggiori informazioni. "
* Si verificano problemi noti quando si elimina un tenant Azure AD B2C esistente e lo si crea nuovamente utilizzando lo stesso nome di dominio. Quando si crea un nuovo tenant di Azure AD B2C, è necessario utilizzare un nome di dominio diverso.
* Se nessuna di queste soluzioni risolve il problema, contattare il supporto di Azure. Per maggiori informazioni, vedere [Presentare richieste di supporto per Azure AD B2C](active-directory-b2c-support.md).



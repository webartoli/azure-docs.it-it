---
title: Endpoint del servizio e regole della rete virtuale per il database SQL di Azure | Microsoft Docs
description: Contrassegnare una subnet come endpoint del servizio Rete virtuale. Contrassegnare quindi l'endpoint come regola della rete virtuale per ACL nel database SQL di Azure. Il database SQL accetta quindi la comunicazione da tutte le macchine virtuali e altri nodi nella subnet.
services: sql-database
documentationcenter: 
author: MightyPen
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: VNet Service endpoints
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 09/15/2017
ms.author: genemi
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: eb409b6e5cb0f6bfbf6bfa8103c01482abf928cf
ms.contentlocale: it-it
ms.lasthandoff: 09/25/2017

---
# <a name="use-virtual-network-service-endpoints-and-rules-for-azure-sql-database"></a>Usare gli endpoint del servizio Rete virtuale e le regole per il database SQL di Azure

Le *regole della rete virtuale* di Microsoft Azure rappresentano una funzionalità firewall che consente di verificare se il server del database SQL di Azure accetta le comunicazioni inviate da subnet specifiche in reti virtuali. Questo articolo spiega i motivi per cui la funzione delle regole della rete virtuale rappresenti a volte la scelta ideale per consentire le comunicazioni con il database SQL di Azure in modo sicuro.

#### <a name="how-to-create-a-virtual-network-rule"></a>Come creare una regola della rete virtuale

Se si crea solo una regola della rete virtuale, è possibile procedere direttamente ai passaggi e alla spiegazione [più avanti in questo articolo](#anchor-how-to-by-using-firewall-portal-59j).






<a name="anch-terminology-and-description-82f" />

## <a name="terminology-and-description"></a>Terminologia e descrizione

**Rete virtuale:** è possibile associare reti virtuali alla sottoscrizione di Azure.

**Subnet:** una rete virtuale contiene **subnet**. Le macchine virtuali (VM) di Azure esistenti vengono assegnate a subnet. Una subnet può contenere varie VM o altri nodi di calcolo. I nodi di calcolo esterni alla rete virtuale non possono accedervi, a meno che non si configuri la sicurezza in modo da consentirne l'accesso.

**Endpoint del servizio Rete virtuale:** un endpoint del servizio Rete virtuale è una subnet in cui i valori di proprietà includono uno o più nomi formali di tipi di servizi Azure. Questo articolo è incentrato sul nome del tipo **Microsoft.Sql**, che fa riferimento al servizio Azure denominato Database SQL.

**Regola della rete virtuale:** una regola della rete virtuale per il server di database SQL è una subnet presente nell'elenco di controllo di accesso (ACL) del server di database SQL. Per essere nell'elenco ACL del database SQL, la subnet deve contenere il nome del tipo **Microsoft.Sql**.

Una regola della rete virtuale indica al server di database SQL di accettare le comunicazioni da ogni nodo che si trova nella subnet.







<a name="anch-benefits-of-a-vnet-rule-68b" />

## <a name="benefits-of-a-virtual-network-rule"></a>Vantaggi di una regola di rete virtuale

Fino a quando non si interviene, le macchine virtuali nelle subnet non possono comunicare con il database SQL. La base logica per la scelta dell'approccio delle regole di rete virtuale finalizzato a consentire le comunicazioni richiede una discussione di confronto riguardo le opzioni di sicurezza concorrenti offerte dal firewall.

#### <a name="a-allow-access-to-azure-services"></a>R. Possibilità di accedere ai servizi di Azure

Il riquadro del firewall contiene un pulsante **ON/OFF** etichettato **Consenti l'accesso a Servizi di Azure**. L'impostazione **ON** consente le comunicazioni da tutti gli indirizzi IP di Azure e tutte le subnet di Azure. Questi indirizzi IP o subnet di Azure potrebbero non essere di proprietà dell'utente. Questa impostazione **ON** è probabilmente più aperta rispetto al livello desiderato per il database SQL. La funzione delle regole della rete virtuale offre un controllo molto più granulare.

#### <a name="b-ip-rules"></a>B. Regole IP

Il firewall del database SQL consente di specificare gli intervalli di indirizzi IP da cui vengono accettate le comunicazioni nel database SQL. Questo approccio è ideale per gli indirizzi IP stabili esterni alla rete privata di Azure, ma molti nodi all'interno della rete privata di Azure sono configurati con indirizzi IP *dinamici*. Gli indirizzi IP dinamici potrebbero cambiare, ad esempio al riavvio di una macchina virtuale. Sarebbe inutile specificare un indirizzo IP dinamico in una regola del firewall, in un ambiente di produzione.

È possibile recuperare l'opzione IP ottenendo un indirizzo IP *statico* per la macchina virtuale. Per i dettagli, vedere [Configurare indirizzi IP privati per una VM mediante il portale di Azure][vm-configure-private-ip-addresses-for-a-virtual-machine-using-the-azure-portal-321w].

Tuttavia, l'approccio IP statico può diventare difficile da gestire ed è dispendioso a livello di scalabilità. Le regole della rete virtuale sono più semplici da creare e gestire.

#### <a name="c-cannot-yet-have-sql-database-on-a-subnet"></a>C. Database SQL non ancora disponibile in una subnet

Se il server di database SQL di Azure è un nodo in una subnet nella rete virtuale, tutti i nodi all'interno della rete virtuale possono comunicare con il database SQL. In questo caso le macchine virtuali sono in grado di comunicare con il database SQL senza aver bisogno di regole di rete virtuale o IP.

Tuttavia, a partire da settembre 2017, il database SQL di Azure non è ancora tra i servizi che possono essere assegnati a una subnet.






<a name="anch-details-about-vnet-rules-38q" />

## <a name="details-about-virtual-network-rules"></a>Dettagli sulle regole della rete virtuale

Questa sezione illustra alcuni dettagli sulle regole della rete virtuale.

#### <a name="only-one-geographic-region"></a>Una sola area geografica

Un endpoint del servizio Rete virtuale si applica a una sola area di Azure. L'endpoint non consente ad altre aree di accettare le comunicazioni dalla subnet.

Ogni regola della rete virtuale è limitata all'area a cui si applica il relativo endpoint sottostante.

#### <a name="server-level-not-database-level"></a>A livello di server, non a livello di database

Ogni regola di rete virtuale si applica all'intero server di database SQL di Azure, non solo a un determinato database nel server. In altre parole, la regola della rete virtuale si applica a livello di server, non a livello di database.

- Al contrario, le regole IP possono essere applicate a qualsiasi livello.

#### <a name="security-administration-roles"></a>Ruoli di amministrazione di sicurezza

I ruoli di sicurezza sono distinti nell'amministrazione degli endpoint del servizio Rete virtuale. Ogni ruolo indicato di seguito deve svolgere determinate azioni:

- **Amministratore di rete:** &nbsp; attivare l'endpoint.
- **Amministratore di database:** &nbsp; aggiornare l'elenco di controllo di accesso (ACL) per aggiungere la subnet specificata al server di database SQL.

*Alternativa del controllo degli accessi in base al ruolo:* 

I ruoli di amministratore di rete e amministratore di database hanno più funzionalità di quelle necessarie a gestire le regole della rete virtuale. È necessario solo un subset delle relative funzionalità.

È possibile scegliere di usare il [controllo degli accessi in base al ruolo (RBAC)] [ rbac-what-is-813s] in Azure per creare un singolo ruolo personalizzato che contiene solo il subset necessario di funzionalità. È possibile usare il ruolo personalizzato anziché coinvolgere l'amministratore di rete o di database. La superficie di attacco dell'esposizione a rischi per la sicurezza è ridotta se si aggiunge un utente a un ruolo personalizzato, anziché aggiungere l'utente agli altri due ruoli di amministratore principali.

#### <a name="limitations"></a>Limitazioni

La funzionalità delle regole della rete virtuale presenta le limitazioni seguenti:

- Ogni server di database SQL di Azure può includere fino a 128 voci ACL di IP per qualsiasi rete virtuale specificata.

- Le regole della rete virtuale si applicano solo alle reti virtuali di Azure Resource Manager e non alle reti con un [modello di distribuzione classica][arm-deployment-model-568f].

- Le regole della rete virtuale non si estendono agli elementi di rete seguenti:
    - In locale mediante [Expressroute][expressroute-indexmd-744v]
    - [VPN (rete privata virtuale) da sito a sito (S2S)][vpn-gateway-indexmd-608y]


<!--
FYI: Re ARM, 'Azure Service Management (ASM)' was the old name of 'classic deployment model'.
When searching for blogs about ASM, you probably need to use this old and now-forbidden name.
-->





<a name="anchor-how-to-by-using-firewall-portal-59j" />

## <a name="how-to-create-a-virtual-network-rule-by-using-the-portal"></a>Come creare una regola della rete virtuale tramite il portale

Questa sezione illustra come usare il [portale di Azure] [http-azure-portal-link-ref-477t] per creare una *regola della rete virtuale* nel database di SQL Azure. La regola indica al database SQL di accettare le comunicazioni da una subnet specifica contrassegnata come *endpoint del servizio Rete virtuale*.

#### <a name="powershell-alternative"></a>Alternativa PowerShell

Anche uno script di PowerShell può creare regole della rete virtuale. Ad esempio, il cmdlet essenziale **New-AzureRmSqlServerVirtualNetworkRule**. Se interessati, vedere [Usare PowerShell per creare un endpoint del servizio virtuale e una regola per il database SQL di Azure][sql-db-vnet-service-endpoint-rule-powershell-md-52d].

#### <a name="prerequisites"></a>Prerequisiti

È necessario avere già una subnet contrassegnata con lo specifico *nome del tipo* di endpoint del servizio Rete virtuale pertinente per il database SQL di Azure.

- Il nome del tipo di endpoint pertinente è **Microsoft.Sql**.
- Se la subnet non è contrassegnata con il nome del tipo, vedere [Verificare che la subnet sia un endpoint][sql-db-vnet-service-endpoint-rule-powershell-md-a-verify-subnet-is-endpoint-ps-100].

<a name="a-portal-steps-for-vnet-rule-200" />

### <a name="azure-portal-steps"></a>Procedure del portale di Azure

1. Accedere al [Portale di Azure][http-azure-portal-link-ref-477t].

2. Nel portale passare a **SQL Server** &gt; **Firewall/Reti virtuali (anteprima)**.

3. Impostare il controllo **Consenti l'accesso a Servizi di Azure** su OFF.

    > [!IMPORTANT]
    > Se si lascia il controllo impostato su ON, il server di database SQL di Azure accetta le comunicazioni da qualsiasi subnet, con il rischio di un eccesso di accessi dal punto di vista della sicurezza. La funzionalità di endpoint del servizio Rete virtuale di Microsoft Azure, in sinergia con la funzionalità delle regole della rete virtuale del database SQL, consente di ridurre la superficie di attacco per la sicurezza.

4. Fare clic sul controllo **+ Aggiungi esistenti** nella sezione **Reti virtuali**.

    ![Fare clic su Aggiungi esistenti (endpoint della subnet, come regola SQL).][image-portal-firewall-vnet-add-existing-10-png]

5. Nel nuovo riquadro **Crea/Aggiorna** compilare i controlli con i nomi delle risorse di Azure.
 
    > [!TIP]
    > È necessario includere il **prefisso dell'indirizzo** corretto per la subnet. Il valore è disponibile nel portale. Passare a **Tutte le risorse** &gt; **Tutti i tipi** &gt; **Reti virtuali**. Il filtro visualizza le reti virtuali. Fare clic sulla rete virtuale desiderata e quindi su **Subnet**. La colonna **INTERVALLO DI INDIRIZZI** contiene il prefisso dell'indirizzo desiderato.

    ![Compilare i campi per la nuova regola.][image-portal-firewall-create-update-vnet-rule-20-png]

6. Fare clic sul pulsante **OK** nella parte inferiore del riquadro.

7.  Osservare la regola della rete virtuale risultante nel riquadro del firewall.

    ![Osservare la nuova regola nel riquadro del firewall.][image-portal-firewall-vnet-result-rule-30-png]






<a name="anchor-how-to-links-60h" />

## <a name="related-articles"></a>Articoli correlati

- [Usare PowerShell per creare un endpoint del servizio virtuale e una regola per il database SQL di Azure][sql-db-vnet-service-endpoint-rule-powershell-md-52d]
- [Regole firewall a livello di server e di database per il database SQL di Azure][sql-db-firewall-rules-config-715d]

La funzionalità di endpoint del servizio Rete virtuale di Microsoft Azure e la funzionalità di regole della rete virtuale per il database SQL di Azure sono entrambe disponibili a partire dalla fine di settembre 2017.





<!-- Link references, to images. -->

[image-portal-firewall-vnet-add-existing-10-png]: media/sql-database-vnet-service-endpoint-rule-overview/portal-firewall-vnet-add-existing-10.png

[image-portal-firewall-create-update-vnet-rule-20-png]: media/sql-database-vnet-service-endpoint-rule-overview/portal-firewall-create-update-vnet-rule-20.png

[image-portal-firewall-vnet-result-rule-30-png]: media/sql-database-vnet-service-endpoint-rule-overview/portal-firewall-vnet-result-rule-30.png



<!-- Link references, to text, Within this same Github repo. -->

[arm-deployment-model-568f]: ../azure-resource-manager/resource-manager-deployment-model.md#classic-deployment-characteristics

[expressroute-indexmd-744v]: ../expressroute/index.md

[rbac-what-is-813s]: ../active-directory/role-based-access-control-what-is.md

[sql-db-firewall-rules-config-715d]: sql-database-firewall-configure.md

[sql-db-vnet-service-endpoint-rule-powershell-md-52d]: sql-database-vnet-service-endpoint-rule-powershell.md

[sql-db-vnet-service-endpoint-rule-powershell-md-a-verify-subnet-is-endpoint-ps-100]: sql-database-vnet-service-endpoint-rule-powershell.md#a-verify-subnet-is-endpoint-ps-100

[vm-configure-private-ip-addresses-for-a-virtual-machine-using-the-azure-portal-321w]: ../virtual-network/virtual-networks-static-private-ip-arm-pportal.md

[vpn-gateway-indexmd-608y]: ../vpn-gateway/index.md



<!-- Link references, to text, Outside this Github repo (HTTP). -->

[http-azure-portal-link-ref-477t]: https://portal.azure.com/




<!-- ??2
#### Syntax related articles

- PowerShell cmdlets

- REST API Reference, including JSON

- Azure CLI

- ARM templates
-->



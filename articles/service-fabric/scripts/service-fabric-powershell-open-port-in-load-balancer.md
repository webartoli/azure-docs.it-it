---
title: Esempio di script di Azure PowerShell - Aprire una porta dell'applicazione nel servizio di bilanciamento del carico | Microsoft Docs
description: Esempio di script di Azure PowerShell - Aprire una porta nel servizio di bilanciamento del carico di Azure per un'applicazione Service Fabric.
services: service-fabric
documentationcenter: 
author: rwike77
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 08/15/2017
ms.author: ryanwi
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: 368589509b163cacf495fd0be893a8953fe2066e
ms.openlocfilehash: 3adc7360b0b61ce69786a990c87f5a36f827ad2b
ms.contentlocale: it-it
ms.lasthandoff: 08/17/2017

---

# <a name="open-an-application-port-in-the-azure-load-balancer"></a>Aprire una porta dell'applicazione nel servizio di bilanciamento del carico di Azure

Il servizio di bilanciamento del carico di Azure è supportato da un'applicazione Service Fabric eseguita in Azure. Questo script di esempio apre una porta in un servizio di bilanciamento del carico di Azure in modo che un'applicazione Service Fabric possa comunicare con client esterni. Personalizzare i parametri in base alle esigenze. 

Se necessario, installare il modulo PowerShell in Service Fabric con il [Service Fabric SDK](../service-fabric-get-started.md). 

## <a name="sample-script"></a>Script di esempio

[!code-powershell[main](../../../powershell_scripts/service-fabric/open-port-in-load-balancer/open-port-in-load-balancer.ps1 "Aprire una porta nel servizio di bilanciamento del carico")]

## <a name="script-explanation"></a>Spiegazione dello script

Questo script usa i comandi seguenti. Ogni comando della tabella include collegamenti alla documentazione specifica del comando.

| Comando | Note |
|---|---|
| [Get-AzureRmResource](/powershell/module/azurerm.resources/get-azurermresource) | Ottiene una risorsa di Azure.  |
| [Get-AzureRmLoadBalancer](/powershell/module/azurerm.network/get-azurermloadbalancer) | Ottiene il servizio di bilanciamento del carico di Azure. |
| [Add-AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/add-azurermloadbalancerprobeconfig) | Aggiunge la configurazione di una porta probe per un servizio di bilanciamento del carico.|
| [Get-AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/get-azurermloadbalancerprobeconfig) | Ottiene la configurazione di un probe per un servizio di bilanciamento del carico. |
| [Add-AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/add-azurermloadbalancerruleconfig) | Aggiunge la configurazione di una regola a un servizio di bilanciamento del carico. |
| [Set-AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer) | Imposta lo stato dell'obiettivo per un servizio di bilanciamento del carico. |

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sul modulo Azure PowerShell, vedere la [documentazione di Azure PowerShell](/powershell/azure/overview).

Altri esempi di PowerShell per Azure Service Fabric sono disponibili in [Esempi di Azure PowerShell](../service-fabric-powershell-samples.md).


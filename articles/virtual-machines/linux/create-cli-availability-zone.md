---
title: Creare una VM Linux suddivisa in zone con l'interfaccia della riga di comando di Azure | Microsoft Docs
description: "Creare una VM Linux in una zona di disponibilità con l'interfaccia della riga di comando di Azure"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: dlepow
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/19/2017
ms.author: danlep
ms.custom: 
ms.translationtype: HT
ms.sourcegitcommit: c863dbdcb242bdea208f7e72f6c1f61b5ba04844
ms.openlocfilehash: 232c2cf1ba0a7de23da10357de9a6e6ad9a0d41d
ms.contentlocale: it-it
ms.lasthandoff: 09/22/2017

---

# <a name="create-a-linux-virtual-machine-in-an-availability-zone-with-the-azure-cli"></a>Creare una macchina virtuale Linux in una zona di disponibilità con l'interfaccia della riga di comando di Azure

Questo articolo descrive come usare l'interfaccia della riga di comando di Azure per creare una VM Linux in una zona di disponibilità di Azure. Una [zona di disponibilità](../../availability-zones/az-overview.md) è una zona fisicamente separata in un'area di Azure. Usare le zone di disponibilità per proteggere app e dati da un poco probabile errore o perdita di un intero data center.

[!INCLUDE [availability-zones-preview-statement.md](../../../includes/availability-zones-preview-statement.md)]

Verificare di avere installato la versione più recente dell'[interfaccia della riga di comando di Azure 2.0](/cli/azure/install-az-cli2) e di avere eseguito l'accesso a un account di Azure con [az login](/cli/azure/#login).

## <a name="create-resource-group"></a>Creare un gruppo di risorse

Creare un gruppo di risorse con il comando [az group create](/cli/azure/group#az_group_create).  

Un gruppo di risorse di Azure è un contenitore logico in cui le risorse di Azure vengono distribuite e gestite. Il gruppo di risorse deve essere creato prima della macchina virtuale. In questo esempio viene creato un gruppo di risorse denominato *myResourceGroupVM* nell'area *eastus2*. Stati Uniti orientali 2 è una delle aree di Azure che supportano le zone di disponibilità in anteprima.

```azurecli-interactive 
az group create --name myResourceGroupVM --location eastus2
```

Il gruppo di risorse viene specificato quando si crea o si modifica una VM, come viene illustrato in questo articolo.

## <a name="create-virtual-machine"></a>Crea macchina virtuale

Crea una macchina virtuale usando il comando [az vm create](/cli/azure/vm#az_vm_create). 

Per la creazione di una macchina virtuale sono disponibili diverse opzioni, ad esempio l'immagine del sistema operativo, il ridimensionamento del disco e le credenziali amministrative. In questo esempio viene creata una macchina virtuale denominata *myVM* che esegue Ubuntu Server. La VM viene creata nella zona disponibilità *1*. Per impostazione predefinita, la VM viene creata nella dimensione *Standard_DS1_v2*. La dimensione è supportata nell'anteprima delle aree di disponibilità.

```azurecli-interactive 
az vm create --resource-group myResourceGroupVM --name myVM --image UbuntuLTS --generate-ssh-keys --zone 1
```

La creazione della macchina virtuale può richiedere alcuni minuti. Dopo la creazione della macchina virtuale, l'interfaccia della riga di comando di Azure restituisce informazioni sulla VM. Annotare il valore `zones` che indica la zona di disponibilità in cui è in esecuzione la VM. 

```azurecli-interactive 
{
  "fqdns": "",
  "id": "/subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroupVM/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus2",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "52.174.34.95",
  "resourceGroup": "myResourceGroupVM",
  "zones": "1"
}
```

## <a name="zone-for-ip-address-and-managed-disk"></a>Zona per l'indirizzo IP e il disco gestito

Quando la VM è distribuita in una zona di disponibilità, l'indirizzo IP e le risorse del disco gestito vengono distribuite nella stessa zona di disponibilità. Gli esempi seguenti ottengono informazioni su queste risorse.

Utilizzare innanzitutto il comando [az vm list-ip-addresses](/cli/azure/vm#az_vm_list_ip_addresses) per restituire il nome della risorsa con indirizzo IP pubblico in *myVM*. In questo esempio il nome è archiviato in una variabile che viene usata in un passaggio successivo.

```azurecli-interactive
ipaddressname=$(az vm list-ip-addresses -g myResourceGroupVM -n myVM --query "[].virtualMachine.network.publicIpAddresses[].name" -o tsv)
```

Ora è possibile ottenere informazioni sull'indirizzo IP:

```azurecli-interactive
az network public-ip show --resource-group myResourceGroupVM --name $ipaddressname
```

L'output indica che l'indirizzo IP si trova nella stessa zona di disponibilità della VM:

```azurecli-interactive
{
  "dnsSettings": null,
  "etag": "W/\"b7ad25eb-3191-4c8f-9cec-c5e4a3a37d35\"",
  "id": "/subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroupVM/providers/Microsoft.Network/publicIPAddresses/myVMPublicIP",
  "idleTimeoutInMinutes": 4,
  "ipAddress": "52.174.34.95",
  "ipConfiguration": {
    "etag": null,
    "id": "/subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroupVM/providers/Microsoft.Network/networkInterfaces/myVMVMNic/ipConfigurations/ipconfigmyVM",
    "name": null,
    "privateIpAddress": null,
    "privateIpAllocationMethod": null,
    "provisioningState": null,
    "publicIpAddress": null,
    "resourceGroup": "myResourceGroupVM",
    "subnet": null
  },
  "location": "eastUS2",
  "name": "myVMPublicIP",
  "provisioningState": "Succeeded",
  "publicIpAddressVersion": "IPv4",
  "publicIpAllocationMethod": "Dynamic",
  "resourceGroup": "myResourceGroupVM",
  "resourceGuid": "8c70a073-09be-4504-0000-000000000000",
  "tags": {},
  "type": "Microsoft.Network/publicIPAddresses",
  "zones": [
    "1"
  ]
}
```

Analogamente, verificare che il disco gestito della VM si trovi nella zona di disponibilità. Usare il comando [az vm show](/cli/azure/vm#az_vm_show) per ottenere l'ID del disco. In questo esempio l'ID del disco viene archiviato in una variabile che viene usata in un passaggio successivo. 

```azurecli-interactive
osdiskname=$(az vm show -g myResourceGroupVM -n myVM --query "storageProfile.osDisk.name" -o tsv)
```
Ora è possibile ottenere informazioni sul disco gestito:

```azurecli-interactive
az disk show --resource-group myResourceGroupVM --name $osdiskname
```

L'output indica che il disco gestito si trova nella stessa zona di disponibilità della VM:

```azurecli-interactive
{
  "creationData": {
    "createOption": "FromImage",
    "imageReference": {
      "id": "/Subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/Providers/Microsoft.Compute/Locations/westeurope/Publishers/Canonical/ArtifactTypes/VMImage/Offers/UbuntuServer/Skus/16.04-LTS/Versions/latest",
      "lun": null
    },
    "sourceResourceId": null,
    "sourceUri": null,
    "storageAccountId": null
  },
  "diskSizeGb": 30,
  "encryptionSettings": null,
  "id": "/subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroupVM/providers/Microsoft.Compute/disks/osdisk_761c570dab",
  "location": "eastus2",
  "managedBy": "/subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroupVM/providers/Microsoft.Compute/virtualMachines/myVM",
  "name": "osdisk_761c570dab",
  "osType": "Linux",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroupVM",
  "sku": {
    "name": "Premium_LRS",
    "tier": "Premium"
  },
  "tags": {},
  "timeCreated": "2017-09-05T22:16:06.892752+00:00",
  "type": "Microsoft.Compute/disks",
  "zones": [
    "1"
  ]
}
```
 


## <a name="next-steps"></a>Passaggi successivi

In questo articolo è stato illustrato come creare una VM in una zona di disponibilità. Per altre informazioni, vedere [Aree e disponibilità](regions-and-availability.md) per le VM di Azure.






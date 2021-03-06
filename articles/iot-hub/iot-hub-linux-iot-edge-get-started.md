---
title: Introduzione ad Azure IoT Edge (Linux) | Microsoft Docs
description: Come creare un gateway in un computer Linux e ottenere informazioni sui concetti chiave in Azure IoT Edge, ad esempio moduli e file di configurazione di JSON.
services: iot-hub
documentationcenter: 
author: chipalost
manager: timlt
editor: 
ms.assetid: cf537bdd-2352-4bb1-96cd-a283fcd3d6cf
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/19/2017
ms.author: andbuc
ms.custom: H1Hack27Feb2017
ms.translationtype: HT
ms.sourcegitcommit: 4f77c7a615aaf5f87c0b260321f45a4e7129f339
ms.openlocfilehash: e2f26c1abe09feff77e1e2633d8bfcf4ca527aee
ms.contentlocale: it-it
ms.lasthandoff: 09/22/2017

---
# <a name="explore-azure-iot-edge-architecture-on-linux"></a>Esplorare l'architettura di Azure IoT Edge in Linux

[!INCLUDE [iot-hub-iot-edge-getstarted-selector](../../includes/iot-hub-iot-edge-getstarted-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-linux](../../includes/iot-hub-iot-edge-install-build-linux.md)]

## <a name="how-to-run-the-sample"></a>Per eseguire l'esempio

Lo script **build.sh** genera l'output nella cartella **build** nella copia locale del repository **iot-edge**. Questo output include anche i due moduli di IoT Edge usati in questo esempio.

Lo script di compilazione scrive **liblogger.so** nella cartella **build/modules/logger/** e **libhello\_world.so** nella cartella **build/modules/hello_world/**. Usare questi percorsi per il valore **module path**, come illustrato nel file di impostazioni JSON di esempio seguente.

Il processo hello\_world\_sample accetta il percorso di un file di configurazione JSON come argomento della riga di comando. Il file JSON di esempio seguente è disponibile nel repository dell'SDK in **samples/hello\_world/src/hello\_world\_lin.json**. Questo file di configurazione funziona così com'è, a meno che non si modifichi lo script di compilazione per inserire moduli di IoT Edge o file eseguibili di esempio in percorsi non predefiniti.

> [!NOTE]
> I percorsi dei moduli sono relativi alla directory di lavoro corrente da cui viene avviato l'eseguibile hello\_world\_sample, non alla directory in cui si trova il file eseguibile. Per impostazione predefinita, il file di configurazione JSON di esempio prevede la scrittura del file "log.txt" nella directory di lavoro corrente.

```json
{
    "modules" :
    [
        {
            "name" : "logger",
            "loader": {
            "name": "native",
            "entrypoint": {
                "module.path": "./modules/logger/liblogger.so"
            }
            },
            "args" : {"filename":"log.txt"}
        },
        {
            "name" : "hello_world",
            "loader": {
            "name": "native",
            "entrypoint": {
                "module.path": "./modules/hello_world/libhello_world.so"
            }
            },
            "args" : null
        }
    ],
    "links":
    [
        {
            "source": "hello_world",
            "sink": "logger"
        }
    ]
}
```

1. Accedere alla cartella **build** nella radice della copia locale dell'archivio **iot-edge**.

1. Eseguire il comando seguente:

    ```sh
    ./samples/hello_world/hello_world_sample ../samples/hello_world/src/hello_world_lin.json`
    ```

[!INCLUDE [iot-hub-iot-edge-getstarted-code](../../includes/iot-hub-iot-edge-getstarted-code.md)]



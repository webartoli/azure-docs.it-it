---
title: Connect a Raspberry Pi to Azure IoT Suite using C with simulated telemetry (Connettere Raspberry Pi ad Azure IoT Suite usando C con telemetria simulata) | Documentazione Microsoft
description: Usare lo Starter Kit di Microsoft Azure IoT per Raspberry Pi 3 e Azure IoT Suite. Usare C per la connessione di Raspberry Pi alla soluzione di monitoraggio remoto, per inviare dati di telemetria simulata al cloud e per rispondere ai metodi richiamati dal dashboard della soluzione.
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-suite
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: dobett
ms.translationtype: Human Translation
ms.sourcegitcommit: be3ac7755934bca00190db6e21b6527c91a77ec2
ms.openlocfilehash: 522fa6a45521ab917a540e8cc6bc619b05ab262e
ms.contentlocale: it-it
ms.lasthandoff: 05/03/2017

---
# <a name="connect-your-raspberry-pi-3-to-the-remote-monitoring-solution-and-send-simulated-telemetry-using-c"></a>Connettere Raspberry Pi 3 alla soluzione di monitoraggio remoto e inviare la telemetria simulata usando C

[!INCLUDE [iot-suite-raspberry-pi-kit-selector](../../includes/iot-suite-raspberry-pi-kit-selector.md)]

Questa esercitazione illustra come usare Raspberry Pi 3 per simulare i dati relativi alla temperatura e all'umidità da inviare al cloud. L'esercitazione usa:

- Il sistema operativo Raspbian, il linguaggio di programmazione C e Microsoft Azure IoT SDK per C per implementare un dispositivo di esempio.
- La soluzione preconfigurata di monitoraggio remoto IoT Suite come back-end basato su cloud.

[!INCLUDE [iot-suite-raspberry-pi-kit-overview-simulator](../../includes/iot-suite-raspberry-pi-kit-overview-simulator.md)]

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> La soluzione di monitoraggio remota esegue il provisioning di un set di servizi di Azure nella sottoscrizione di Azure. La distribuzione riflette un'architettura enterprise reale. Per evitare costi di consumo di Azure non necessari, eliminare l'istanza della soluzione preconfigurata in azureiotsuite.com al completamento. Se la soluzione preconfigurata occorre nuovamente, è possibile ricrearla facilmente. Per altre informazioni sulla riduzione dei consumi durante l'esecuzione della soluzione di monitoraggio remoto, vedere [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config] (Configurazione di soluzioni preconfigurate di Azure IoT Suite per scopi dimostrativi).

[!INCLUDE [iot-suite-raspberry-pi-kit-view-solution](../../includes/iot-suite-raspberry-pi-kit-view-solution.md)]

[!INCLUDE [iot-suite-raspberry-pi-kit-prepare-pi-simulator](../../includes/iot-suite-raspberry-pi-kit-prepare-pi-simulator.md)]

## <a name="download-and-configure-the-sample"></a>Scaricare e configurare l'esempio

È ora possibile scaricare e configurare l'applicazione client di monitoraggio remoto su Raspberry Pi.

### <a name="clone-the-repositories"></a>Clonare i repository

Se non è già stato fatto, clonare i repository necessari eseguendo i comandi seguenti in un terminale in Pi:

```sh
cd ~
git clone --recursive https://github.com/Azure-Samples/iot-remote-monitoring-c-raspberrypi-getstartedkit.git
```

### <a name="update-the-device-connection-string"></a>Aggiornare la stringa di connessione del dispositivo

Aprire il file origine di esempio nell'editor **nano** usando il comando seguente:

```sh
nano ~/iot-remote-monitoring-c-raspberrypi-getstartedkit/simulator/remote_monitoring/remote_monitoring.c
```

Individuare le righe seguenti:

```c
static const char* deviceId = "[Device Id]";
static const char* connectionString = "HostName=[IoTHub Name].azure-devices.net;DeviceId=[Device Id];SharedAccessKey=[Device Key]";
```

Sostituire i valori segnaposto con il dispositivo e le informazioni dell'hub IoT creati e salvati all'inizio di questa esercitazione. Salvare le modifiche (**CTRL+O**, **INVIO**) e chiudere l'editor (**CTRL+X**).

## <a name="build-the-sample"></a>Compilare l'esempio

Installare i pacchetti di prerequisiti per Azure IoT SDK per dispositivi di Microsoft per C eseguendo i comandi seguenti in un terminale di Raspberry Pi:

```sh
sudo apt-get update
sudo apt-get install g++ make cmake git libcurl4-openssl-dev libssl-dev uuid-dev
```

È ora possibile compilare la soluzione di esempio aggiornata su Raspberry Pi:

```sh
chmod +x ~/iot-remote-monitoring-c-raspberrypi-getstartedkit/simulator/build.sh
~/iot-remote-monitoring-c-raspberrypi-getstartedkit/simulator/build.sh
```

È ora possibile eseguire il programma di esempio su Raspberry Pi. Immettere il comando:

```sh
sudo ~/cmake/remote_monitoring/remote_monitoring
```

L'output seguente riporta un esempio dell'output visualizzato al prompt dei comandi su Raspberry Pi:

![Output dell'app Raspberry Pi][img-raspberry-output]

Premere **CTRL+C** per uscire dal programma in qualsiasi momento.

[!INCLUDE [iot-suite-raspberry-pi-kit-view-telemetry-simulator](../../includes/iot-suite-raspberry-pi-kit-view-telemetry-simulator.md)]

## <a name="next-steps"></a>Passaggi successivi

Visitare il [Centro per sviluppatori Azure IoT](https://azure.microsoft.com/develop/iot/) per altri esempi e documentazione su Azure IoT.

[img-raspberry-output]: ./media/iot-suite-raspberry-pi-kit-c-get-started-simulator/appoutput.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md


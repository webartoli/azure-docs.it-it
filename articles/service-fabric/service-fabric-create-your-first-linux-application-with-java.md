---
title: Creare un'applicazione Java Reliable Actors di Azure Service Fabric in Linux | Microsoft Docs
description: Informazioni su come creare e distribuire un'applicazione Java Reliable Actors di Service Fabric in cinque minuti.
services: service-fabric
documentationcenter: java
author: rwike77
manager: timlt
editor: 
ms.assetid: 02b51f11-5d78-4c54-bb68-8e128677783e
ms.service: service-fabric
ms.devlang: java
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/20/2017
ms.author: ryanwi
ms.translationtype: HT
ms.sourcegitcommit: a29f1e7b39b7f35073aa5aa6c6bd964ffaa6ffd0
ms.openlocfilehash: 68f9492231d367b1ede6ab032ec1c66c75150957
ms.contentlocale: it-it
ms.lasthandoff: 09/21/2017

---
# <a name="create-your-first-java-service-fabric-reliable-actors-application-on-linux"></a>Creare la prima applicazione Java Reliable Actors di Service Fabric in Linux
> [!div class="op_single_selector"]
> * [C# - Windows](service-fabric-create-your-first-application-in-visual-studio.md)
> * [Java - Linux](service-fabric-create-your-first-linux-application-with-java.md)
> * [C# - Linux](service-fabric-create-your-first-linux-application-with-csharp.md)
>
>

Questa guida introduttiva consente di creare in pochi minuti la prima applicazione Java di Azure Service Fabric in un ambiente di sviluppo Linux.  Al termine, si avrà una semplice applicazione Java con un singolo servizio in esecuzione nel cluster di sviluppo locale.  

## <a name="prerequisites"></a>Prerequisiti
Prima di iniziare, installare Service Fabric SDK e l'interfaccia della riga di comando di Service Fabric e configurare un cluster di sviluppo nell'[ambiente di sviluppo Linux](service-fabric-get-started-linux.md). Se si usa Mac OS X, è possibile [configurare un ambiente di sviluppo Linux in una macchina virtuale usando Vagrant](service-fabric-get-started-mac.md).

È consigliabile installare anche l'[interfaccia della riga di comando di Service Fabric](service-fabric-cli.md).

### <a name="install-and-set-up-the-generators-for-java"></a>Installare e configurare i generatori per Java
Service Fabric offre gli strumenti di scaffolding che consentono di creare un'applicazione Java di Service Fabric dal terminale tramite il generatore di modelli Yeoman. Seguire questa procedura per assicurarsi che nella macchina virtuale sia disponibile il generatore di modelli Yeoman di Service Fabric per Java.
1. Installare nodejs e NPM nella macchina virtuale

  ```bash
  sudo apt-get install npm
  sudo apt install nodejs-legacy
  ```
2. Installare il generatore di modelli [Yeoman](http://yeoman.io/) nella macchina virtuale da NPM

  ```bash
  sudo npm install -g yo
  ```
3. Installare il generatore di applicazioni Java Yeo di Service Fabric da NPM

  ```bash
  sudo npm install -g generator-azuresfjava
  ```

## <a name="create-the-application"></a>Creazione dell'applicazione
Un'applicazione di Service Fabric contiene uno o più servizi, ognuno dei quali contribuisce alle funzionalità dell'applicazione con un ruolo specifico. Il generatore installato nell'ultima sezione semplifica la creazione del primo servizio e l'aggiunta di altri servizi in un secondo momento.  È anche possibile creare, compilare e distribuire applicazioni Java di Service Fabric usando un plug-in per Eclipse. Vedere [Creare e distribuire la prima applicazione Java usando Eclipse](service-fabric-get-started-eclipse.md). Per questa guida introduttiva, usare Yeoman per creare un'applicazione con un unico servizio per archiviare e ottenere un valore del contatore.

1. In un terminale digitare ``yo azuresfjava``.
2. Assegnare un nome all'applicazione.
3. Scegliere il tipo del primo servizio e assegnargli un nome. Per questa esercitazione, scegliere un servizio Reliable Actor. Per altre informazioni sugli altri tipi di servizi, vedere [Panoramica dei modelli di programmazione di Service Fabric](service-fabric-choose-framework.md).
   ![Generatore Yeoman di Service Fabric per Java][sf-yeoman]

## <a name="build-the-application"></a>Compilare l'applicazione.
I modelli Yeoman di Service Fabric includono uno script di compilazione per [Gradle](https://gradle.org/), che può essere usato per compilare l'applicazione dal terminale.
Le dipendenze Java di Service Fabric vengono recuperate da Maven. Per compilare e usare le applicazioni Java di Service Fabric, è necessario verificare che siano installati JDK e Gradle. Se non sono ancora installati, è possibile eseguire questi comandi per installare JDK (openjdk-8-jdk) e Gradle:

  ```bash
  sudo apt-get install openjdk-8-jdk-headless
  sudo apt-get install gradle
  ```

Per compilare l'applicazione e creare il pacchetto, eseguire questo comando:

  ```bash
  cd myapp
  gradle
  ```

## <a name="deploy-the-application"></a>Distribuire l'applicazione
Dopo aver compilato l'applicazione, è possibile distribuirla nel cluster locale.

1. Connettersi al cluster locale di Service Fabric.

    ```bash
    sfctl cluster select --endpoint http://localhost:19080
    ```

2. Eseguire lo script di installazione messo a disposizione nel modello per copiare il pacchetto dell'applicazione nell'archivio immagini del cluster, registrare il tipo di applicazione e creare un'istanza dell'applicazione.

    ```bash
    ./install.sh
    ```

La distribuzione dell'applicazione compilata è uguale a quella di qualsiasi altra applicazione di Service Fabric. Per istruzioni dettagliate, vedere la documentazione sulla [gestione di un'applicazione di Service Fabric con l'interfaccia della riga di comando di Service Fabric](service-fabric-application-lifecycle-sfctl.md).

I parametri per questi comandi si trovano nei manifesti generati nel pacchetto dell'applicazione.

Dopo la distribuzione dell'applicazione, aprire un browser e passare a [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) all'indirizzo [http://localhost:19080/Explorer](http://localhost:19080/Explorer).
Espandere quindi il nodo **Applicazioni**, nel quale sarà ora presente una voce per il tipo di applicazione e un'altra per la prima istanza del tipo.

## <a name="start-the-test-client-and-perform-a-failover"></a>Avviare il client di test ed eseguire un failover
Gli attori non eseguono alcuna operazione in modo indipendente, ma richiedono un altro servizio o client per l'invio dei messaggi. Il modello Actor include un semplice script di test che è possibile usare per interagire con il servizio Actor.

1. Eseguire lo script tramite l'utilità delle espressioni di controllo per visualizzare l'output del servizio Actor.  Lo script di test chiama il metodo `setCountAsync()` nell'attore per incrementare un contatore, chiama il metodo `getCountAsync()` nell'attore per ottenere il nuovo valore del contatore e visualizza tale valore nella console.

    ```bash
    cd myactorsvcTestClient
    watch -n 1 ./testclient.sh
    ```

2. In Service Fabric Explorer individuare il nodo che ospita la replica primaria del servizio Actor. Nello screenshot seguente si tratta del nodo 3. La replica di servizi primaria gestisce le operazioni di lettura e scrittura.  Le modifiche allo stato del servizio vengono quindi replicate nelle repliche secondarie, in esecuzione nei nodi 0 e 1 nello screenshot seguente.

    ![Ricerca della replica primaria in Service Fabric Explorer][sfx-primary]

3. In **Nodi** fare clic sul nodo trovato nel passaggio precedente e quindi scegliere **Disattiva (riavvio)** dal menu Azioni. Questa azione consente di riavviare il nodo che esegue la replica di servizi primaria e di forzare il failover in una delle repliche secondarie in esecuzione in un altro nodo.  Questa replica secondaria viene alzata di livello a primaria, un'altra replica secondaria viene creata in un nodo diverso e la replica primaria inizia a eseguire operazioni di lettura/scrittura. Durante il riavvio del nodo, osservare l'output dal client di test e notare che l'incremento del contatore prosegue nonostante il failover.

## <a name="remove-the-application"></a>Rimuovere l'applicazione
Usare lo script di disinstallazione incluso nel modello per eliminare l'istanza dell'applicazione, annullare la registrazione del pacchetto dell'applicazione e rimuovere tale pacchetto dall'archivio immagini del cluster.

```bash
./uninstall.sh
```

In Service Fabric Explorer si noterà che l'applicazione e il tipo di applicazione non sono più visualizzati nel nodo **Applicazioni**.

## <a name="service-fabric-java-libraries-on-maven"></a>Librerie Java di Service Fabric in Maven
Le librerie Java di Service Fabric sono ospitate in Maven. È possibile aggiungere le dipendenze nel file ``pom.xml`` o ``build.gradle`` dei progetti per usare le librerie Java di Service Fabric da **mavenCentral**. 

### <a name="actors"></a>Actor

Supporto di Service Fabric Reliable Actors per l'applicazione.

  ```XML
  <dependency>
      <groupId>com.microsoft.servicefabric</groupId>
      <artifactId>sf-actors-preview</artifactId>
      <version>0.12.0</version>
  </dependency>
  ```

  ```gradle
  repositories {
      mavenCentral()
  }
  dependencies {
      compile 'com.microsoft.servicefabric:sf-actors-preview:0.12.0'
  }
  ```

### <a name="services"></a>Services

Supporto delle applicazioni Reliable Services di Service Fabric per l'applicazione in uso.

  ```XML
  <dependency>
      <groupId>com.microsoft.servicefabric</groupId>
      <artifactId>sf-services-preview</artifactId>
      <version>0.12.0</version>
  </dependency>
  ```

  ```gradle
  repositories {
      mavenCentral()
  }
  dependencies {
      compile 'com.microsoft.servicefabric:sf-services-preview:0.12.0'
  }
  ```

### <a name="others"></a>Altro
#### <a name="transport"></a>Trasporto

Supporto del livello trasporto per l'applicazione Java di Service Fabric. Non è necessario aggiungere esplicitamente questa dipendenza alle applicazioni Reliable Actors o Services, a meno che non si esegua la programmazione al livello trasporto.

  ```XML
  <dependency>
      <groupId>com.microsoft.servicefabric</groupId>
      <artifactId>sf-transport-preview</artifactId>
      <version>0.12.0</version>
  </dependency>
  ```

  ```gradle
  repositories {
      mavenCentral()
  }
  dependencies {
      compile 'com.microsoft.servicefabric:sf-transport-preview:0.12.0'
  }
  ```

#### <a name="fabric-support"></a>Supporto di Fabric

Supporto a livello di sistema per Service Fabric, che comunica con il runtime nativo di Service Fabric. Non è necessario aggiungere esplicitamente questa dipendenza alle applicazioni Reliable Actors o Services. Viene recuperata automaticamente da Maven quando si includono le altre dipendenze riportate sopra.

  ```XML
  <dependency>
      <groupId>com.microsoft.servicefabric</groupId>
      <artifactId>sf-preview</artifactId>
      <version>0.12.0</version>
  </dependency>
  ```

  ```gradle
  repositories {
      mavenCentral()
  }
  dependencies {
      compile 'com.microsoft.servicefabric:sf-preview:0.12.0'
  }
  ```

## <a name="migrating-old-service-fabric-java-applications-to-be-used-with-maven"></a>Migrazione di applicazioni Java di Service Fabric precedenti da usare con Maven
Le librerie Java di Service Fabric sono state recentemente spostate da Service Fabric Java SDK al repository di Maven. Benché le nuove applicazioni generate con Yeoman o Eclipse genereranno i progetti aggiornati più recenti (in grado di interagire con Maven), è possibile aggiornare le applicazioni Java esistenti di Service Fabric senza stato o actor, che in precedenza usavano Service Fabric Java SDK, in modo che usino le dipendenze Java di Service Fabric da Maven. Seguire la procedura riportata [qui](service-fabric-migrate-old-javaapp-to-use-maven.md) per assicurare che l'applicazione precedente funzioni con Maven.

## <a name="next-steps"></a>Passaggi successivi

* [Creare la prima applicazione Java di Service Fabric in Linux usando Eclipse](service-fabric-get-started-eclipse.md)
* [Altre informazioni su Reliable Actors](service-fabric-reliable-actors-introduction.md)
* [Interagire con un cluster di Service Fabric usando l'interfaccia della riga di comando di Service Fabric](service-fabric-cli.md)
* Informazioni sulle [opzioni di supporto di Service Fabric](service-fabric-support.md)
* [Introduzione all'interfaccia della riga di comando di Service Fabric](service-fabric-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-yeoman.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-java/sfx-primary.png
[sf-eclipse-templates]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-eclipse-templates.png


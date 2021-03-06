---
title: Supporto SSH per l'app Web per i contenitori del Servizio app di Azure | Microsoft Docs
description: Informazioni sull'uso di SSH con l'app Web di Azure per i contenitori.
keywords: Servizio app di Azure, app Web, Linux, OSS
services: app-service
documentationcenter: 
author: wesmc7777
manager: erikre
editor: 
ms.assetid: 66f9988f-8ffa-414a-9137-3a9b15a5573c
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/25/2017
ms.author: wesmc
ms.translationtype: HT
ms.sourcegitcommit: 8f9234fe1f33625685b66e1d0e0024469f54f95c
ms.openlocfilehash: 7ce9b23e8925d4c79827c7c4e8bec63067ce33e0
ms.contentlocale: it-it
ms.lasthandoff: 09/20/2017

---
# <a name="ssh-support-for-azure-web-app-for-containers"></a>Supporto SSH per l'app Web di Azure per i contenitori

[Secure Shell (SSH)](https://en.wikipedia.org/wiki/Secure_Shell) è un protocollo di rete di crittografia per l'uso di servizi di rete in modo sicuro. Più comunemente viene usato per accedere in modo sicuro a un sistema da una riga di comando ed eseguire comandi amministrativi in remoto.

L'app Web per i contenitori offre il supporto SSH nel contenitore di app con ognuna delle immagini Docker predefinite usate per lo stack di runtime di nuove app web. 

![Stack di runtime](./media/app-service-linux-ssh-support/app-service-linux-runtime-stack.png)

È anche possibile usare SSH con le immagini Docker personalizzate includendo il server SSH come parte dell'immagine e configurandolo come descritto in questo argomento.

## <a name="making-a-client-connection"></a>Creare una connessione client

Per creare una connessione client SSH, è necessario avviare il sito principale.

Incollare l'endpoint di Gestione controllo codice sorgente, SCM, per l'app Web nel browser tramite il modulo seguente:

```bash
https://<your sitename>.scm.azurewebsites.net/webssh/host
```

Se non lo si è già fatto, è necessario eseguire l'autenticazione con la sottoscrizione di Azure per la connessione.

![Connessione SSH](./media/app-service-linux-ssh-support/app-service-linux-ssh-connection.png)


## <a name="ssh-support-with-custom-docker-images"></a>Supporto SSH con immagini Docker personalizzate

Affinché un'immagine Docker personalizzata supporti la comunicazione SSH tra il contenitore e il client nel portale di Azure, eseguire la procedura seguente per l'immagine Docker.

Questa procedura viene mostrata nell'archivio del Servizio app di Azure come esempio [qui](https://github.com/Azure-App-Service/node/blob/master/6.9.3/).

1. Includere l'installazione `openssh-server` nell'[istruzione `RUN`](https://docs.docker.com/engine/reference/builder/#run) in Dockerfile per l'immagine e impostare la password per l'account radice su `"Docker!"`. 

    > [!NOTE]
    > Questa configurazione non consente connessioni esterne al contenitore. SSH è accessibile solo tramite il sito Kudu/sito, che viene autenticato tramite le credenziali di pubblicazione.
    
    ```docker
    # ------------------------
    # SSH Server support
    # ------------------------
    RUN apt-get update \
        && apt-get install -y --no-install-recommends openssh-server \
        && echo "root:Docker!" | chpasswd
    ```

1. Aggiungere un'[istruzione `COPY`](https://docs.docker.com/engine/reference/builder/#copy) a Dockerfile per copiare un file [sshd_config](http://man.openbsd.org/sshd_config) nella directory */etc/ssh/*. Il file di configurazione deve essere basato sul file sshd_config nell'archivio GitHub di Azure-App-Service [qui](https://github.com/Azure-App-Service/node/blob/master/6.11.0/sshd_config).

    > [!NOTE]
    > Per fare in modo che la connessione abbia esito positivo, il file *sshd_config* deve includere quanto segue: 
    > * `Ciphers` deve includere almeno uno dei seguenti: `aes128-cbc,3des-cbc,aes256-cbc`.
    > * `MACs` deve includere almeno uno dei seguenti: `hmac-sha1,hmac-sha1-96`.
    
    ```docker
    COPY sshd_config /etc/ssh/
    ```

1. Includere la porta 2222 nell'[istruzione `EXPOSE`](https://docs.docker.com/engine/reference/builder/#expose) per Dockerfile. Anche se la password radice è nota, la porta 2222 non è accessibile da Internet. È una porta interna accessibile solo dai contenitori all'interno della rete bridge di una rete virtuale privata.

    ```docker
    EXPOSE 2222 80
    ```

1. Assicurarsi di avviare il servizio SSH. Nell'esempio [qui](https://github.com/Azure-App-Service/node/blob/master/6.9.3/startup/init_container.sh) viene usato uno script della shell nella directory */bin*.

    ```bash
    #!/bin/bash
    service ssh start
    ```

Dockerfile usa l'[istruzione `CMD`](https://docs.docker.com/engine/reference/builder/#cmd) per eseguire lo script.

    ```docker
    COPY init_container.sh /bin/
    ...
    RUN chmod 755 /bin/init_container.sh
    ...
    CMD ["/bin/init_container.sh"]
    ```

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sull'app Web per i contenitori, vedere i collegamenti seguenti. È possibile pubblicare domande e dubbi nel [forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).

* [Come usare un'immagine Docker personalizzata per l'app Web di Azure per i contenitori](quickstart-custom-docker-image.md)
* [Uso di .NET Core nell'app Web di Azure per i contenitori](quickstart-dotnetcore.md)
* [Uso di Ruby nell'app Web di Azure per i contenitori](quickstart-ruby.md)
* [Azure App Service Web App for Containers FAQ (Domande frequenti sulle app Web per contenitori del servizio app di Azure)](app-service-linux-faq.md)

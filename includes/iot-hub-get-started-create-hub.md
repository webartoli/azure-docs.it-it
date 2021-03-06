## <a name="create-an-iot-hub"></a>Creare un hub IoT
Creare un hub IoT per connettere l'app per dispositivo simulato. La procedura seguente illustra come completare questa attività usando il portale di Azure.

1. Accedere al [portale di Azure][lnk-portal].
1. Nell'indice fare clic su **Nuovo** > **Internet delle cose** > **Hub IoT**.
   
    ![Indice del portale di Azure][1]
1. Nella finestra **Hub IoT** scegliere la configurazione per l'hub IoT.
   
    ![Finestra Hub IoT][2]
   
   1. Nella casella **Nome** immettere un nome per l'hub IoT. Se il **Nome** è valido e disponibile, appare un segno di spunta verde nella casella **Nome**.
    [!INCLUDE [iot-hub-pii-note-naming-hub](iot-hub-pii-note-naming-hub.md)]
   
   1. Selezionare un [piano tariffario e un livello di scalabilità][lnk-pricing]. Per questa esercitazione non è necessario un livello specifico. Per questa esercitazione, usare il livello F1 gratuito.
   1. In **Gruppo di risorse** creare un gruppo di risorse o selezionarne uno esistente. Per altre informazioni, vedere [Using resource groups to manage your Azure resources][lnk-resource-groups] (Uso di Gruppi di risorse per gestire le risorse di Azure).
   1. In **Percorso**selezionare il percorso per ospitare l'hub IoT. Per questa esercitazione, scegliere la località più vicina.
1. Dopo aver scelto le opzioni di configurazione dell'hub IoT, fare clic su **Crea**.  La creazione dell'hub IoT da parte di Azure può richiedere alcuni minuti. Per verificare lo stato, è possibile monitorare l'avanzamento nella Schermata iniziale o nel pannello Notifiche.
   
1. Dopo avere creato l'hub IoT, fare clic sul nuovo riquadro dell'hub IoT nel portale di Azure per aprire la finestra delle proprietà. Annotare il **Nome host**, quindi fare clic su **Criteri di accesso condiviso**.
   
    ![Finestra del nuovo hub IoT][4]
1. In **Criteri di accesso condivisi** fare clic sul criterio **iothubowner**, quindi copiare e annotare la stringa di connessione dell'hub IoT nella finestra **iothubowner**. Per altre informazioni, vedere [Controllo di accesso][lnk-access-control] nella "Guida per gli sviluppatori dell'hub IoT".
   
    ![Criteri di accesso condivisi][5]

<!-- Images. -->
[1]: ./media/iot-hub-get-started-create-hub/create-iot-hub1.png
[2]: ./media/iot-hub-get-started-create-hub/create-iot-hub2.png
[4]: ./media/iot-hub-get-started-create-hub/create-iot-hub4.png
[5]: ./media/iot-hub-get-started-create-hub/create-iot-hub5.png

<!-- Links -->
[lnk-resource-groups]: ../articles/azure-resource-manager/resource-group-portal.md
[lnk-portal]: https://portal.azure.com/
[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub/
[lnk-access-control]: ../articles/iot-hub/iot-hub-devguide-security.md

---
title: Caricare file dai dispositivi nell'hub IoT con .NET | Microsoft Docs
description: Come caricare file da un dispositivo al cloud usando Azure IoT SDK per dispositivi per .NET. I file caricati vengono salvati in un contenitore BLOB di archiviazione di Azure.
author: fsautomata
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: conceptual
ms.date: 07/04/2017
ms.author: elioda
ms.openlocfilehash: 25ec3a158d1eca77a7ca622af9b249789ef3b5e2
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51259327"
---
# <a name="upload-files-from-your-device-to-the-cloud-with-iot-hub-using-net"></a>Eseguire l'upload di file dal dispositivo al cloud con l'hub IoT usando .NET

[!INCLUDE [iot-hub-file-upload-language-selector](../../includes/iot-hub-file-upload-language-selector.md)]

Questa esercitazione si basa sul codice dell'esercitazione [Inviare messaggi da cloud a dispositivo con l'hub IoT](iot-hub-csharp-csharp-c2d.md) che illustra come usare le funzionalità di caricamento di file dell'hub IoT. Illustra le operazioni seguenti:

- Specificare in modo sicuro un dispositivo con un URI del BLOB di Azure per il caricamento di un file.

- Usare le notifiche di caricamento di file dell'hub IoT per attivare l'elaborazione del file nel back-end dell'app.

Gli articoli [Inviare dati di telemetria da un dispositivo a un hub IoT](quickstart-send-telemetry-dotnet.md) e [Inviare messaggi da cloud a dispositivo con l'hub IoT](iot-hub-csharp-csharp-c2d.md) illustrano le funzionalità di messaggistica di base da dispositivo a cloud e da cloud a dispositivo dell'hub IoT. L'esercitazione [Configurare il routing dei messaggi con l'IoT Hub](tutorial-routing.md) illustra come archiviare in modo affidabile i messaggi da dispositivo a cloud nell'archivio BLOB di Azure. Tuttavia in alcuni scenari non è possibile mappare facilmente i dati che i dispositivi inviano in messaggi relativamente ridotti da dispositivo a cloud, che l'hub IoT accetta. Ad esempio: 

* File di grandi dimensioni che contengono immagini
* Video
* Dati di vibrazione campionati ad alta frequenza
* Qualche tipo di dati pre-elaborati

Questi dati in genere vengono elaborati in batch nel cloud con strumenti come [Azure Data Factory](../data-factory/introduction.md) o lo stack [Hadoop](../hdinsight/index.yml). Quando è necessario caricare i file da un dispositivo, è comunque possibile usare la sicurezza e affidabilità dell'hub IoT.

Al termine di questa esercitazione vengono eseguite due app console .NET:

* **SimulatedDevice**, una versione modificata dell'applicazione creata nell'esercitazione [Inviare messaggi da cloud a dispositivo con l'hub IoT](iot-hub-csharp-csharp-c2d.md). Ciò consente di caricare un file nell'archivio tramite un URI con firma di accesso condiviso fornito dall'hub IoT.

* **ReadFileUploadNotification**riceve le notifiche di caricamento file dall'hub IoT.

> [!NOTE]
> L'hub IoT supporta numerose piattaforme e linguaggi (inclusi C, Java e Javascript) tramite gli Azure IoT SDK per dispositivi. Vedere il [Centro per sviluppatori di IoT di Azure](https://azure.microsoft.com/develop/iot) per istruzioni dettagliate su come connettere il dispositivo all'Hub IoT di Azure.

Per completare l'esercitazione, sono necessari gli elementi seguenti:

* Visual Studio 2017
* Un account Azure attivo. Se non si ha un account, è possibile crearne uno [gratuito](https://azure.microsoft.com/pricing/free-trial/) in pochi minuti.

[!INCLUDE [iot-hub-associate-storage](../../includes/iot-hub-associate-storage.md)]

## <a name="upload-a-file-from-a-device-app"></a>Caricare un file da un'app per dispositivi

In questa sezione si modifica l'app per dispositivi creata in [Inviare messaggi da cloud a dispositivo con l'hub IoT](iot-hub-csharp-csharp-c2d.md) per ricevere i messaggi da cloud a dispositivo dall'hub IoT.

1. In Visual Studio fare clic con il pulsante destro del mouse sul progetto **SimulatedDevice**, scegliere **Aggiungi** e quindi **Elemento esistente**. Passare a un file di immagine e includerlo nel progetto. Nell'esercitazione si presuppone che l'immagine sia denominata `image.jpg`.

1. Fare clic con il pulsante destro del mouse sull'immagine e quindi scegliere **Proprietà**. Controllare che **Copia nella directory di output** sia impostato su **Copia sempre**.

    ![Mostra la posizione in cui aggiornare le proprietà dell'immagine per la copia nella directory di output](./media/iot-hub-csharp-csharp-file-upload/image-properties.png)

1. Nel file **Program.cs** aggiungere le istruzioni seguenti all’inizio del file:

    ```csharp
    using System.IO;
    ```

1. Aggiungere il metodo seguente alla classe **Program** :

    ```csharp
    private static async void SendToBlobAsync()
    {
        string fileName = "image.jpg";
        Console.WriteLine("Uploading file: {0}", fileName);
        var watch = System.Diagnostics.Stopwatch.StartNew();

        using (var sourceData = new FileStream(@"image.jpg", FileMode.Open))
        {
            await deviceClient.UploadToBlobAsync(fileName, sourceData);
        }

        watch.Stop();
        Console.WriteLine("Time to upload file: {0}ms\n", watch.ElapsedMilliseconds);
    }
    ```

    Il metodo `UploadToBlobAsync` accetta il nome del file e l'origine del flusso del file da caricare e gestisce il caricamento nell'archiviazione. Nell'app console viene visualizzato il tempo necessario per caricare il file.

1. Aggiungere il metodo seguente al metodo **Main** immediatamente prima della riga `Console.ReadLine()`:

    ```csharp
    SendToBlobAsync();
    ```

> [!NOTE]
> Per semplicità, in questa esercitazione non si implementa alcun criterio di nuovi tentativi. Nel codice di produzione è consigliabile implementare criteri di ripetizione dei tentativi, ad esempio un backoff esponenziale, come suggerito nell'articolo [Gestione degli errori temporanei](/azure/architecture/best-practices/transient-faults).

## <a name="receive-a-file-upload-notification"></a>Ricevere la notifica di caricamento di un file

In questa sezione verrà scritta un'app console di .NET che riceve messaggi di notifica di caricamento dall'hub IoT.

1. Nella soluzione di Visual Studio corrente creare un progetto di Windows in Visual C# usando il modello di progetto **Applicazione console** . Denominare il progetto **ReadFileUploadNotification**.

    ![Nuovo progetto in Visual Studio](./media/iot-hub-csharp-csharp-file-upload/file-upload-project-csharp1.png)

2. In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto **ReadFileUploadNotification** e quindi scegliere **Gestisci pacchetti NuGet...**.

3. Nella finestra **Gestisci pacchetti NuGet** cercare **Microsoft.Azure.Devices**, fare clic su **Installa** e accettare le condizioni per l'utilizzo.

    L'azione consente di scaricare, installare e aggiungere un riferimento al [pacchetto NuGet Azure IoT Service SDK](https://www.nuget.org/packages/Microsoft.Azure.Devices/) nel progetto **ReadFileUploadNotification**.

4. Nel file **Program.cs** aggiungere le istruzioni seguenti all’inizio del file:

    ```csharp
    using Microsoft.Azure.Devices;
    ```

5. Aggiungere i campi seguenti alla classe **Program** . Sostituire il valore del segnaposto con la stringa di connessione all'hub IoT ottenuta in [Inviare dati di telemetria da un dispositivo a un hub IoT](quickstart-send-telemetry-dotnet.md):

    ```csharp
    static ServiceClient serviceClient;
    static string connectionString = "{iot hub connection string}";
    ```

6. Aggiungere il metodo seguente alla classe **Program** :

    ```csharp
    private async static void ReceiveFileUploadNotificationAsync()
    {
        var notificationReceiver = serviceClient.GetFileNotificationReceiver();

        Console.WriteLine("\nReceiving file upload notification from service");
        while (true)
        {
            var fileUploadNotification = await notificationReceiver.ReceiveAsync();
            if (fileUploadNotification == null) continue;

            Console.ForegroundColor = ConsoleColor.Yellow;
            Console.WriteLine("Received file upload notification: {0}", 
              string.Join(", ", fileUploadNotification.BlobName));
            Console.ResetColor();

            await notificationReceiver.CompleteAsync(fileUploadNotification);
        }
    }   
    ```

    Si noti che il modello di ricezione è identico a quello usato per ricevere messaggi da cloud a dispositivo dall'app per dispositivo.

7. Aggiungere infine le righe seguenti al metodo **Main** :

    ```csharp
    Console.WriteLine("Receive file upload notifications\n");
    serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
    ReceiveFileUploadNotificationAsync();
    Console.WriteLine("Press Enter to exit\n");
    Console.ReadLine();
    ```

## <a name="run-the-applications"></a>Eseguire le applicazioni

A questo punto è possibile eseguire le applicazioni.

1. In Visual Studio fare clic con il pulsante destro del mouse sulla soluzione e selezionare **Imposta progetti di avvio**. Selezionare **Progetti di avvio multipli**, quindi selezionare l'azione **Avvia** per **ReadFileUploadNotification** e **SimulatedDevice**.

2. Premere **F5**. Si avviano entrambe le applicazioni. Verrà visualizzato il messaggio di completamento del caricamento in un'applicazione console e il messaggio di notifica di caricamento ricevuto da altre applicazioni console. È possibile usare il [portale di Azure](https://portal.azure.com/) o Esplora server di Visual Studio per verificare la presenza del file caricato nell'account di archiviazione di Azure.

    ![Schermata che mostra le opzioni di output](./media/iot-hub-csharp-csharp-file-upload/run-apps1.png)

## <a name="next-steps"></a>Passaggi successivi

In questa esercitazione si è appreso come usare le funzionalità di caricamento file dell'hub IoT per semplificare i caricamenti di file dai dispositivi. È possibile continuare a esplorare le funzionalità e gli scenari dell'hub IoT vedendo i seguenti articoli:

* [Creare un hub IoT a livello di codice](iot-hub-rm-template-powershell.md)
* [Introduzione a C SDK](iot-hub-device-sdk-c-intro.md)
* [Azure IoT SDK](iot-hub-devguide-sdks.md)

Per altre informazioni sulle funzionalità dell'hub IoT, vedere:

* [Distribuzione dell'intelligenza artificiale in dispositivi perimetrali con Azure IoT Edge](../iot-edge/tutorial-simulate-device-linux.md)


---
title: Inviare eventi a Hub eventi di Azure usando C | Microsoft Docs
description: Inviare eventi a Hub di eventi di Azure usando C
services: event-hubs
documentationcenter: ''
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: c
ms.devlang: csharp
ms.topic: article
ms.date: 10/16/2018
ms.author: shvija
ms.openlocfilehash: 32345b0f064aa78dbf1cbb84cb2309138e7bf4f7
ms.sourcegitcommit: 668b486f3d07562b614de91451e50296be3c2e1f
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/19/2018
ms.locfileid: "49455386"
---
# <a name="send-events-to-azure-event-hubs-using-c"></a>Inviare eventi a Hub di eventi di Azure usando C

## <a name="introduction"></a>Introduzione
Hub eventi di Azure è una piattaforma di Big Data streaming e un servizio di inserimento di eventi che consente di ricevere ed elaborare milioni di eventi al secondo. Hub eventi consente di elaborare e archiviare eventi, dati o dati di telemetria generati dal software distribuito e dai dispositivi. I dati inviati a un hub eventi possono essere trasformati e archiviati usando qualsiasi provider di analisi in tempo reale o adattatori di invio in batch/archiviazione. Per una panoramica dettagliata di Hub eventi, vedere [Panoramica di Hub eventi](event-hubs-about.md) e [Funzionalità di Hub eventi](event-hubs-features.md).

Questa esercitazione illustra come inviare eventi a un hub eventi usando un'applicazione console in C. 

## <a name="prerequisites"></a>Prerequisiti
Per completare l'esercitazione, sono necessari gli elementi seguenti:

* Ambiente di sviluppo in C. Questa esercitazione presuppone l'uso di uno stack gcc in una VM Linux di Azure con Ubuntu 14.04.
* [Microsoft Visual Studio](https://www.visualstudio.com/).

## <a name="write-code-to-send-messages-to-event-hubs"></a>Scrivere codice per inviare messaggi ad Hub eventi
In questa sezione viene illustrato come scrivere un'app C per inviare eventi all'hub eventi. Il codice usa la libreria Proton AMQP dal [progetto Apache Qpid](http://qpid.apache.org/). Il procedimento è simile a quello adottato per l'uso di code e argomenti del bus di servizio con AMQP da C, come illustrato [in questo esempio](https://code.msdn.microsoft.com/Using-Apache-Qpid-Proton-C-afd76504). Per altre informazioni, vedere la [documentazione di Qpid Proton](http://qpid.apache.org/proton/index.html).

1. Dalla [pagina di Qpid AMQP Messenger](https://qpid.apache.org/proton/messenger.html) seguire le istruzioni per l'installazione di Qpid Proton a seconda dell'ambiente corrente.
2. Per compilare la libreria Proton, installare i seguenti pacchetti:
   
    ```shell
    sudo apt-get install build-essential cmake uuid-dev openssl libssl-dev
    ```
3. Scaricare la [libreria Qpid Proton](http://qpid.apache.org/proton/index.html) e quindi estrarla, ad esempio:
   
    ```shell
    wget http://archive.apache.org/dist/qpid/proton/0.7/qpid-proton-0.7.tar.gz
    tar xvfz qpid-proton-0.7.tar.gz
    ```
4. Creare una directory di compilazione, compilare e installare:
   
    ```shell
    cd qpid-proton-0.7
    mkdir build
    cd build
    cmake -DCMAKE_INSTALL_PREFIX=/usr ..
    sudo make install
    ```
5. Nella directory di lavoro creare un nuovo file denominato **sender.c** con il codice seguente. Ricordare di sostituire i valori per la chiave o il nome della firma di accesso condiviso, il nome dell'hub eventi e lo spazio dei nomi. È anche necessario sostituire una versione codificata con URL della chiave per l'elemento **SendRule** creato in precedenza. È possibile creare la versione codificata con URL [qui](http://www.w3schools.com/tags/ref_urlencode.asp).
   
    ```c
    #include "proton/message.h"
    #include "proton/messenger.h"
   
    #include <getopt.h>
    #include <proton/util.h>
    #include <sys/time.h>
    #include <stddef.h>
    #include <stdio.h>
    #include <string.h>
    #include <unistd.h>
    #include <stdlib.h>
   
    #define check(messenger)                                                     \
      {                                                                          \
        if(pn_messenger_errno(messenger))                                        \
        {                                                                        \
          printf("check\n");                                                     \
          die(__FILE__, __LINE__, pn_error_text(pn_messenger_error(messenger))); \
        }                                                                        \
      }
   
    pn_timestamp_t time_now(void)
    {
      struct timeval now;
      if (gettimeofday(&now, NULL)) pn_fatal("gettimeofday failed\n");
      return ((pn_timestamp_t)now.tv_sec) * 1000 + (now.tv_usec / 1000);
    }  
   
    void die(const char *file, int line, const char *message)
    {
      printf("Dead\n");
      fprintf(stderr, "%s:%i: %s\n", file, line, message);
      exit(1);
    }
   
    int sendMessage(pn_messenger_t * messenger) {
        char * address = (char *) "amqps://{SAS Key Name}:{SAS key}@{namespace name}.servicebus.windows.net/{event hub name}";
        char * msgtext = (char *) "Hello from C!";
   
        pn_message_t * message;
        pn_data_t * body;
        message = pn_message();
   
        pn_message_set_address(message, address);
        pn_message_set_content_type(message, (char*) "application/octect-stream");
        pn_message_set_inferred(message, true);
   
        body = pn_message_body(message);
        pn_data_put_binary(body, pn_bytes(strlen(msgtext), msgtext));
   
        pn_messenger_put(messenger, message);
        check(messenger);
        pn_messenger_send(messenger, 1);
        check(messenger);
   
        pn_message_free(message);
    }
   
    int main(int argc, char** argv) {
        printf("Press Ctrl-C to stop the sender process\n");
   
        pn_messenger_t *messenger = pn_messenger(NULL);
        pn_messenger_set_outgoing_window(messenger, 1);
        pn_messenger_start(messenger);
   
        while(true) {
            sendMessage(messenger);
            printf("Sent message\n");
            sleep(1);
        }
   
        // release messenger resources
        pn_messenger_stop(messenger);
        pn_messenger_free(messenger);
   
        return 0;
    }
    ```
6. Compilare il file (si presuppone **gcc**):
   
    ```
    gcc sender.c -o sender -lqpid-proton
    ```

    > [!NOTE]
    > Questo codice usa una finestra in uscita pari a 1 per imporre un invio dei messaggi il più rapido possibile. È opportuno che l'applicazione provi a inviare i messaggi in batch per aumentare la velocità effettiva. Vedere la [pagina di Qpid AMQP Messenger](https://qpid.apache.org/proton/messenger.html) per informazioni sull'uso della libreria Qpid Proton in questo e in altri ambienti e anche nelle piattaforme per le quali sono disponibili associazioni, al momento Perl, PHP, Python e Ruby.

Eseguire l'applicazione per inviare messaggi all'hub eventi. 

Congratulazioni! Sono stati inviati messaggi a un hub eventi.

## <a name="next-steps"></a>Passaggi successivi
Per informazioni su come ricevere gli eventi dall'hub eventi, fare clic sul linguaggio di ricezione appropriato nel nodo **Ricevere eventi da un hub eventi** del sommario.


<!-- Images. -->
[21]: ./media/event-hubs-c-ephcs-getstarted/run-csharp-ephcs1.png
[24]: ./media/event-hubs-c-ephcs-getstarted/receive-eph-c.png

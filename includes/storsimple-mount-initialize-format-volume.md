---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 9a3cb06e662699b214d9cdf0aa9699b8953e4aa9
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/26/2018
ms.locfileid: "50165680"
---
<!--author=SharS last changed: 9/17/15-->

#### <a name="to-mount-initialize-and-format-a-volume"></a>Per montare, avviare e formattare un volume
1. Avviare l'iniziatore iSCSI di Microsoft.
2. Nella scheda **Individuazione** della finestra delle **proprietà dell'iniziatore iSCSI** fare clic su **Individua portale**.
3. Nella finestra di dialogo **Individua portale destinazione** specificare l'indirizzo IP dell'interfaccia di rete abilitata per iSCSI e quindi fare clic su **OK**. 
4. Nella scheda **Destinazioni** della finestra delle **proprietà dell'iniziatore iSCSI** individuare **Destinazioni individuate**. Lo stato del dispositivo deve apparire come **Inattivo**.
5. Selezionare il dispositivo di destinazione, quindi fare clic su **Connetti**. Dopo aver connesso il dispositivo, lo stato deve essere modificato in **Connesso**. Per altre informazioni sull'uso dell'iniziatore iSCSI Microsoft, vedere [Installing and Configuring Microsoft iSCSI Initiator][1] (Installazione e configurazione dell'iniziatore iSCSI Microsoft).
6. Nell'host di Windows, premere il tasto Logo Windows + X, quindi fare clic su **Esegui**. 
7. Nella finestra di dialogo **Esegui** digitare **Diskmgmt.msc**. Fare clic su **OK**. Verrà visualizzata la finestra di dialogo **Gestione disco**. Nel riquadro destro vengono visualizzati i volumi sul proprio host.
8. Nella finestra **Gestione disco** , i volumi montati verranno visualizzati come indicato nella figura seguente. Fare clic con il pulsante destro del mouse sul volume individuato (fare clic sul nome del disco) e fare clic su **Online**.
   
     ![Avvia formattazione volume](./media/storsimple-mount-initialize-format-volume/HCS_InitializeFormatVolume-include.png) 
9. Fare nuovamente clic con il pulsante destro del mouse sul volume (fare clic sul nome del disco) e fare clic su **Online**.
10. Per formattare un volume semplice, attenersi alla seguente procedura:
    
    1. Fare clic con il pulsante destro del mouse sul volume per selezionarlo (fare clic nell'area destra) e scegliere **Nuovo Volume semplice**.
    2. Nella procedura guidata Nuovo volume semplice, specificare la dimensione del volume e la lettera di unità, quindi configurare il volume come file system NTFS.
    3. Specificare una dimensione unità di allocazione pari a 64 KB. Queste dimensioni unità di allocazione funzionano correttamente con gli algoritmi di deduplicazione utilizzati nella soluzione StorSimple.
    4. Eseguire una formattazione veloce.

![Video disponibile](./media/storsimple-mount-initialize-format-volume/Video_icon.png)**Video disponibile**

Per guardare un video che illustra come montare, inizializzare, e formattare un volume StorSimple, fare clic [qui](https://azure.microsoft.com/documentation/videos/mount-initialize-and-format-a-storsimple-volume/).

<!--Link references-->
[1]: https://technet.microsoft.com/library/ee338480(WS.10).aspx

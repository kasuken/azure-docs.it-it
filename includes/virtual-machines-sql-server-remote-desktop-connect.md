---
author: rothja
ms.service: virtual-machines-sql
ms.topic: include
ms.date: 10/26/2018
ms.author: jroth
ms.openlocfilehash: fe5daa38c43723c85fb464e191ee4a3e85700e0b
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/29/2018
ms.locfileid: "50227336"
---
1. Dopo che la macchina virtuale di Azure è stata creata ed è in esecuzione, fare clic sull'icona di Macchine virtuali nel portale di Azure per visualizzare le VM.

1. Fare clic sui puntini di sospensione, **...**, per la nuova VM.

1. Fare clic su **Connetti**.

   ![Connettersi alla VM nel portale](./media/virtual-machines-sql-server-remote-desktop-connect/azure-virtual-machine-connect.png)

1. Aprire il file **RDP** scaricato dal browser per la VM.

1. La Connessione Desktop remoto invia una notifica che indica che l'autore della connessione remota non può essere identificato. Fare clic su **Connetti** per continuare.

1. Nella finestra di dialogo **Sicurezza di Windows** fare clic su **Usa un account diverso**. Potrebbe essere necessario fare clic su **Altre opzioni** per visualizzare l'opzione. Specificare il nome utente e la password configurati durante la creazione della VM. È necessario aggiungere una barra rovesciata prima del nome utente.

   ![Autenticazione di Desktop remoto](./media/virtual-machines-sql-server-remote-desktop-connect/remote-desktop-connect.png)

1. Fare clic su **OK** per connettersi.

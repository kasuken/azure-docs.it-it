---
title: Complessità delle password in Azure Active Directory B2C | Microsoft Docs
description: Come configurare i requisiti di complessità delle password specificate dagli utenti in Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/16/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: b16ac10e10655bbc7e41d9336378228097ca19ff
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/05/2018
ms.locfileid: "51014721"
---
# <a name="azure-ad-b2c-configure-complexity-requirements-for-passwords"></a>Azure AD B2C: Configurare i requisiti di complessità delle password

> [!NOTE]
> **Questa funzionalità è disponibile in anteprima pubblica.**

Azure Active Directory B2C (Azure AD B2C) supporta la modifica dei requisiti di complessità delle password specificate da un utente finale durante la creazione di un account.  Per impostazione predefinita, Azure AD B2C usa password di tipo `Strong`.  Azure AD B2C supporta anche opzioni di configurazione per controllare la complessità delle password che i clienti possono usare.

## <a name="when-password-rules-are-enforced"></a>Quando vengono applicate regole sulle password

Durante la registrazione o la reimpostazione di una password, l'utente finale deve specificare una password che soddisfi le regole di complessità,  che vengono applicate in base ai criteri di riferimento.  È possibile che, durante la registrazione, alcuni criteri richiedano un pin a quattro cifre, mentre altri una stringa di otto caratteri.  È possibile, ad esempio, usare criteri con una complessità delle password diversa per gli adulti e per i bambini.

La complessità delle password non viene mai applicata durante l'accesso.  Durante la registrazione, infatti, agli utenti non viene mai chiesto di modificare la password perché non soddisfa i requisiti di complessità correnti.

Di seguito sono elencati i tipi di criteri in cui è possibile configurare la complessità delle password:

* Criteri di registrazione o di accesso
* Criteri di reimpostazione delle password
* Criteri personalizzati ([Configurare la complessità delle password nei criteri personalizzati](active-directory-b2c-reference-password-complexity-custom.md))

## <a name="how-to-configure-password-complexity"></a>Come configurare la complessità delle password

1. Aprire **Sign-up or sign-in polices** (Criteri di registrazione o accesso).
2. Selezionare i criteri e fare clic su **Modifica**.
3. Aprire **Complessità password**.
4. Impostare la complessità delle password per questi criteri su **Semplice**, **Alta** o **Personalizzata**.

### <a name="comparison-chart"></a>Grafico di confronto

| Complessità | DESCRIZIONE |
| --- | --- |
| Semplice | Una password con un numero di caratteri compreso tra 8 e 64. |
| Assoluta | Una password con un numero di caratteri compreso tra 8 e 64. Richiede almeno tre dei quattro tipi di carattere seguenti: lettere minuscole, lettere maiuscole, numeri e simboli. |
| Personalizzate | Questa opzione offre il massimo controllo sulle regole di complessità delle password.  Consente infatti di configurare una lunghezza personalizzata  o di accettare password solo numeriche (PIN). |

## <a name="options-available-under-custom"></a>Opzioni disponibili nella configurazione personalizzata

### <a name="character-set"></a>Set di caratteri

Consente di accettare solo cifre (PIN) o l'intero set di caratteri.

* **Solo numeri** consente di immettere solo cifre (0-9) durante la configurazione di una password.
* **Tutti** consente qualsiasi lettera, numero o simbolo.

### <a name="length"></a>Length

Consente di controllare i requisiti di lunghezza della password.

* Il valore **Lunghezza minima** deve essere almeno 4.
* Il valore **Lunghezza massima** deve essere maggiore o uguale alla lunghezza minima e può essere al massimo di 64 caratteri.

### <a name="character-classes"></a>Classi di caratteri

Consente di controllare i diversi tipi di carattere usati nella password.

* **2 of 4: Lowercase character, Uppercase character, Number (0-9), Symbol** (2 di 4: carattere minuscolo, carattere maiuscolo, numero (0-9), simbolo): garantisce che la password contenga almeno due tipi di carattere, ad esempio un numero e un carattere minuscolo.
* **3 of 4: Lowercase character, Uppercase character, Number (0-9), Symbol** (3 di 4: carattere minuscolo, carattere maiuscolo, numero (0-9), simbolo): garantisce che la password contenga almeno tre tipi di carattere, ad esempio un numero, un carattere minuscolo e un carattere maiuscolo.
* **4 of 4: Lowercase character, Uppercase character, Number (0-9), Symbol** (4 di 4: carattere minuscolo, carattere maiuscolo, numero (0-9), simbolo): garantisce che la password contenga tutti i quattro tipi di carattere.

    > [!NOTE]
    > L'opzione **4 of 4** (4 di 4) può determinare frustrazione nell'utente finale. Alcuni studi hanno dimostrato inoltre che questo requisito non migliora l'entropia delle password. Vedere [NIST Password Guidelines](https://pages.nist.gov/800-63-3/sp800-63b.html#appA) (Linee guida sulle password NIST)

---
title: Scrittura di espressioni per il mapping degli attributi in Azure Active Directory | Documentazione Microsoft
description: Informazioni su come usare i mapping di espressioni per trasformare i valori degli attributi in un formato accettabile durante il provisioning automatizzato di oggetti SaaS in Azure Active Directory.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: barbkess
ms.openlocfilehash: 7a7f959f54281dcce5b8d1349f5d6607f0e5da30
ms.sourcegitcommit: 96527c150e33a1d630836e72561a5f7d529521b7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/09/2018
ms.locfileid: "51345794"
---
# <a name="writing-expressions-for-attribute-mappings-in-azure-active-directory"></a>Scrittura di espressioni per il mapping degli attributi in Azure Active Directory
Quando si configura il provisioning in un'applicazione SaaS, come mapping degli attributi è possibile specificare il mapping di espressioni. Per questo tipo di mapping è necessario scrivere un'espressione analoga a uno script, che permette di trasformare i dati utente in formati più idonei all'applicazione SaaS.

## <a name="syntax-overview"></a>Panoramica della sintassi
La sintassi per le espressioni per i mapping degli attributi è simile a quella delle funzioni di Visual Basic for Applications (VBA).

* L'intera espressione deve essere definita in termini di funzioni, che sono costituite da un nome seguito da argomenti racchiusi tra parentesi:  <br>
  *FunctionName(<<argument 1>>,<<argument N>>)*
* È possibile annidare le funzioni in altre funzioni. Ad esempio:  <br> *FunzioneUno(FunzioneDue(&lt;<argument1>&gt;))*
* È possibile passare tre tipi diversi di argomenti nelle funzioni:
  
  1. Attributi, che devono essere racchiusi tra parentesi quadre. Ad esempio: [NomeAttributo]
  2. Costanti di stringa, che devono essere racchiuse tra virgolette doppie. Ad esempio: "Stati Uniti"
  3. Altre funzioni. Ad esempio: FunzioneUno(<<argument1>>, FunzioneDue(<<argument2>>))
* Eventuali barre rovesciate ( \ ) o virgolette ( " ) da inserire nella costante di stringa dovranno essere precedute dal simbolo di barra rovesciata ( \ ) come carattere di escape. Ad esempio: "Nome società: \"Contoso\""

## <a name="list-of-functions"></a>Elenco di funzioni
[Append](#append) &nbsp;&nbsp;&nbsp;&nbsp; [FormatDateTime](#formatdatetime) &nbsp;&nbsp;&nbsp;&nbsp; [Join](#join) &nbsp;&nbsp;&nbsp;&nbsp; [Mid](#mid) &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp; [NormalizeDiacritics](#normalizediacritics) [Not](#not) &nbsp;&nbsp;&nbsp;&nbsp; [Replace](#replace) &nbsp;&nbsp;&nbsp;&nbsp; [SelectUniqueValue](#selectuniquevalue)&nbsp;&nbsp;&nbsp;&nbsp; [SingleAppRoleAssignment](#singleapproleassignment)&nbsp;&nbsp;&nbsp;&nbsp; [StripSpaces](#stripspaces) &nbsp;&nbsp;&nbsp;&nbsp; [Switch](#switch)

- - -
### <a name="append"></a>Append
**Funzione:**<br>  Append(source, suffix)

**Descrizione:**<br>  Accetta un valore di stringa di origine e aggiunge un suffisso alla fine del valore.

**Parametri:**<br> 

| NOME | Obbligatorio/Ripetuto | type | Note |
| --- | --- | --- | --- |
| **source** |Obbligatoria |string |In genere è il nome dell'attributo dell'oggetto di origine. |
| **suffix** |Obbligatoria |string |Stringa da aggiungere alla fine del valore di origine. |

- - -
### <a name="formatdatetime"></a>FormatDateTime
**Funzione:**<br>  FormatDateTime(source, inputFormat, outputFormat)

**Descrizione:**<br>  Accetta una stringa data in un formato e la converte in un formato diverso.

**Parametri:**<br> 

| NOME | Obbligatorio/Ripetuto | type | Note |
| --- | --- | --- | --- |
| **source** |Obbligatoria |string |In genere è il nome dell'attributo dell'oggetto di origine. |
| **inputFormat** |Obbligatoria |string |Formato previsto del valore source. Per informazioni sui formati supportati, vedere [http://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx). |
| **outputFormat** |Obbligatoria |string |Formato della data di output. |

- - -
### <a name="join"></a>Join
**Funzione:**<br>  Join(separator, source1, source2, …)

**Descrizione:**<br> Join() è simile ad Append(), ma può combinare più valori di stringa **source** in un singola stringa e ogni valore sarà separato da una stringa **separator**.

Se uno dei valori di origine è un attributo con più valori, verranno aggiunti tutti i valori dell'attributo, separati dal valore separatore.

**Parametri:**<br> 

| NOME | Obbligatorio/Ripetuto | type | Note |
| --- | --- | --- | --- |
| **separator** |Obbligatoria |string |Stringa usata per separare i valori di origine quando sono concatenati in una stringa. Può essere "" se non sono necessari separatori. |
| **source1 … sourceN** |Obbligatorio per un numero variabile di volte |string |Valori stringa da unire. |

- - -
### <a name="mid"></a>Mid
**Funzione:**<br>  Mid(source, start, length)

**Descrizione:**<br>  Restituisce una sottostringa del valore source. Una sottostringa è una stringa che contiene solo alcuni caratteri della stringa di origine.

**Parametri:**<br> 

| NOME | Obbligatorio/Ripetuto | type | Note |
| --- | --- | --- | --- |
| **source** |Obbligatoria |string |Corrisponde in genere al nome dell'attributo. |
| **start** |Obbligatoria |numero intero |Indice nella stringa **source** che indica il punto di inizio della sottostringa. L'indice del primo carattere della stringa sarà pari a 1, quello del secondo carattere a 2 e così via. |
| **length** |Obbligatoria |numero intero |Lunghezza della sottostringa. Se la lunghezza eccede la stringa **source**, la funzione restituirà una sottostringa dall'indice **start** fino alla fine della stringa **source**. |

- - -
### <a name="normalizediacritics"></a>NormalizeDiacritics
**Funzione:**<br> NormalizeDiacritics(source)

**Descrizione:**<br> Richiede un argomento stringa. Restituisce la stringa, ma con i caratteri diacritici sostituiti dagli equivalenti caratteri non diacritici. Viene in genere usata per convertire i nomi e i cognomi contenenti caratteri diacritici (accenti) in valori validi che possono essere usati in vari ID utente, ad esempio nomi dell'entità utente, nomi di account SAM e indirizzi di posta elettronica.

**Parametri:**<br> 

| NOME | Obbligatorio/Ripetuto | type | Note |
| --- | --- | --- | --- |
| **source** |Obbligatoria |string | In genere un attributo nome o cognome |

- - -
### <a name="not"></a>not
**Funzione:**<br>  Not(source)

**Descrizione:**<br> Inverte il valore booleano di **source**. Se il valore di **source** è "*True*", restituisce "*False*". In caso contrario, restituisce "*True*".

**Parametri:**<br> 

| NOME | Obbligatorio/Ripetuto | type | Note |
| --- | --- | --- | --- |
| **source** |Obbligatoria |Stringa booleana |I valori previsti per **source** sono "True" o "False". |

- - -
### <a name="replace"></a>Replace
**Funzione:**<br> Replace(source, oldValue, regexPattern, regexGroupName, replacementValue, replacementAttributeName, template)

**Descrizione:**<br>
 Sostituisce i valori all'interno di una stringa. Funziona in modo diverso a seconda dei parametri forniti:

* Se vengono forniti **oldValue** e **replacementValue**:
  
  * Sostituisce tutte le occorrenze di oldValue in source con replacementValue
* Se vengono forniti **oldValue** e **template**:
  
  * Sostituisce tutte le occorrenze di **oldValue** in **template** con il valore **source**
* Se vengono forniti **regexPattern**, **regexGroupName** e **replacementValue**:
  
  * Sostituisce tutti i valori corrispondenti a oldValueRegexPattern nella stringa source con replacementValue.
* Se vengono forniti **regexPattern**, **regexGroupName** e **replacementPropertyName**:
  
  * Se **source** non ha alcun valore, viene restituito **source**
  * Se **source** ha un valore, usa **regexPattern** e **regexGroupName** per estrarre il valore di sostituzione dalla proprietà con **replacementPropertyName**. Il valore di sostituzione viene restituito come risultato.

**Parametri:**<br> 
| NOME | Obbligatorio/Ripetuto | type | Note |
| --- | --- | --- | --- |
| **source** |Obbligatoria |string |In genere è il nome dell'attributo dell'oggetto di origine. |
| **oldValue** |Facoltativo |string |Valore da sostituire in **source** o **template**. |
| **regexPattern** |Facoltativo |string |Criterio di espressione regolare per il valore da sostituire in **source**. Se invece si usa replacementPropertyName, corrisponde al modello usato per estrarre il valore dalla proprietà di sostituzione. |
| **regexGroupName** |Facoltativo |string |Nome del gruppo in **regexPattern**. Solo se si usa replacementPropertyName, il valore di questo gruppo verrà estratto come replacementValue dalla proprietà di sostituzione. |
| **replacementValue** |Facoltativo |string |Nuovo valore con cui sostituire il precedente. |
| **replacementAttributeName** |Facoltativo |string |Nome dell'attributo da usare per il valore di sostituzione, se non sono presenti valori per source. |
| **template** |Facoltativo |string |Se viene fornito il valore **template**, il valore **oldValue** verrà cercato in template e sostituito con il valore source. |

- - -
### <a name="selectuniquevalue"></a>SelectUniqueValue
**Funzione:**<br> SelectUniqueValue(uniqueValueRule1, uniqueValueRule2, uniqueValueRule3, …)

**Descrizione:**<br> Richiede almeno due argomenti, costituiti da regole di generazione di valori univoci definite tramite espressioni. La funzione valuta ogni regola e controlla quindi il valore generato per verificarne l'univocità nella directory/app di destinazione. Il primo valore univoco trovato sarà quello restituito. Se tutti i valori trovati sono già presenti nella destinazione, la voce verrà depositata e il motivo verrà registrato nei log di controllo. Non è previsto alcun limite relativamente al numero di argomenti che è possibile specificare.

> [!NOTE]
>1. Si tratta di una funzione di primo livello che non può essere annidata.
>2. Questa funzione è destinata a essere usata solo per la creazione di voci. Se viene usata con un attributo, impostare la proprietà **Applica questo mapping** su **Solo durante la creazione dell'oggetto**.


**Parametri:**<br> 

| NOME | Obbligatorio/Ripetuto | type | Note |
| --- | --- | --- | --- |
| **uniqueValueRule1  … uniqueValueRuleN ** |Sono necessari almeno 2 argomenti, nessun limite superiore |string | Elenco delle regole di generazione di valori univoci da valutare |


- - -
### <a name="singleapproleassignment"></a>SingleAppRoleAssignment
**Funzione:**<br> SingleAppRoleAssignment([appRoleAssignments])

**Descrizione:**<br> Richiede un argomento stringa. Restituisce la stringa, ma con i caratteri diacritici sostituiti dagli equivalenti caratteri non diacritici.

**Parametri:**<br> 

| NOME | Obbligatorio/Ripetuto | type | Note |
| --- | --- | --- | --- |
| **[appRoleAssignments]** |Obbligatoria |string |Oggetto **[appRoleAssignments]**. |

- - -
### <a name="stripspaces"></a>StripSpaces
**Funzione:**<br>  StripSpaces(source)

**Descrizione:**<br>  Rimuove tutti i caratteri di spazio (" ") dalla stringa di origine.

**Parametri:**<br> 

| NOME | Obbligatorio/Ripetuto | type | Note |
| --- | --- | --- | --- |
| **source** |Obbligatoria |string |**source** da aggiornare. |

- - -
### <a name="switch"></a>Switch
**Funzione:**<br>  Switch(source, defaultValue, key1, value1, key2, value2, …)

**Descrizione:**<br> Quando il valore **source** corrisponde a **key**, verrà restituito un parametro **value** per tale oggetto **key**. Se il valore del parametro **source** non corrisponde ad alcuna chiave, verrà restituito **defaultValue**.  I parametri **key** e **value** devono essere sempre accoppiati. Le funzioni prevedono sempre un numero pari di parametri.

**Parametri:**<br> 

| NOME | Obbligatorio/Ripetuto | type | Note |
| --- | --- | --- | --- |
| **source** |Obbligatoria |string |**Source** da aggiornare. |
| **defaultValue** |Facoltativo |string |Valore predefinito da usare se l'origine non corrisponde ad alcuna chiave. Può essere una stringa vuota (""). |
| **key** |Obbligatoria |string |Parametro **key** con cui confrontare il valore di **source**. |
| **value** |Obbligatoria |string |Valore di sostituzione per il valore **source** corrispondente al parametro key. |

## <a name="examples"></a>Esempi
### <a name="strip-known-domain-name"></a>Rimuovere un nome di dominio noto
Occorre rimuovere un nome di dominio noto dall'indirizzo di posta elettronica di un utente per ottenere il nome utente. <br>
 Ad esempio, se il dominio è "contoso.com", è possibile usare l'espressione seguente:

**Espressione:** <br>
`Replace([mail], "@contoso.com", , ,"", ,)`

**Input/output di esempio:** <br>

* **INPUT** (mail): "john.doe@contoso.com"
* **OUTPUT**: "john.doe"

### <a name="append-constant-suffix-to-user-name"></a>Aggiungere un suffisso costante al nome utente
Se si usa un ambiente sandbox Salesforce, potrebbe essere necessario aggiungere un suffisso a tutti i nomi utente prima di sincronizzarli.

**Espressione:** <br>
`Append([userPrincipalName], ".test")`

**Input/output di esempio:** <br>

* **INPUT** (userPrincipalName): "John.Doe@contoso.com"
* **OUTPUT**: "John.Doe@contoso.com.test"

### <a name="generate-user-alias-by-concatenating-parts-of-first-and-last-name"></a>Generare un alias utente concatenando parti del nome e del cognome
Occorre generare un alias utente contenente le prime tre lettere del nome e le prime cinque lettere del cognome dell'utente.

**Espressione:** <br>
`Append(Mid([givenName], 1, 3), Mid([surname], 1, 5))`

**Input/output di esempio:** <br>

* **INPUT** (givenName): "John"
* **INPUT** (surname): "Doe"
* **OUTPUT**: "JohDoe"

### <a name="remove-diacritics-from-a-string"></a>Rimuovere i segni diacritici da una stringa
È necessario sostituire i caratteri contenenti accenti con gli equivalenti caratteri non contenenti accenti.

**Espressione:** <br>
NormalizeDiacritics([givenName])

**Input/output di esempio:** <br>

* **INPUT** (givenName): "Zoë"
* **OUTPUT**: "Zoe"

### <a name="output-date-as-a-string-in-a-certain-format"></a>Eseguire l'output della data come stringa in un formato specifico

Occorre inviare date a un'applicazione SaaS in un formato specifico, <br>
Ad esempio, formattare le date per ServiceNow.

**Espressione:** <br>

`FormatDateTime([extensionAttribute1], "yyyyMMddHHmmss.fZ", "yyyy-MM-dd")`

**Input/output di esempio:**

* **INPUT** (extensionAttribute1): "20150123105347.1Z"
* **OUTPUT**: "2015-01-23"

### <a name="replace-a-value-based-on-predefined-set-of-options"></a>Sostituire un valore in base a un set di opzioni predefinito

È necessario definire il fuso orario dell'utente in base al codice di stato archiviato in Azure AD. <br>
 Se il codice di stato non corrisponde ad alcuna opzione predefinita, usare il valore predefinito "Australia/Sydney".

**Espressione:** <br>

`Switch([state], "Australia/Sydney", "NSW", "Australia/Sydney","QLD", "Australia/Brisbane", "SA", "Australia/Adelaide")`

**Input/output di esempio:**

* **INPUT** (state): "QLD"
* **OUTPUT**: "Australia/Brisbane"

### <a name="generate-unique-value-for-userprincipalname-upn-attribute"></a>Generare un valore univoco per l'attributo userPrincipalName (UPN)

In base al nome, al secondo nome e al cognome dell'utente, è necessario generare un valore per l'attributo UPN e verificarne l'univocità nella directory di AD di destinazione prima di assegnare il valore all'attributo UPN.

**Espressione:** <br>

    SelectUniqueValue( 
        Join("@", NormalizeDiacritics(StripSpaces(Join(".",  [PreferredFirstName], [PreferredLastName]))), "contoso.com"), 
        Join("@", NormalizeDiacritics(StripSpaces(Join(".",  Mid([PreferredFirstName], 1, 1), [PreferredLastName]))), "contoso.com")
        Join("@", NormalizeDiacritics(StripSpaces(Join(".",  Mid([PreferredFirstName], 1, 2), [PreferredLastName]))), "contoso.com")
    )

**Input/output di esempio:**

* **INPUT** (PreferredFirstName): "John"
* **INPUT** (PreferredLastName): "Smith"
* **OUTPUT**: "John.Smith@contoso.com" se il valore UPN di John.Smith@contoso.com non esiste già nella directory
* **OUTPUT**: "J.Smith@contoso.com" se il valore UPN di John.Smith@contoso.com esiste già nella directory
* **OUTPUT**: "Jo.Smith@contoso.com" se i due valori UPN precedenti esistono già nella directory

## <a name="related-articles"></a>Articoli correlati
* [Automatizzare il provisioning e il deprovisioning utenti in app SaaS](user-provisioning.md)
* [Personalizzazione dei mapping degli attributi per il Provisioning dell’utente](customize-application-attributes.md)
* [Ambito dei filtri per il Provisioning utente](define-conditional-rules-for-provisioning-user-accounts.md)
* [Uso di SCIM per abilitare il provisioning automatico di utenti e gruppi da Azure Active Directory alle applicazioni](use-scim-to-provision-users-and-groups.md)
* [Notifiche relative al provisioning dell'account](user-provisioning.md)
* [Elenco di esercitazioni pratiche sulla procedura di integrazione delle applicazioni SaaS](../saas-apps/tutorial-list.md)


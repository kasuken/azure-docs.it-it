---
title: Filtro geografico in un dominio per il servizio Frontdoor di Azure | Microsoft Docs
description: Questo articolo fornisce informazioni sui criteri di filtro geografico per il servizio Frontdoor di Azure
services: frontdoor
documentationcenter: ''
author: sharad4u
editor: ''
ms.service: frontdoor
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/09/2018
ms.author: sharadag
ms.openlocfilehash: e36253600dd8039940209cb5912cb2e4c2e0fbcf
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/24/2018
ms.locfileid: "49988618"
---
# <a name="geo-filtering-geographic-based-access-control-to-azure-front-door-service-frontends"></a>Filtro geografico: controllo di accesso in base a criteri geografici ai front-end del servizio Frontdoor di Azure

Per impostazione predefinita, il servizio Frontdoor di Azure risponde alle richieste degli utenti indipendentemente dalla posizione dell'utente che effettua la richiesta. In alcuni casi, tuttavia, si può avere l'esigenza di limitare l'accesso alle applicazioni Web in base al paese. La sicurezza del livello dell'applicazione del servizio Frontdoor di Azure consente di definire criteri mediante regole di protezione personalizzate per un percorso specifico nell'endpoint per consentire o bloccare l'accesso da paesi specifici. 

I criteri di sicurezza delle applicazioni includono in genere un set di regole personalizzate. Una regola è costituita da condizioni di corrispondenza, un'azione e una priorità. Nella condizione di corrispondenza si definisce una variabile di corrispondenza, un operatore e un valore di corrispondenza.  Per la regola di filtro geografico, la variabile di corrispondenza è REMOTE_ADDR, l'operatore è GeoMatch e il valore è il codice di due lettere del paese che interessa. È possibile combinare una condizione GeoMatch e una condizione di corrispondenza stringa REQUEST_URI per creare una regola di filtro geografico basato sul percorso.

È possibile configurare criteri di filtro geografico per il proprio Frontdoor usando [Azure PowerShell](front-door-tutorial-geo-filtering.md) oppure il nostro [modello di avvio rapido](https://github.com/Azure/azure-quickstart-templates/tree/master/101-front-door-geo-filtering).

## <a name="country-code-reference"></a>Riferimento per gli indicativi dei paesi

|Prefisso internazionale | Nome paese |
| ----- | ----- |
| AD | Federazione russa |
| AE | Emirati Arabi Uniti|
| AF | Afghanistan|
| AG | Antigua e Barbuda|
| AL | Albania|
| AM | Armenia|
| AO | Angola|
| AR | Argentina|
| AS | Isole Samoa americane|
| AT | Austria|
| AU | Australia|
| AZ | Azerbaigian|
| BA | Bosnia ed Erzegovina|
| BB | Barbados|
| BD | Bangladesh|
| BE | Belgio|
| BF | Burkina Faso|
| BG | Bulgaria|
| BH | Bahrain|
| BI | Burundi|
| BJ | Benin|
| BL | Saint-Barthélemy|
| BN | Brunei Darussalam|
| BO | Bolivia|
| BR | Brasile|
| BS | Bahamas|
| BT | Bhutan|
| BW | Botswana|
| BY | Bielorussia|
| BZ | Belize|
| CA | Canada|
| CD | Congo, Repubblica democratica del|
| CF | Repubblica centrafricana|
| CH | Svizzera|
| CI | Costa d'Avorio|
| CL | Cile|
| CM | Camerun|
| CN | Cina|
| CO | Colombia|
| CR | Costa Rica|
| CU | Cuba|
| CV | Cabo Verde|
| CY | Cipro|
| CZ | Repubblica ceca|
| DE | Germania|
| DK | Danimarca|
| DO | Repubblica dominicana|
| DZ | Algeria|
| EC | Ecuador|
| EE | Estonia|
| EG | Egitto|
| ES | Spagna|
| ET | Etiopia|
| FI | Finlandia|
| FJ | Figi|
| FM | Micronesia, stati di|
| VF | Francia|
| GB | Regno Unito|
| GE | Georgia|
| GF | Guayana francese|
| GH | Ghana|
| GN | Guinea|
| GP | Guadalupa|
| GR | Grecia|
| GT | Guatemala|
| GY | Guiana|
| HK | RAS di Hong Kong|
| HN | Honduras|
| HR | Croazia|
| HT | Haiti|
| HU | Ungheria|
| ID | Indonesia|
| IE | Irlanda|
| IL | Israele|
| IN | India|
| IQ | Iraq|
| IR | Iran, Repubblica islamica|
| IS | Islanda|
| IT | Italia|
| JM | Giamaica|
| JO | Giordania|
| JP | Giappone|
| KE | Kenya|
| KG | Kirghizistan|
| KH | Cambogia|
| KI | Kiribati|
| KN | Saint Kitts e Nevis|
| KP | Repubblica democratica popolare di Corea|
| KR | Repubblica di Corea|
| KW | Kuwait|
| KY | Isole Cayman|
| KZ | Kazakhstan|
| LA | Repubblica democratica del Laos|
| LB | Libano|
| LI | Liechtenstein|
| LK | Sri Lanka|
| LR | Liberia|
| LS | Lesotho|
| LT | Lituania|
| LU | Lussemburgo|
| LV | Lettonia|
| LY | Libia araba popolare socialista|
| MA | Marocco|
| MD | Moldova, Repubblica|
| MG | Madagascar|
| MK | Macedonia, ex Repubblica Jugoslava di|
| ML | Mali|
| MM | Myanmar|
| MN | Mongolia|
| MO | Macao - R.A.S.|
| MQ | Martinica|
| MR | Mauritania|
| MT | Malta|
| MV | Maldive|
| MW | Malawi|
| MX | Messico|
| MY | Malaysia|
| MZ | Mozambico|
| ND | Namibia|
| NE | Niger|
| NG | Nigeria|
| NI | Nicaragua|
| NL | Paesi Bassi|
| NO | Norvegia|
| NP | Nepal|
| NR | Nauru|
| NZ | Nuova Zelanda|
| OM | Oman|
| PA | Panama|
| PE | Perù|
| PH | Filippine|
| PK | Pakistan|
| PL | Polonia|
| PR | Portorico|
| PT | Portogallo|
| PW | Palau|
| PY | Paraguay|
| QA | Qatar|
| RE | Riunione|
| RO | Romania|
| RS | Serbia|
| RU | Federazione russa|
| RW | Ruanda|
| SA | Arabia Saudita|
| SD | Sudan|
| SE | Svezia|
| SG | Singapore|
| SI | Slovenia|
| SK | Slovacchia|
| SN | Senegal|
| SO | Somalia|
| SR | Suriname|
| SS | Sudan del Sud|
| SV | El Salvador|
| SY | Repubblica araba siriana|
| SZ | Swaziland|
| TC | Isole Turks e Caicos|
| TG | Togo|
| TH | Thailandia|
| TN | Tunisia|
| TR | Turchia|
| TT | Trinidad e Tobago|
| TW | Taiwan|
| TZ | Tanzania, Repubblica del|
| UA | Ucraina|
| UG | Uganda|
| Stati Uniti | Stati Uniti|
| UY | Uruguay|
| UZ | Uzbekistan|
| VC | Saint Vincent e Grenadine|
| VE | Venezuela|
| VG | Isole Vergini britanniche|
| VI | Isole Vergini americane|
| VN | Vietnam|
| ZA | Sud Africa|
| ZM | Zambia|
| ZW | Zimbabwe|

## <a name="next-steps"></a>Passaggi successivi

- Informazioni sulla [sicurezza del livello dell'applicazione con Frontdoor](front-door-application-security.md).
- Informazioni su come [creare una Frontdoor](quickstart-create-front-door.md).

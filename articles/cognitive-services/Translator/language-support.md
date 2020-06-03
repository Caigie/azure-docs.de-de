---
title: 'Sprachunterstützung: Translator'
titleSuffix: Azure Cognitive Services
description: Von Cognitive Services Translator werden die folgenden Sprachen für die Übersetzung von Texten mithilfe der neuronalen maschinellen Übersetzung (Neural Machine Translation, NMT) unterstützt.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 05/26/2020
ms.author: swmachan
ms.openlocfilehash: 359079ad73fcf162fb742afe74c1c1de5896c35c
ms.sourcegitcommit: fc718cc1078594819e8ed640b6ee4bef39e91f7f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2020
ms.locfileid: "83996073"
---
# <a name="language-and-region-support-for-translator"></a>Sprach- und Regionsunterstützung für Translator

Von Translator werden die folgenden Sprachen für die Übersetzung von Texten unterstützt. Die neuronale maschinelle Übersetzung (Neural Machine Translation, NMT) ist der neue Standard für qualitativ hochwertige, auf künstlicher Intelligenz basierende maschinelle Übersetzungen und steht standardmäßig in V3 von Translator zur Verfügung, wenn ein neuronales System verfügbar ist.

[Weitere Informationen zur Funktionsweise von maschineller Übersetzung](https://www.microsoft.com/translator/mt.aspx)

## <a name="translation"></a>Sprachübersetzung

**V2 von Translator**

> [!NOTE]
> V2 wurde am 30. April 2018 eingestellt. Migrieren Sie Ihre Anwendungen zu V3, um die Vorteile der neuen Funktionen zu nutzen, die ausschließlich in V3 verfügbar sind.

* Nur statistisches System: Für diese Sprache ist kein neuronales System verfügbar.
* Neuronales System verfügbar: Es ist ein neuronales System verfügbar. Verwenden Sie den Parameter `category=generalnn`, um auf das neuronale System zuzugreifen.
* Neuronales System als Standard: Das neuronale System ist als standardmäßiges Übersetzungssystem festgelegt. Verwenden Sie den Parameter `category=smt`, um auf das statistische System zur Verwendung mit Microsoft Translator Hub zuzugreifen.
* Nur neuronales System: Es ist nur die neuronale Übersetzung verfügbar.

**V3 von Translator**: In der Version 3 von Translator wird standardmäßig ein neuronales System verwendet. Die statistischen Systeme sind nur verfügbar, wenn kein neuronales System vorhanden ist.

> [!NOTE]
> Derzeit ist eine Teilmenge der neuronalen Sprachen in Custom Translator verfügbar und es werden nach und nach zusätzliche Sprachen hinzugefügt. [Zeigen Sie die zurzeit verfügbaren Sprachen im benutzerdefinierten Translator an](#customization).

|Sprache|    Sprachcode|    V3-API|
|:-----|:-----:|:-----|
|Afrikaans|    `af`|    Neuronal|
|Arabisch|    `ar`    |    Neuronal|
|Bengalisch|    `bn`    |    Neuronal|
|Bosnisch (Lateinisch)|    `bs`    |    Neuronal|
|Bulgarisch|    `bg`    |    Neuronal|
|Chinesisch (traditionell)|    `yue`|    Statistisch|
|Katalanisch|    `ca`    |    Statistisch|
|Chinesisch (vereinfacht)|    `zh-Hans`|Neuronal|
|Chinesisch (traditionell)|    `zh-Hant`        |Neuronal|
|Kroatisch|    `hr`    |Neuronal|
|Tschechisch|    `cs`    |    Neuronal|
|Dänisch|    `da`        |Neuronal|
|Niederländisch|    `nl`|    Neuronal|
|Englisch|    `en`    |    Neuronal|
|Estnisch|    `et`    |    Neuronal|
|Fidschi|    `fj`    |    Statistisch|
|Filipino|    `fil`    |    Statistisch|
|Finnisch|    `fi`    |    Neuronal|
|Französisch|    `fr`    |    Neuronal|
|Deutsch|    `de`    |    Neuronal|
|Griechisch|    `el`    |    Neuronal|
|Gujarati|    `gu`    |    Neuronal|
|Haitianisches Kreolisch|    `ht`        |Statistisch|
|Hebräisch    |`he`    |Neuronal
|Hindi|    `hi`    |    Neuronal|
|Hmong Daw|    `mww`    |    Statistisch|
|Ungarisch|    `hu`    |    Neuronal|
|Isländisch|    `is`    |    Neuronal|
|Indonesisch|    `id`    |    Statistisch|
|Irisch | `ga`| Neuronal
|Italienisch|    `it`    |    Neuronal|
|Japanisch|    `ja`    |    Neuronal|
|Kannada|`kn`| Neuronal
|Suaheli|    `sw`    |    Statistisch|
|Klingonisch|    `tlh`    |    Statistisch|
|Klingonisch (plqaD)|    `tlh-Qaak`    |    Statistisch|
|Koreanisch    |`ko`    |    Neuronal|
|Lettisch|    `lv`    |    Neuronal|
|Litauisch|    `lt`    |    Neuronal|
|Madagassisch|    `mg`    |    Statistisch|
|Malaiisch|    `ms`        |Statistisch|
|Malayalam| `ml` | Neuronal
|Maltesisch|    `mt`    |    Statistisch|
|Maori| `mi`  | Neuronal|
|Marathi| `mr`  | Neuronal|
|Norwegisch|    `nb`    |    Neuronal|
|Persisch|    `fa`    |    Neuronal|
|Polnisch|    `pl`    |    Neuronal|
|Portugiesisch (Brasilien)|    `pt-br`    |    Neuronal|
|Portugiesisch (Portugal)| `pt-pt` | Neuronal
|Pandschabi|`pa`|Neuronal
|Queretaro-Otomi|    `otq`    |    Statistisch|
|Rumänisch|    `ro`    |    Neuronal|
|Russisch|    `ru`    |    Neuronal|
|Samoanisch|    `sm`    |    Statistisch|
|Serbisch (Kyrillisch)|    `sr-Cyrl`|    Statistisch|
|Serbisch (Lateinisch)|    `sr-Latn`        |Statistisch|
|Slowakisch|    `sk`    |    Neuronal|
|Slowenisch|    `sl`    |    Neuronal|
|Spanisch|    `es`    |    Neuronal|
|Schwedisch|    `sv`    |Neuronal|
|Tahitisch|    `ty`    |Statistisch|
|Tamilisch|    `ta`    |    Neuronal|
|Telugu|    `te`    |    Neuronal|
|Thailändisch|    `th`    |    Neuronal|
|Tongaisch|    `to`    |    Statistisch|
|Türkisch|    `tr`        |Neuronal|
|Ukrainisch|    `uk`    |    Neuronal|
|Urdu|    `ur`    |    Statistisch|
|Vietnamesisch|    `vi`    |    Neuronal|
|Walisisch|    `cy`    |    Neuronal|
|Yukatekisches Maya|    `yua`    |    Statistisch|

> [!NOTE]
> Der Sprachcode `pt` wird standardmäßig auf `pt-br`, Portugiesisch (Brasilien), festgelegt.

## <a name="transliteration"></a>Transliteration

Die „Transliterate“-Methode unterstützt die folgenden Sprachen. In der Spalte „Von/In“ wird durch „<-->“ angezeigt, dass für die Sprache eine Transliteration beider Skripts in das jeweils andere erfolgen kann. Mit „-->“ wird angezeigt, dass für die Sprache nur eine Transliteration von einem Skript in das andere möglich ist.

| Sprache    | Sprachcode | Skript | Von/In | Skript|
|:----------- |:-------------:|:-------------:|:-------------:|:-------------:|
| Arabisch | `ar` | Arabisch `Arab` | <--> | Latein `Latn` |
|Bengalisch  | `bn` | Bangla `Beng` | <--> | Latein `Latn` |
| Chinesisch (vereinfacht) | `zh-Hans` | Chinesisch (vereinfacht) `Hans`| <--> | Latein `Latn` |
| Chinesisch (vereinfacht) | `zh-Hans` | Chinesisch (vereinfacht) `Hans`| <--> | Chinesisch (traditionell) `Hant`|
| Chinesisch (traditionell) | `zh-Hant` | Chinesisch (traditionell) `Hant`| <--> | Latein `Latn` |
| Chinesisch (traditionell) | `zh-Hant` | Chinesisch (traditionell) `Hant`| <--> | Chinesisch (vereinfacht) `Hans` |
| Gujarati | `gu`  | Gujarati `Gujr` | --> | Latein `Latn` |
| Hebräisch | `he` | Hebräisch `Hebr` | <--> | Latein `Latn` |
| Hindi | `hi` | Devanagari `Deva` | <--> | Latein `Latn` |
| Japanisch | `ja` | Japanisch `Jpan` | <--> | Latein `Latn` |
| Kannada | `kn` | Kannada `Knda` | --> | Latein `Latn` |
| Malayalam | `ml` | Malayalam `Mlym` | --> | Latein `Latn` |
| Marathi | `mr` | Devanagari `Deva` | --> | Latein `Latn` |
| Oriya | `or` | Oriya `Orya` | <--> | Latein `Latn` |
| Pandschabi | `pa` | Gurmukhi `Guru`  | <--> | Latein `Latn`  |
| Serbisch (Kyrillisch) | `sr-Cyrl` | Kyrillisch `Cyrl`  | --> | Latein `Latn` |
| Serbisch (Lateinisch) | `sr-Latn` | Latein `Latn` | --> | Kyrillisch `Cyrl`|
| Tamilisch | `ta` | Tamilisch `Taml` | --> | Latein `Latn` |
| Telugu | `te` | Telugu `Telu` | --> | Latein `Latn` |
| Thailändisch | `th` | Thailändisch `Thai` | --> | Latein `Latn` |

## <a name="dictionary"></a>Wörterbuch

Das Wörterbuch unterstützt die Übertragung der folgenden Sprachen in das oder aus dem Englischen anhand der „Lookup“- und „Examples“-Methode.

| Sprache    | Sprachcode |
|:----------- |:-------------:|
| Afrikaans      | `af`          |
| Arabisch       | `ar`          |
| Bengalisch      | `bn`          |
| Bosnisch (Lateinisch)      | `bs`          |
| Bulgarisch      | `bg`          |
| Katalanisch      | `ca`          |
| Chinesisch (vereinfacht)      | `zh-Hans`          |
| Kroatisch      | `hr`          |
| Tschechisch      | `cs`          |
| Dänisch      | `da`          |
| Niederländisch      | `nl`          |
| Estnisch      | `et`          |
| Finnisch      | `fi`          |
| Französisch      | `fr`          |
| Deutsch      | `de`          |
| Griechisch      | `el`          |
| Haitianisches Kreolisch      | `ht`          |
| Hebräisch      | `he`          |
| Hindi      | `hi`          |
| Hmong Daw      | `mww`          |
| Ungarisch      | `hu`          |
| Isländisch    | `is`  |
| Indonesisch      | `id`          |
| Italienisch      | `it`          |
| Japanisch      | `ja`          |
| Suaheli      | `sw`          |
| Klingonisch      | `tlh`          |
| Koreanisch      | `ko`          |
| Lettisch      | `lv`          |
| Litauisch      | `lt`          |
| Malaiisch      | `ms`          |
| Maltesisch      | `mt`          |
| Norwegisch      | `nb`          |
| Persisch      | `fa`          |
| Polnisch      | `pl`          |
| Portugiesisch (Brasilien)     | `pt-br`          |
| Rumänisch      | `ro`          |
| Russisch      | `ru`          |
| Serbisch (Lateinisch)      | `sr-Latn`          |
| Slowakisch     | `sk`          |
| Slowenisch      | `sl`          |
| Spanisch      | `es`          |
| Schwedisch      | `sv`          |
| Tamilisch      | `ta`          |
| Thailändisch      | `th`          |
| Türkisch      | `tr`          |
| Ukrainisch      | `uk`          |
| Urdu      | `ur`          |
| Vietnamesisch      | `vi`          |
| Walisisch      | `cy`          |

## <a name="detect"></a>Detect

Von Translator werden alle verfügbaren Sprachen für die Übersetzung und Transliteration erkannt.


## <a name="access-the-translator-language-list-programmatically"></a>Programmgesteuertes Zugreifen auf die Translator-Sprachliste

Mithilfe der Methode „Languages“ kann eine Liste der unterstützten Sprachen für Translator 3.0 abgerufen werden. Sie können die Liste nach Funktion, Sprachcode sowie Sprachname in Englisch oder jeder anderen unterstützten Sprache anzeigen. Sobald neue Sprachen verfügbar gemacht werden, wird diese Liste automatisch vom Microsoft Translator-Dienst aktualisiert.

[Anzeigen der Referenzdokumentation zum Vorgang „Languages“](reference/v3-0-languages.md)

## <a name="customization"></a>Anpassung

Die folgenden Sprachen sind für die Anpassung an das Englische oder aus dem Englischen mit dem [benutzerdefinierten Translator](https://aka.ms/CustomTranslator) verfügbar.

| Sprache    | Sprachcode |
|:----------- |:-------------:|
| Arabisch       | `ar`          |
| Bengalisch      | `bn`          |
| Bosnisch (Lateinisch)      | `bs`          |
| Bulgarisch      | `bg`          |
| Chinesisch (vereinfacht)      | `zh-Hans`          |
|Chinesisch (traditionell)|    `zh-Hant`    |
| Kroatisch      | `hr`          |
| Tschechisch      | `cs`          |
| Dänisch      | `da`          |
| Niederländisch      | `nl`          |
| Englisch    | `en`     |
| Estnisch      | `et`          |
| Finnisch      | `fi`          |
| Französisch      | `fr`          |
| Deutsch      | `de`          |
| Griechisch      | `el`          |
| Hebräisch      | `he`          |
| Hindi      | `hi`          |
| Ungarisch      | `hu`          |
| Isländisch | `is` |
| Indonesisch|    `id`    |
| Irisch | `ga`    |
| Italienisch      | `it`          |
| Japanisch      | `ja`          |
| Suaheli|    `sw`    |
| Koreanisch      | `ko`          |
| Lettisch      | `lv`          |
| Litauisch      | `lt`          |
| Madagassisch|    `mg`    |
| Maori| `mi`  |
| Norwegisch      | `nb`          |
| Persisch      | `fa`          |
| Polnisch      | `pl`          |
| Portugiesisch (Brasilien) | `pt-br` |
| Rumänisch      | `ro`          |
| Russisch      | `ru`          |
| Samoanisch|    `sm`    |
| Serbisch (Lateinisch)      | `sr-Latn`          |
| Slowakisch     | `sk`          |
| Slowenisch      | `sl`          |
| Spanisch      | `es`          |
| Schwedisch      | `sv`          |
| Thailändisch      | `th`          |
| Türkisch      | `tr`          |
| Ukrainisch      | `uk`          |
| Vietnamesisch      | `vi`          |
| Walisisch | `cy` |

## <a name="access-the-list-on-the-microsoft-translator-website"></a>Zugreifen auf die Liste auf der Microsoft Translator-Website

Für einen schnellen Überblick über die Sprachen werden auf der Microsoft Translator-Website alle Sprachen angezeigt, die von den Translator- und Speech-APIs unterstützt werden. Diese Liste enthält keine entwicklerspezifischen Informationen wie z.B. Sprachcodes.

[Die Liste der Sprachen](https://www.microsoft.com/translator/languages.aspx)

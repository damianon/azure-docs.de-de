---
title: Zurückgeben von N besten Übersetzungen – Textübersetzung
titleSuffix: Azure Cognitive Services
description: Geben Sie die N besten Übersetzungen mit der Textübersetzungs-API zurück.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 12/14/2017
ms.author: swmachan
ROBOTS: NOINDEX
ms.openlocfilehash: eff25877165ac365e0af77651147fcdd1eebe294
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "73837238"
---
# <a name="how-to-return-n-best-translations"></a>Zurückgeben von N besten Übersetzungen

> [!NOTE]
> Diese Methode ist als veraltet markiert. Sie ist unter V3.0 der Textübersetzungs-API nicht verfügbar.

Die Methoden GetTranslations() und GetTranslationsArray() der Microsoft Translator-API enthalten ein optionales boolesches Flag „IncludeMultipleMTAlternatives“.
Die Methode gibt bis zu „maxTranslations“ Alternativen zurück, wobei das Delta aus der Übersetzer-Engine-Liste mit den N besten Übersetzungen stammt.

Die Signatur ist:

**Syntax**

| C# |
|:---|
| GetTranslationsResponse Microsoft.Translator.GetTranslations(appId, text, from, to, maxTranslations, options); |

**Parameter**

| Parameter | BESCHREIBUNG |
|:---|:---|
| appId | **Erforderlich** Lassen Sie das Feld „appId“ leer, wenn sie den Autorisierungsheader verwenden. Geben Sie andernfalls eine Zeichenfolge mit „Bearer“ + „ “ + Zugriffstoken an.|
| text | **Erforderlich** Eine Zeichenfolge, die den Text darstellt, der übersetzt werden soll. Die Textgröße darf 10.000 Zeichen nicht überschreiten.|
| from | **Erforderlich** Eine Zeichenfolge, die den Code der Sprache darstellt, aus der der Text übersetzt werden soll. |
| zu | **Erforderlich** Eine Zeichenfolge, die den Code der Sprache darstellt, in die der Text übersetzt werden soll. |
| maxTranslations | **Erforderlich** Ein Integer, der die maximale Anzahl von Übersetzungen darstellt, die zurückgegeben werden sollen. |
| Optionen | **Optional** Ein TranslateOptions-Objekt, das die unten aufgeführten Werte enthält. Diese sind alle optional und entsprechen den Standardwerten für die häufigsten Einstellungen.

* Category: „general“ ist der einzige unterstützte und standardmäßige Wert.
* ContentType: „text/plain“ ist der einzige unterstützte und standardmäßige Wert.
* State: Mit diesem Benutzerstatus können Anforderung und Antwort korreliert werden. Die gleichen Inhalte werden in der Antwort zurückgegeben.
* IncludeMultipleMTAlternatives: Dieses Flag bestimmt, ob mehr als eine Alternative aus der MT-Engine zurückgegeben wird. Der Standardwert ist „FALSE“ und enthält nur eine Alternative.

## <a name="ratings"></a>Ratings
Bewertungen werden folgendermaßen angewendet: Die beste automatische Übersetzung hat die Bewertung von 5.
Die automatisch generierten (N besten) Übersetzungsalternativen haben eine Bewertung von 0 und einen Übereinstimmungsgrad von 100.

## <a name="number-of-alternatives"></a>Anzahl von Alternativen
Die Anzahl zurückgegebener Alternativen beträgt höchstens „maxTranslations“.

## <a name="language-pairs"></a>Sprachenpaare
Diese Funktion ist für Übersetzungen zwischen einfachem und traditionellem Chinesisch (in beide Richtungen) nicht verfügbar. Sie ist für alle anderen von Microsoft Translator unterstützten Sprachenpaare verfügbar.

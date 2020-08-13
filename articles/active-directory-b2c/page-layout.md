---
title: Seitenlayoutversionen
titleSuffix: Azure AD B2C
description: Versionsgeschichte des Seitenlayouts für die Anpassung der Benutzeroberfläche in benutzerdefinierten Richtlinien
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 07/30/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 4548b50e4168f260cb401c40dd4e61192cea1015
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2020
ms.locfileid: "87489536"
---
# <a name="page-layout-versions"></a>Seitenlayoutversionen

Seitenlayoutpakete werden regelmäßig aktualisiert, um Korrekturen und Verbesserungen in ihre Seitenelemente aufzunehmen. Das folgende Änderungsprotokoll gibt die in den einzelnen Versionen eingeführten Änderungen an.

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

## <a name="210"></a>2.1.0

- Selbstbestätigte Seite (`selfasserted`)
  - Korrekturen für Lokalisierung und Barrierefreiheit.
- Einheitliche SSP-Seite (`unifiedssp`)
  - Unterstützung für mehrere Registrierungslinks hinzugefügt.
  - Unterstützung für die Validierung von Benutzereingaben gemäß den in der Richtlinie definierten Prädikatsregeln hinzugefügt.

## <a name="200"></a>2.0.0

- Selbstbestätigte Seite (`selfasserted`)
  - Unterstützung für [Anzeigesteuerelemente](display-controls.md) in benutzerdefinierten Richtlinien hinzugefügt.

## <a name="120"></a>1.2.0

- Alle Seiten
  - Korrekturen zur Barrierefreiheit
  - Sie können nun [in den HTML-Tags](custom-policy-ui-customization.md#guidelines-for-using-custom-page-content) das Attribut `data-preload="true"` hinzufügen, um die Ladereihenfolge für CSS und JavaScript zu steuern.
    - Laden Sie verknüpfte CSS-Dateien zur gleichen Zeit wie Ihre HTML-Vorlage, damit zwischen dem Laden der Dateien kein „Flackern“ auftritt.
    - Sie steuern damit die Reihenfolge, in der die `script`-Tags vor dem Laden der Seite abgerufen und ausgeführt werden.
  - Als E-Mail-Feld wird jetzt `type=email` verwendet, und bei Tastaturen von Mobilgeräten werden jetzt die richtigen Vorschläge angezeigt
  - Unterstützung für den Google Chrome-Übersetzer
- Einheitliche und selbstbestätigte Seiten
  - Die Felder für Benutzername/E-Mail-Adresse und Kennwort verwenden jetzt das HTML-Element `form`, damit Microsoft Edge und Internet Explorer (IE) diese Informationen ordnungsgemäß speichern können.
- Selbstbestätigte Seite
  - Konfigurierbare Verzögerung bei Validierung von Benutzereingaben für verbesserte Benutzerfreundlichkeit hinzugefügt.

## <a name="110"></a>1.1.0

- Seite mit Ausnahmen (globalexception)
  - Korrektur zur Barrierefreiheit
  - Standardmeldung bei fehlendem Kontakt wurde aus der Richtlinie entfernt.
  - Standard-CSS wurde entfernt.
- MFA-Seite (Multi-Factor)
  - Schaltfläche zur Codebestätigung wurde entfernt.
  - Das Codeeingabefeld nimmt jetzt nur noch Eingaben mit bis zu sechs (6) Zeichen entgegen.
  - Die Seite versucht automatisch, den eingegebenen Code zu überprüfen, wenn ein 6-stelliger Code eingegeben wird, ohne dass auf eine Schaltfläche geklickt werden muss.
  - Wenn der Code falsch ist, wird das Eingabefeld automatisch gelöscht.
  - Nach drei (3) Versuchen mit einem falschen Code sendet B2C eine Fehlermeldung an die vertrauende Seite.
  - Korrekturen zur Barrierefreiheit
  - Standard-CSS wurde entfernt.
- Selbstbestätigte Seite (selbstbestätigt)
  - Abbruchwarnung wurde entfernt.
  - CSS-Klasse für Fehlerelemente
  - Anzeigen/Ausblenden der Fehlerlogik wurde verbessert.
  - Standard-CSS wurde entfernt.
- SSP vereinheitlicht (unifiedssp)
  - „Angemeldet bleiben“-Steuerelement (Keep Me Signed In, KMSI) wurde hinzugefügt.

## <a name="100"></a>1.0.0

- Erste Veröffentlichung

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen dazu, wie Sie die Benutzeroberfläche Ihrer Anwendungen in benutzerdefinierten Richtlinien anpassen können, finden Sie unter [Anpassen der Benutzeroberfläche einer Anwendung mithilfe einer benutzerdefinierten Richtlinie](custom-policy-ui-customization.md).

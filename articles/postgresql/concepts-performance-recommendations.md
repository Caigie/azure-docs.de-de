---
title: Leistungsempfehlungen – Azure Database for PostgreSQL – Einzelserver
description: In diesem Artikel wird das Feature für Leistungsempfehlungen in Azure Database for PostgreSQL (Einzelserver) beschrieben.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 08/21/2019
ms.openlocfilehash: a30af0c8bef47a37fe3439e885d3895a2c826225
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "74768468"
---
# <a name="performance-recommendations-in-azure-database-for-postgresql---single-server"></a>Leistungsempfehlungen in Azure Database for PostgreSQL – Einzelserver

**Anwendungsbereich:** Azure Database for PostgreSQL – Einzelserverversionen 9.6, 10, 11

Das Feature „Leistungsempfehlungen“ analysiert Ihre Datenbanken, um angepasste Vorschläge für eine verbesserte Leistung zu erstellen. Um die Empfehlungen zu generieren, untersucht die Analyse verschiedene Datenbankmerkmale einschließlich des Schemas. Aktivieren Sie den [Abfragespeicher](concepts-query-store.md) auf Ihren Server, um das Feature „Leistungsempfehlungen“ vollständig nutzen zu können. Nach der Implementierung von Leistungsempfehlungen sollten Sie die Leistung testen, um die Auswirkungen dieser Änderungen auszuwerten. 

## <a name="permissions"></a>Berechtigungen
Zum Ausführen einer Analyse mit dem Leistungsempfehlungen-Feature sind die Berechtigungen **Besitzer** oder **Mitwirkender** erforderlich.

## <a name="performance-recommendations"></a>Empfehlungen zur Leistung
Das Feature [Leistungsempfehlungen](concepts-performance-recommendations.md) analysiert Workloads auf Ihrem Server zum Identifizieren von Indizes mit der Möglichkeit zur Verbesserung der Leistung.

Öffnen Sie im Abschnitt **Intelligente Leistung** auf der Menüleiste der Azure-Portalseite für Ihren PostgreSQL-Server die Option **Leistungsempfehlungen**.

![Leistungsempfehlungen-Startseite](./media/concepts-performance-recommendations/performance-recommendations-page.png)

Wählen Sie **Analysieren** aus, und wählen Sie eine Datenbank aus, mit der die Analyse beginnt. Je nach Workload kann die Analyse einige Minuten dauern. Wenn die Analyse abgeschlossen ist, wird im Portal eine Benachrichtigung angezeigt. Die Analyse führt eine umfassende Untersuchung der Datenbank aus. Es wird empfohlen, Analysen während der Nebenzeiten durchzuführen. 

Das Fenster **Empfehlungen** zeigt eine Liste mit den gefundenen Empfehlungen an.

![Neue Seite „Leistungsempfehlungen“](./media/concepts-performance-recommendations/performance-recommendations-result.png)

Empfehlungen werden nicht automatisch angewendet. Kopieren Sie zum Anwenden der Empfehlung den Abfragetext, und führen Sie ihn auf dem gewünschten Client aus. Denken Sie daran, die implementierte Empfehlung zu testen und zu überwachen, um sie auszuwerten. 

## <a name="recommendation-types"></a>Empfehlungstypen

Zurzeit werden zwei Arten von Empfehlungen unterstützt: *Index erstellen* und *Index löschen*.

### <a name="create-index-recommendations"></a>Empfehlungen zum Erstellen eines Index
Empfehlungen des Typs *Index erstellen* schlagen neue Indizes vor, um die am häufigsten ausgeführten oder zeitaufwändigen Abfragen in der Workload zu beschleunigen. Für diese Art von Empfehlung muss der [Abfragespeicher](concepts-query-store.md) aktiviert sein. Der Abfragespeicher erfasst Abfrageinformationen und stellt die detaillierten Statistiken zur Laufzeit und Häufigkeit der Abfrage zur Verfügung, die die Analyse verwendet, um die Empfehlung abzugeben.

### <a name="drop-index-recommendations"></a>Empfehlungen zum Löschen eines Index
Zusätzlich zum Erkennen fehlender Indizes analysiert Azure Database for PostgreSQL kontinuierlich die Leistung der vorhandenen Indizes. Wenn ein Index nur selten verwendet wird oder redundant ist, empfiehlt das Analysetool, ihn zu löschen.

## <a name="considerations"></a>Überlegungen
* Für [Lesereplikate](concepts-read-replicas.md) sind keine Leistungsempfehlungen verfügbar.
## <a name="next-steps"></a>Nächste Schritte
- Weitere Informationen zu [Überwachung und Optimierung](concepts-monitoring.md) in Azure Database for PostgreSQL.


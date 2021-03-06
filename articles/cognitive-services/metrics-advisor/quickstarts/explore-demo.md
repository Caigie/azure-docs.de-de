---
title: Erkunden der Metrics Advisor-Demo
titleSuffix: Azure Cognitive Services
description: Hier können Sie sich anhand der bereitgestellten Demo mit der Weboberfläche von Metrics Advisor vertraut machen.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: metrics-advisor
ms.topic: quickstart
ms.date: 09/14/2020
ms.author: aahi
ms.openlocfilehash: fbc73e20b8cc2baa1cc5c5a5b2f674fb1b2dde84
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90944620"
---
# <a name="quickstart-explore-the-metrics-advisor-demo-with-example-data"></a>Schnellstart: Erkunden der Metrics Advisor-Demo mit Beispieldaten

> [!Note]
> Die Metrics Advisor-Demo ist schreibgeschützt, und es ist nicht möglich, Ihre eigenen Daten zu integrieren. Wenn Sie den Dienst für Ihre eigenen Daten verwenden möchten, müssen Sie [eine Metrics Advisor-Ressource erstellen](web-portal.md).

Verwenden Sie diesen Artikel, um schnell die wichtigsten Features in Metrics Advisor zu erkunden. Wir stellen eine Demowebsite mit Beispieldaten und vorgegebenen Konfigurationen bereit, damit Sie sich mit den umfassenden Features des Webportals vertraut machen können.

Klicken Sie auf [diesen Link](https://aka.ms/MetricsAdvisor/Demo), um zur Demowebsite zu gelangen.

## <a name="view-the-available-sample-data"></a>Anzeigen der verfügbaren Beispieldaten

Nach der Anmeldung bei der Demowebsite wird die Seite **Datenfeed** angezeigt. Bei einem Datenfeed handelt es sich um eine logische Gruppe von Zeitreihendaten, die aus Ihrer Datenquelle abgefragt werden. Weitere Informationen zu in Metrics Advisor verwendeten Begriffen und Konzepten finden Sie im [Glossar](../glossary.md). 

Es werden mehrere Datenfeeds aufgelistet. Diese werden aus verschiedenen Arten von Datenquellen erfasst (beispielsweise aus einer Azure SQL-Datenbank oder aus einer Azure-Tabelle). Von den einzelnen Instanzen werden jeweils etwas andere Konfigurationseinstellungen verwendet, um eine Verbindung mit den zugehörigen Datenspeichern herzustellen.

:::image type="content" source="../media/sample-datafeeds.png" alt-text="Liste mit Beispieldaten" lightbox="../media/sample-datafeeds.png":::

## <a name="explore-the-data-feed-configurations"></a>Erkunden der Datenfeedkonfigurationen

Klicken Sie auf den Datenfeed *Sample - Cost/Revenue - City/Category* (Beispiel: Kosten/Umsatz: Stadt/Kategorie). Daraufhin werden mehrere Abschnitte mit Details für den Feed angezeigt:

* Name und Erfassungsstatus des Datenfeeds.
* Eine Liste mit Metriken, die aus der Datenquelle abgefragt werden. Beispielsweise *Kosten* und *Umsatz*. 
* Warnungsverlauf, für den Fall, dass der Datenfeed nicht mehr verfügbar ist. 
* Protokolle zu Datenfeedaktualisierungen.   
* Informationen und Einstellungen für den Datenfeed.

:::image type="content" source="../media/data-feed-view.png" alt-text="Datenfeedansicht" lightbox="../media/data-feed-view.png":::


## <a name="view-time-series-visualizations-and-configurations"></a>Anzeigen von Zeitreihenvisualisierungen und -konfigurationen

Klicken Sie im Datenfeed *Sample - Cost/Revenue - City/Category* (Beispiel: Kosten/Umsatz: Stadt/Kategorie) auf die Metrik *cost* (Kosten). Die zugeordnete Zeitreihe wird mit Dimensionssegmenten und mit Visualisierungen gemäß den historischen Metrikdaten dargestellt. Das blaue Band um die Metrikdaten stellt den erwarteten Wertebereich aus den Machine Learning-Modellen von Metrics Advisor dar. Punkte außerhalb dieses Bands werden als rote Punkte dargestellt. Hierbei handelt es sich um erkannte Anomalien. 

:::image type="content" source="../media/series-visualization.png" alt-text="Reihenvisualisierung" lightbox="../media/series-visualization.png":::

Die Anomalieerkennung kann durch Optimieren der **Erkennungskonfigurationen** im linken Bereich der Metrikdetailseite konfiguriert werden. Ihnen stehen mehrere Anomalieerkennungsmethoden zur Verfügung. Diese können auch miteinander kombiniert werden. Außerdem können Sie verschiedene Empfindlichkeiten, Erkennungsrichtungen und andere Konfigurationen ausprobieren. Über den Link **Advanced configuration** (Erweiterte Konfiguration) im unteren Bereich der **Erkennungskonfigurationen** können Sie komplexere und stärker angepasste Erkennungseinstellungen für Gruppen oder einzelne Reihen erstellen. 

Darüber hinaus können Sie die Anomalieerkennung durch Feedback für den Erkennungsalgorithmus optimieren. Klicken Sie auf eine Anomalie, und verwenden Sie den Bereich **Add feedback** (Feedback hinzufügen), um den Anomaliestatus und die Saisonalität zu konfigurieren und den Punktstatus zu ändern. Dieses Feedback wird bei der Erkennung für zukünftige Punkte berücksichtigt.  

Am unteren Rand des Bereichs **Add feedback** (Feedback hinzufügen) befindet sich der Link **To incident hub** (Zum Incidenthub). Über diesen Link gelangen Sie zur Seite für die Incidentanalyse sowie zur Ursachenanalyse für den Incident.  

:::image type="content" source="../media/incident-link.png" alt-text="Incidentlink" lightbox="../media/incident-link.png":::

## <a name="explore-anomaly-detection-results-and-perform-root-cause-analysis"></a>Erkunden der Ergebnisse der Anomalieerkennung und Durchführen einer Ursachenanalyse

Wenn Sie in einer Anomalie auf den Link **To incident hub** (Zum Incidenthub) klicken, wird eine Seite für die Incidentanalyse angezeigt. Diese Seite enthält Diagnoseerkenntnisse für den Incident. Hierzu zählen beispielsweise der Schweregrad, die Anzahl beteiligter Anomalien und die Start-/Endzeit. Im Abschnitt **Grundursache** werden automatisierte Ratschläge angezeigt. Hierzu wird die Incidentstruktur unter Berücksichtigung von Abweichung, Verteilung und Anteil an übergeordneten Anomalien analysiert, um die Grundursache des Incidents zu ermitteln.

Im Abschnitt **Diagnose** werden eine Struktur des Incidents sowie mehrere Registerkarten für die Incidentdiagnose angezeigt.

:::image type="content" source="../media/incident-diagnostic.png" alt-text="Incidentdiagnose" lightbox="../media/incident-diagnostic.png":::

Die Ermittlung der Grundursache ermöglicht die Ergreifung von Problembehandlungsmaßnahmen, bevor sich die Situation verschlechtert. Durch Klicken auf die anderen bereitgestellten Diagnosefeatures können auch weitere Erkenntnisse erkundet werden. 

## <a name="next-steps"></a>Nächste Schritte

- [Verwenden des Webportals](web-portal.md)
- [Verwenden der REST-API](rest-api.md)
- [Durchführen des Onboardings für Ihre Datenfeeds](../how-tos/onboard-your-data.md)
    - [Verwalten von Datenfeeds](../how-tos/manage-data-feeds.md)
    - [Konfigurationen für unterschiedliche Datenquellen](../data-feeds-from-different-sources.md)

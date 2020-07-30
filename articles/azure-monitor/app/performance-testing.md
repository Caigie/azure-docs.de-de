---
title: Leistungs- und Auslastungstests mit Azure Application Insights | Microsoft-Dokumentation
description: Einrichten von Leistungs- und Auslastungstests mit Azure Application Insights
ms.topic: conceptual
ms.date: 06/19/2019
ms.reviewer: sdash
ms.openlocfilehash: 8885a2e511c11317f593706255437e52819adcae
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87024349"
---
# <a name="performance-testing"></a>Leistungstests

> [!NOTE]
> Der cloudbasierte Dienst für Auslastungstests ist veraltet. Weitere Informationen zu Veralten, Dienstverfügbarkeit und alternativen Diensten finden Sie [hier](/azure/devops/test/load-test/overview?view=azure-devops).

Mithilfe von Application Insights können Sie Auslastungstests für Ihre Websites generieren. Genauso wie bei [Verfügbarkeitstest](monitor-web-app-availability.md) können Sie entweder einfache Anforderungen oder [mehrstufige Anforderungen](availability-multistep.md) über unsere weltweit vorhandenen Azure-Test-Agents senden. Mithilfe von Leistungstests können Sie bis zu 20.000 Benutzer gleichzeitig für einen Zeitraum von bis zu 60 Minuten simulieren.

## <a name="create-an-application-insights-resource"></a>Erstellen einer Application Insights-Ressource

Damit Sie einen Leistungstest erstellen können, müssen Sie zunächst eine Application Insights-Ressource erstellen. Wenn Sie bereits eine solche Ressource erstellt haben, gehen Sie weiter zum nächsten Abschnitt.

Klicken Sie im Azure-Portal auf **Ressource erstellen** > **Entwicklertools** > **Application Insights**, und erstellen Sie eine Application Insights-Ressource.

## <a name="configure-performance-testing"></a>Konfigurieren von Leistungstests

Wenn Sie zum ersten Mal einen Leistungstest erstellen, klicken Sie auf **Organisation festlegen**, und wählen Sie eine Azure DevOps-Organisation als Quelle für Ihre Leistungstests aus.

Wechseln Sie unter **Konfigurieren** zu **Leistungstests**, und klicken Sie auf **Neu**, um einen Test zu erstellen.

![Mindestens die URL der Website eintragen](./media/performance-testing/new-performance-test.png)

Wenn Sie einen einfachen Leistungstest erstellen möchten, wählen Sie den Testtyp **Manueller Test** aus, und legen Sie die gewünschten Einstellungen für Ihren Test fest.

|Einstellung| Maximalwert
|----------|------------|
| Benutzerauslastung | 20.000 |
| Dauer (Minuten)  | 60 |  

Klicken Sie auf **Test ausführen**, sobald Sie Ihren Test erstellt haben.

Sobald der Test abgeschlossen ist, sollten Ihnen in etwa die folgenden Ergebnisse angezeigt werden:

![Testergebnisse](./media/performance-testing/test-results.png)

## <a name="configure-visual-studio-web-test"></a>Konfigurieren eines Visual Studio-Webtests

Die erweiterten Funktionen für Leistungstests mit Application Insights basieren auf Visual Studio-Leistungs- und Auslastungstestprojekten.

![Visual Studio ](./media/performance-testing/visual-studio-test.png)

## <a name="next-steps"></a>Nächste Schritte

* [Multi-step web tests (Mehrstufige Webtests)](availability-multistep.md)
* [URL ping tests (URL-Pingtests)](monitor-web-app-availability.md)

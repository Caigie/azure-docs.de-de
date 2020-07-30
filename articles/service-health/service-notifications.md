---
title: Anzeigen von Dienstintegritätsbenachrichtigungen im Azure-Portal
description: Mit Dienstintegritätsbenachrichtigungen können Sie von Microsoft Azure veröffentlichte Dienstintegritätsmeldungen anzeigen.
ms.topic: conceptual
ms.date: 6/27/2019
ms.openlocfilehash: 9f8297ae708d3f4e7921221f2c4bacee12a7a2b1
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87089565"
---
# <a name="view-service-health-notifications-by-using-the-azure-portal"></a>Anzeigen von Dienstintegritätsbenachrichtigungen im Azure-Portal

Benachrichtigungen zur Dienstintegrität werden von der Azure-Infrastruktur im [Azure-Aktivitätsprotokoll](../azure-monitor/platform/platform-logs-overview.md) veröffentlicht.  Die Benachrichtigungen enthalten Informationen über die Ressourcen in Ihrem Abonnement. Angesichts der möglicherweise großen Menge an Informationen, die im Aktivitätsprotokoll gespeichert werden, ist eine separate Benutzeroberfläche vorhanden, um die Anzeige und Einrichtung von Benachrichtigungen zur Dienstintegrität zu erleichtern. 

Dienstintegritätsbenachrichtigungen können je nach Klasse informativ oder handlungsrelevant sein.

Weitere Informationen zu den verschiedenen Klassen von Dienstintegritätsbenachrichtigungen finden Sie unter [Eigenschaften von Dienstintegritätsbenachrichtigungen](service-health-notifications-properties.md).

## <a name="view-your-service-health-notifications-in-the-azure-portal"></a>Anzeigen Ihrer Dienstintegritätsbenachrichtigungen im Azure-Portal

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) die Option **Überwachen** aus.

    ![Screenshot des Menüs im Azure-Portal mit ausgewählter Überwachungsoption](./media/service-notifications/home-monitor.png)

    Azure Monitor fasst alle Überwachungseinstellungen und -daten in einer konsolidierten Ansicht zusammen. Zunächst wird der Abschnitt **Aktivitätsprotokoll** geöffnet.

1. Wählen Sie **Warnungen**.

    ![Screenshot des Überwachungsaktivitätsprotokolls mit ausgewählten Warnungen](./media/service-notifications/service-health-summary.png)

1. Wählen Sie **+ Aktivitätsprotokollwarnung hinzufügen** aus, und richten Sie eine Warnung ein, um sicherzustellen, dass Sie zukünftige Dienstbenachrichtigungen erhalten. Weitere Informationen finden Sie unter [Erstellen von Aktivitätsprotokollwarnungen zu Dienstbenachrichtigungen](./alerts-activity-log-service-notifications-portal.md).

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über [Aktivitätsprotokollwarnungen](../azure-monitor/platform/activity-log-alerts.md).

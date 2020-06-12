---
title: Planen von Azure-Wartungsereignissen
description: Es wird beschrieben, wie Sie Ereignisse der geplanten Wartung für Azure SQL-Datenbank und für verwaltete Azure SQL-Instanzen vorbereiten.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: aamalvea
ms.author: aamalvea
ms.reviewer: carlrab
ms.date: 01/30/2019
ms.openlocfilehash: 2f5f69a5f145ae8bbf23aa1e5dbca07b30db0e21
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2020
ms.locfileid: "84027751"
---
# <a name="planning-for-azure-maintenance-events-in-azure-sql-database-and-azure-sql-managed-instance"></a>Planen von Azure-Wartungsereignissen in Azure SQL-Datenbank und für verwaltete Azure SQL-Instanzen
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

Es wird beschrieben, wie Sie Ereignisse der geplanten Wartung in Ihrer Datenbank für Azure SQL-Datenbank und für verwaltete Azure SQL-Instanzen vorbereiten.

## <a name="what-is-a-planned-maintenance-event"></a>Was ist ein geplantes Wartungsereignis?

Für jede Datenbank halten Azure SQL-Datenbank und verwaltete Azure SQL-Instanzen ein bestimmtes Quorum an Datenbankreplikaten vor, von denen eines das primäre Replikat ist. Zu jeder Zeit muss ein primäres Replikat online in Betrieb sein, und mindestens ein sekundäres Replikat muss integer sein. Während der geplanten Wartung werden die Mitglieder des Datenbankquorums nacheinander mit der Absicht offline geschaltet, dass ein antwortendes primäres Replikat und mindestens ein sekundäres Replikat online vorhanden ist, um sicherzustellen, dass es keine Ausfallzeiten des Clients auftreten. Wenn das primäre Replikat offline geschaltet werden muss, erfolgt ein Neukonfigurations-/Failoverprozess, bei dem ein sekundäres Replikat zum neuen primären Replikat wird.  

## <a name="what-to-expect-during-a-planned-maintenance-event"></a>Was Sie bei einem geplanten Wartungsereignis erwarten können

Neukonfigurationen/Failover werden in der Regel innerhalb von 30 Sekunden abgeschlossen. Der Durchschnittswert sind 8 Sekunden. Wenn sie bereits verbunden ist, muss Ihre Anwendung mit der integren Kopie des neuen primären Replikats Ihrer Datenbank erneut eine Verbindung herstellen. Wenn eine neue Verbindung versucht wird, während für die Datenbank eine Neukonfiguration durchgeführt wird, bevor das neue primäre Replikat online ist, erhalten Sie Fehler 40613 (Datenbank nicht verfügbar): „Datenbank '{Datenbankname}' auf Server '{Servername}' ist zurzeit nicht verfügbar. Wiederholen Sie den Verbindungsversuch später.“ Wenn Ihre Datenbank eine zeitintensive Abfrage aufweist, wird diese Abfrage während einer Neukonfiguration unterbrochen und muss neu gestartet werden.

## <a name="retry-logic"></a>Wiederholungslogik

Jede Clientproduktionsanwendung, die eine Verbindung mit einem Clouddatenbankdienst herstellt, sollte eine stabile [Wiederholungslogik](troubleshoot-common-connectivity-issues.md#retry-logic-for-transient-errors) für Verbindungen implementieren. Dies trägt zur Lösung dieser Situationen bei und sollte normalerweise dazu führen, dass die Fehler für den Endbenutzer transparent sind.

## <a name="frequency"></a>Häufigkeit

Durchschnittlich sollten 1,7 geplante Wartungsereignisse pro Monat auftreten.

## <a name="resource-health"></a>Resource Health

Wenn für Ihre Datenbank Anmeldefehler auftreten, sollten Sie im Fenster [Resource Health](../../service-health/resource-health-overview.md#get-started) im [Azure-Portal](https://portal.azure.com) den aktuellen Status überprüfen. Der Abschnitt „Integritätsverlauf“ enthält den Grund für die Ausfallzeit für jedes Ereignis (wenn verfügbar).

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich über [Resource Health](resource-health-to-troubleshoot-connectivity.md) für Azure SQL-Datenbank und verwaltete Azure SQL-Instanzen.
- Weitere Informationen zur Wiederholungslogik finden Sie unter [Wiederholungslogik für vorübergehende Fehler](troubleshoot-common-connectivity-issues.md#retry-logic-for-transient-errors).

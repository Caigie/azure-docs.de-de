---
title: Verwalten von Lesereplikaten – Azure-Portal – Azure Database for MariaDB
description: In diesem Artikel wird beschrieben, wie Sie im Portal Lesereplikate in Azure Database for MariaDB einrichten und verwalten.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: how-to
ms.date: 6/10/2020
ms.openlocfilehash: fc435194975c0b043e74a47632d6e38f12d04c2a
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86121196"
---
# <a name="how-to-create-and-manage-read-replicas-in-azure-database-for-mariadb-using-the-azure-portal"></a>Informationen zum Erstellen und Verwalten von Lesereplikaten in Azure Database for MariaDB mithilfe des Azure-Portals

In diesem Artikel erfahren Sie, wie Sie Lesereplikate im Azure Database for MariaDB-Dienst über das Azure-Portal erstellen und verwalten.

## <a name="prerequisites"></a>Voraussetzungen

- Ein [Azure Database for MariaDB-Server](quickstart-create-mariadb-server-database-using-azure-portal.md), der als Masterserver verwendet wird.

> [!IMPORTANT]
> Das Feature für Lesereplikate ist nur für Azure Database for MariaDB-Server in den Tarifen „Universell“ oder „Arbeitsspeicheroptimiert“ verfügbar. Stellen Sie sicher, dass für den Masterserver einer dieser Tarife festgelegt ist.

## <a name="create-a-read-replica"></a>Erstellen eines Lesereplikats

> [!IMPORTANT]
> Wenn Sie ein Replikat für einen Master erstellen, der keine vorhandenen Replikate hat, startet der Master zunächst neu, um sich auf die Replikation vorzubereiten. Beachten Sie dies, und führen Sie diese Vorgänge nicht zu Spitzenzeiten durch.

Ein Lesereplikatserver kann mit den folgenden Schritten erstellt werden:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

2. Wählen Sie den vorhandenen Azure Database for MariaDB-Server aus, den Sie als Masterserver verwenden möchten. Mit dieser Aktion wird die Seite **Übersicht** geöffnet.

3. Wählen Sie im Menü unter **EINSTELLUNGEN** die Option **Replikation** aus.

4. Wählen Sie **Replikat hinzufügen**.

   ![Azure Database for MariaDB: Replikation](./media/howto-read-replica-portal/add-replica.png)

5. Geben Sie einen Namen für den Replikatserver ein.

    ![Azure Database for MariaDB: Replikatname](./media/howto-read-replica-portal/replica-name.png)

6. Wählen Sie den Standort für den Replikatserver aus. Der Standardstandort ist mit dem des Masterservers identisch.

    ![Azure Database for MariaDB – Replikatstandort](./media/howto-read-replica-portal/replica-location.png)

7. Wählen Sie **OK** aus, um die Erstellung des Replikats zu bestätigen.

> [!NOTE]
> Lesereplikate werden mit der gleichen Serverkonfiguration wie der Masterserver erstellt. Die Replikatserverkonfiguration kann nach der Erstellung geändert werden. Für die Konfiguration des Replikatservers sollten mindestens die gleichen Werte verwendet werden wie für den Masterserver, damit das Replikat über genügend Kapazität verfügt.

Nach der Erstellung des Replikatservers kann dieser auf dem Blatt **Replikation** angezeigt werden.

   ![Azure Database for MariaDB: Auflisten der Replikate](./media/howto-read-replica-portal/list-replica.png)

## <a name="stop-replication-to-a-replica-server"></a>Beenden der Replikation auf einem Replikatserver

> [!IMPORTANT]
> Das Beenden der Replikation auf einem Server kann nicht rückgängig gemacht werden. Wenn die Replikation zwischen einem Master und dem Replikat beendet wurde, kann dies nicht rückgängig gemacht werden. Der Replikatserver wird zu einem eigenständigen Server und unterstützt nun Lese- und Schreibvorgänge. Der Server kann nicht wieder in ein Replikat umgewandelt werden.

Führen Sie die folgenden Schritte aus, um die Replikation zwischen einem Masterserver und einem Replikatserver im Azure-Portal zu beenden:

1. Wählen Sie im Azure-Portal Ihren Azure Database for MariaDB-Masterserver aus. 

2. Wählen Sie im Menü unter **EINSTELLUNGEN** die Option **Replikation** aus.

3. Wählen Sie den Replikatserver aus, für den Sie die Replikation beenden möchten.

   ![Azure Database for MariaDB: Beenden der Replikation, Auswählen des Servers](./media/howto-read-replica-portal/stop-replication-select.png)

4. Wählen Sie **Replikation beenden** aus.

   ![Azure Database for MariaDB: Beenden der Replikation](./media/howto-read-replica-portal/stop-replication.png)

5. Klicken Sie auf **OK**, um zu bestätigen, dass Sie die Replikation beenden möchten.

   ![Azure Database for MariaDB: Beenden der Replikation, Bestätigen](./media/howto-read-replica-portal/stop-replication-confirm.png)

## <a name="delete-a-replica-server"></a>Löschen eines Replikatservers

Führen Sie die folgenden Schritte aus, um einen Lesereplikatserver im Azure-Portal zu löschen:

1. Wählen Sie im Azure-Portal Ihren Azure Database for MariaDB-Masterserver aus.

2. Wählen Sie im Menü unter **EINSTELLUNGEN** die Option **Replikation** aus.

3. Wählen Sie den Replikatserver aus, den Sie löschen möchten.

   ![Azure Database for MariaDB: Löschen des Replikatservers](./media/howto-read-replica-portal/delete-replica-select.png)

4. Wählen Sie **Replikat löschen** aus.

   ![Azure Database for MariaDB: Löschen des Replikats](./media/howto-read-replica-portal/delete-replica.png)

5. Geben Sie den Namen des Replikats ein, und klicken Sie auf **Löschen**, um das Löschen des Replikats zu bestätigen.  

   ![Azure Database for MariaDB: Löschen des Replikats, Bestätigen](./media/howto-read-replica-portal/delete-replica-confirm.png)

## <a name="delete-a-master-server"></a>Löschen eines Masterservers

> [!IMPORTANT]
> Wenn Sie einen Masterserver löschen, wird die Replikation auf allen Replikatservern beendet und der Masterserver selbst gelöscht. Replikatserver werden zu eigenständigen Servern, die nun Lese- und Schreibvorgänge unterstützen.

Führen Sie die folgenden Schritte aus, um einen Masterserver im Azure-Portal zu löschen:

1. Wählen Sie im Azure-Portal Ihren Azure Database for MariaDB-Masterserver aus.

2. Wählen Sie unter **Übersicht** die Option **Löschen** aus.

   ![Azure Database for MariaDB: Löschen des Masters](./media/howto-read-replica-portal/delete-master-overview.png)

3. Geben Sie den Namen des Masterservers ein, und klicken Sie auf **Löschen**, um das Löschen des Masterservers zu bestätigen.  

   ![Azure Database for MariaDB: Löschen des Masters](./media/howto-read-replica-portal/delete-master-confirm.png)

## <a name="monitor-replication"></a>Überwachen der Replikation

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) den zu überwachenden Azure Database for MariaDB-Replikatserver aus.

2. Wählen Sie auf der Seitenleiste im Abschnitt **Überwachung** die Option **Metriken** aus:

3. Wählen Sie in der Dropdownliste der verfügbaren Metriken die Option **Replication lag in seconds** (Replikationsverzögerung in Sekunden) aus.

   ![Auswählen der Replikationsverzögerung](./media/howto-read-replica-portal/monitor-select-replication-lag.png)

4. Wählen Sie den Zeitraum aus, den Sie anzeigen möchten. In der folgenden Abbildung wird ein Zeitraum von 30 Minuten ausgewählt.

   ![Auswählen eines Zeitbereichs](./media/howto-read-replica-portal/monitor-replication-lag-time-range.png)

5. Zeigen Sie die Replikationsverzögerung für den ausgewählten Zeitraum an. Die folgende Abbildung zeigt die letzten 30 Minuten für eine große Workload.

   ![Auswählen eines Zeitbereichs](./media/howto-read-replica-portal/monitor-replication-lag-time-range-thirty-mins.png)

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu [Lesereplikaten](concepts-read-replicas.md)
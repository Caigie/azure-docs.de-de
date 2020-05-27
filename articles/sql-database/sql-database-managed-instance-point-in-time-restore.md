---
title: 'Verwaltete Instanz: Point-in-Time-Wiederherstellung (PITR)'
description: Wiederherstellen eines früheren Zeitpunkts einer SQL-Datenbank in einer verwalteten Instanz
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein, carlrab, mathoma
ms.date: 08/25/2019
ms.openlocfilehash: 21af96e8527bb3fc18666c0353ab3bdbe2a010f1
ms.sourcegitcommit: 318d1bafa70510ea6cdcfa1c3d698b843385c0f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/21/2020
ms.locfileid: "83773807"
---
# <a name="restore-a-sql-database-in-a-managed-instance-to-a-previous-point-in-time"></a>Wiederherstellen eines früheren Zeitpunkts einer SQL-Datenbank in einer verwalteten Instanz

Verwenden Sie die Point-in-Time-Wiederherstellung (Point-In-Time Restore, PITR), um eine Datenbank als Kopie einer anderen Datenbank in einem früheren Zustand zu erstellen. In diesem Artikel erfahren Sie, wie Sie eine Point-in-Time-Wiederherstellung einer Datenbank in einer verwalteten Azure SQL-Datenbank-Instanz durchführen.

Die Point-in-Time-Wiederherstellung ist in Wiederherstellungsszenarien hilfreich, die durch Fehler, falsch geladene Daten oder das Löschen wichtiger Daten verursacht wurden. Sie können sie aber auch einfach für Tests oder für die Überwachung verwenden. Sicherungsdateien werden je nach Datenbankeinstellungen für einen Zeitraum zwischen sieben und 35 Tagen aufbewahrt.

Mit der Point-in-Time-Wiederherstellung kann eine Datenbank wiederhergestellt werden:

- aus einer vorhandenen Datenbank.
- aus einer gelöschten Datenbank.
- in derselben verwalteten Instanz oder in einer anderen verwalteten Instanz. 

## <a name="limitations"></a>Einschränkungen

Die Point-in-Time-Wiederherstellung in einer verwalteten Instanz ist mit folgenden Einschränkungen verbunden:

- Bei der Wiederherstellung von einer verwalteten Instanz in eine andere müssen sich beide Instanzen im gleichen Abonnement und in der gleichen Region befinden. Die regions- und abonnementübergreifende Wiederherstellung wird derzeit nicht unterstützt.
- Eine Point-in-Time-Wiederherstellung einer gesamten verwalteten Instanz ist nicht möglich. Dieser Artikel beschreibt nur, was möglich ist: eine Point-in-Time-Wiederherstellung einer in einer verwalteten Instanz gehosteten Datenbank.

> [!WARNING]
> Berücksichtigen Sie die Speichergröße Ihrer verwalteten Instanz. Abhängig von der Größe der wiederherzustellenden Daten kann es vorkommen, dass der Instanzspeicher nicht ausreicht. Wenn nicht ausreichend Speicherplatz für die wiederhergestellten Daten verfügbar ist, müssen Sie anders vorgehen.

Die folgende Tabelle enthält Point-in-Time-Wiederherstellungsszenarien für verwaltete Instanzen:

|           |Wiederherstellen vorhandener Datenbanken in der gleichen verwalteten Instanz| Wiederherstellen vorhandener Datenbanken in einer anderen verwalteten Instanz|Wiederherstellen gelöschter Datenbanken in der gleichen verwalteten Instanz|Wiederherstellen gelöschter Datenbanken in einer anderen verwalteten Instanz|
|:----------|:----------|:----------|:----------|:----------|
|**Azure portal**| Ja|Nein |Ja|Nein|
|**Azure-Befehlszeilenschnittstelle**|Ja |Ja |Nein|Nein|
|**PowerShell**| Ja|Ja |Ja|Ja|

## <a name="restore-an-existing-database"></a>Wiederherstellen einer vorhandenen Datenbank

Eine vorhandene Datenbank kann über das Azure-Portal, mithilfe von PowerShell oder über die Azure CLI in derselben Instanz wiederhergestellt werden. Wenn Sie eine Datenbank in einer anderen Instanz wiederherstellen möchten, verwenden Sie PowerShell oder die Azure CLI, damit Sie die Eigenschaften für die verwaltete Zielinstanz und die Ressourcengruppe festlegen können. Wenn Sie diese Parameter nicht festlegen, wird die Datenbank standardmäßig in der vorhandenen Instanz wiederhergestellt. Das Azure-Portal unterstützt derzeit keine Wiederherstellung in einer anderen Instanz.

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. 
2. Navigieren Sie zu Ihrer verwalteten Instanz, und wählen Sie die wiederherzustellende Datenbank aus.
3. Wählen Sie auf der Datenbankseite die Option **Wiederherstellen** aus:

    ![Wiederherstellen einer Datenbank mithilfe des Azure-Portals](media/sql-database-managed-instance-point-in-time-restore/restore-database-to-mi.png)

4. Wählen Sie auf der Seite **Wiederherstellen** den Zeitpunkt (Datum und Uhrzeit) für die Datenbankwiederherstellung aus.
5. Wählen Sie **Bestätigen** aus, um Ihre Datenbank wiederherzustellen. Daraufhin wird der Wiederherstellungsprozess gestartet. Hierbei wird eine neue Datenbank erstellt und mit Daten aus der ursprünglichen Datenbank zum angegebenen Zeitpunkt aufgefüllt. Weitere Informationen zum Wiederherstellungsprozess finden Sie unter [Wiederherstellungszeit](sql-database-recovery-using-backups.md#recovery-time).

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Sollte Azure PowerShell noch nicht installiert sein, finden Sie unter [Installieren des Azure PowerShell-Moduls](https://docs.microsoft.com/powershell/azure/install-az-ps) weitere Informationen.

Wenn Sie die Datenbank unter Verwendung von PowerShell wiederherstellen möchten, legen Sie die Werte für die Parameter im folgenden Befehl fest. Führen Sie anschließend den folgenden Befehl aus:

```powershell-interactive
$subscriptionId = "<Subscription ID>"
$resourceGroupName = "<Resource group name>"
$managedInstanceName = "<Managed instance name>"
$databaseName = "<Source-database>"
$pointInTime = "2018-06-27T08:51:39.3882806Z"
$targetDatabase = "<Name of new database to be created>"

Get-AzSubscription -SubscriptionId $subscriptionId
Select-AzSubscription -SubscriptionId $subscriptionId

Restore-AzSqlInstanceDatabase -FromPointInTimeBackup `
                              -ResourceGroupName $resourceGroupName `
                              -InstanceName $managedInstanceName `
                              -Name $databaseName `
                              -PointInTime $pointInTime `
                              -TargetInstanceDatabaseName $targetDatabase `
```

Wenn Sie die Datenbank in einer anderen verwalteten Instanz wiederherstellen möchten, geben Sie auch die Namen der Zielressourcengruppe und der verwalteten Zielinstanz an:  

```powershell-interactive
$targetResourceGroupName = "<Resource group of target managed instance>"
$targetInstanceName = "<Target managed instance name>"

Restore-AzSqlInstanceDatabase -FromPointInTimeBackup `
                              -ResourceGroupName $resourceGroupName `
                              -InstanceName $managedInstanceName `
                              -Name $databaseName `
                              -PointInTime $pointInTime `
                              -TargetInstanceDatabaseName $targetDatabase `
                              -TargetResourceGroupName $targetResourceGroupName `
                              -TargetInstanceName $targetInstanceName 
```

Ausführliche Informationen finden Sie unter [Restore-AzSqlInstanceDatabase](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqlinstancedatabase).

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Sollte die Azure CLI noch nicht installiert sein, lesen Sie [Installieren der Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest).

Wenn Sie die Datenbank unter Verwendung der Azure CLI wiederherstellen möchten, legen Sie die Werte für die Parameter im folgenden Befehl fest. Führen Sie anschließend den folgenden Befehl aus:

```azurecli-interactive
az sql midb restore -g mygroupname --mi myinstancename |
-n mymanageddbname --dest-name targetmidbname --time "2018-05-20T05:34:22"
```

Wenn Sie die Datenbank in einer anderen verwalteten Instanz wiederherstellen möchten, geben Sie auch die Namen der Zielressourcengruppe und der verwalteten Zielinstanz an:  

```azurecli-interactive
az sql midb restore -g mygroupname --mi myinstancename -n mymanageddbname |
       --dest-name targetmidbname --time "2018-05-20T05:34:22" |
       --dest-resource-group mytargetinstancegroupname |
       --dest-mi mytargetinstancename
```

Eine ausführliche Erläuterung der verfügbaren Parameter finden Sie in der [CLI-Dokumentation zum Wiederherstellen einer Datenbank in einer verwalteten Instanz](https://docs.microsoft.com/cli/azure/sql/midb?view=azure-cli-latest#az-sql-midb-restore).

---

## <a name="restore-a-deleted-database"></a>Wiederherstellung einer gelöschten Datenbank

Eine gelöschte Datenbank kann nur über PowerShell oder das Azure-Portal wiederhergestellt werden. Zum Wiederherstellen einer gelöschten Datenbank in derselben Instanz verwenden Sie entweder das Azure-Portal oder PowerShell. Zum Wiederherstellen einer gelöschten Datenbank in einer anderen Instanz verwenden Sie PowerShell. 

### <a name="portal"></a>Portal 


Öffnen Sie zum Wiederherstellen einer verwalteten Datenbank über das Azure-Portal die Übersichtsseite der verwalteten Instanzen, und wählen Sie **Gelöschte Datenbanken** aus. Wählen Sie eine gelöschte Datenbank aus, die Sie wiederherstellen möchten, und geben Sie den Namen für die neue Datenbank ein, die mit den aus der Sicherung wiederhergestellten Daten erstellt wird.

  ![Screenshot: Wiederherstellen einer gelöschten Azure SQL-Datenbank-Instanz](./media/sql-database-recovery-using-backups/restore-deleted-sql-managed-instance-annotated.png)

### <a name="powershell"></a>PowerShell

Wenn Sie eine Datenbank in derselben Instanz wiederherstellen möchten, aktualisieren Sie die Parameterwerte, und führen Sie dann den folgenden PowerShell-Befehl aus: 

```powershell-interactive
$subscriptionId = "<Subscription ID>"
Get-AzSubscription -SubscriptionId $subscriptionId
Select-AzSubscription -SubscriptionId $subscriptionId

$resourceGroupName = "<Resource group name>"
$managedInstanceName = "<Managed instance name>"
$deletedDatabaseName = "<Source database name>"
$targetDatabaseName = "<target database name>"

$deletedDatabase = Get-AzSqlDeletedInstanceDatabaseBackup -ResourceGroupName $resourceGroupName `
-InstanceName $managedInstanceName -DatabaseName $deletedDatabaseName

Restore-AzSqlinstanceDatabase -Name $deletedDatabase.Name `
   -InstanceName $deletedDatabase.ManagedInstanceName `
   -ResourceGroupName $deletedDatabase.ResourceGroupName `
   -DeletionDate $deletedDatabase.DeletionDate `
   -PointInTime UTCDateTime `
   -TargetInstanceDatabaseName $targetDatabaseName
```

Wenn Sie die Datenbank in einer anderen verwalteten Instanz wiederherstellen möchten, geben Sie auch die Namen der Zielressourcengruppe und der verwalteten Zielinstanz an:

```powershell-interactive
$targetResourceGroupName = "<Resource group of target managed instance>"
$targetInstanceName = "<Target managed instance name>"

Restore-AzSqlinstanceDatabase -Name $deletedDatabase.Name `
   -InstanceName $deletedDatabase.ManagedInstanceName `
   -ResourceGroupName $deletedDatabase.ResourceGroupName `
   -DeletionDate $deletedDatabase.DeletionDate `
   -PointInTime UTCDateTime `
   -TargetInstanceDatabaseName $targetDatabaseName `
   -TargetResourceGroupName $targetResourceGroupName `
   -TargetInstanceName $targetInstanceName 
```

## <a name="overwrite-an-existing-database"></a>Überschreiben einer vorhandenen Datenbank

Wenn Sie eine vorhandene Datenbank überschreiben möchten, ist außerdem Folgendes erforderlich:

1. Löschen Sie die vorhandene Datenbank, die Sie überschreiben möchten.
2. Ändern Sie den Namen der mittels Point-in-Time-Wiederherstellung wiederhergestellten Datenbank in den Namen der gelöschten Datenbank.

### <a name="drop-the-original-database"></a>Löschen der ursprünglichen Datenbank

Sie können die Datenbank über das Azure-Portal, über PowerShell oder über T-SQL löschen.

Sie können die Datenbank auch löschen, indem Sie eine direkte Verbindung mit der verwalteten Instanz herstellen, SQL Server Management Studio (SSMS) starten und anschließend den folgenden T-SQL-Befehl (Transact-SQL) ausführen:

```sql
DROP DATABASE WorldWideImporters;
```

Verwenden Sie eine der folgenden Methoden, um eine Verbindung mit der Datenbank der verwalteten Instanz herzustellen:

- [SSMS/Azure Data Studio über einen virtuellen Azure-Computer](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-configure-vm)
- [Point-to-Site](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-configure-p2s)
- [Öffentlicher Endpunkt](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-public-endpoint-configure)

# <a name="portal"></a>[Portal](#tab/azure-portal)

Wählen Sie im Azure-Portal die Datenbank der verwalteten Instanz und anschließend die Option **Löschen** aus.

   ![Löschen einer Datenbank über das Azure-Portal](media/sql-database-managed-instance-point-in-time-restore/delete-database-from-mi.png)

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Verwenden Sie den folgenden PowerShell-Befehl, um eine vorhandene Datenbank aus einer verwalteten Instanz zu löschen:

```powershell
$resourceGroupName = "<Resource group name>"
$managedInstanceName = "<Managed instance name>"
$databaseName = "<Source database>"

Remove-AzSqlInstanceDatabase -Name $databaseName -InstanceName $managedInstanceName -ResourceGroupName $resourceGroupName
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Verwenden Sie den folgenden Azure CLI-Befehl, um eine vorhandene Datenbank aus einer verwalteten Instanz zu löschen:

```azurecli-interactive
az sql midb delete -g mygroupname --mi myinstancename -n mymanageddbname
```

---

### <a name="alter-the-new-database-name-to-match-the-original-database-name"></a>Ändern des Namens der neuen Datenbank, damit sie mit dem ursprünglichen Datenbanknamen übereinstimmt

Stellen Sie eine direkte Verbindung mit der verwalteten Instanz her und starten Sie SQL Server Management Studio. Führen Sie anschließend die folgende T-SQL-Abfrage (Transact-SQL) aus. Die Abfrage ändert den Namen der wiederhergestellten Datenbank in den Namen der gelöschten Datenbank, die Sie überschreiben möchten.

```sql
ALTER DATABASE WorldWideImportersPITR MODIFY NAME = WorldWideImporters;
```

Verwenden Sie eine der folgenden Methoden, um eine Verbindung mit der Datenbank der verwalteten Instanz herzustellen:

- [Virtueller Azure-Computer](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-configure-vm)
- [Point-to-Site](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-configure-p2s)
- [Öffentlicher Endpunkt](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-public-endpoint-configure)

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über [Automatisierte Sicherungen](sql-database-automated-backups.md).

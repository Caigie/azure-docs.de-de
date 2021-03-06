---
title: Logische Decodierung – Azure Database for PostgreSQL (Einzelserver)
description: Im Folgenden werden die logische Decodierung und wal2json für Change Data Capture in Azure Database for PostgreSQL (Einzelserver) beschrieben.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 06/22/2020
ms.openlocfilehash: bd886bea90c1092e38fac191a60a118aab0bef1f
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90903895"
---
# <a name="logical-decoding"></a>Logische Decodierung
 
Die [logische Decodierung in PostgreSQL](https://www.postgresql.org/docs/current/logicaldecoding.html) ermöglicht das Streamen von Datenänderungen an externe Consumer. Die logische Decodierung wird besonders häufig für das Ereignisstreaming und Change Data Capture-Szenarien verwendet.

Für die logische Decodierung wird ein Ausgabe-Plug-In verwendet, um das Write-Ahead-Protokoll (WAL) von Postgres in ein lesbares Format zu konvertieren. Azure Database for PostgreSQL bietet die Ausgabe-Plug-Ins [wal2json](https://github.com/eulerto/wal2json), [test_decoding](https://www.postgresql.org/docs/current/test-decoding.html) und „pgoutput“. „pgoutput“ wird ab der Postgres-Version 10 bereitgestellt.

Einen Überblick über die Funktionsweise der logischen Decodierung von Postgres finden Sie in [unserem Blog](https://techcommunity.microsoft.com/t5/azure-database-for-postgresql/change-data-capture-in-postgres-how-to-use-logical-decoding-and/ba-p/1396421). 

> [!NOTE]
> Die logische Decodierung in Azure Database for PostgreSQL (Einzelserver) befindet sich in der Public Preview.


## <a name="set-up-your-server"></a>Einrichten des Servers 
Die logische Decodierung und [Lesereplikate](concepts-read-replicas.md) erhalten Informationen jeweils vom Write-Ahead-Protokoll (Write Ahead Log, WAL) von Postgres. Die beiden Features benötigen unterschiedliche Protokolliergrade von Postgres. Die logische Decodierung erfordert einen höheren Grad an Protokollierung als Lesereplikate.

Um den richtigen Grad an Protokollierung zu konfigurieren, verwenden Sie den Parameter zur Unterstützung der Azure-Replikation. Für die Unterstützung der Azure-Replikation gibt es drei Einstellungsoptionen:

* **Off**: legt die wenigsten Informationen im Write-Ahead-Protokoll ab. Diese Einstellung ist für die meisten Azure Database for PostgreSQL-Server nicht verfügbar.  
* **Replica**: ausführlichere Informationen als bei Wahl von **Off**. Dies ist der erforderlich Mindestgrad an Protokollierung, damit [Lesereplikate](concepts-read-replicas.md) funktionieren. Das ist die Standardeinstellung auf den meisten Servern.
* **Logical**: noch ausführlichere Informationen als bei Wahl von **Replica**. Dies ist der erforderlich Mindestgrad an Protokollierung, damit die logische Decodierung funktioniert. Lesereplikate funktionieren auch bei dieser Einstellung.

Der Server muss nach einer Änderung dieses Parameters neu gestartet werden. Intern werden durch diesen Parameter die Postgres-Parameter `wal_level`, `max_replication_slots` und `max_wal_senders` festgelegt.

### <a name="using-azure-cli"></a>Verwenden der Azure-Befehlszeilenschnittstelle

1. Legen Sie „azure.replication_support“ auf `logical` fest.
   ```
   az postgres server configuration set --resource-group mygroup --server-name myserver --name azure.replication_support --value logical
   ``` 

2. Starten Sie den Server neu, um die Änderung zu übernehmen.
   ```
   az postgres server restart --resource-group mygroup --name myserver
   ```

### <a name="using-azure-portal"></a>Verwenden des Azure-Portals

1. Legen Sie die Azure-Replikationsunterstützung auf **logical** (logisch) fest. Wählen Sie **Speichern** aus.

   :::image type="content" source="./media/concepts-logical/replication-support.png" alt-text="Azure Database for PostgreSQL: Replikation: Azure-Replikationsunterstützung":::

2. Starten Sie den Server neu, um die Änderung zu übernehmen, indem Sie **Ja** auswählen.

   :::image type="content" source="./media/concepts-logical/confirm-restart.png" alt-text="Azure Database for PostgreSQL: Replikation: Bestätigen des Neustarts":::


## <a name="start-logical-decoding"></a>Starten der logischen Decodierung

Die logische Decodierung kann über das Streamingprotokoll oder die SQL-Schnittstelle genutzt werden. Beide Methoden verwenden [Replikationsslots](https://www.postgresql.org/docs/current/logicaldecoding-explanation.html#LOGICALDECODING-REPLICATION-SLOTS). Ein Slot stellt einen Datenstrom von Änderungen aus einem Singleton dar.

Die Verwendung eines Replikationsslots erfordert die Replikationsberechtigungen von Postgres. Derzeit ist die Replikationsberechtigung nur für den Serveradministrator verfügbar. 

### <a name="streaming-protocol"></a>Streamingprotokoll
Häufig ist das Verarbeiten von Änderungen mithilfe des Streamingprotokolls vorzuziehen. Sie können einen eigenen Consumer/Connector erstellen oder ein Tool wie [Debezium](https://debezium.io/) verwenden. 

In der wal2json-Dokumentation finden Sie [ein Beispiel für die Verwendung des Streamingprotokolls mit pg_recvlogical](https://github.com/eulerto/wal2json#pg_recvlogical).


### <a name="sql-interface"></a>SQL-Schnittstelle
Im folgenden Beispiel wird die SQL-Schnittstelle mit dem wal2json-Plug-In verwendet.
 
1. Erstellen Sie einen Slot.
   ```SQL
   SELECT * FROM pg_create_logical_replication_slot('test_slot', 'wal2json');
   ```
 
2. Geben Sie SQL-Befehle aus. Beispiel:
   ```SQL
   CREATE TABLE a_table (
      id varchar(40) NOT NULL,
      item varchar(40),
      PRIMARY KEY (id)
   );
   
   INSERT INTO a_table (id, item) VALUES ('id1', 'item1');
   DELETE FROM a_table WHERE id='id1';
   ```

3. Verarbeiten Sie die Änderungen.
   ```SQL
   SELECT data FROM pg_logical_slot_get_changes('test_slot', NULL, NULL, 'pretty-print', '1');
   ```

   Die Ausgabe sieht wie folgt aus:
   ```
   {
         "change": [
         ]
   }
   {
         "change": [
                  {
                           "kind": "insert",
                           "schema": "public",
                           "table": "a_table",
                           "columnnames": ["id", "item"],
                           "columntypes": ["character varying(40)", "character varying(40)"],
                           "columnvalues": ["id1", "item1"]
                  }
         ]
   }
   {
         "change": [
                  {
                           "kind": "delete",
                           "schema": "public",
                           "table": "a_table",
                           "oldkeys": {
                                 "keynames": ["id"],
                                 "keytypes": ["character varying(40)"],
                                 "keyvalues": ["id1"]
                           }
                  }
         ]
   }
   ```

4. Löschen Sie den Slot, wenn Sie ihn nicht mehr benötigen.
   ```SQL
   SELECT pg_drop_replication_slot('test_slot'); 
   ```


## <a name="monitoring-slots"></a>Überwachen von Slots

Sie müssen die logische Decodierung überwachen. Alle nicht verwendeten Replikationsslots müssen gelöscht werden. Slots halten Verbindungen zu den Postgres-WAL-Protokollen und den relevanten Systemkatalogen aufrecht, bis Änderungen von einem Consumer gelesen wurden. Wenn Ihr Consumer ausfällt oder nicht ordnungsgemäß konfiguriert wurde, werden die nicht verarbeiteten Protokolle aufbewahrt und füllen den Speicher. Außerdem erhöhen nicht verarbeitete Protokolle das Risiko von Transaktions-ID-Umläufen. Beide Situationen können dazu führen, dass der Server nicht mehr verfügbar ist. Daher ist es wichtig, dass die logischen Replikationsslots kontinuierlich verarbeitet werden. Wenn ein logischer Replikationsslot nicht mehr verwendet wird, löschen Sie ihn sofort.

In der Spalte „active“ (aktiv) in der Ansicht „pg_replication_slots“ wird angegeben, ob ein Consumer mit einem Slot verbunden ist.
```SQL
SELECT * FROM pg_replication_slots;
```

[Legen Sie Warnungen](howto-alert-on-metric.md) für die Metriken *Verwendeter Speicher* und *Maximale Verzögerung zwischen Replikaten* fest, damit Sie benachrichtigt werden, wenn diese Werte die normalen Schwellenwerte überschreiten. 

> [!IMPORTANT]
> Sie müssen nicht verwendete Replikationsslots löschen. Andernfalls kann es vorkommen, dass der Server nicht mehr verfügbar wird.

## <a name="how-to-drop-a-slot"></a>Löschen von Slots
Wenn Sie einen Replikationsslot nicht aktiv nutzen, sollten Sie ihn löschen.

So löschen Sie einen Replikationsslot namens `test_slot` mithilfe von SQL
```SQL
SELECT pg_drop_replication_slot('test_slot');
```

> [!IMPORTANT]
> Wenn Sie die logische Decodierung nicht mehr verwenden, ändern Sie „azure.replication_support“ zurück in `replica` oder `off`. Die von `logical` verwalteten WAL-Details sind ausführlicher und sollten deaktiviert werden, wenn die logische Decodierung nicht verwendet wird. 

 
## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zur [logischen Decodierung](https://www.postgresql.org/docs/current/logicaldecoding-explanation.html) finden Sie in der Postgres-Dokumentation.
* Wenden Sie sich an [unser Team](mailto:AskAzureDBforPostgreSQL@service.microsoft.com), wenn Sie Fragen zur logischen Decodierung haben.
* Informieren Sie sich über [Lesereplikate](concepts-read-replicas.md).


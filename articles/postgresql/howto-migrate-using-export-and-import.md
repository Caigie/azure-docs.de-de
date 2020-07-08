---
title: Migrieren einer Datenbank – Azure Database for PostgreSQL (Einzelserver)
description: Hier wird beschrieben, wie Sie eine PostgreSQL-Datenbank in eine Skriptdatei extrahieren und die Daten aus dieser Datei in die Zieldatenbank importieren.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: how-to
ms.date: 09/24/2019
ms.openlocfilehash: 5d97f7b976ef1dfcf0493a8bef6ddcdd2cd63960
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86116367"
---
# <a name="migrate-your-postgresql-database-using-export-and-import"></a>Migrieren der PostgreSQL-Datenbank mit Export und Import
Sie können mit [pg_dump](https://www.postgresql.org/docs/current/static/app-pgdump.html) eine PostgreSQL-Datenbank in eine Skriptdatei extrahieren und die Daten mit [psql](https://www.postgresql.org/docs/current/static/app-psql.html) aus dieser Datei in die Zieldatenbank importieren.

## <a name="prerequisites"></a>Voraussetzungen
Zum Ausführen der Schritte in dieser Anleitung benötigen Sie Folgendes:
- Einen [Server für Azure-Datenbank für PostgreSQL](quickstart-create-server-database-portal.md) mit Firewallregeln, um den Zugriff und eine Datenbank darunter zu ermöglichen
- Das Befehlszeilenprogramm [pg_dump](https://www.postgresql.org/docs/current/static/app-pgdump.html) muss installiert sein.
- Das Befehlszeilenprogramm [psql](https://www.postgresql.org/docs/current/static/app-psql.html) muss installiert sein.

Führen Sie die nachfolgend aufgeführten Schritte zum Exportieren und Importieren Ihrer PostgreSQL-Datenbank aus.

## <a name="create-a-script-file-using-pg_dump-that-contains-the-data-to-be-loaded"></a>Erstellen einer Skriptdatei mit pg_dump, die die zu ladenden Daten enthält
Führen Sie zum Exportieren der lokalen oder auf einem virtuellen Computer vorhandenen PostgreSQL-Datenbank in eine SQL-Skriptdatei den folgenden Befehl in Ihrer vorhandenen Umgebung aus:
```bash
pg_dump –-host=<host> --username=<name> --dbname=<database name> --file=<database>.sql
```
Wenn Sie z.B. einen lokalen Server mit einer Datenbank namens **testdb** verwenden:
```bash
pg_dump --host=localhost --username=masterlogin --dbname=testdb --file=testdb.sql
```

## <a name="import-the-data-on-target-azure-database-for-postgresql"></a>Importieren der Daten auf den Azure Database for PostgreSQL-Zielserver
Sie können die psql-Befehlszeile und den Parameter --dbname (-d) verwenden, um die Daten auf den Azure Database for PostgreSQL-Server zu importieren und Daten aus der SQL-Datei zu laden.
```bash
psql --file=<database>.sql --host=<server name> --port=5432 --username=<user@servername> --dbname=<target database name>
```
In diesem Beispiel werden das psql-Hilfsprogramm und eine Skriptdatei namens **testdb.sql** aus dem vorherigen Schritt verwendet, um Daten in die Datenbank **mypgsqldb** auf dem Zielserver **mydemoserver.postgres.database.azure.com** zu importieren.
```bash
psql --file=testdb.sql --host=mydemoserver.database.windows.net --port=5432 --username=mylogin@mydemoserver --dbname=mypgsqldb
```

## <a name="next-steps"></a>Nächste Schritte
- Informationen zum Migrieren einer PostgreSQL-Datenbank durch Sichern und Wiederherstellen finden Sie unter [Migrieren der PostgreSQL-Datenbank durch Sichern und Wiederherstellen](howto-migrate-using-dump-and-restore.md).
- Weitere Informationen zum Migrieren von Datenbanken zu Azure Database for PostgreSQL finden Sie im [Leitfaden zur Datenbankmigration](https://aka.ms/datamigration). 

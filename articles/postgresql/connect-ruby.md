---
title: 'Schnellstart: Herstellen einer Verbindung mit Ruby: Azure Database for PostgreSQL-Einzelserver'
description: Dieser Schnellstart enthält ein Ruby-Codebeispiel, das Sie nutzen können, um eine Verbindung mit einem Azure Database for PostgreSQL-Einzelserver herzustellen und Daten daraus abzufragen.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.custom: mvc
ms.devlang: ruby
ms.topic: quickstart
ms.date: 5/6/2019
ms.openlocfilehash: ede55196793e7eb19a5f64e539ee870ff0fada9d
ms.sourcegitcommit: 3fc3457b5a6d5773323237f6a06ccfb6955bfb2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2020
ms.locfileid: "90031415"
---
# <a name="quickstart-use-ruby-to-connect-and-query-data-in-azure-database-for-postgresql---single-server"></a>Schnellstart: Verwenden von Ruby zum Herstellen einer Verbindung mit einem Azure Database for PostgreSQL-Einzelserver sowie zum Abfragen von Daten

Dieser Schnellstart zeigt, wie Sie mit einer [Ruby](https://www.ruby-lang.org)-Anwendung eine Verbindung mit einer Azure-Datenbank für PostgreSQL herstellen. Es wird veranschaulicht, wie Sie SQL-Anweisungen zum Abfragen, Einfügen, Aktualisieren und Löschen von Daten in der Datenbank verwenden. Bei den Schritten in diesem Abschnitt wird davon ausgegangen, dass Sie mit der Ruby-Entwicklung vertraut sind und noch keine Erfahrung mit Azure Database for PostgreSQL haben.

## <a name="prerequisites"></a>Voraussetzungen
In diesem Schnellstart werden die Ressourcen, die in den folgenden Anleitungen erstellt wurden, als Startpunkt verwendet:
- [Erstellen einer Datenbank – Portal](quickstart-create-server-database-portal.md)
- [Erstellen einer Datenbank – Azure-CLI](quickstart-create-server-database-azure-cli.md)

Darüber hinaus muss Folgendes installiert sein:
- [Ruby](https://www.ruby-lang.org/en/downloads/)
- Ruby pg, das PostgreSQL-Modul für Ruby

## <a name="get-connection-information"></a>Abrufen von Verbindungsinformationen
Rufen Sie die Verbindungsinformationen ab, die zum Herstellen einer Verbindung mit der Azure-Datenbank für PostgreSQL erforderlich sind. Sie benötigen den vollqualifizierten Servernamen und die Anmeldeinformationen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Klicken Sie im Azure-Portal im linken Menü auf **Alle Ressourcen**, und suchen Sie dann nach dem soeben erstellten Server, z.B. **mydemoserver**.
3. Klicken Sie auf den Servernamen.
4. Notieren Sie sich im Bereich **Übersicht** des Servers den **Servernamen** und den **Anmeldenamen des Serveradministrators**. Wenn Sie Ihr Kennwort vergessen haben, können Sie es in diesem Bereich auch zurücksetzen.
 ![Azure Database for PostgreSQL-Servername](./media/connect-ruby/1-connection-string.png)

> [!NOTE]
> Das Symbol `@` beim Azure-Postgres-Benutzernamen wurde in allen Verbindungszeichenfolgen als URL mit `%40` codiert. 

## <a name="connect-and-create-a-table"></a>Herstellen einer Verbindung und Erstellen einer Tabelle
Verwenden Sie den folgenden Code, um eine Verbindung herzustellen und eine Tabelle zu erstellen, indem Sie eine **CREATE TABLE**-SQL-Anweisung gefolgt von **INSERT INTO**-SQL-Anweisungen zum Hinzufügen von Zeilen zur Tabelle nutzen.

Der Code verwendet ein [PG::Connection](https://www.rubydoc.info/gems/pg/PG/Connection)-Objekt mit dem Konstruktor [new()](https://www.rubydoc.info/gems/pg/PG/Connection#initialize-instance_method), um eine Verbindung mit Azure-Datenbank für PostgreSQL herzustellen. Anschließend wird die [exec()](https://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method)-Methode aufgerufen, um die Befehle DROP, CREATE TABLE und INSERT INTO auszuführen. Der Code führt mit der [PG::Error](https://www.rubydoc.info/gems/pg/PG/Error)-Klasse eine Überprüfung auf Fehler durch. Anschließend wird die [close()](https://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method)-Methode aufgerufen, um die Verbindung vor dem Beenden zu schließen.

Ersetzen Sie die Zeichenfolgen `host`, `database`, `user` und `password` durch Ihre eigenen Werte. 


```ruby
require 'pg'

begin
    # Initialize connection variables.
    host = String('mydemoserver.postgres.database.azure.com')
    database = String('postgres')
    user = String('mylogin%40mydemoserver')
    password = String('<server_admin_password>')

    # Initialize connection object.
    connection = PG::Connection.new(:host => host, :user => user, :dbname => database, :port => '5432', :password => password)
    puts 'Successfully created connection to database'

    # Drop previous table of same name if one exists
    connection.exec('DROP TABLE IF EXISTS inventory;')
    puts 'Finished dropping table (if existed).'

    # Drop previous table of same name if one exists.
    connection.exec('CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);')
    puts 'Finished creating table.'

    # Insert some data into table.
    connection.exec("INSERT INTO inventory VALUES(1, 'banana', 150)")
    connection.exec("INSERT INTO inventory VALUES(2, 'orange', 154)")
    connection.exec("INSERT INTO inventory VALUES(3, 'apple', 100)")
    puts 'Inserted 3 rows of data.'

rescue PG::Error => e
    puts e.message 
    
ensure
    connection.close if connection
end
```

## <a name="read-data"></a>Lesen von Daten
Verwenden Sie den folgenden Code, um die Daten mit einer **SELECT**-SQL-Anweisung zu verbinden und zu lesen. 

Der Code verwendet ein [PG::Connection](https://www.rubydoc.info/gems/pg/PG/Connection)-Objekt mit dem Konstruktor [new()](https://www.rubydoc.info/gems/pg/PG/Connection#initialize-instance_method), um eine Verbindung mit Azure-Datenbank für PostgreSQL herzustellen. Anschließend wird die [exec()](https://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method)-Methode aufgerufen, um den SELECT-Befehl auszuführen, und die Ergebnisse werden in einem Resultset vorgehalten. Die Resultset-Sammlung wird mit der Schleife `resultSet.each do` durchlaufen, wobei sich die aktuellen Zeilenwerte in der Variablen `row` befinden. Der Code führt mit der [PG::Error](https://www.rubydoc.info/gems/pg/PG/Error)-Klasse eine Überprüfung auf Fehler durch. Anschließend wird die [close()](https://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method)-Methode aufgerufen, um die Verbindung vor dem Beenden zu schließen.

Ersetzen Sie die Zeichenfolgen `host`, `database`, `user` und `password` durch Ihre eigenen Werte. 

```ruby
require 'pg'

begin
    # Initialize connection variables.
    host = String('mydemoserver.postgres.database.azure.com')
    database = String('postgres')
    user = String('mylogin%40mydemoserver')
    password = String('<server_admin_password>')

    # Initialize connection object.
    connection = PG::Connection.new(:host => host, :user => user, :database => dbname, :port => '5432', :password => password)
    puts 'Successfully created connection to database.'

    resultSet = connection.exec('SELECT * from inventory;')
    resultSet.each do |row|
        puts 'Data row = (%s, %s, %s)' % [row['id'], row['name'], row['quantity']]
    end

rescue PG::Error => e
    puts e.message 
    
ensure
    connection.close if connection
end
```

## <a name="update-data"></a>Aktualisieren von Daten
Verwenden Sie den folgenden Code, um eine Verbindung herzustellen und die Daten per **UPDATE**-SQL-Anweisung zu aktualisieren.

Der Code verwendet ein [PG::Connection](https://www.rubydoc.info/gems/pg/PG/Connection)-Objekt mit dem Konstruktor [new()](https://www.rubydoc.info/gems/pg/PG/Connection#initialize-instance_method), um eine Verbindung mit Azure-Datenbank für PostgreSQL herzustellen. Anschließend wird die [exec()](https://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method)-Methode aufgerufen, um den UPDATE-Befehl auszuführen. Der Code führt mit der [PG::Error](https://www.rubydoc.info/gems/pg/PG/Error)-Klasse eine Überprüfung auf Fehler durch. Anschließend wird die [close()](https://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method)-Methode aufgerufen, um die Verbindung vor dem Beenden zu schließen.

Ersetzen Sie die Zeichenfolgen `host`, `database`, `user` und `password` durch Ihre eigenen Werte. 

```ruby
require 'pg'

begin
    # Initialize connection variables.
    host = String('mydemoserver.postgres.database.azure.com')
    database = String('postgres')
    user = String('mylogin%40mydemoserver')
    password = String('<server_admin_password>')

    # Initialize connection object.
    connection = PG::Connection.new(:host => host, :user => user, :dbname => database, :port => '5432', :password => password)
    puts 'Successfully created connection to database.'

    # Modify some data in table.
    connection.exec('UPDATE inventory SET quantity = %d WHERE name = %s;' % [200, '\'banana\''])
    puts 'Updated 1 row of data.'

rescue PG::Error => e
    puts e.message 
    
ensure
    connection.close if connection
end
```


## <a name="delete-data"></a>Löschen von Daten
Verwenden Sie den folgenden Code, um die Daten mit einer **DELETE**-SQL-Anweisung zu verbinden und zu lesen. 

Der Code verwendet ein [PG::Connection](https://www.rubydoc.info/gems/pg/PG/Connection)-Objekt mit dem Konstruktor [new()](https://www.rubydoc.info/gems/pg/PG/Connection#initialize-instance_method), um eine Verbindung mit Azure-Datenbank für PostgreSQL herzustellen. Anschließend wird die [exec()](https://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method)-Methode aufgerufen, um den UPDATE-Befehl auszuführen. Der Code führt mit der [PG::Error](https://www.rubydoc.info/gems/pg/PG/Error)-Klasse eine Überprüfung auf Fehler durch. Anschließend wird die [close()](https://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method)-Methode aufgerufen, um die Verbindung vor dem Beenden zu schließen.

Ersetzen Sie die Zeichenfolgen `host`, `database`, `user` und `password` durch Ihre eigenen Werte. 

```ruby
require 'pg'

begin
    # Initialize connection variables.
    host = String('mydemoserver.postgres.database.azure.com')
    database = String('postgres')
    user = String('mylogin%40mydemoserver')
    password = String('<server_admin_password>')

    # Initialize connection object.
    connection = PG::Connection.new(:host => host, :user => user, :dbname => database, :port => '5432', :password => password)
    puts 'Successfully created connection to database.'

    # Modify some data in table.
    connection.exec('DELETE FROM inventory WHERE name = %s;' % ['\'orange\''])
    puts 'Deleted 1 row of data.'

rescue PG::Error => e
    puts e.message 
    
ensure
    connection.close if connection
end
```

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Migrieren der Datenbank mit Export und Import](./howto-migrate-using-export-and-import.md)

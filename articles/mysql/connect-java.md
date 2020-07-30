---
title: 'Herstellen einer Verbindung unter Verwendung von Java: Azure Database for MySQL'
description: Dieser Schnellstart enthält ein Java-Codebeispiel, das Sie nutzen können, um mit den Daten von Azure Database for MySQL eine Verbindung herzustellen und Abfragen dafür durchzuführen.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.custom: mvc, devcenter, seo-java-july2019, seo-java-august2019, devx-track-java
ms.topic: quickstart
ms.devlang: java
ms.date: 5/26/2020
ms.openlocfilehash: 4da7efc40177937aad8f1a97e909d90c4cf5f466
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/28/2020
ms.locfileid: "87322855"
---
# <a name="quickstart-use-java-to-connect-to-and-query-data-in-azure-database-for-mysql"></a>Schnellstart: Verwenden von Java zum Herstellen einer Verbindung mit Azure Database for MySQL und zum Abfragen von Daten

In dieser Schnellstartanleitung stellen Sie unter Verwendung einer Java-Anwendung und des JDBC-Treibers „MariaDB Connector/J“ eine Verbindung mit einer Azure Database for MySQL-Instanz her. Anschließend verwenden Sie SQL-Anweisungen, um Daten in der Datenbank über Mac-, Ubuntu Linux- und Windows-Plattformen abzufragen, einzufügen, zu aktualisieren und zu löschen. 

In diesem Thema wird davon ausgegangen, dass Sie mit der Java-Entwicklung vertraut sind, aber noch keine Erfahrung mit Azure Database for MySQL haben.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
- Ein Azure Database for MySQL-Server. [Erstellen Sie einen Azure Database for MySQL-Server mithilfe des Azure-Portals](quickstart-create-mysql-server-database-using-azure-portal.md), oder [erstellen Sie einen Azure Database for MySQL-Server mithilfe der Azure CLI](quickstart-create-mysql-server-database-using-azure-cli.md).
- Die Sicherheit Ihrer Azure Database for MySQL-Verbindung ist so konfiguriert, dass die Firewall geöffnet ist und SSL-Verbindungseinstellungen für Ihre Anwendung festgelegt sind.

> [!IMPORTANT] 
> Sicherstellen, dass der IP-Adresse, über die Sie eine Verbindung herstellen, die Firewallregeln des Servers über das [Azure-Portal](./howto-manage-firewall-using-portal.md) oder die [Azure CLI](./howto-manage-firewall-using-cli.md) hinzugefügt wurden

## <a name="obtain-the-mariadb-connector"></a>Beziehen des MariaDB-Connectors

Rufen Sie den Connector [MariaDB Connector/J](https://mariadb.com/kb/en/library/mariadb-connector-j/) mit einer der folgenden Methoden ab:
   - Verwenden Sie das Maven-Paket [mariadb-java-client](https://search.maven.org/search?q=a:mariadb-java-client), um die [mariadb-java-client-Abhängigkeit](https://mvnrepository.com/artifact/org.mariadb.jdbc/mariadb-java-client) in die POM-Datei für Ihr Projekt aufzunehmen.
   - Laden Sie den JDBC-Treiber [MariaDB Connector/J](https://downloads.mariadb.org/connector-java/) herunter, und nehmen Sie die JDBC-JAR-Datei (beispielsweise „mariadb-java-client-2.4.3.jar“) in den Klassenpfad Ihrer Anwendung auf. Ausführlichere Informationen zu Klassenpfaden finden Sie in der Dokumentation der Umgebung (beispielsweise [Apache Tomcat](https://tomcat.apache.org/tomcat-7.0-doc/class-loader-howto.html) oder [Java SE](https://docs.oracle.com/javase/7/docs/technotes/tools/windows/classpath.html)).

## <a name="get-connection-information"></a>Abrufen von Verbindungsinformationen

Rufen Sie die Verbindungsinformationen ab, die zum Herstellen einer Verbindung mit der Azure SQL-Datenbank für MySQL erforderlich sind. Sie benötigen den vollqualifizierten Servernamen und die Anmeldeinformationen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Wählen Sie im Azure-Portal im linken Menü **Alle Ressourcen** aus, und suchen Sie dann nach dem Server, den Sie erstellt haben (z.B. **mydemoserver**).
3. Wählen Sie den Servernamen aus.
4. Notieren Sie sich im Bereich **Übersicht** des Servers den **Servernamen** und den **Anmeldenamen des Serveradministrators**. Wenn Sie Ihr Kennwort vergessen haben, können Sie es in diesem Bereich auch zurücksetzen.
 ![Servername für Azure-Datenbank für MySQL](./media/connect-java/azure-database-mysql-server-name.png)

## <a name="connect-create-table-and-insert-data"></a>Herstellen der Verbindung, Erstellen der Tabelle und Einfügen von Daten

Verwenden Sie den folgenden Code, um die Verbindung herzustellen und die Daten zu laden, indem Sie die Funktion mit einer **INSERT**-SQL-Anweisung nutzen. Die [getConnection()](https://mariadb.com/kb/en/library/about-mariadb-connector-j/#using-drivermanager)-Methode wird verwendet, um eine Verbindung mit MySQL herzustellen. Mit den Methoden [createStatement()](https://mariadb.com/kb/en/library/about-mariadb-connector-j/#creating-a-table-on-a-mariadb-or-mysql-server) und „execute()“ wird die Tabelle verworfen und erstellt. Mit dem „prepareStatement“-Objekt werden die Einfügebefehle erstellt, und „setString()“ und „setInt()“ werden zum Binden der Parameterwerte genutzt. Mit der „executeUpdate()“-Methode wird der Befehl für jeden Parametersatz ausgeführt, um die Werte einzufügen. 

Ersetzen Sie die Parameter „host“, „database“, „user“ und „password“ durch die Werte, die Sie beim Erstellen Ihres eigenen Servers und der Datenbank angegeben haben.

```java
import java.sql.*;
import java.util.Properties;

public class CreateTableInsertRows {

    public static void main (String[] args)  throws Exception
    {
        // Initialize connection variables. 
        String host = "mydemoserver.mysql.database.azure.com";
        String database = "quickstartdb";
        String user = "myadmin@mydemoserver";
        String password = "<server_admin_password>";

        Connection connection = null;

        // Initialize connection object
        try
        {
            String url = String.format("jdbc:mariadb://%s/%s", host, database);

            // Set connection properties.
            Properties properties = new Properties();
            properties.setProperty("user", user);
            properties.setProperty("password", password);
            properties.setProperty("useSSL", "true");
            properties.setProperty("verifyServerCertificate", "true");
            properties.setProperty("requireSSL", "false");

            // get connection
            connection = DriverManager.getConnection(url, properties);
        }
        catch (SQLException e)
        {
            throw new SQLException("Failed to create connection to database.", e);
        }
        if (connection != null) 
        { 
            System.out.println("Successfully created connection to database.");
        
            // Perform some SQL queries over the connection.
            try
            {
                // Drop previous table of same name if one exists.
                Statement statement = connection.createStatement();
                statement.execute("DROP TABLE IF EXISTS inventory;");
                System.out.println("Finished dropping table (if existed).");
    
                // Create table.
                statement.execute("CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);");
                System.out.println("Created table.");
                
                // Insert some data into table.
                int nRowsInserted = 0;
                PreparedStatement preparedStatement = connection.prepareStatement("INSERT INTO inventory (name, quantity) VALUES (?, ?);");
                preparedStatement.setString(1, "banana");
                preparedStatement.setInt(2, 150);
                nRowsInserted += preparedStatement.executeUpdate();

                preparedStatement.setString(1, "orange");
                preparedStatement.setInt(2, 154);
                nRowsInserted += preparedStatement.executeUpdate();

                preparedStatement.setString(1, "apple");
                preparedStatement.setInt(2, 100);
                nRowsInserted += preparedStatement.executeUpdate();
                System.out.println(String.format("Inserted %d row(s) of data.", nRowsInserted));
    
                // NOTE No need to commit all changes to database, as auto-commit is enabled by default.
    
            }
            catch (SQLException e)
            {
                throw new SQLException("Encountered an error when executing given sql statement.", e);
            }       
        }
        else {
            System.out.println("Failed to create connection to database.");
        }
        System.out.println("Execution finished.");
    }
}

```

## <a name="read-data"></a>Lesen von Daten

Verwenden Sie den folgenden Code, um die Daten mit einer **SELECT**-SQL-Anweisung zu lesen. Die [getConnection()](https://mariadb.com/kb/en/library/about-mariadb-connector-j/#using-drivermanager)-Methode wird verwendet, um eine Verbindung mit MySQL herzustellen. Die Methoden [createStatement()](https://mariadb.com/kb/en/library/about-mariadb-connector-j/#creating-a-table-on-a-mariadb-or-mysql-server) und „executeQuery()“ werden verwendet, um die SELECT-Anweisung zu verbinden und auszuführen. Die Ergebnisse werden mit einem ResultSet-Objekt verarbeitet. 

Ersetzen Sie die Parameter „host“, „database“, „user“ und „password“ durch die Werte, die Sie beim Erstellen Ihres eigenen Servers und der Datenbank angegeben haben.

```java
import java.sql.*;
import java.util.Properties;

public class ReadTable {

    public static void main (String[] args)  throws Exception
    {
        // Initialize connection variables.
        String host = "mydemoserver.mysql.database.azure.com";
        String database = "quickstartdb";
        String user = "myadmin@mydemoserver";
        String password = "<server_admin_password>";

        Connection connection = null;

        // Initialize connection object
        try
        {
            String url = String.format("jdbc:mariadb://%s/%s", host, database);

            // Set connection properties.
            Properties properties = new Properties();
            properties.setProperty("user", user);
            properties.setProperty("password", password);
            properties.setProperty("useSSL", "true");
            properties.setProperty("verifyServerCertificate", "true");
            properties.setProperty("requireSSL", "false");
            
            // get connection
            connection = DriverManager.getConnection(url, properties);
        }
        catch (SQLException e)
        {
            throw new SQLException("Failed to create connection to database", e);
        }
        if (connection != null) 
        { 
            System.out.println("Successfully created connection to database.");
        
            // Perform some SQL queries over the connection.
            try
            {
    
                Statement statement = connection.createStatement();
                ResultSet results = statement.executeQuery("SELECT * from inventory;");
                while (results.next())
                {
                    String outputString = 
                        String.format(
                            "Data row = (%s, %s, %s)",
                            results.getString(1),
                            results.getString(2),
                            results.getString(3));
                    System.out.println(outputString);
                }
            }
            catch (SQLException e)
            {
                throw new SQLException("Encountered an error when executing given sql statement", e);
            }       
        }
        else {
            System.out.println("Failed to create connection to database."); 
        }
        System.out.println("Execution finished.");
    }
}
```

## <a name="update-data"></a>Aktualisieren von Daten

Verwenden Sie den folgenden Code, um die Daten mit einer **UPDATE**-SQL-Anweisung zu ändern. Die [getConnection()](https://mariadb.com/kb/en/library/about-mariadb-connector-j/#using-drivermanager)-Methode wird verwendet, um eine Verbindung mit MySQL herzustellen. Die Methoden [prepareStatement()](https://docs.oracle.com/javase/tutorial/jdbc/basics/prepared.html) und „executeUpdate()“ werden verwendet, um die Verbindung für die UPDATE-Anweisung herzustellen und sie vorzubereiten und auszuführen. 

Ersetzen Sie die Parameter „host“, „database“, „user“ und „password“ durch die Werte, die Sie beim Erstellen Ihres eigenen Servers und der Datenbank angegeben haben.

```java
import java.sql.*;
import java.util.Properties;

public class UpdateTable {
    public static void main (String[] args)  throws Exception
    {
        // Initialize connection variables. 
        String host = "mydemoserver.mysql.database.azure.com";
        String database = "quickstartdb";
        String user = "myadmin@mydemoserver";
        String password = "<server_admin_password>";

        Connection connection = null;

        // Initialize connection object
        try
        {
            String url = String.format("jdbc:mariadb://%s/%s", host, database);
            
            // set up the connection properties
            Properties properties = new Properties();
            properties.setProperty("user", user);
            properties.setProperty("password", password);
            properties.setProperty("useSSL", "true");
            properties.setProperty("verifyServerCertificate", "true");
            properties.setProperty("requireSSL", "false");

            // get connection
            connection = DriverManager.getConnection(url, properties);
        }
        catch (SQLException e)
        {
            throw new SQLException("Failed to create connection to database.", e);
        }
        if (connection != null) 
        { 
            System.out.println("Successfully created connection to database.");
        
            // Perform some SQL queries over the connection.
            try
            {
                // Modify some data in table.
                int nRowsUpdated = 0;
                PreparedStatement preparedStatement = connection.prepareStatement("UPDATE inventory SET quantity = ? WHERE name = ?;");
                preparedStatement.setInt(1, 200);
                preparedStatement.setString(2, "banana");
                nRowsUpdated += preparedStatement.executeUpdate();
                System.out.println(String.format("Updated %d row(s) of data.", nRowsUpdated));
    
                // NOTE No need to commit all changes to database, as auto-commit is enabled by default.
            }
            catch (SQLException e)
            {
                throw new SQLException("Encountered an error when executing given sql statement.", e);
            }       
        }
        else {
            System.out.println("Failed to create connection to database.");
        }
        System.out.println("Execution finished.");
    }
}
```

## <a name="delete-data"></a>Löschen von Daten

Verwenden Sie den folgenden Code, um Daten mit einer **DELETE**-SQL-Anweisung zu entfernen. Die [getConnection()](https://mariadb.com/kb/en/library/about-mariadb-connector-j/#using-drivermanager)-Methode wird verwendet, um eine Verbindung mit MySQL herzustellen.  Die Methoden [prepareStatement()](https://docs.oracle.com/javase/tutorial/jdbc/basics/prepared.html) und „executeUpdate()“ werden verwendet, um DELETE-Anweisung vorzubereiten und auszuführen. 

Ersetzen Sie die Parameter „host“, „database“, „user“ und „password“ durch die Werte, die Sie beim Erstellen Ihres eigenen Servers und der Datenbank angegeben haben.

```java
import java.sql.*;
import java.util.Properties;

public class DeleteTable {
    public static void main (String[] args)  throws Exception
    {
        // Initialize connection variables.
        String host = "mydemoserver.mysql.database.azure.com";
        String database = "quickstartdb";
        String user = "myadmin@mydemoserver";
        String password = "<server_admin_password>";

        Connection connection = null;

        // Initialize connection object
        try
        {
            String url = String.format("jdbc:mariadb://%s/%s", host, database);
            
            // set up the connection properties
            Properties properties = new Properties();
            properties.setProperty("user", user);
            properties.setProperty("password", password);
            properties.setProperty("useSSL", "true");
            properties.setProperty("verifyServerCertificate", "true");
            properties.setProperty("requireSSL", "false");
            
            // get connection
            connection = DriverManager.getConnection(url, properties);
        }
        catch (SQLException e)
        {
            throw new SQLException("Failed to create connection to database", e);
        }
        if (connection != null) 
        { 
            System.out.println("Successfully created connection to database.");
        
            // Perform some SQL queries over the connection.
            try
            {
                // Delete some data from table.
                int nRowsDeleted = 0;
                PreparedStatement preparedStatement = connection.prepareStatement("DELETE FROM inventory WHERE name = ?;");
                preparedStatement.setString(1, "orange");
                nRowsDeleted += preparedStatement.executeUpdate();
                System.out.println(String.format("Deleted %d row(s) of data.", nRowsDeleted));
    
                // NOTE No need to commit all changes to database, as auto-commit is enabled by default.
            }
            catch (SQLException e)
            {
                throw new SQLException("Encountered an error when executing given sql statement.", e);
            }       
        }
        else {
            System.out.println("Failed to create connection to database.");
        }
        System.out.println("Execution finished.");
    }
}
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Migrieren der MySQL-Datenbank auf Azure-Datenbank für MySQL durch Sicherungen und Wiederherstellungen](concepts-migrate-dump-restore.md)

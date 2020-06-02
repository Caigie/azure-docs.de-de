---
title: Herstellen einer Verbindung mit SQL Server oder einer Azure SQL-Datenbank
description: Automatisieren von Aufgaben für SQL-Datenbanken lokal oder in der Cloud mithilfe von Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: estfan, jonfan, logicappspm
ms.topic: conceptual
ms.date: 05/12/2020
tags: connectors
ms.openlocfilehash: c32e17aaf83c233ad77bbbf607c30cc526253352
ms.sourcegitcommit: 90d2d95f2ae972046b1cb13d9956d6668756a02e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/14/2020
ms.locfileid: "83402598"
---
# <a name="automate-workflows-for-sql-server-or-azure-sql-database-by-using-azure-logic-apps"></a>Automatisieren von Workflows für SQL Server oder Azure SQL-Datenbank mithilfe von Azure Logic Apps

In diesem Artikel wird beschrieben, wie Sie über eine Logik-App mit dem SQL Server-Connector auf Daten in Ihrer SQL-Datenbank zugreifen können. Auf diese Weise können Sie durch Erstellung von Logik-Apps Aufgaben, Prozesse oder Workflows automatisieren, die SQL-Daten und Ressourcen verwalten. Der SQL Server-Connector funktioniert sowohl für [lokale SQL Server-Instanzen](https://docs.microsoft.com/sql/sql-server/sql-server-technical-documentation) als auch für die [cloudbasierte Azure SQL-Datenbank](https://docs.microsoft.com/azure/sql-database/sql-database-technical-overview).

Sie können Logik-Apps erstellen, deren Ausführung durch Ereignisse in Ihrer SQL-Datenbank oder in anderen Systemen (wie z. B. Dynamics CRM Online) ausgelöst wird. Ihre Logik-Apps können auch Daten abrufen, einfügen und löschen sowie SQL-Abfragen und gespeicherte Prozeduren ausführen. Sie können beispielsweise eine Logik-App erstellen, die Dynamics CRM Online automatisch auf neue Datensätze überprüft, Ihrer SQL-Datenbank Elemente für alle neuen Datensätze hinzufügt und dann E-Mail-Benachrichtigungen über die hinzugefügten Elemente sendet.

Falls Sie noch nicht mit Logik-Apps vertraut sind, finden Sie weitere Informationen unter [Was ist Azure Logic Apps?](../logic-apps/logic-apps-overview.md) und [Schnellstart: Erstellen Ihres ersten automatisierten Workflows mit Azure Logic Apps – Azure-Portal](../logic-apps/quickstart-create-first-logic-app-workflow.md). Connectorspezifische technische Informationen, Einschränkungen und bekannte Probleme finden Sie auf der [Referenzseite zum SQL Server-Connector](https://docs.microsoft.com/connectors/sql/).

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Falls Sie kein Abonnement besitzen, können Sie sich [für ein kostenloses Azure-Konto registrieren](https://azure.microsoft.com/free/).

* Eine [SQL Server-Datenbank](https://docs.microsoft.com/sql/relational-databases/databases/create-a-database) oder eine [Azure SQL-Datenbank](../sql-database/sql-database-get-started-portal.md)

  Ihre Tabellen müssen Daten enthalten, damit Ihre Logik-App beim Aufrufen von Vorgängen Ergebnisse zurückgeben kann. Wenn Sie eine Azure SQL-Datenbank erstellen, können Sie die enthaltenen Beispieldatenbanken verwenden.

* Der Name Ihres SQL-Servers, Datenbankname, Ihr Benutzername und Ihr Kennwort. Sie benötigen diese Anmeldeinformationen, um Ihre Logik für den Zugriff auf Ihren SQL-Server autorisieren zu können.

  * Für SQL Server finden Sie diese Details in der Verbindungszeichenfolge:

    `Server={your-server-address};Database={your-database-name};User Id={your-user-name};Password={your-password};`

  * Für Azure SQL-Datenbank finden Sie diese Details in der Verbindungszeichenfolge oder im Azure-Portal unter den Eigenschaften der SQL-Datenbank:

    `Server=tcp:{your-server-name}.database.windows.net,1433;Initial Catalog={your-database-name};Persist Security Info=False;User ID={your-user-name};Password={your-password};MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;`

* Für diese Szenarien muss das [lokale Datengateway](../logic-apps/logic-apps-gateway-install.md) auf einem lokalen Computer installiert sein, und eine [Azure-Datengatewayressource muss im Azure-Portal erstellt worden sein](../logic-apps/logic-apps-gateway-connection.md):

  * Ihre Logik-Apps werden nicht in einer [Integrationsdienstumgebung (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) ausgeführt.

  * Ihre Logik-Apps *werden* in einer Integrationsdienstumgebung ausgeführt, aber Sie müssen Windows-Authentifizierung für Ihre SQL Server-Verbindung verwenden. Verwenden Sie für dieses Szenario die Nicht-ISE-Version des SQL Server-Connectors zusammen mit dem Datengateway, da die ISE-Version keine Windows-Authentifizierung unterstützt.

* Die Logik-App, über die Sie auf Ihre SQL-Datenbank zugreifen müssen. Um Ihre Logik-App mit einem SQL-Trigger starten zu können, benötigen Sie eine [leere Logik-App](../logic-apps/quickstart-create-first-logic-app-workflow.md).

<a name="add-sql-trigger"></a>

## <a name="add-a-sql-trigger"></a>Hinzufügen eines SQL-Triggers

In Azure Logic Apps muss jede Logik-App mit einem [Trigger](../logic-apps/logic-apps-overview.md#logic-app-concepts) beginnen, der ausgelöst wird, wenn ein bestimmtes Ereignis eintritt oder eine bestimmte Bedingung erfüllt wird. Bei jeder Auslösung des Triggers erstellt das Logic Apps-Modul eine Logik-App-Instanz und startet die Ausführung des Workflows Ihrer Logik-App.

1. Erstellen Sie im Azure-Portal oder in Visual Studio eine leere Logik-App, die den Logic Apps-Designer öffnet. In diesem Beispiel wird das Azure-Portal verwendet.

1. Geben Sie im Suchfeld des Designers „sql server“ als Filter ein. Wählen Sie in der Triggerliste den gewünschten SQL-Trigger aus.

   In diesem Beispiel wird der Trigger **Wenn ein Element erstellt wird** verwendet.

   ![Trigger „Wenn ein Element erstellt wird“ auswählen](./media/connectors-create-api-sqlazure/select-sql-server-trigger.png)

1. Wenn Sie zum Erstellen einer Verbindung aufgefordert werden, [erstellen Sie jetzt Ihre SQL-Verbindung](#create-connection). Wenn die Verbindung bereits vorhanden ist, wählen Sie einen **Tabellennamen** aus.

   ![Gewünschte Tabelle auswählen](./media/connectors-create-api-sqlazure/azure-sql-database-table.png)

1. Legen Sie die Eigenschaften **Intervall** und **Häufigkeit** fest, die angeben, wie oft Ihre Logik-App die Tabelle überprüft.

   Mit diesem Trigger wird nur eine Zeile aus der ausgewählten Tabelle zurückgegeben, sonst nichts. Zum Ausführen weiterer Aufgaben fügen Sie weitere Aktionen hinzu, mit denen die gewünschten Aufgaben ausgeführt werden. Wenn Sie z.B. die Daten in dieser Zeile anzeigen möchten, können Sie weitere Aktionen hinzufügen, die eine Datei erstellen, die die Felder aus der zurückgegebenen Zeile enthält, und dann E-Mail-Benachrichtigungen senden. Informationen zu anderen verfügbaren Aktionen für diesen Connector finden Sie auf der [Referenzseite zum Connector](https://docs.microsoft.com/connectors/sql/).

1. Wenn Sie fertig sind, wählen Sie auf der Symbolleiste des Designers die Option **Speichern** aus.

   Durch diesen Schritt wird Ihre Logik-App in Azure automatisch aktiviert und live veröffentlicht.

<a name="add-sql-action"></a>

## <a name="add-a-sql-action"></a>Hinzufügen einer SQL-Aktion

In Azure Logic Apps handelt es sich bei einer [Aktion](../logic-apps/logic-apps-overview.md#logic-app-concepts) um einen Schritt in Ihrem Workflow, der einem Trigger oder einer anderen Aktion folgt. In diesem Beispiel beginnt die Logik-App mit dem [Wiederholungstrigger](../connectors/connectors-native-recurrence.md) und ruft eine Aktion auf, die eine Zeile aus einer SQL-Datenbank abruft.

1. Öffnen Sie im Azure-Portal oder in Visual Studio Ihre Logik-App im Logic Apps-Designer. In diesem Beispiel wird das Azure-Portal verwendet.

1. Wählen Sie unter dem Trigger oder der Aktion, dem bzw. der Sie die SQL-Aktion hinzufügen möchten, die Option **Neuer Schritt** aus.

   ![Hinzufügen eines neuen Schritts zu ihrer Logik-App](./media/connectors-create-api-sqlazure/select-new-step-logic-app.png)

   Um eine Aktion zwischen vorhandenen Schritten hinzuzufügen, bewegen Sie den Mauszeiger über den Verbindungspfeil. Wählen Sie das angezeigte Pluszeichen ( **+** ) aus, und wählen Sie dann **Aktion hinzufügen** aus.

1. Geben Sie unter **Aktion auswählen** im Suchfeld „sql server“ als Filter ein. Wählen Sie in der Liste der Aktionen die gewünschte SQL-Aktion aus.

   In diesem Beispiel wird die Aktion **Zeile abrufen** verwendet, die einen einzelnen Datensatz abruft.

   ![SQL-Aktion „Zeile abrufen“ suchen und auswählen](./media/connectors-create-api-sqlazure/find-select-sql-get-row-action.png)

   Mit dieser Aktion wird nur eine Zeile aus der ausgewählten Tabelle zurückgegeben, sonst nichts. Um die Daten in dieser Zeile anzuzeigen, können Sie weitere Aktionen hinzufügen, die eine Datei erstellen, die die Felder aus der zurückgegebenen Zeile enthält,und diese Datei in einem Cloudspeicherkonto speichern. Informationen zu anderen verfügbaren Aktionen für diesen Connector finden Sie auf der [Referenzseite zum Connector](https://docs.microsoft.com/connectors/sql/).

1. Wenn Sie zum Erstellen einer Verbindung aufgefordert werden, [erstellen Sie jetzt Ihre SQL-Verbindung](#create-connection). Falls Ihre Verbindung bereits vorhanden ist, wählen Sie einen **Tabellennamen** aus, und geben Sie die **Zeilen-ID** für den gewünschten Datensatz ein.

   ![Den Tabellennamen und die Zeilen-ID eingeben](./media/connectors-create-api-sqlazure/specify-table-row-id-property-value.png)

1. Wenn Sie fertig sind, wählen Sie auf der Symbolleiste des Designers die Option **Speichern** aus.

   Durch diesen Schritt wird Ihre Logik-App in Azure automatisch aktiviert und live veröffentlicht.

<a name="create-connection"></a>

## <a name="connect-to-your-database"></a>Verbinden mit der Datenbank

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

[!INCLUDE [Create a connection to SQL Server or Azure SQL Database](../../includes/connectors-create-api-sqlazure.md)]

## <a name="handle-bulk-data"></a>Verarbeiten von Massendaten

Gelegentlich müssen Sie mit Resultsets arbeiten, die so groß sind, dass der Connector nicht alle Ergebnisse gleichzeitig zurückgibt. Oder Sie wünschen sich eine bessere Kontrolle über die Größe und Struktur Ihrer Resultsets. Hier sind einige Möglichkeiten, wie Sie derartig große Resultsets verarbeiten können:

* Aktivieren Sie *Paginierung*, um die Ergebnisse in kleineren Gruppen zu verwalten. Weitere Informationen finden Sie unter [Abrufen von Massendaten, Datensätzen und Elementen mithilfe der Paginierung](../logic-apps/logic-apps-exceed-default-page-size-with-pagination.md).

* Erstellen Sie eine gespeicherte Prozedur, die die Ergebnisse nach Ihren Vorstellungen organisiert.

  Beim Abrufen oder Einfügen mehrerer Zeilen kann Ihre Logik-App diese Zeilen mithilfe einer [*Until-Schleife*](../logic-apps/logic-apps-control-flow-loops.md#until-loop) innerhalb dieser [Grenzwerte](../logic-apps/logic-apps-limits-and-config.md) durchlaufen. Wenn Ihre Logik-App jedoch mit so großen Datensatzgruppen arbeiten muss, die z. B. Tausende oder Millionen von Zeilen umfassen, dass Sie die Kosten für Aufrufe der Datenbank minimieren möchten.

  Um die Ergebnisse in der von Ihnen gewünschten Weise zu organisieren, können Sie eine [*gespeicherte Prozedur*](https://docs.microsoft.com/sql/relational-databases/stored-procedures/stored-procedures-database-engine) erstellen, die in Ihrer SQL-Instanz ausgeführt wird und die **SELECT - ORDER BY**-Anweisung verwendet. Diese Lösung bietet Ihnen mehr Kontrolle über die Größe und Struktur Ihrer Ergebnisse. Ihre Logik-App ruft die gespeicherte Prozedur mithilfe der Aktion **Gespeicherte Prozedur ausführen** des SQL Server-Connectors auf.

  Weitere Details dieser Lösung finden Sie in den folgenden Artikeln:

  * [SQL-Paginierung für die Massendatenübertragung mit Logic Apps](https://social.technet.microsoft.com/wiki/contents/articles/40060.sql-pagination-for-bulk-data-transfer-with-logic-apps.aspx)

  * [SELECT - ORDER BY-Klausel](https://docs.microsoft.com/sql/t-sql/queries/select-order-by-clause-transact-sql)

### <a name="handle-dynamic-bulk-data"></a>Behandeln dynamischer Massendaten

Wenn Sie eine gespeicherte Prozedur im SQL Server-Connector aufzurufen, ist die zurückgegebene Ausgabe manchmal dynamisch. Führen Sie in diesem Szenario die folgenden Schritte aus:

1. Öffnen Sie den **Logik-App-Designer**.
1. Führen Sie einen Testlauf Ihrer Logik-App aus, um das Ausgabeformat anzuzeigen. Kopieren Sie die Beispielausgabe.
1. Wählen Sie im Designer unter der Aktion, in der die gespeicherte Prozedur aufgerufen wird, **Neuer Schritt** aus.
1. Suchen Sie unter **Aktion auswählen** nach der Aktion [**JSON analysieren**](../logic-apps/logic-apps-perform-data-operations.md#parse-json-action), und wählen Sie sie aus.
1. Wählen Sie in der Aktion **Parse JSON** die Option **Beispielnutzlast zum Generieren eines Schemas verwenden**.
1. Fügen Sie im Fenster **Geben oder fügen Sie eine JSON-Beispielnutzlast ein** Ihre Beispielnutzlast ein, und wählen Sie dann **Fertig** aus.
1. Wenn Sie eine Fehlermeldung erhalten, dass Logic Apps kein Schema generieren kann, überprüfen Sie, ob die Syntax der Beispielausgabe richtig formatiert ist. Wenn Sie das Schema noch immer nicht generieren können, geben Sie es manuell in das Feld **Schema** ein.
1. Wählen Sie auf der Symbolleiste des Designers **Speichern** aus.
1. Wenn Sie auf die JSON-Inhaltseigenschaften zugreifen möchten, verwenden Sie die Datentoken, die in der Liste mit den dynamischen Inhalten unter der [Aktion **JSON analysieren**](../logic-apps/logic-apps-perform-data-operations.md#parse-json-action) angezeigt werden.

## <a name="connector-specific-details"></a>Connectorspezifische Details

Technische Informationen zu den Triggern, Aktionen und Limits dieses Connectors finden Sie auf der [Referenzseite zum Connector](https://docs.microsoft.com/connectors/sql/).

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über andere [Connectors für Azure Logic Apps](../connectors/apis-list.md).

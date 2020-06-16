---
title: Herstellen einer Verbindung mit SAP-Systemen
description: Zugreifen auf und Verwalten von SAP-Ressourcen durch das Automatisieren von Workflows mit Azure Logic Apps
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: estfan, daviburg, logicappspm
ms.topic: article
ms.date: 05/29/2020
tags: connectors
ms.openlocfilehash: 557e162d9d7f0238d5554c32cb3ae96885877dbe
ms.sourcegitcommit: 12f23307f8fedc02cd6f736121a2a9cea72e9454
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/30/2020
ms.locfileid: "84220499"
---
# <a name="connect-to-sap-systems-from-azure-logic-apps"></a>Herstellen einer Verbindung zu SAP-Systemen: Azure Logic Apps

> [!IMPORTANT]
> Die älteren Connectors für SAP-Anwendungsserver und SAP-Nachrichtenserver sind seit dem 29. Februar 2020 veraltet. Der aktuelle SAP-Connector fasst diese älteren SAP-Connectors zusammen, damit Sie den Verbindungstyp nicht ändern müssen. Darüber hinaus ist er vollständig mit vorherigen Connectors kompatibel, bietet zahlreiche Zusatzfunktionen und verwendet weiterhin die SAP-.NET-Connectorbibliothek (SAP NCo).
>
> Für Logik-Apps, die die älteren Connectors verwenden, muss vor dem Veraltungsdatum eine [Migration zum neuesten Connector](#migrate) durchgeführt werden. Andernfalls treten bei diesen Logik-Apps Ausführungsfehler auf, und es können keine Nachrichten an Ihr SAP-System gesendet werden.

In diesem Artikel wird gezeigt, wie Sie aus einer Logik-App auf Ihre lokalen SAP-Ressourcen zugreifen, indem Sie den SAP-Connector verwenden. Der Connector arbeitet mit den klassischen SAP-Releases wie R/3- und ECC-Systemen lokal zusammen. Der Connector ermöglicht auch die Integration mit den neueren HANA-basierten SAP-Systemen wie S/4 HANA, unabhängig davon, ob sie lokal oder in der Cloud gehostet werden. Der SAP-Connector unterstützt die Nachrichten- oder Datenintegration in SAP NetWeaver-basierten Systemen über Intermediate Document (IDoc), Business Application Programming Interface (BAPI) oder Remote Function Call (RFC).

Der SAP-Connector verwendet die [SAP-NCo-Bibliothek (.NET-Connector)](https://support.sap.com/en/product/connectors/msnet.html) und stellt die folgenden Aktionen bereit:

* **Nachricht an SAP senden:** Senden von IDoc über tRFC, Abrufen von BAPI-Funktionen über RFC und Abrufen von RFC/tRFC in SAP-Systemen

* **Beim Empfang einer Nachricht von SAP:** Empfangen von IDoc über tRFC, Abrufen von BAPI-Funktionen über tRFC und Abrufen von RFC/tRFC in SAP-Systemen

* **Schemas generieren:** Generieren von Schemas für die SAP-Artefakte für IDoc, BAPI oder RFC.

Für diese Vorgänge unterstützt der SAP-Connector die Basisauthentifizierung über Benutzername und Kennwort. Der Connector unterstützt auch [Secure Network Communications (SNC)](https://help.sap.com/doc/saphelp_nw70/7.0.31/e6/56f466e99a11d1a5b00000e835363f/content.htm?no_cache=true). SNC kann für SAP NetWeaver-SSO (einmaliges Anmelden) oder für zusätzliche Sicherheitsfunktionen eines externen Sicherheitsprodukts verwendet werden.

In diesem Artikel wird das Erstellen von in SAP integrierbaren Beispiel-Logik-Apps gezeigt. Gleichzeitig werden die zuvor beschriebenen Integrationsszenarien veranschaulicht. Für Logik-Apps, die die älteren SAP-Connectors verwenden, wird in diesem Artikel erläutert, wie Sie Ihre Logik-Apps zum neuesten SAP-Connector migrieren.

<a name="pre-reqs"></a>

## <a name="prerequisites"></a>Voraussetzungen

Um diesem Artikel weiter folgen zu können, benötigen Sie Folgendes:

* Ein Azure-Abonnement. Wenn Sie noch kein Azure-Abonnement haben, [melden Sie sich für ein kostenloses Azure-Konto an](https://azure.microsoft.com/free/).

* Die Logik-App, von der aus Sie auf Ihr SAP-System zugreifen möchten, und einen Trigger, der Ihren Logik-App-Workflow startet. Falls Sie noch nicht mit Logik-Apps vertraut sind, finden Sie weitere Informationen unter [Was ist Azure Logic Apps?](../logic-apps/logic-apps-overview.md) und [Schnellstart: Erstellen Ihres ersten automatisierten Workflows mit Azure Logic Apps – Azure-Portal](../logic-apps/quickstart-create-first-logic-app-workflow.md).

* Ihr [SAP-Anwendungsserver](https://wiki.scn.sap.com/wiki/display/ABAP/ABAP+Application+Server) oder der [SAP-Nachrichtenserver](https://help.sap.com/saphelp_nw70/helpdata/en/40/c235c15ab7468bb31599cc759179ef/frameset.htm)

* Nachrichteninhalte, die Sie an Ihren SAP-Server senden können, z. B. eine IDoc-Beispieldatei, müssen im XML-Format vorliegen und den Namespace für die SAP-Aktion enthalten, die Sie verwenden möchten.

* Wenn Sie den Trigger **Beim Empfang einer Nachricht von SAP** verwenden möchten, müssen Sie zudem die folgenden Setupschritte ausführen:

  * Richten Sie die Sicherheitsberechtigungen für Ihr SAP-Gateway mit der folgenden Einstellung ein:

    `"TP=Microsoft.PowerBI.EnterpriseGateway HOST=<gateway-server-IP-address> ACCESS=*"`

  * Richten Sie die Sicherheitsprotokollierung für Ihr SAP-Gateway ein, mit der Sie nach Fehlern in der Zugriffssteuerungsliste suchen können. Diese Funktion ist standardmäßig deaktiviert. Andernfalls wird die folgende Fehlermeldung angezeigt:

    `"Registration of tp Microsoft.PowerBI.EnterpriseGateway from host <host-name> not allowed"`

    Weitere Informationen finden Sie im SAP-Hilfethema [Einrichten der Gatewayprotokollierung](https://help.sap.com/erp_hcm_ias2_2015_02/helpdata/en/48/b2a710ca1c3079e10000000a42189b/frameset.htm).

<a name="multi-tenant"></a>

### <a name="multi-tenant-azure-prerequisites"></a>Voraussetzungen für eine mehrinstanzenfähige Azure-Umgebung

Diese Voraussetzungen müssen erfüllt werden, wenn Ihre Logik-Apps in einer mehrinstanzenfähigen Azure-Umgebung ausgeführt werden und Sie den verwalteten SAP-Connector verwenden möchten. Dieser Connector wird nativ nicht in einer [Integrationsdienstumgebung (Integration Service Environment, ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) ausgeführt. Wenn Sie hingegen eine ISE der Premium-Ebene sowie den nativ in ISEs ausgeführten SAP-Connector verwenden möchten, finden Sie weitere Informationen im Abschnitt [Voraussetzungen für die Integrationsdienstumgebung (ISE)](#sap-ise).

Der verwaltete SAP-Connector (Nicht-ISE) kann über das [lokale Datengateway](../logic-apps/logic-apps-gateway-connection.md) mit lokalen SAP-Systemen integriert werden. In Szenarios, in denen beispielsweise eine Nachricht von einer Logik-App an ein SAP-System gesendet wird, agiert das Datengateway als RFC-Client und leitet die von der Logik-App erhaltenen Anforderungen an SAP weiter. Ebenso fungiert das Datengateway in Empfangsszenarios als RFC-Server, der Anforderungen von SAP empfängt und an die Logik-App weiterleitet.

* [Laden Sie das lokale Datengateway](../logic-apps/logic-apps-gateway-install.md) auf Ihren lokalen Computer herunter, und installieren Sie es. Erstellen Sie anschließend im Azure-Portal eine [Azure-Gatewayressource](../logic-apps/logic-apps-gateway-connection.md#create-azure-gateway-resource) für dieses Gateway. Das Gateway hilft Ihnen, sicher auf lokale Daten und Ressourcen zuzugreifen.

  Als bewährte Vorgehensweise wird die Verwendung einer unterstützten Version des lokalen Datengateways empfohlen. Microsoft veröffentlicht jeden Monat eine neue Version. Derzeit werden von Microsoft die letzten sechs Versionen unterstützt. Führen Sie bei einem Problem mit dem Gateway ein [Upgrade auf die neueste Version](https://aka.ms/on-premises-data-gateway-installer) durch, die möglicherweise Updates zur Behebung des Problems enthält.

* [Laden Sie die neueste SAP-Clientbibliothek](#sap-client-library-prerequisites) auf denselben Computer herunter, auf dem sich bereits das lokale Datengateway befindet, und installieren Sie sie.

<a name="sap-ise"></a>

### <a name="integration-service-environment-ise-prerequisites"></a>Voraussetzungen für die Integrationsdienstumgebung (ISE)

Diese Voraussetzungen müssen erfüllt sein, wenn Ihre Logik-Apps in einer [Integrationsdienstumgebung (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) der Premium-Ebene (nicht der Entwicklerebene) ausgeführt werden und Sie den nativ in ISEs ausgeführten SAP-Connector verwenden möchten. Eine ISE ermöglicht den Zugriff auf Ressourcen, die von einem virtuellen Azure-Netzwerk geschützt werden, und stellt weitere nativ in ISEs ausgeführte Connectors bereit, mit denen Logik-Apps direkt auf lokale Ressourcen zugreifen können, ohne das lokale Datengateway verwenden zu müssen.

> [!NOTE]
> Obwohl der SAP-Connector für die ISE in einer Integrationsdienstumgebung der Entwicklerebene angezeigt wird, kann er von dort nicht installiert werden.

1. Wenn Sie noch nicht über ein Azure Storage-Konto und einen Blobcontainer verfügen, erstellen Sie einen Container im [Azure-Portal](../storage/blobs/storage-quickstart-blobs-portal.md) oder im [Azure Storage-Explorer](../storage/blobs/storage-quickstart-blobs-storage-explorer.md).

1. [Laden Sie die neueste SAP-Clientbibliothek](#sap-client-library-prerequisites) auf Ihren lokalen Computer herunter, und installieren Sie sie. Sie sollten über die folgenden Assemblydateien verfügen:

   * libicudecnumber.dll
   * rscp4n.dll
   * sapnco.dll
   * sapnco_utils.dll

1. Erstellen Sie eine ZIP-Datei, die diese Assemblys enthält, und laden Sie dieses Paket in Ihren Blobcontainer in Azure Storage hoch.

1. Navigieren Sie entweder im Azure-Portal oder im Azure Storage-Explorer zum Speicherort des Containers, in den Sie die ZIP-Datei hochgeladen haben.

1. Kopieren Sie die URL für diesen Speicherort, und achten Sie darauf, dass Sie das SAS-Token (Shared Access Signature) miteinschließen.

   Andernfalls wird das SAS-Token nicht autorisiert, und die Bereitstellung des SAP-Connectors für die ISE schlägt fehl.

1. Bevor Sie den SAP-Connector für die ISE verwenden können, müssen Sie ihn in Ihrer ISE installieren und bereitstellen.

   1. Suchen Sie im [Azure-Portal](https://portal.azure.com) nach Ihrer ISE, und öffnen Sie sie.
   
   1. Klicken Sie im ISE-Menü auf **Verwaltete Connectors** > **Hinzufügen**. Suchen Sie in der Liste der Connectors nach **SAP**, und wählen Sie diesen Connector aus.
   
   1. Fügen Sie im Bereich **Neuen verwalteten Connector hinzufügen** im Feld **SAP-Paket** die URL für die ZIP-Datei mit den SAP-Assemblys ein. *Vergewissern Sie sich, dass das SAS-Token enthalten ist.*

   1. Wählen Sie **Erstellen**, wenn Sie fertig sind.

   Weitere Informationen finden Sie unter [Hinzufügen von ISE-Connectors](../logic-apps/add-artifacts-integration-service-environment-ise.md#add-ise-connectors-environment).

1. Wenn sich Ihre SAP-Instanz und die ISE in unterschiedlichen virtuellen Netzwerken befinden, müssen Sie [mittels Peering](../virtual-network/tutorial-connect-virtual-networks-portal.md) eine Verbindung zwischen dem virtuellen Netzwerk der ISE und dem virtuellen Netzwerk Ihrer SAP-Instanz herstellen.

<a name="sap-client-library-prerequisites"></a>

### <a name="sap-client-library-prerequisites"></a>Voraussetzungen für die SAP-Clientbibliothek

* Stellen Sie sicher, dass Sie über die neueste Version verfügen: [SAP-Connector (NCo 3.0) für Microsoft .NET 3.0.22.0 mit .NET Framework 4.0 kompiliert – Windows 64-Bit (x64)](https://softwaredownloads.sap.com/file/0020000001000932019). Ältere Versionen können zu Kompatibilitätsproblemen führen. Weitere Informationen finden Sie im Abschnitt [Versionen der SAP-Clientbibliothek](#sap-library-versions).

* Standardmäßig speichert das SAP-Installationsprogramm die Assemblydateien im Standardinstallationsordner. Kopieren Sie auf folgende Weise die Assemblydateien gemäß Ihrem Szenario an einen anderen Speicherort:

  Führen Sie für Logik-Apps, die in einer ISE ausgeführt werden, die im Abschnitt [Voraussetzungen für die Integrationsdienstumgebung (ISE)](#sap-ise) beschriebenen Schritte aus. Kopieren Sie für Logik-Apps, die in einer mehrinstanzenfähigen Azure-Umgebung ausgeführt werden und das lokale Datengateway verwenden, die Assemblydateien aus dem Standardinstallationsordner in den Installationsordner des Datengateways. Überprüfen Sie bei Problemen mit dem Datengateway die folgenden Punkte:

  * Da das Datengateway nur in 64-Bit-Systemen ausgeführt werden kann, müssen Sie die 64-Bit-Version der SAP-Clientbibliothek installieren. Andernfalls erhalten Sie einen Fehler „Ungültige Image“, weil der Gateway-Hostdienst 32-Bit-Assemblys nicht unterstützt.

  * Tritt für die SAP-Verbindung die Fehlermeldung „Bitte überprüfen Sie Ihre Kontoinformationen und/oder Berechtigungen, und wiederholen Sie den Vorgang“ auf, befinden sich die Assemblydateien möglicherweise nicht am richtigen Speicherort. Überprüfen Sie, ob Sie die Assemblydateien in den Installationsordner des Datengateways kopiert haben.

    Mithilfe der [.NET-Assemblybindungs-Protokollanzeige](https://docs.microsoft.com/dotnet/framework/tools/fuslogvw-exe-assembly-binding-log-viewer) können Sie überprüfen, ob sich die Assemblydateien am richtigen Speicherort befinden. Optional können Sie bei der Installation der SAP-Clientbibliothek auch die Option **Globalen Assemblycache registrieren** aktivieren.

<a name="sap-library-versions"></a>

#### <a name="sap-client-library-versions"></a>Versionen der SAP-Clientbibliothek

Bei früheren SAP-NCo-Versionen konnte es zu einem Deadlock kommen, wenn mehrere IDoc-Nachrichten gleichzeitig gesendet werden. Dadurch werden auch alle späteren Nachrichten blockiert, die an das SAP-Ziel gesendet werden, und es kommt zu einem Timeout der Nachricht.

Zwischen der SAP-Clientbibliothek, .NET Framework, der .NET-Laufzeit und dem Gateway bestehen folgende Beziehungen:

* Sowohl der SAP-Adapter von Microsoft als auch der Gatewayhostdienst verwenden .NET Framework 4.5.

* Der SAP .NET-Connector für .NET Framework 4.0 funktioniert mit Prozessen, die .NET Common Language Runtime 4.0 bis 4.7.1 verwenden.

* Der SAP .NET-Connector für .NET Framework 2.0 funktioniert mit Prozessen, die die .NET-Laufzeit 2.0 bis 3.5 verwenden. Er kann jedoch nicht mehr mit dem neuesten Gateway verwendet werden.

### <a name="secure-network-communications-prerequisites"></a>Voraussetzungen für Secure Network Communications (SNC)

Wenn Sie das lokale Datengateway mit optionalem SNC (Secure Network Communications) verwenden, das nur in einer mehrinstanzenfähigen Azure-Umgebung unterstützt wird, müssen Sie auch die folgenden Einstellungen konfigurieren:

* Stellen Sie bei Verwendung von SNC mit SSO (Einmaliges Anmelden) sicher, dass das Datengateway als Benutzer ausgeführt wird, der dem SAP-Benutzer zugeordnet ist. Wählen Sie **Konto ändern** aus, und geben Sie die Anmeldeinformationen des Benutzers ein, um das Standardkonto zu ändern.

  ![Ändern des Datengatewaykontos](./media/logic-apps-using-sap-connector/gateway-account.png)

* Wenn Sie SNC mit einem externen Sicherheitsprodukt aktivieren, kopieren Sie die SNC-Bibliothek oder -Dateien auf denselben Computer, auf dem das Gateway installiert ist. Einige Beispiele für SNC-Produkte sind [sapseculib](https://help.sap.com/saphelp_nw74/helpdata/en/7a/0755dc6ef84f76890a77ad6eb13b13/frameset.htm), Kerberos und NTLM.

Weitere Informationen zum Aktivieren von SNC für das Datengateway finden Sie im Abschnitt[Aktivieren von Secure Network Communications (SNC)](#secure-network-communications).

<a name="migrate"></a>

## <a name="migrate-to-current-connector"></a>Migrieren zum aktuellen Connector

Führen Sie die folgenden Schritte aus, um von einem älteren verwalteten SAP-Connector (Nicht-ISE) zum aktuellen zu migrieren:

1. Aktualisieren Sie bei Bedarf Ihr [lokales Datengateway](https://www.microsoft.com/download/details.aspx?id=53127), damit Sie über die neueste Version verfügen. Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit einem lokalen SAP-System in Logik-Apps mit dem SAP-Connector](../logic-apps/logic-apps-gateway-install.md).

1. Löschen Sie in der Logik-App, die den älteren SAP-Connector verwendet, die Aktion **Send to SAP** (An SAP senden).

1. Fügen Sie über den neuesten SAP-Connector die Aktion **Nachricht an SAP senden** hinzu. Diese Aktion kann erst nach erneuter Erstellung der Verbindung mit Ihrem SAP-System verwendet werden.

1. Wenn Sie fertig sind, speichern Sie Ihre Logik-App.

<a name="add-trigger"></a>

## <a name="send-message-to-sap"></a>Senden von Nachrichten an SAP

In diesem Beispiel wird eine Logik-App verwendet, die Sie mit einer HTTP-Anforderung auslösen können. Die Logik-App sendet eine IDoc-Datei an einen SAP-Server und gibt eine Antwort an den Anforderer zurück, der die Logik-App aufgerufen hat.

### <a name="add-an-http-request-trigger"></a>Hinzufügen eines HTTP-Anforderungstriggers

In Azure Logic Apps muss jede Logik-App mit einem [Trigger](../logic-apps/logic-apps-overview.md#logic-app-concepts) beginnen, der ausgelöst wird, wenn ein bestimmtes Ereignis eintritt oder eine bestimmte Bedingung erfüllt wird. Bei jeder Auslösung des Triggers erstellt das Logic Apps-Modul eine Logik-App-Instanz und startet die Ausführung des Workflows Ihrer App.

> [!NOTE]
> Empfängt eine Logik-App IDoc-Pakete von SAP, wird das „einfache“, von der WE60 IDoc-Dokumentation von SAP generierte XML-Schema nicht vom [Anforderungstrigger](https://docs.microsoft.com/azure/connectors/connectors-native-reqres) unterstützt. Das „einfache“ XML-Schema wird jedoch für Szenarios unterstützt, in denen Nachrichten von einer Logik-App *an* SAP gesendet werden. Sie können den Anforderungstrigger für das IDoc-XML-Format von SAP verwenden, jedoch nicht für IDoc über RFC. Alternativ dazu können Sie den XML-Code auch in das erforderliche Format umwandeln. 

In diesem Beispiel erstellen Sie eine Logik-App mit einem Endpunkt in Azure, damit Sie *HTTP POST-Anforderungen* an Ihre Logik-App senden können. Wenn Ihre Logik-App diese HTTP-Anforderungen empfängt, wird der Trigger ausgelöst und führt den nächsten Schritt im Workflow aus.

1. Erstellen Sie im [Azure-Portal](https://portal.azure.com) eine leere Logik-App, die den Logik-App-Designer öffnet.

1. Geben Sie im Suchfeld den Begriff `http request` als Filter ein. Wählen Sie in der Liste der **Trigger** die Option **Beim Empfang einer HTTP-Anforderung** aus.

   ![Hinzufügen eines HTTP-Anforderungstriggers](./media/logic-apps-using-sap-connector/add-http-trigger-logic-app.png)

1. Speichern Sie jetzt Ihre Logik-App, damit Sie eine Endpunkt-URL dafür erstellen können. Wählen Sie auf der Symbolleiste des Designers **Speichern** aus.

   Die Endpunkt-URL wird nun im Trigger angezeigt, z.B.:

   ![URL für den Endpunkt generieren](./media/logic-apps-using-sap-connector/generate-http-endpoint-url.png)

<a name="add-action"></a>

### <a name="add-an-sap-action"></a>Hinzufügen einer SAP-Aktion

In Azure Logic Apps handelt es sich bei einer [Aktion](../logic-apps/logic-apps-overview.md#logic-app-concepts) um einen Schritt in Ihrem Workflow, der einem Trigger oder einer anderen Aktion folgt. Wenn Sie Ihrer Logik-App noch keinen Trigger hinzugefügt haben und mit diesem Beispiel weitermachen möchten, [fügen Sie den Trigger wie in diesem Abschnitt beschrieben hinzu](#add-trigger).

1. Wählen Sie im Logik-App-Designer unter dem Trigger die Option **Neuer Schritt** aus.

   ![Hinzufügen eines neuen Schritts zum Logik-App](./media/logic-apps-using-sap-connector/add-sap-action-logic-app.png)

1. Geben Sie im Suchfeld den Begriff `sap` als Filter ein. Wählen Sie in der Liste **Aktionen** die Option **Nachricht an SAP senden** aus.
  
   ![Auswählen der Aktion „Nachricht an SAP senden“](media/logic-apps-using-sap-connector/select-sap-send-action.png)

   Alternativ können Sie die Registerkarte **Unternehmen** und dann die SAP-Aktion auswählen.

   ![Auswählen der Aktion „Nachricht an SAP senden“ auf der Registerkarte „Unternehmen“](media/logic-apps-using-sap-connector/select-sap-send-action-ent-tab.png)

1. Sollte die Verbindung bereits bestehen, fahren Sie mit dem nächsten Schritt fort, damit Sie Ihre SAP-Aktion einrichten können. Wenn Sie jedoch zur Eingabe von Verbindungsdetails aufgefordert werden, sollten Sie die Informationen angeben, um eine Verbindung mit Ihrem lokalen SAP-Server herstellen zu können.

   1. Geben Sie einen Namen für die Verbindung an.

   1. Wenn Sie das Datengateway verwenden, führen Sie die folgenden Schritte aus:
   
      1. Wählen Sie im Abschnitt **Datengateway** unter **Abonnement** das Azure-Abonnement für die Datengatewayressource aus, die Sie im Azure-Portal für die Installation Ihres Datengateways erstellt haben.
   
      1. Wählen Sie unter **Verbindungsgateway** Ihre Datengatewayressource in Azure aus.

   1. Geben Sie weitere Informationen zu Verbindung an. Führen Sie die Schritte für die Eigenschaft **Anmeldetyp** abhängig davon aus, ob die Eigenschaft auf **Anwendungsserver** oder **Gruppe** festgelegt ist:
   
      * Bei **Anwendungsserver** sind die folgenden Eigenschaften erforderlich, die sonst optional sind:

        ![Verbindung zum SAP-Anwendungsserver herstellen](media/logic-apps-using-sap-connector/create-SAP-application-server-connection.png)

      * Bei **Gruppe** sind die folgenden Eigenschaften erforderlich, die sonst optional sind:

        ![Verbindung zum SAP-Nachrichtenserver herstellen](media/logic-apps-using-sap-connector/create-SAP-message-server-connection.png)  

      Standardmäßig wird starke Typisierung verwendet, um nach ungültigen Werten zu suchen, indem eine XML-Validierung anhand des Schemas durchgeführt wird. Durch dieses Verhalten können Sie Probleme früher feststellen. Die Option **Sichere Typisierung** ist aus Gründen der Abwärtskompatibilität verfügbar und überprüft nur die Länge der Zeichenfolge. Erfahren Sie mehr über die Option [Sichere Typisierung](#safe-typing).

   1. Wählen Sie **Erstellen** aus, wenn Sie fertig sind.

      Die Verbindung wird in Logic Apps eingerichtet und getestet, sodass sichergestellt ist, dass sie ordnungsgemäß funktioniert.

1. Suchen Sie nun eine Aktion für Ihren SAP-Server, und wählen Sie sie aus.

   1. Wählen Sie im Feld **SAP-Aktion** das Ordnersymbol aus. Suchen Sie in der Dateiliste nach der gewünschten SAP-Nachricht, und wählen Sie sie aus. Verwenden Sie die Pfeile zum Durchlaufen der Liste.

      In diesem Beispiel wird ein IDoc mit dem Typ **Aufträge** ausgewählt.

      ![Aktion „IDoc“ suchen und auswählen](./media/logic-apps-using-sap-connector/SAP-app-server-find-action.png)

      Wenn Sie die Aktion, die Sie verwenden möchten, nicht finden können, können Sie einen Pfad auch manuell eingeben:

      ![Pfad zur Aktion „IDoc“ manuell bereitstellen](./media/logic-apps-using-sap-connector/SAP-app-server-manually-enter-action.png)

      > [!TIP]
      > Geben Sie den Wert für die **SAP-Aktion** über den Ausdrucks-Editor ein. Auf diese Weise können Sie dieselbe Aktion für verschiedene Nachrichtentypen verwenden.

      Weitere Informationen zu IDoc-Vorgängen finden Sie unter [Nachrichtenschemas für IDoc-Vorgänge](https://docs.microsoft.com/biztalk/adapters-and-accelerators/adapter-sap/message-schemas-for-idoc-operations).

   1. Klicken Sie in das Feld **Input Message** (Eingabenachricht), damit die dynamische Inhaltsliste angezeigt wird. Klicken Sie in dieser Liste unter **Beim Empfang einer HTTP-Anforderung** auf das Feld **Hauptteil**.

      Dadurch wird der Hauptteil des HTTP-Anforderungstriggers eingefügt und diese Ausgabe an Ihren SAP-Server gesendet.

      ![Auswählen der Eigenschaft „Text“ über den Auslöser](./media/logic-apps-using-sap-connector/SAP-app-server-action-select-body.png)

      Ihre SAP-Aktion sieht dann wie im folgenden Beispiel aus:

      ![Vervollständigen der SAP-Aktion](./media/logic-apps-using-sap-connector/SAP-app-server-complete-action.png)

1. Speichern Sie Ihre Logik-App. Wählen Sie auf der Symbolleiste des Designers **Speichern** aus.

<a name="add-response"></a>

### <a name="add-an-http-response-action"></a>Hinzufügen einer HTTP-Antwortaktion

Fügen Sie nun dem Workflow Ihrer Logik-App eine Antwortaktion hinzu, und fügen Sie die Ausgabe der SAP-Aktion ein. So gibt die Logik-App die Ergebnisse Ihres SAP-Servers an den ursprünglichen Anforderer zurück.

1. Wählen Sie im Logik-App-Designer unter der SAP-Aktion die Option **Neuer Schritt** aus.

1. Geben Sie im Suchfeld den Begriff `response` als Filter ein. Wählen Sie in der Liste **Aktionen** die Option **Antwort** aus.

1. Klicken Sie in das Feld **Hauptteil**, damit die dynamische Inhaltsliste angezeigt wird. Wählen Sie aus dieser Liste unter **Send message to SAP** (Nachricht an SAP senden) das Feld **Hauptteil** aus.

   ![Vollständige SAP-Aktion](./media/logic-apps-using-sap-connector/select-sap-body-for-response-action.png)

1. Speichern Sie Ihre Logik-App.

### <a name="test-your-logic-app"></a>Testen Ihrer Logik-App

1. Wenn Ihre Logik-App nicht bereits aktiviert ist, wählen Sie im Menü der Logik-App die Option **Überblick** aus. Wählen Sie auf der Symbolleiste **Aktivieren** aus.

1. Wählen Sie auf der Symbolleiste des Designers **Ausführen** aus. Dadurch wird die Logik-App manuell gestartet.

1. Lösen Sie Ihre Logik-App aus, indem Sie eine HTTP POST-Anforderung an die URL im HTTP-Anforderungstrigger senden.
Fügen Sie den Nachrichteninhalt in Ihre Anforderung ein. Um die Anforderung zu senden, können Sie ein Tool wie [Postman](https://www.getpostman.com/apps) nutzen.

   In diesem Artikel sendet die Anforderung eine IDoc-Datei, die im XML-Format vorliegen muss, und enthält den Namespace der SAP-Aktion, die Sie verwenden möchten, z.B.:

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <Send xmlns="http://Microsoft.LobServices.Sap/2007/03/Idoc/2/ORDERS05//720/Send">
      <idocData>
         <...>
      </idocData>
   </Send>
   ```

1. Nachdem Sie die HTTP-Anforderung gesendet haben, warten Sie auf die Antwort der Logik-App.

   > [!NOTE]
   > Es kann ein Timeout auftreten, wenn nicht alle für die Antwort erforderlichen Schritte innerhalb des [Timeoutlimits der Anforderung](./logic-apps-limits-and-config.md) abgeschlossen wurden. Wenn dies passiert, können Anforderungen blockiert werden. Um die Problemdiagnose zu vereinfachen, sollten Sie sich mit dem [Überprüfen und Überwachen Ihrer Logik-Apps](../logic-apps/monitor-logic-apps.md) vertraut machen.

Sie haben eine Logik-App erstellt, die mit dem SAP-Server kommunizieren kann. Da Sie eine SAP-Verbindung für die Logik-App eingerichtet haben, können Sie nun andere SAP Aktionen kennenlernen, z.B. BAPI und RFC.

<a name="receive-from-sap"></a>

## <a name="receive-message-from-sap"></a>Empfangen von Nachrichten von SAP

Dieses Beispiel verwendet eine Logik-App, die bei Empfang einer Nachricht von einem SAP-System ausgelöst wird.

### <a name="add-an-sap-trigger"></a>Hinzufügen eines SAP-Triggers

1. Erstellen Sie im Azure-Portal eine leere Logik-App, die den Logik-App-Designer öffnet.

1. Geben Sie im Suchfeld den Begriff `sap` als Filter ein. Wählen Sie in der Liste der **Trigger** die Option **Beim Empfang einer Nachricht von SAP** aus.

   ![Hinzufügen eines SAP-Triggers](./media/logic-apps-using-sap-connector/add-sap-trigger-logic-app.png)

   Alternativ können Sie die Registerkarte **Unternehmen** und dann den Auslöser auswählen:

   ![Hinzufügen eines SAP-Triggers auf der Registerkarte „Unternehmen“](./media/logic-apps-using-sap-connector/add-sap-trigger-ent-tab.png)

1. Sollte die Verbindung bereits bestehen, fahren Sie mit dem nächsten Schritt fort, damit Sie Ihre SAP-Aktion einrichten können. Wenn Sie jedoch zur Eingabe von Verbindungsdetails aufgefordert werden, sollten Sie die Informationen angeben, damit Sie eine Verbindung mit Ihrem lokalen SAP-Server herstellen können.

   1. Geben Sie einen Namen für die Verbindung an.

   1. Wenn Sie das Datengateway verwenden, führen Sie die folgenden Schritte aus:

      1. Wählen Sie im Abschnitt **Datengateway** unter **Abonnement** das Azure-Abonnement für die Datengatewayressource aus, die Sie im Azure-Portal für die Installation Ihres Datengateways erstellt haben.

      1. Wählen Sie unter **Verbindungsgateway** Ihre Datengatewayressource in Azure aus.

   1. Geben Sie weitere Informationen zu Verbindung an. Führen Sie die Schritte für die Eigenschaft **Anmeldetyp** abhängig davon aus, ob die Eigenschaft auf **Anwendungsserver** oder **Gruppe** festgelegt ist:

      * Bei **Anwendungsserver** sind die folgenden Eigenschaften erforderlich, die sonst optional sind:

        ![Verbindung zum SAP-Anwendungsserver herstellen](media/logic-apps-using-sap-connector/create-SAP-application-server-connection.png)

      * Bei **Gruppe** sind die folgenden Eigenschaften erforderlich, die sonst optional sind:

        ![Verbindung zum SAP-Nachrichtenserver herstellen](media/logic-apps-using-sap-connector/create-SAP-message-server-connection.png)

      Standardmäßig wird starke Typisierung verwendet, um nach ungültigen Werten zu suchen, indem eine XML-Validierung anhand des Schemas durchgeführt wird. Durch dieses Verhalten können Sie Probleme früher feststellen. Die Option **Sichere Typisierung** ist aus Gründen der Abwärtskompatibilität verfügbar und überprüft nur die Länge der Zeichenfolge. Erfahren Sie mehr über die Option [Sichere Typisierung](#safe-typing).

   1. Wählen Sie **Erstellen** aus, wenn Sie fertig sind.

      Die Verbindung wird in Logic Apps eingerichtet und getestet, sodass sichergestellt ist, dass sie ordnungsgemäß funktioniert.

1. Geben Sie die [erforderlichen Parameter](#parameters) basierend auf der Konfiguration Ihres SAP-Systems an.

   Optional können Sie eine oder mehrere SAP-Aktionen angeben. Diese Liste mit Aktionen gibt die Nachrichten an, die der Trigger von Ihrem SAP-Server empfängt. Eine leere Liste gibt an, dass der Trigger alle Nachrichten empfängt. Wenn die Liste mehrere Nachrichten enthält, empfängt der Trigger nur die Nachrichten, die in der Liste angegeben wurden. Alle anderen von Ihrem SAP-Server gesendeten Nachrichten werden abgelehnt.

   Sie können eine SAP-Aktion aus der Dateiauswahl auswählen:

   ![Hinzufügen der SAP-Aktion zur Logik-App](media/logic-apps-using-sap-connector/select-SAP-action-trigger.png)  

   Sie können eine Aktion auch manuell angeben:

   ![SAP-Aktion manuell eingeben](media/logic-apps-using-sap-connector/manual-enter-SAP-action-trigger.png)

   Dieses Beispiel stellt dar, wie die Aktion angezeigt wird, wenn Sie den Trigger für den Empfang mehrerer Nachrichten einrichten.

   ![Beispiel für einen Auslöser, der mehrere Nachrichten empfängt](media/logic-apps-using-sap-connector/example-trigger.png)

   Weitere Informationen zur SAP-Aktion finden Sie unter [Nachrichtenschemas für IDoc-Vorgänge](https://docs.microsoft.com/biztalk/adapters-and-accelerators/adapter-sap/message-schemas-for-idoc-operations).

1. Speichern Sie jetzt Ihre Logik-App, um den Empfang von Nachrichten von Ihrem SAP-System zu starten. Wählen Sie auf der Symbolleiste des Designers **Speichern** aus.

Ihre Logik-App kann jetzt Nachrichten von Ihrem SAP-System empfangen.

> [!NOTE]
> Der SAP-Trigger ist kein Abfragetrigger, sondern er basiert auf Webhooks. Bei Verwendung des Datengateways wird der Trigger nur dann vom Datengateway aufgerufen, wenn eine Nachricht vorhanden ist. Abrufe sind daher nicht erforderlich.

<a name="parameters"></a>

#### <a name="parameters"></a>Parameter

Neben einfachen Zeichenfolgen- und Zahleneingaben werden vom SAP-Connector auch die folgenden Tabellenparameter (`Type=ITAB`-Eingaben) akzeptiert:

* Direktionale Tabellenparameter (Ein- und Ausgabe) für ältere SAP-Releases
* Änderungsparameter, die die direktionalen Tabellenparameter bei neueren SAP-Releases ersetzen
* Hierarchische Tabellenparameter

### <a name="test-your-logic-app"></a>Testen Ihrer Logik-App

1. Um Ihre Logik-App auszulösen, senden Sie eine Nachricht von Ihrem SAP-System.

1. Wählen Sie in der Logik-App-Menü **Übersicht** aus. Überprüfen Sie den **Ausführungsverlauf** für alle neuen Ausführungen für Ihre Logik-App.

1. Öffnen Sie die letzte Ausführung, in der die von Ihrem SAP-System gesendete Nachricht im Abschnitt mit den Triggerausgaben angezeigt wird.

## <a name="receive-idoc-packets-from-sap"></a>Empfangen von IDoc-Paketen von SAP

Sie können SAP für das [Senden von IDoc-Paketen](https://help.sap.com/viewer/8f3819b0c24149b5959ab31070b64058/7.4.16/en-US/4ab38886549a6d8ce10000000a42189c.html) einrichten. Dabei handelt es sich um IDoc-Batches oder -Gruppen. Zum Empfangen von IDoc-Paketen ist für den SAP-Connector und insbesondere für den Trigger keine zusätzliche Konfiguration erforderlich. Damit Sie jedoch alle Elemente in einem IDoc-Paket verarbeiten können, das vom Trigger empfangen wurde, sind einige zusätzliche Schritte erforderlich, um das Paket in einzelne IDoc-Elemente aufzuteilen.

Im folgenden Beispiel wird veranschaulicht, wie die einzelnen IDoc-Elemente eines Pakets mithilfe der [`xpath()`-Funktion](./workflow-definition-language-functions-reference.md#xpath) extrahiert werden:

1. Zunächst benötigen Sie eine Logik-App mit einem SAP-Trigger. Wenn Sie diese Logik-App noch nicht besitzen, führen Sie die vorherigen Schritte in diesem Artikel aus, um [eine Logik-App mit einem SAP-Trigger](#receive-from-sap) einzurichten.

   Beispiel:

   ![Hinzufügen eines SAP-Auslösers zur Logik-App](./media/logic-apps-using-sap-connector/first-step-trigger.png)

1. Rufen Sie den Stammnamespace aus dem XML-IDoc-Element ab, das Ihre Logik-App von SAP empfängt. Fügen Sie zum Extrahieren dieses Namespace aus dem XML-Dokument einen Schritt hinzu, der eine lokale Zeichenfolgenvariable erstellt und diesen Namespace mithilfe eines `xpath()`-Ausdrucks speichert:

   `xpath(xml(triggerBody()?['Content']), 'namespace-uri(/*)')`

   ![Abrufen des Stammnamespace aus dem IDoc-Element](./media/logic-apps-using-sap-connector/get-namespace.png)

1. Fügen Sie zum Extrahieren eines einzelnen IDoc-Elements einen Schritt hinzu, in dem eine Arrayvariable erstellt und die IDoc-Sammlung mithilfe eines anderen `xpath()`-Ausdrucks gespeichert wird:

   `xpath(xml(triggerBody()?['Content']), '/*[local-name()="Receive"]/*[local-name()="idocData"]')`

   ![Abrufen eines Arrays von Elementen](./media/logic-apps-using-sap-connector/get-array.png)

   Die Arrayvariable stellt alle IDoc-Elemente durch Auflisten der Sammlung bereit, damit sie einzeln von Ihrer Logik-App verarbeitet werden können. In diesem Beispiel wird jedes IDoc-Element von der Logik-App mithilfe einer Schleife an einen SFTP-Server übertragen:

   ![Senden von IDoc-Elementen an einen SFTP-Server](./media/logic-apps-using-sap-connector/loop-batch.png)

   Der Stammnamespace muss in jedem IDoc-Element enthalten sein. Daher wird der Inhalt der Datei zusammen mit dem Stammnamespace von einem `<Receive></Receive`-Element umschlossen, bevor das IDoc-Element an die Downstream-App oder in diesem Fall an den SFTP-Server gesendet wird.

Sie können die Schnellstartvorlage für dieses Muster verwenden, indem Sie diese Vorlage im Logik-App-Designer auswählen, wenn Sie eine neue Logik-App erstellen.

![Auswählen der Vorlage Batch-Logik-App](./media/logic-apps-using-sap-connector/select-batch-logic-app-template.png)

## <a name="generate-schemas-for-artifacts-in-sap"></a>Generieren von Schemas für Artefakte in SAP

In diesem Beispiel wird eine Logik-App verwendet, die Sie mit einer HTTP-Anforderung auslösen können. Die SAP-Aktion sendet eine Anforderung an ein SAP-System, um die Schemas für die IDoc- und BAPI-Angabe zu generieren. Schemas, die in der Antwort zurückgegeben werden, werden über den Azure Resource Manager-Connector in ein Integrationskonto hochgeladen.

### <a name="add-an-http-request-trigger"></a>Hinzufügen eines HTTP-Anforderungstriggers

1. Erstellen Sie im Azure-Portal eine leere Logik-App, die den Logik-App-Designer öffnet.

1. Geben Sie im Suchfeld den Begriff `http request` als Filter ein. Wählen Sie in der Liste der **Trigger** die Option **Beim Empfang einer HTTP-Anforderung** aus.

   ![Hinzufügen eines HTTP-Anforderungstriggers](./media/logic-apps-using-sap-connector/add-http-trigger-logic-app.png)

1. Speichern Sie jetzt Ihre Logik-App, damit Sie eine Endpunkt-URL dafür erstellen können.
Wählen Sie auf der Symbolleiste des Designers **Speichern** aus.

   Die Endpunkt-URL wird nun im Trigger angezeigt, z.B.:

   ![URL für den Endpunkt generieren](./media/logic-apps-using-sap-connector/generate-http-endpoint-url.png)

### <a name="add-an-sap-action-to-generate-schemas"></a>Hinzufügen einer SAP-Aktion zum Generieren von Schemas

1. Wählen Sie im Logik-App-Designer unter dem Trigger die Option **Neuer Schritt** aus.

   ![Hinzufügen eines neuen Schritts zum Logik-App](./media/logic-apps-using-sap-connector/add-sap-action-logic-app.png)

1. Geben Sie im Suchfeld den Begriff `sap` als Filter ein. Wählen Sie in der Liste mit den **Aktionen** die Aktion **Schemas generieren** aus.
  
   ![Hinzufügen der Aktion „Schemas generieren“ zur Logik-App](media/logic-apps-using-sap-connector/select-sap-schema-generator-action.png)

   Alternativ können Sie die Registerkarte **Unternehmen** und dann die SAP-Aktion auswählen.

   ![SAP-Sendeaktion auf der Registerkarte „Unternehmen“ auswählen](media/logic-apps-using-sap-connector/select-sap-schema-generator-ent-tab.png)

1. Sollte die Verbindung bereits bestehen, fahren Sie mit dem nächsten Schritt fort, damit Sie Ihre SAP-Aktion einrichten können. Wenn Sie jedoch zur Eingabe von Verbindungsdetails aufgefordert werden, sollten Sie die Informationen angeben, damit Sie eine Verbindung mit Ihrem lokalen SAP-Server herstellen können.

   1. Geben Sie einen Namen für die Verbindung an.

   1. Wählen Sie im Abschnitt **Datengateway** unter **Abonnement** das Azure-Abonnement für die Datengatewayressource aus, die Sie im Azure-Portal für die Installation Ihres Datengateways erstellt haben. 
   
   1. Wählen Sie unter **Verbindungsgateway** Ihre Datengatewayressource in Azure aus.

   1. Geben Sie weitere Informationen zu Verbindung an. Führen Sie die Schritte für die Eigenschaft **Anmeldetyp** abhängig davon aus, ob die Eigenschaft auf **Anwendungsserver** oder **Gruppe** festgelegt ist:
   
      * Bei **Anwendungsserver** sind die folgenden Eigenschaften erforderlich, die sonst optional sind:

        ![Verbindung zum SAP-Anwendungsserver herstellen](media/logic-apps-using-sap-connector/create-SAP-application-server-connection.png)

      * Bei **Gruppe** sind die folgenden Eigenschaften erforderlich, die sonst optional sind:

        ![Verbindung zum SAP-Nachrichtenserver herstellen](media/logic-apps-using-sap-connector/create-SAP-message-server-connection.png)  

      Standardmäßig wird starke Typisierung verwendet, um nach ungültigen Werten zu suchen, indem eine XML-Validierung anhand des Schemas durchgeführt wird. Durch dieses Verhalten können Sie Probleme früher feststellen. Die Option **Sichere Typisierung** ist aus Gründen der Abwärtskompatibilität verfügbar und überprüft nur die Länge der Zeichenfolge. Erfahren Sie mehr über die Option [Sichere Typisierung](#safe-typing).

   1. Wählen Sie **Erstellen** aus, wenn Sie fertig sind.

      Die Verbindung wird in Logic Apps eingerichtet und getestet, sodass sichergestellt ist, dass sie ordnungsgemäß funktioniert.

1. Geben Sie den Pfad zu dem Artefakt an, für das Sie das Schema generieren möchten.

   Sie können die SAP-Aktion über die Dateiauswahl auswählen:

   ![SAP-Aktion auswählen](media/logic-apps-using-sap-connector/select-SAP-action-schema-generator.png)  

   Alternativ dazu können Sie die Aktion manuell eingeben:

   ![SAP-Aktion manuell eingeben](media/logic-apps-using-sap-connector/manual-enter-SAP-action-schema-generator.png)

   Um Schemas für mehrere Artefakte zu generieren, geben Sie die Details der SAP-Aktion für jedes Artefakt an. Beispiel:

   ![„Neues Element hinzufügen“ auswählen](media/logic-apps-using-sap-connector/schema-generator-array-pick.png)

   ![Zwei Elemente anzeigen](media/logic-apps-using-sap-connector/schema-generator-example.png)

   Weitere Informationen zur SAP-Aktion finden Sie unter [Nachrichtenschemas für IDoc-Vorgänge](https://docs.microsoft.com/biztalk/adapters-and-accelerators/adapter-sap/message-schemas-for-idoc-operations).

1. Speichern Sie Ihre Logik-App. Wählen Sie auf der Symbolleiste des Designers **Speichern** aus.

### <a name="test-your-logic-app"></a>Testen Ihrer Logik-App

1. Wählen Sie in der Designer-Symbolleiste die Option **Ausführen**, um eine Ausführung für Ihre Logik-App auszulösen.

1. Öffnen Sie die Ausführung, und überprüfen Sie die Ausgaben für die Aktion **Schemas generieren**.

   Die Ausgaben zeigen die generierten Schemas für die angegebene Liste von Nachrichten.

### <a name="upload-schemas-to-an-integration-account"></a>Hochladen von Schemas in ein Integrationskonto

Optional können Sie die generierten Schemas in Repositorys wie ein Blob-, Speicher- oder Integrationskonto herunterladen oder speichern. Integrationskonten eignen sich hervorragend für die Verwendung mit anderen XML-Aktionen. Daher zeigt dieses Beispiel, wie Sie Schemas mithilfe des Azure Resource Manager-Connectors in ein Integrationskonto für die gleiche Logik-App hochladen.

1. Wählen Sie im Logik-App-Designer unter dem Trigger die Option **Neuer Schritt** aus.

1. Geben Sie im Suchfeld den Begriff `Resource Manager` als Filter ein. Wählen Sie **Ressource erstellen oder aktualisieren** aus.

   ![Azure Resource Manager-Aktion auswählen](media/logic-apps-using-sap-connector/select-azure-resource-manager-action.png)

1. Geben Sie die Details für die Aktion einschließlich des Azure-Abonnements, der Azure-Ressourcengruppe und des Integrationskontos ein. Klicken Sie in die Felder für diese Felder, und wählen Sie aus der angezeigten dynamischen Inhaltsliste die SAP-Token aus, die den Feldern hinzugefügt werden sollen.

   1. Öffnen Sie die Liste **Neuen Parameter hinzufügen**, und wählen Sie die Felder **Speicherort** und **Eigenschaften** aus.

   1. Geben Sie wie in diesem Beispiel veranschaulicht Details für diese neuen Felder an.

      ![Details für Azure Resource Manager-Aktion eingeben](media/logic-apps-using-sap-connector/azure-resource-manager-action.png)

   Die SAP-Aktion **Schemas generieren** generiert Schemas als Sammlung, daher fügt der Designer der Aktion automatisch eine **For each**-Schleife hinzu. In diesem Beispiel wird die Darstellung dieser Aktion gezeigt:

   ![Azure Resource Manager-Aktion mit „for each“-Schleife](media/logic-apps-using-sap-connector/azure-resource-manager-action-foreach.png)

   > [!NOTE]
   > Die Schemas verwenden das Base64-codierte Format. Damit die Schemas in ein Integrationskonto hochgeladen werden können, müssen sie mithilfe der Funktion `base64ToString()` decodiert werden. Das folgende Beispiel zeigt den Code für das Element `"properties"`:
   >
   > ```json
   > "properties": {
   >    "Content": "@base64ToString(items('For_each')?['Content'])",
   >    "ContentType": "application/xml",
   >    "SchemaType": "Xml"
   > }
   > ```

1. Speichern Sie Ihre Logik-App. Wählen Sie auf der Symbolleiste des Designers **Speichern** aus.

### <a name="test-your-logic-app"></a>Testen Ihrer Logik-App

1. Wählen Sie in der Designer-Symbolleiste die Option **Ausführen**, um Ihre Logik-App manuell auszulösen.

1. Wechseln Sie nach einer erfolgreichen Ausführung zum Integrationskonto, und überprüfen Sie, ob die generierten Schemas vorhanden sind.

<a name="secure-network-communications"></a>

## <a name="enable-secure-network-communications"></a>Aktivieren von Secure Network Communications (SNC)

Vergewissern Sie sich zunächst, dass Sie die zuvor aufgeführten [Voraussetzungen](#pre-reqs) erfüllen. Diese gelten jedoch nur, wenn Sie das Datengateway und Ihre Logik-Apps in einer mehrinstanzenfähigen Azure-Umgebung verwenden:

* Das lokale Datengateway muss auf einem Computer installiert sein, der sich im selben Netzwerk wie Ihr SAP-System befindet.

* Für SSO (Einmaliges Anmelden) wird das Datengateway als Benutzer ausgeführt, der einem SAP-Benutzer zugeordnet ist.

* Die SNC-Bibliothek, die die zusätzlichen Sicherheitsfunktionen bereitstellt, wurde auf demselben Computer wie das Datengateway installiert. Einige Beispiele sind [sapseculib](https://help.sap.com/saphelp_nw74/helpdata/en/7a/0755dc6ef84f76890a77ad6eb13b13/frameset.htm), Kerberos und NTLM.

   Um SNC für Ihre Anforderungen an oder aus dem SAP-System zu aktivieren, aktivieren Sie das Kontrollkästchen **SNC verwenden** in der SAP-Verbindung, und geben Sie diese Eigenschaften an:

   ![Konfigurieren von SAP-SNC in der Verbindung](media/logic-apps-using-sap-connector/configure-sapsnc.png)

   | Eigenschaft | BESCHREIBUNG |
   |----------| ------------|
   | **SNC-Bibliothekspfad** | Der Name oder Pfad der SNC-Bibliothek, der relativ zum NCo-Installationspfad oder absoluten Pfad ist. Bespiele sind `sapsnc.dll`, `.\security\sapsnc.dll` oder `c:\security\sapsnc.dll`. |
   | **SNC SSO** | Bei einer Verbindung über SNC wird die SNC-Identität typischerweise zur Authentifizierung des Aufrufers verwendet. Eine weitere Möglichkeit besteht darin, die Benutzer- und Kennwortinformationen so zu überschreiben, dass sie für die Authentifizierung des Aufrufers verwendet werden können, die Verbindung aber dennoch verschlüsselt ist. |
   | **SNC Mein Name** | In den meisten Fällen kann diese Eigenschaft ausgelassen werden. Die installierte SNC-Lösung kennt in der Regel den eigenen SNC-Namen. Nur bei Lösungen, die mehrere Identitäten unterstützen, müssen Sie möglicherweise die Identität angeben, die für dieses spezielle Ziel oder diesen Server verwendet werden soll. |
   | **Name des SNC-Partners** | Name für das Back-End-SNC |
   | **SNC-Qualität des Schutzes** | Die Servicequalität, die für die SNC-Kommunikation dieses bestimmten Ziels oder Servers verwendet werden soll. Der Standardwert wird durch das Back-End-System definiert. Der Maximalwert wird durch das für SNC verwendete Sicherheitsprodukt definiert. |
   |||

   > [!NOTE]
   > Legen Sie die Umgebungsvariablen SNC_LIB und SNC_LIB_64 nicht auf dem Computer fest, auf dem Sie das Datengateway und die SNC-Bibliothek betreiben. Wenn sie festgelegt werden, haben sie Vorrang vor dem Wert der SNC-Bibliothek, der über den Connector übergeben wird.

<a name="safe-typing"></a>

## <a name="safe-typing"></a>Sichere Typisierung

Standardmäßig wird beim Erstellen der SAP-Verbindung starke Typisierung verwendet, um nach ungültigen Werten zu suchen, indem eine XML-Validierung anhand des Schemas durchgeführt wird. Durch dieses Verhalten können Sie Probleme früher feststellen. Die Option **Sichere Typisierung** ist aus Gründen der Abwärtskompatibilität verfügbar und überprüft nur die Länge der Zeichenfolge. Wenn Sie **Sichere Typisierung** auswählen, werden die DATS- und TIMS-Typen in SAP als Zeichenfolgen und nicht als ihre XML-Entsprechungen (`xs:date` und `xs:time`, `xmlns:xs="http://www.w3.org/2001/XMLSchema"`) behandelt. Die sichere Typisierung beeinflusst das Verhalten für die gesamte Schemagenerierung, die Sendenachricht für die Nutzlast „wurde gesendet“ und die Antwort „wurde empfangen“ sowie den Trigger. 

Wenn starke Typisierung verwendet wird (**Sichere Typisierung** ist nicht aktiviert) ordnet das Schema die DATS-und TIMS-Typen einfacheren XML-Typen zu:

```xml
<xs:element minOccurs="0" maxOccurs="1" name="UPDDAT" nillable="true" type="xs:date"/>
<xs:element minOccurs="0" maxOccurs="1" name="UPDTIM" nillable="true" type="xs:time"/>
```

Beim Senden von Nachrichten mit starker Typisierung entspricht die DATS- und TIMS-Antwort dem entsprechenden XML-Typformat:

```xml
<DATE>9999-12-31</DATE>
<TIME>23:59:59</TIME>
```

Wenn **Sichere Typisierung** aktiviert ist, ordnet das Schema die DATS- und TIMS-Typen nur XML-Zeichenfolgenfeldern mit Längeneinschränkungen zu, z.B.:

```xml
<xs:element minOccurs="0" maxOccurs="1" name="UPDDAT" nillable="true">
  <xs:simpleType>
    <xs:restriction base="xs:string">
      <xs:maxLength value="8" />
    </xs:restriction>
  </xs:simpleType>
</xs:element>
<xs:element minOccurs="0" maxOccurs="1" name="UPDTIM" nillable="true">
  <xs:simpleType>
    <xs:restriction base="xs:string">
      <xs:maxLength value="6" />
    </xs:restriction>
  </xs:simpleType>
</xs:element>
```

Beim Senden von Nachrichten mit aktivierter **Sicherer Typisierung** sieht die DATS- und TIMS-Antwort wie im folgenden Beispiel aus:

```xml
<DATE>99991231</DATE>
<TIME>235959</TIME>
```

## <a name="advanced-scenarios"></a>Erweiterte Szenarien

### <a name="confirm-transaction-explicitly"></a>Explizites Bestätigen der Transaktion

Wenn Sie Transaktionen von Logic Apps an SAP senden, erfolgt dieser Austausch in zwei Schritten, wie im SAP-Dokument [Transactional RFC Server Programs](https://help.sap.com/doc/saphelp_nwpi71/7.1/en-US/22/042ad7488911d189490000e829fbbd/content.htm?no_cache=true) (Transaktionsbezogene RFC-Serverprogramme) beschrieben. Von der Aktion **Send to SAP** (An SAP senden) werden standardmäßig sowohl die Schritte für die Funktionsübertragung als auch die Schritte für die Transaktionsbestätigung in einem einzelnen Aufruf abgewickelt. Der SAP-Connector ermöglicht die Entkoppelung dieser Schritte. Sie können ein IDoc-Element senden und anstatt der automatischen Transaktionsbestätigung die explizite Aktion **Transaktions-ID bestätigen** verwenden.

Diese Entkoppelung der Transaktions-ID-Bestätigung ist hilfreich, wenn Sie in SAP keine Transaktionen duplizieren möchten (etwa in Szenarien, in denen beispielsweise Fehler aufgrund von Netzwerkproblemen auftreten können). Durch die separate Bestätigung der Transaktions-ID wird die Transaktion nur einmal in Ihrem SAP-System durchgeführt.

Hier sehen Sie ein Beispiel für dieses Muster:

1. Erstellen Sie eine leere Logik-App, und fügen Sie einen HTTP-Trigger hinzu.

1. Fügen Sie über den SAP-Connector die Aktion **Send IDOC** (IDOC senden) hinzu. Geben Sie die Details für das IDoc-Element an, das Sie an Ihr SAP-System senden.

1. Wählen Sie im Feld **TID bestätigen** die Option **Nein** aus, um die Transaktions-ID explizit in einem separaten Schritt zu bestätigen. Für das optionale Feld **Transaktions-ID-GUID** können Sie den Wert entweder manuell angeben oder diese GUID automatisch durch den Connector generieren und in der Antwort auf die Aktion „IDoc senden“ zurückgeben lassen.

   ![Eigenschaften der Aktion „Send IDOC“ (IDOC senden)](./media/logic-apps-using-sap-connector/send-idoc-action-details.png)

1. Fügen Sie zur expliziten Bestätigung der Transaktions-ID die Aktion **Confirm transaction ID** (Transaktions-ID bestätigen) hinzu. Klicken Sie auf das Feld **Transaction ID** (Transaktions-ID), um die dynamische Inhaltsliste anzuzeigen. Wählen Sie in dieser Liste den **Transaktions-ID-Wert** aus, der von der Aktion **Send IDOC** (IDOC senden) zurückgegeben wurde.

   ![Aktion zum Bestätigen der Transaktions-ID](./media/logic-apps-using-sap-connector/explicit-transaction-id.png)

   Im Anschluss an diesen Schritt wird die aktuelle Transaktion auf beiden Seiten (also aufseiten des SAP-Connectors und aufseiten des SAP-Systems) als abgeschlossen markiert.

## <a name="known-issues-and-limitations"></a>Einschränkungen und bekannte Probleme

Zurzeit sind für den verwalteten SAP-Connector (Nicht-ISE) folgende Probleme und Einschränkungen bekannt:

* Der SAP-Trigger unterstützt keine Datengatewaycluster. In einigen Failoverfällen unterscheidet sich der Datengatewayknoten, der mit dem SAP-System kommuniziert, von dem aktiven Knoten. Dies führt zu unerwartetem Verhalten. In Sendeszenarien werden Datengatewaycluster unterstützt.

* Der SAP-Connector unterstützt derzeit keine SAP-Router-Zeichenfolgen. Das lokale Datengateway muss in demselben LAN wie das SAP-System vorhanden sein, mit dem eine Verbindung hergestellt werden soll.

## <a name="connector-reference"></a>Connector-Referenz

Weitere technische Details zu diesem Connector, z. B. Trigger, Aktionen und Grenzwerte, wie sie in der Swagger-Datei des Connectors beschrieben werden, finden Sie auf der [Referenzseite des Connectors](https://docs.microsoft.com/connectors/sap/).

> [!NOTE]
> Für Logik-Apps in einer [Integrationsdienstumgebung (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) verwendet die mit ISE bezeichnete Version dieses Connectors stattdessen die [ISE-Nachrichtengrenzwerte](../logic-apps/logic-apps-limits-and-config.md#message-size-limits).

## <a name="next-steps"></a>Nächste Schritte

* [Herstellen einer Verbindung von Azure Logic Apps zu lokalen Systemen](../logic-apps/logic-apps-gateway-connection.md)
* In diesem Artikel lernen Sie die Überprüfung, die Transformation und die Verwendung anderer Nachrichtenvorgänge im [Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md) kennen.
* Informationen zu anderen [Logic Apps-Connectors](../connectors/apis-list.md)

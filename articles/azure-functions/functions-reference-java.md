---
title: Java-Entwicklerreferenz zu Azure Functions
description: Erfahren Sie, wie Sie mithilfe von Java Funktionen entwickeln können.
ms.topic: conceptual
ms.date: 09/14/2018
ms.openlocfilehash: 339615ac99f231fd293a7ea15c853d43da8f998a
ms.sourcegitcommit: bcb962e74ee5302d0b9242b1ee006f769a94cfb8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/07/2020
ms.locfileid: "86057601"
---
# <a name="azure-functions-java-developer-guide"></a>Java-Entwicklerhandbuch für Azure Functions

Die Azure Functions-Runtime unterstützt [Java SE 8 LTS (zulu8.31.0.2-jre8.0.181-win_x64)](https://repos.azul.com/azure-only/zulu/packages/zulu-8/8u181/). Dieser Leitfaden enthält Informationen zu den Feinheiten beim Schreiben von Azure Functions mit Java.

Wie in anderen Sprachen auch kann eine Funktions-App über eine oder mehrere Funktionen verfügen. Eine Java-Funktion ist eine `public`-Methode, die mit der Anmerkung `@FunctionName` versehen ist. Diese Methode definiert die Eingabe für eine Java-Funktion und muss in einem bestimmten Paket eindeutig sein. Eine in Java geschriebene Funktions-App kann mehrere Klassen mit mehreren öffentlichen Methoden enthalten, die mit `@FunctionName` kommentiert sind.

In diesem Artikel wird davon ausgegangen, dass Sie bereits die [Entwicklerreferenz zu Azure Functions](functions-reference.md)gelesen haben. Sie sollten auch einen der folgenden Functions-Schnellstarts nutzen: [Schnellstart: Erstellen einer Funktion in Azure mit Visual Studio Code](/azure/azure-functions/functions-create-first-function-vs-code?pivots=programming-language-java) oder [Schnellstart: Erstellen einer Funktion in Azure, die auf HTTP-Anforderungen antwortet](/azure/azure-functions/functions-create-first-azure-function-azure-cli?pivots=programming-language-java).

## <a name="programming-model"></a>Programmiermodell 

Die Konzepte von [Triggern und Bindungen](functions-triggers-bindings.md) sind für Azure Functions von grundlegender Bedeutung. Trigger starten die Ausführung Ihres Codes. Bindungen bieten Ihnen eine Möglichkeit, Daten an eine Funktion zu übergeben und von einer Funktion zurückgeben zu lassen, ohne benutzerdefinierten Datenzugriffscode schreiben zu müssen.

## <a name="create-java-functions"></a>Erstellen von Java-Funktionen

Um das Erstellen von Java-Funktionen zu erleichtern, gibt es Maven-basierte Tools und Archetypen, die vordefinierte Java-Vorlagen verwenden, um Ihnen beim Erstellen von Projekten mit einem bestimmten Funktionstrigger zu helfen.    

### <a name="maven-based-tooling"></a>Maven-basierte Tools

Die folgenden Entwicklerumgebungen verfügen über Azure Functions-Tools, mit denen Sie Java-Funktionsprojekte erstellen können: 

+ [Visual Studio Code](https://code.visualstudio.com/docs/java/java-azurefunctions)
+ [Eclipse](functions-create-maven-eclipse.md)
+ [IntelliJ](functions-create-maven-intellij.md)

Die voranstehenden Links zu Artikeln zeigen Ihnen, wie Sie Ihre ersten Funktionen mithilfe der IDE Ihrer Wahl erstellen. 

### <a name="project-scaffolding"></a>Projektgerüst

Wenn Sie die Entwicklung mit der Befehlszeile über das Terminal bevorzugen, besteht die einfachste Möglichkeit für den Gerüstbau von Java-basierten Funktionsprojekten darin, `Apache Maven`-Archetypes zu verwenden. Der Java Maven-Archetyp wird unter der folgenden _groupId_:_artifactId_ veröffentlicht: [com.microsoft.azure:azure-functions-archetype](https://search.maven.org/artifact/com.microsoft.azure/azure-functions-archetype/). 

Mit dem folgenden Befehl wird ein neues Java-Funktionsprojekt mit diesem Archetyp generiert:

```
mvn archetype:generate \
    -DarchetypeGroupId=com.microsoft.azure \
    -DarchetypeArtifactId=azure-functions-archetype 
```

Informationen zu den ersten Schritten bei der Verwendung dieses Archetyps finden Sie im [Java-Schnellstart](/azure/azure-functions/functions-create-first-azure-function-azure-cli?pivots=programming-language-java). 

## <a name="folder-structure"></a>Ordnerstruktur

Im Folgenden wird die Ordnerstruktur eines Azure Functions-Java-Projekts gezeigt:

```
FunctionsProject
 | - src
 | | - main
 | | | - java
 | | | | - FunctionApp
 | | | | | - MyFirstFunction.java
 | | | | | - MySecondFunction.java
 | - target
 | | - azure-functions
 | | | - FunctionApp
 | | | | - FunctionApp.jar
 | | | | - host.json
 | | | | - MyFirstFunction
 | | | | | - function.json
 | | | | - MySecondFunction
 | | | | | - function.json
 | | | | - bin
 | | | | - lib
 | - pom.xml
```

Sie können die freigegebene Datei [host.json](functions-host-json.md) zum Konfigurieren der Funktions-App verwenden. Jede Funktion verfügt über eine eigene Codedatei (JAVA-Datei) sowie über eine eigene Bindungskonfigurationsdatei („function.json“).

Sie können mehr als eine Funktion in ein Projekt einfügen. Vermeiden Sie es, Ihre Funktionen in separaten JAR-Dateien hinzuzufügen. Die `FunctionApp` im Zielverzeichnis wird in Ihrer Funktions-App in Azure bereitgestellt.

## <a name="triggers-and-annotations"></a>Trigger und Anmerkungen

 Funktionen werden durch einen Trigger, z.B. eine HTTP-Anforderung, einen Timer oder ein Datenupdate, aufgerufen. Ihre Funktion muss diesen Trigger und alle weiteren Eingaben verarbeiten, um mindestens eine Ausgabe zu erstellen.

Verwenden Sie die Java-Anmerkungen im Paket [com.microsoft.azure.functions.annotation.*](/java/api/com.microsoft.azure.functions.annotation), um Eingaben und Ausgaben an Ihre Methoden zu binden. Weitere Informationen finden Sie in der [Java-Referenzdokumentation](/java/api/com.microsoft.azure.functions.annotation).

> [!IMPORTANT] 
> Sie müssen ein Azure Storage-Konto in [local.settings.json](/azure/azure-functions/functions-run-local#local-settings-file) konfigurieren, um Trigger für Azure Blob Storage, Azure Queue Storage oder Azure Table Storage lokal auszuführen.

Beispiel:

```java
public class Function {
    public String echo(@HttpTrigger(name = "req", 
      methods = {"post"},  authLevel = AuthorizationLevel.ANONYMOUS) 
        String req, ExecutionContext context) {
        return String.format(req);
    }
}
```

Hier wird die entsprechende `function.json` gezeigt, die durch [azure-functions-maven-plugin](https://mvnrepository.com/artifact/com.microsoft.azure/azure-functions-maven-plugin) generiert wurde:

```json
{
  "scriptFile": "azure-functions-example.jar",
  "entryPoint": "com.example.Function.echo",
  "bindings": [
    {
      "type": "httpTrigger",
      "name": "req",
      "direction": "in",
      "authLevel": "anonymous",
      "methods": [ "post" ]
    },
    {
      "type": "http",
      "name": "$return",
      "direction": "out"
    }
  ]
}

```

## <a name="jdk-runtime-availability-and-support"></a>Verfügbarkeit und Unterstützung der JDK-Runtime 

Für die lokale Entwicklung von Java-Funktions-Apps laden Sie die [Azul Zulu Enterprise for Azure](https://assets.azul.com/files/Zulu-for-Azure-FAQ.pdf) Java 8-JDKs von [Azul Systems](https://www.azul.com/downloads/azure-only/zulu/) herunter, und verwenden Sie diese. Azure Functions verwendet die Azul Java 8 JDK-Runtime, wenn Sie Ihre Funktions-Apps in der Cloud bereitstellen.

[Azure-Support](https://azure.microsoft.com/support/) bei Problemen mit den JDKs und Funktions-Apps steht mit einem [qualifizierten Supportplan](https://azure.microsoft.com/support/plans/) zur Verfügung.

## <a name="customize-jvm"></a>Anpassen der JVM

Mit Functions können Sie die Java Virtual Machine (JVM) anpassen, mit der Sie Ihre Java-Funktionen ausführen. Die [folgenden JVM-Optionen](https://github.com/Azure/azure-functions-java-worker/blob/master/worker.config.json#L7) werden standardmäßig verwendet:

* `-XX:+TieredCompilation`
* `-XX:TieredStopAtLevel=1`
* `-noverify` 
* `-Djava.net.preferIPv4Stack=true`
* `-jar`

Sie können weitere Argumente in einer App-Einstellung namens `JAVA_OPTS` angeben. Sie können App-Einstellungen zu Ihrer Funktions-App hinzufügen, die im Azure-Portal oder über die Azure CLI in Azure bereitgestellt wird.

> [!IMPORTANT]  
> Im Verbrauchstarif müssen Sie auch die Einstellung WEBSITE_USE_PLACEHOLDER mit einem Wert von null (0) hinzufügen, damit die Anpassung funktioniert. Diese Einstellung erhöht die Kaltstartzeiten für Java-Funktionen.

### <a name="azure-portal"></a>Azure-Portal

Verwenden Sie im [Azur-Portal](https://portal.azure.com) die Registerkarte [Anwendungseinstellungen](functions-how-to-use-azure-function-app-settings.md#settings), um die Einstellung `JAVA_OPTS` hinzuzufügen.

### <a name="azure-cli"></a>Azure CLI

Mit dem Befehl [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings) können Sie `JAVA_OPTS` wie im folgenden Beispiel einstellen:

#### <a name="consumption-plan"></a>[Verbrauchstarif](#tab/consumption)
```azurecli-interactive
az functionapp config appsettings set \
--settings "JAVA_OPTS=-Djava.awt.headless=true" \
"WEBSITE_USE_PLACEHOLDER=0" \
--name <APP_NAME> --resource-group <RESOURCE_GROUP>
```
#### <a name="dedicated-plan--premium-plan"></a>[Dedizierter Tarif/Premium-Tarif](#tab/dedicated+premium)
```azurecli-interactive
az functionapp config appsettings set \
--settings "JAVA_OPTS=-Djava.awt.headless=true" \
--name <APP_NAME> --resource-group <RESOURCE_GROUP>
```
---

In diesem Beispiel wird der monitorlose Modus aktiviert. Ersetzen Sie `<APP_NAME>` durch den Namen Ihrer Funktions-App und `<RESOURCE_GROUP>` durch die Ressourcengruppe. 

## <a name="third-party-libraries"></a>Drittanbieterbibliotheken 

Azure Functions unterstützt die Verwendung von Drittanbieterbibliotheken. Standardmäßig werden alle in der Projektdatei `pom.xml` angegebenen Abhängigkeiten automatisch während des Ziels von [`mvn package`](https://github.com/Microsoft/azure-maven-plugins/blob/master/azure-functions-maven-plugin/README.md#azure-functionspackage) gebündelt. Bibliotheken, die nicht in der Datei `pom.xml` als Abhängigkeiten angegeben sind, platzieren Sie in einem `lib`-Verzeichnis im Stammverzeichnis der Funktion. Abhängigkeiten im Verzeichnis `lib` werden dem Systemklassen-Ladeprogramm zur Laufzeit hinzugefügt.

Die Abhängigkeit `com.microsoft.azure.functions:azure-functions-java-library` wird standardmäßig im Klassenpfad bereitgestellt und muss nicht in das Verzeichnis `lib` eingeschlossen werden. Darüber hinaus fügt [azure-functions-java-worker](https://github.com/Azure/azure-functions-java-worker) die [hier](https://github.com/Azure/azure-functions-java-worker/wiki/Azure-Java-Functions-Worker-Dependencies) aufgelisteten Abhängigkeiten dem Klassenpfad hinzu.

## <a name="data-type-support"></a>Datentypunterstützung

Zum Binden an Eingabe- oder Ausgabebindungen können Sie POJOs (Plain Old Java Objects), in `azure-functions-java-library` definierte Typen oder primitive Datentypen wie z.B. „String“ und „Integer“ verwenden.

### <a name="pojos"></a>POJOs

Zum Konvertieren von Eingabedaten in POJO verwendet [azure-functions-java-worker](https://github.com/Azure/azure-functions-java-worker) die [gson](https://github.com/google/gson)-Bibliothek. Als Eingabe für Funktionen verwendete POJO-Typen müssen `public` sein.

### <a name="binary-data"></a>Binärdaten

Binden Sie binäre Eingaben oder Ausgaben an `byte[]`, indem Sie das Feld `dataType` in der Datei „function.json“ auf `binary` festlegen:

```java
   @FunctionName("BlobTrigger")
    @StorageAccount("AzureWebJobsStorage")
     public void blobTrigger(
        @BlobTrigger(name = "content", path = "myblob/{fileName}", dataType = "binary") byte[] content,
        @BindingName("fileName") String fileName,
        final ExecutionContext context
    ) {
        context.getLogger().info("Java Blob trigger function processed a blob.\n Name: " + fileName + "\n Size: " + content.length + " Bytes");
    }
```

Wenn Sie NULL-Werte erwarten, verwenden Sie `Optional<T>`.

## <a name="bindings"></a>Bindungen

Eingabe- und Ausgabebindungen bieten eine deklarative Möglichkeit, aus Code heraus Verbindungen mit Daten herzustellen. Eine Funktion kann mehrere Eingabe- und Ausgabebindungen aufweisen.

### <a name="input-binding-example"></a>Beispiel für eine Eingabebindung

```java
package com.example;

import com.microsoft.azure.functions.annotation.*;

public class Function {
    @FunctionName("echo")
    public static String echo(
        @HttpTrigger(name = "req", methods = { "put" }, authLevel = AuthorizationLevel.ANONYMOUS, route = "items/{id}") String inputReq,
        @TableInput(name = "item", tableName = "items", partitionKey = "Example", rowKey = "{id}", connection = "AzureWebJobsStorage") TestInputData inputData
        @TableOutput(name = "myOutputTable", tableName = "Person", connection = "AzureWebJobsStorage") OutputBinding<Person> testOutputData,
    ) {
        testOutputData.setValue(new Person(httpbody + "Partition", httpbody + "Row", httpbody + "Name"));
        return "Hello, " + inputReq + " and " + inputData.getKey() + ".";
    }

    public static class TestInputData {
        public String getKey() { return this.RowKey; }
        private String RowKey;
    }
    public static class Person {
        public String PartitionKey;
        public String RowKey;
        public String Name;

        public Person(String p, String r, String n) {
            this.PartitionKey = p;
            this.RowKey = r;
            this.Name = n;
        }
    }
}
```

Diese Funktion rufen Sie mit einer HTTP-Anforderung auf. 
- Die Nutzlast der HTTP-Anforderung wird dem Argument `inputReq` als `String` übergeben.
- Ein Eintrag wird aus Table Storage abgerufen und dem Argument `inputData` als `TestInputData` übergeben.

Sie können eine Bindung mit `String[]`, `POJO[]`, `List<String>` oder `List<POJO>` herstellen, um eine Reihe von Eingaben zu erhalten.

```java
@FunctionName("ProcessIotMessages")
    public void processIotMessages(
        @EventHubTrigger(name = "message", eventHubName = "%AzureWebJobsEventHubPath%", connection = "AzureWebJobsEventHubSender", cardinality = Cardinality.MANY) List<TestEventData> messages,
        final ExecutionContext context)
    {
        context.getLogger().info("Java Event Hub trigger received messages. Batch size: " + messages.size());
    }
    
    public class TestEventData {
    public String id;
}

```

Diese Funktion wird immer dann ausgelöst, wenn im konfigurierten Event Hub neue Daten vorliegen. Da die `cardinality` auf `MANY` festgelegt ist, empfängt die Funktion einen Batch von Nachrichten vom Event Hub. `EventData` aus Event Hub wird für die Ausführung der Funktion in `TestEventData` konvertiert.

### <a name="output-binding-example"></a>Beispiel für eine Ausgabebindung

Mit `$return` können Sie eine Ausgabebindung an den Rückgabewert binden. 

```java
package com.example;

import com.microsoft.azure.functions.annotation.*;

public class Function {
    @FunctionName("copy")
    @StorageAccount("AzureWebJobsStorage")
    @BlobOutput(name = "$return", path = "samples-output-java/{name}")
    public static String copy(@BlobTrigger(name = "blob", path = "samples-input-java/{name}") String content) {
        return content;
    }
}
```

Wenn mehrere Ausgabebindungen vorhanden sind, verwenden Sie den Rückgabewert für nur eine davon.

Um mehrere Ausgabewerte zu senden, verwenden Sie `OutputBinding<T>` gemäß der Definition im Paket `azure-functions-java-library`. 

```java
@FunctionName("QueueOutputPOJOList")
    public HttpResponseMessage QueueOutputPOJOList(@HttpTrigger(name = "req", methods = { HttpMethod.GET,
            HttpMethod.POST }, authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Optional<String>> request,
            @QueueOutput(name = "itemsOut", queueName = "test-output-java-pojo", connection = "AzureWebJobsStorage") OutputBinding<List<TestData>> itemsOut, 
            final ExecutionContext context) {
        context.getLogger().info("Java HTTP trigger processed a request.");
       
        String query = request.getQueryParameters().get("queueMessageId");
        String queueMessageId = request.getBody().orElse(query);
        itemsOut.setValue(new ArrayList<TestData>());
        if (queueMessageId != null) {
            TestData testData1 = new TestData();
            testData1.id = "msg1"+queueMessageId;
            TestData testData2 = new TestData();
            testData2.id = "msg2"+queueMessageId;

            itemsOut.getValue().add(testData1);
            itemsOut.getValue().add(testData2);

            return request.createResponseBuilder(HttpStatus.OK).body("Hello, " + queueMessageId).build();
        } else {
            return request.createResponseBuilder(HttpStatus.INTERNAL_SERVER_ERROR)
                    .body("Did not find expected items in CosmosDB input list").build();
        }
    }

     public static class TestData {
        public String id;
    }
```

Diese Funktion rufen Sie über HttpRequest auf. Sie schreibt mehrere Werte in Queue Storage.

## <a name="httprequestmessage-and-httpresponsemessage"></a>HttpRequestMessage und HttpResponseMessage

 Diese sind in `azure-functions-java-library` definiert. Dies sind Hilfstypen, die mit HttpTrigger-Funktionen verwendet werden.

| Spezialisierter Typ      |       Ziel        | Typische Verwendung                  |
| --------------------- | :-----------------: | ------------------------------ |
| `HttpRequestMessage<T>`  |    HTTP-Trigger     | Ruft die Methode, Header oder Abfragen ab |
| `HttpResponseMessage` | HTTP-Ausgabebindung | Gibt einen anderen Status als 200 zurück   |

## <a name="metadata"></a>Metadaten

Einige Trigger senden [Triggermetadaten](/azure/azure-functions/functions-triggers-bindings) zusammen mit den Eingabedaten. Sie können die Anmerkung `@BindingName` für die Bindung an Triggermetadaten verwenden.


```Java
package com.example;

import java.util.Optional;
import com.microsoft.azure.functions.annotation.*;


public class Function {
    @FunctionName("metadata")
    public static String metadata(
        @HttpTrigger(name = "req", methods = { "get", "post" }, authLevel = AuthorizationLevel.ANONYMOUS) Optional<String> body,
        @BindingName("name") String queryValue
    ) {
        return body.orElse(queryValue);
    }
}
```
Im vorherigen Beispiel ist `queryValue` an den Abfragezeichenfolgen-Parameter `name` in der HTTP-Anforderungs-URL `http://{example.host}/api/metadata?name=test` gebunden. Es folgt ein weiteres Beispiel für die Bindung an die `Id` aus den Metadaten des Warteschlangentriggers.

```java
 @FunctionName("QueueTriggerMetadata")
    public void QueueTriggerMetadata(
        @QueueTrigger(name = "message", queueName = "test-input-java-metadata", connection = "AzureWebJobsStorage") String message,@BindingName("Id") String metadataId,
        @QueueOutput(name = "output", queueName = "test-output-java-metadata", connection = "AzureWebJobsStorage") OutputBinding<TestData> output,
        final ExecutionContext context
    ) {
        context.getLogger().info("Java Queue trigger function processed a message: " + message + " with metadaId:" + metadataId );
        TestData testData = new TestData();
        testData.id = metadataId;
        output.setValue(testData);
    }
```

> [!NOTE]
> Der in der Anmerkung angegebene Name muss der Metadateneigenschaft entsprechen.

## <a name="execution-context"></a>Ausführungskontext

Der in der `azure-functions-java-library` definierte `ExecutionContext` enthält Hilfsmethoden für die Kommunikation mit der Funktionsruntime. Weitere Informationen finden Sie im Referenzartikel zur [Schnittstelle „ExecutionContext“](/java/api/com.microsoft.azure.functions.executioncontext).

### <a name="logger"></a>Protokollierungstool

Verwenden Sie den im `ExecutionContext` definierten `getLogger` zum Schreiben von Protokollen aus Funktionscode.

Beispiel:

```java

import com.microsoft.azure.functions.*;
import com.microsoft.azure.functions.annotation.*;

public class Function {
    public String echo(@HttpTrigger(name = "req", methods = {"post"}, authLevel = AuthorizationLevel.ANONYMOUS) String req, ExecutionContext context) {
        if (req.isEmpty()) {
            context.getLogger().warning("Empty request body received by function " + context.getFunctionName() + " with invocation " + context.getInvocationId());
        }
        return String.format(req);
    }
}
```

## <a name="view-logs-and-trace"></a>Anzeigen von Protokollen und Ablaufverfolgung

Mit der Azure CLI können Sie die Java-Protokollierung von stdout und stderr sowie andere Anwendungsprotokolle streamen. 

Nachfolgend wird erläutert, wie Sie Ihre Funktions-App so konfigurieren, dass die Anwendungsprotokollierung mithilfe der Azure CLI geschrieben wird:

```azurecli-interactive
az webapp log config --name functionname --resource-group myResourceGroup --application-logging true
```

Zum Streaming der Protokollausgabe Ihrer Funktions-App mithilfe der Azure CLI öffnen Sie eine neue Eingabeaufforderungs-, Bash- oder Terminalsitzung und geben den folgenden Befehl ein:

```azurecli-interactive
az webapp log tail --name webappname --resource-group myResourceGroup
```
Der Befehl [az webapp log tail](/cli/azure/webapp/log) ermöglicht das Filtern der Ausgabe mithilfe der Option `--provider`. 

Zum Herunterladen der Protokolldateien als eine einzelne ZIP-Datei mithilfe der Azure CLI öffnen Sie eine neue Eingabeaufforderungs-, Bash- oder Terminalsitzung, und geben Sie den folgenden Befehl ein:

```azurecli-interactive
az webapp log download --resource-group resourcegroupname --name functionappname
```

Die Dateisystemprotokollierung muss im Azure-Portal oder in der Azure CLI aktiviert sein, bevor Sie diesen Befehl ausführen können.

## <a name="environment-variables"></a>Umgebungsvariablen

In Functions werden [App-Einstellungen](functions-app-settings.md), z.B. Dienstverbindungszeichenfolgen, während der Ausführung als Umgebungsvariablen verfügbar gemacht. Sie können über `System.getenv("AzureWebJobsStorage")` auf diese Einstellungen zugreifen.

Im folgenden Beispiel wird die [Anwendungseinstellung](functions-how-to-use-azure-function-app-settings.md#settings) mit dem Schlüssel `myAppSetting` ermittelt:

```java

public class Function {
    public String echo(@HttpTrigger(name = "req", methods = {"post"}, authLevel = AuthorizationLevel.ANONYMOUS) String req, ExecutionContext context) {
        context.getLogger().info("My app setting value: "+ System.getenv("myAppSetting"));
        return String.format(req);
    }
}

```

> [!NOTE]
> Der Wert von AppSetting FUNCTIONS_EXTENSION_VERSION sollte ~2 oder ~3 sein, um eine optimierte Kaltstarterfahrung zu erhalten.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Java-Entwicklung für Azure Functions finden Sie in den folgenden Ressourcen:

* [Bewährte Methoden für Azure Functions](functions-best-practices.md)
* [Entwicklerreferenz zu Azure Functions](functions-reference.md)
* [Trigger und Bindungen in Azure Functions](functions-triggers-bindings.md)
* Lokale Entwicklung und Debuggen mit [Visual Studio Code](https://code.visualstudio.com/docs/java/java-azurefunctions), [IntelliJ](functions-create-maven-intellij.md) und [Eclipse](functions-create-maven-eclipse.md)
* [Remotedebuggen von Java-Funktionen mit Visual Studio Code](https://code.visualstudio.com/docs/java/java-serverless#_remote-debug-functions-running-in-the-cloud)
* [Maven-Plug-In für Azure Functions](https://github.com/Microsoft/azure-maven-plugins/blob/develop/azure-functions-maven-plugin/README.md) 
* Optimieren der Erstellung von Funktionen mithilfe des Ziels `azure-functions:add` und Vorbereiten eines Stagingverzeichnisses für die [Bereitstellung von ZIP-Dateien](deployment-zip-push.md).

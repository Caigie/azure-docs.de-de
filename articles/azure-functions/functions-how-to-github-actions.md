---
title: Verwenden von GitHub Actions zum Vornehmen von Codeaktualisierungen in Azure Functions
description: Erfahren Sie, wie Sie mit GitHub Actions einen Workflow zum Erstellen und Bereitstellen von Azure Functions-Projekten in GitHub definieren.
author: craigshoemaker
ms.topic: conceptual
ms.date: 04/16/2020
ms.author: cshoe
ms.custom: devx-track-python
ms.openlocfilehash: 3fcf26670ec03b1d3a496574e58ae706acdf5c53
ms.sourcegitcommit: 7fe8df79526a0067be4651ce6fa96fa9d4f21355
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2020
ms.locfileid: "87853290"
---
# <a name="continuous-delivery-by-using-github-action"></a>Continuous Delivery mit GitHub Actions

Mit [GitHub Actions](https://github.com/features/actions) können Sie einen Workflow definieren, um Ihren Funktionscode für eine Funktions-App in Azure automatisch zu erstellen und bereitzustellen. 

In GitHub Actions ist ein [Workflow](https://help.github.com/articles/about-github-actions#workflow) ein automatisierter Prozess, den Sie in Ihrem GitHub-Repository definieren. Dieser Prozess informiert GitHub, wie Ihr Funktions-App-Projekt auf GitHub erstellt und bereitgestellt werden soll. 

Ein Workflow wird durch eine YAML-Datei im Pfad `/.github/workflows/` in Ihrem Repository definiert. Diese Definition enthält die verschiedenen Schritte und Parameter, die den Workflow bilden. 

Bei einem Azure Functions-Workflow umfasst die Datei drei Abschnitte: 

| `Section` | Aufgaben |
| ------- | ----- |
| **Authentifizierung** | <ol><li>Definieren eines Dienstprinzipals.</li><li>Herunterladen des Veröffentlichungsprofils</li><li>Erstellen eines GitHub-Geheimnisses.</li></ol>|
| **Build** | <ol><li>Einrichten der Umgebung.</li><li>Erstellen Sie die Funktions-App.</li></ol> |
| **Bereitstellen** | <ol><li>Bereitstellen der Funktions-App.</li></ol>|

> [!NOTE]
> Sie müssen keinen Dienstprinzipal erstellen, wenn Sie das Veröffentlichungsprofil für die Authentifizierung verwenden.

## <a name="create-a-service-principal"></a>Erstellen eines Dienstprinzipals

Sie können mit dem Befehl [az ad sp create-for-rbac](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) in der [Azure CLI](/cli/azure/) einen [Dienstprinzipal](../active-directory/develop/app-objects-and-service-principals.md#service-principal-object) erstellen. Sie können diesen Befehl mit [Azure Cloud Shell](https://shell.azure.com) im Azure-Portal oder durch Auswählen der Schaltfläche **Ausprobieren** ausführen.

```azurecli-interactive
az ad sp create-for-rbac --name "myApp" --role contributor --scopes /subscriptions/<SUBSCRIPTION_ID>/resourceGroups/<RESOURCE_GROUP>/providers/Microsoft.Web/sites/<APP_NAME> --sdk-auth
```

Ersetzen Sie in diesem Beispiel die Platzhalter in der Ressource durch Ihre Abonnement-ID, die Ressourcengruppe und den Namen der Funktions-App. Bei der Ausgabe handelt es sich um die Anmeldeinformationen für die Rollenzuweisung, die Zugriff auf ihre Funktions-App bereitstellen. Kopieren Sie dieses JSON-Objekt, das Sie zum Authentifizieren aus GitHub verwenden können.

> [!IMPORTANT]
> Es ist immer empfehlenswert, den minimalen Zugriff zu gewähren. Aus diesem Grund ist der Bereich im vorherigen Beispiel auf die spezifische Funktions-App und nicht auf die gesamte Ressourcengruppe eingeschränkt.

## <a name="download-the-publishing-profile"></a>Herunterladen des Veröffentlichungsprofils

So laden Sie das Veröffentlichungs Profil Ihrer Funktions-App herunter:

1. Wählen Sie die Seite **Übersicht** der Funktions-App aus, und wählen Sie dann **Veröffentlichungsprofil abrufen** aus.

   :::image type="content" source="media/functions-how-to-github-actions/get-publish-profile.png" alt-text="Herunterladen des Veröffentlichungsprofils":::

1. Speichern und kopieren Sie den Inhalt der Datei mit den Veröffentlichungseinstellungen.

## <a name="configure-the-github-secret"></a>Konfigurieren des GitHub-Geheimnisses

1. Navigieren Sie in [GitHub](https://github.com) zu Ihrem Repository, und wählen Sie **Einstellungen** > **Geheimnisse** > **Neues Geheimnis hinzufügen** aus.

   :::image type="content" source="media/functions-how-to-github-actions/add-secret.png" alt-text="Hinzufügen des Geheimnisses":::

1. Fügen Sie ein neues Geheimnis hinzu.

   * Wenn Sie den Dienstprinzipal verwenden, den Sie mit der Azure CLI erstellt haben, verwenden Sie `AZURE_CREDENTIALS` als **Namen**. Fügen Sie dann die kopierte JSON-Objektausgabe für den **Wert** ein, und wählen Sie **Geheimnis hinzufügen** aus.
   * Wenn Sie ein Veröffentlichungsprofil verwenden, verwenden Sie `SCM_CREDENTIALS` für den **Namen**. Verwenden Sie dann den Dateiinhalt des Veröffentlichungsprofils für den **Wert**, und wählen Sie **Geheimnis hinzufügen** aus.

GitHub kann sich jetzt bei ihrer Funktions-App in Azure authentifizieren.

## <a name="set-up-the-environment"></a>Einrichten der Umgebung 

Das Einrichten der Umgebung erfolgt mithilfe einer sprachspezifischen Aktionen zum Veröffentlichen des Setups.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Die folgenden Beispiele zeigen den Teil des Workflows, der die `actions/setup-node`-Aktion verwendet, um die Umgebung einzurichten:

```yaml
    - name: 'Login via Azure CLI'
      uses: azure/login@v1
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}
    - name: Setup Node 10.x
      uses: actions/setup-node@v1
      with:
        node-version: '10.x'
```

# <a name="python"></a>[Python](#tab/python)

Die folgenden Beispiele zeigen den Teil des Workflows, der die `actions/setup-python`-Aktion verwendet, um die Umgebung einzurichten:

```yaml
    - name: 'Login via Azure CLI'
      uses: azure/login@v1
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}
    - name: Setup Python 3.6
      uses: actions/setup-python@v1
      with:
        python-version: 3.6
```

# <a name="c"></a>[C#](#tab/csharp)

Die folgenden Beispiele zeigen den Teil des Workflows, der die `actions/setup-dotnet`-Aktion verwendet, um die Umgebung einzurichten:

```yaml
    - name: 'Login via Azure CLI'
      uses: azure/login@v1
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}
    - name: Setup Dotnet 2.2.300
      uses: actions/setup-dotnet@v1
      with:
        dotnet-version: '2.2.300'
```

# <a name="java"></a>[Java](#tab/java)

Die folgenden Beispiele zeigen den Teil des Workflows, der die `actions/setup-java`-Aktion verwendet, um die Umgebung einzurichten:

```yaml
    - name: 'Login via Azure CLI'
      uses: azure/login@v1
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}
    - name: Setup Java 1.8.x
      uses: actions/setup-java@v1
      with:
        # If your pom.xml <maven.compiler.source> version is not in 1.8.x
        # Please change the Java version to match the version in pom.xml <maven.compiler.source>
        java-version: '1.8.x'
```
---

## <a name="build-the-function-app"></a>Erstellen der Funktions-App

Dies hängt von der Sprache und den von Azure Functions unterstützten Sprachen ab. Dieser Abschnitt sollte die Standardbuildschritte der einzelnen Sprachen beinhalten.

Die folgenden Beispiele zeigen den Teil des Workflows, der die Funktions-App erstellt, die sprachspezifisch ist:

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```yaml
    - name: 'Run npm'
      shell: bash
      run: |
        # If your function app project is not located in your repository's root
        # Please change your directory for npm in pushd
        pushd .
        npm install
        npm run build --if-present
        npm run test --if-present
        popd
```

# <a name="python"></a>[Python](#tab/python)

```yaml
    - name: 'Run pip'
      shell: bash
      run: |
        # If your function app project is not located in your repository's root
        # Please change your directory for pip in pushd
        pushd .
        python -m pip install --upgrade pip
        pip install -r requirements.txt --target=".python_packages/lib/python3.6/site-packages"
        popd
```

# <a name="c"></a>[C#](#tab/csharp)

```yaml
    - name: 'Run dotnet build'
      shell: bash
      run: |
        # If your function app project is not located in your repository's root
        # Please consider using pushd to change your path
        pushd .
        dotnet build --configuration Release --output ./output
        popd
```

# <a name="java"></a>[Java](#tab/java)

```yaml
    - name: 'Run mvn'
      shell: bash
      run: |
        # If your function app project is not located in your repository's root
        # Please change your directory for maven build in pushd
        pushd . ./POM_ARTIFACT_ID
        mvn clean package
        mvn azure-functions:package
        popd
```
---

## <a name="deploy-the-function-app"></a>Bereitstellen der Funktionen-App

Wenn Sie Ihren Code in einer Funktions-App bereitstellen möchten, müssen Sie die Aktion `Azure/functions-action` verwenden. Diese Aktion verfügt über zwei Parameter:

|Parameter |Erklärung  |
|---------|---------|
|**_app-name_** | (Erforderlich) Der Name Ihrer Funktions-App. |
|_**slot-name**_ | (Optional) Der Name des [Bereitstellungsslots](functions-deployment-slots.md), in dem die Bereitstellung erfolgen soll. Der Slot muss in ihrer Funktions-App bereits definiert sein. |


Im folgenden Beispiel wird Version 1 von `functions-action` verwendet:

```yaml
    - name: 'Run Azure Functions Action'
      uses: Azure/functions-action@v1
      id: fa
      with:
        app-name: PLEASE_REPLACE_THIS_WITH_YOUR_FUNCTION_APP_NAME
```

## <a name="next-steps"></a>Nächste Schritte

Eine vollständige YAML-Datei des Workflows finden Sie in einer der Dateien mit `functionapp` im Namen im [Repository mit Workflowbeispielen für Azure GitHub Actions ](https://aka.ms/functions-actions-samples). Sie können diese Beispiele als Ausgangspunkt für den Workflow verwenden.

> [!div class="nextstepaction"]
> [Weitere Informationen zu GitHub Actions](https://help.github.com/en/articles/about-github-actions)

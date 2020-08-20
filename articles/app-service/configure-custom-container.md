---
title: Konfigurieren eines benutzerdefinierten Linux-Containers
description: Erfahren Sie, wie Sie einen benutzerdefinierten Linux-Container in Azure App Service konfigurieren. In diesem Artikel werden die gängigsten Konfigurationsaufgaben vorgestellt.
ms.topic: article
ms.date: 03/28/2019
ms.openlocfilehash: 2f26f1b041b2d369b68aeb11755c8e8053862b16
ms.sourcegitcommit: 2ffa5bae1545c660d6f3b62f31c4efa69c1e957f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2020
ms.locfileid: "88082794"
---
# <a name="configure-a-custom-linux-container-for-azure-app-service"></a>Konfigurieren eines benutzerdefinierten Linux-Containers für Azure App Service

In diesem Artikel erfahren Sie, wie Sie einen benutzerdefinierten Linux-Container für die Ausführung unter Azure App Service konfigurieren.

Dieser Leitfaden enthält die wichtigsten Konzepte und Anweisungen für die Containerstellung von Linux-Apps in App Service. Wenn Sie Azure App Service noch nie verwendet haben, befolgen Sie zunächst den [Schnellstart zu benutzerdefinierten Containern](quickstart-custom-container.md?pivots=container-linux) und das [Tutorial](tutorial-custom-container.md?pivots=container-linux). Es gibt auch eine Schnellstartanleitung für [Apps mit mehreren Containern](quickstart-multi-container.md) und ein [Tutorial](tutorial-multi-container-app.md).

## <a name="configure-port-number"></a>Konfigurieren der Portnummer

App Service geht standardmäßig davon aus, dass der benutzerdefinierte Container an Port 80 lauscht. Der Webserver in Ihrem benutzerdefinierten Image kann einen anderen Port als 80 verwenden. Sie teilen Azure den von Ihrem benutzerdefinierten Container verwendeten Port über die App-Einstellung `WEBSITES_PORT` mit. Die GitHub-Seite für das [Python-Beispiel in diesem Tutorial](https://github.com/Azure-Samples/docker-django-webapp-linux) zeigt an, dass Sie für `WEBSITES_PORT` den Wert _8000_ festlegen müssen. Sie können den Wert mit dem Befehl [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) in der Cloud Shell festlegen. Beispiel:

```azurecli-interactive
az webapp config appsettings set --resource-group <resource-group-name> --name <app-name> --settings WEBSITES_PORT=8000
```

## <a name="configure-environment-variables"></a>Konfigurieren von Umgebungsvariablen

Ihr benutzerdefinierter Container kann Umgebungsvariablen verwenden, die extern bereitgestellt werden müssen. Sie können sie mit dem Befehl [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) in der Cloud Shell übergeben. Beispiel:

```azurecli-interactive
az webapp config appsettings set --resource-group <resource-group-name> --name <app-name> --settings WORDPRESS_DB_HOST="myownserver.mysql.database.azure.com"
```

Diese Methode funktioniert sowohl für Apps mit einem einzelnen Container als auch für Apps mit mehreren Containern, wobei die Umgebungsvariablen in der Datei *docker-compose.yml* angegeben sind.

## <a name="use-persistent-shared-storage"></a>Verwenden von beständig freigegebenem Speicher

Sie können das Verzeichnis */home* im Dateisystem Ihrer App verwenden, damit Dateien auch nach einem Neustart erhalten bleiben und instanzübergreifend freigegeben werden können. Das `/home` wird in Ihrer App bereitgestellt, damit Ihre Container-App auf den beständigen Speicher zugreifen kann.

Wenn der beständige Speicher deaktiviert ist, bleiben Schreibvorgänge in das Verzeichnis `/home` nicht über App-Neustarts oder mehrere Instanzen hinweg erhalten. Die einzige Ausnahme ist das Verzeichnis `/home/LogFiles`, das zum Speichern der Docker- und Containerprotokolle verwendet wird. Wenn der beständige Speicher aktiviert ist, bleiben alle Schreibvorgänge in das Verzeichnis `/home` erhalten und können von allen Instanzen einer horizontal skalierten App aufgerufen werden.

Standardmäßig ist der persistente Speicher *aktiviert* und die Einstellung wird in den Anwendungseinstellungen nicht angezeigt. Um ihn zu deaktivieren, legen Sie die App-Einstellung `WEBSITES_ENABLE_APP_SERVICE_STORAGE` fest, indem Sie den [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set)-Befehl in der Cloud Shell ausführen. Beispiel:

```azurecli-interactive
az webapp config appsettings set --resource-group <resource-group-name> --name <app-name> --settings WEBSITES_ENABLE_APP_SERVICE_STORAGE=false
```

> [!NOTE]
> Sie können auch [Ihren eigenen beständigen Speicher konfigurieren](configure-connect-to-azure-storage.md?pivots=platform-linux).

## <a name="enable-ssh"></a>Aktivieren von SSH

SSH ermöglicht die sichere Kommunikation zwischen einem Container und einem Client. Damit ein benutzerdefinierter Container SSH unterstützt, müssen Sie ihn in die Docker-Datei selbst einfügen.

> [!TIP]
> Alle integrierten Linux-Container haben die SSH-Anweisungen in ihren Imagerepositorys hinzugefügt. Sie können die folgenden Anweisungen mit dem [Node.js 10.14-Repository](https://github.com/Azure-App-Service/node/blob/master/10.14) durchgehen, um zu prüfen, wie es dort aktiviert ist.

- Verwenden Sie die Anweisung [AUSFÜHREN](https://docs.docker.com/engine/reference/builder/#run), um den SSH-Server zu installieren und das Kennwort für den Superuser auf `"Docker!"` festzulegen. Für ein Image, das auf [Alpine Linux](https://hub.docker.com/_/alpine) basiert, benötigen Sie z. B. die folgenden Befehle:

    ```Dockerfile
    RUN apk add openssh \
         && echo "root:Docker!" | chpasswd 
    ```

    Diese Konfiguration erlaubt keine externen Verbindungen zum Container. SSH ist nur über `https://<app-name>.scm.azurewebsites.net` verfügbar und wird mit den Anmeldeinformationen für die Veröffentlichung authentifiziert.

- Fügen Sie [diese sshd_config-Datei](https://github.com/Azure-App-Service/node/blob/master/10.14/sshd_config) zu Ihrem Imagerepository hinzu, und kopieren Sie die Datei mit dem Befehl [COPY](https://docs.docker.com/engine/reference/builder/#copy) in das Verzeichnis */etc/ssh/*. Weitere Informationen zu *sshd_config*-Dateien finden Sie in der [OpenBSD-Dokumentation](https://man.openbsd.org/sshd_config).

    ```Dockerfile
    COPY sshd_config /etc/ssh/
    ```

    > [!NOTE]
    > Die *sshd_config*-Datei muss Folgendes enthalten:
    > - `Ciphers` muss mindestens ein Element aus dieser Liste enthalten: `aes128-cbc,3des-cbc,aes256-cbc`.
    > - `MACs` muss mindestens ein Element aus dieser Liste enthalten: `hmac-sha1,hmac-sha1-96`.

- Öffnen Sie mit der Anweisung [EXPOSE](https://docs.docker.com/engine/reference/builder/#expose) den Port 2222 im Container. Selbst wenn das Root-Kennwort bekannt ist, kann auf Port 2222 nicht aus dem Internet zugegriffen werden. Er ist nur für Containern innerhalb des Brückennetzwerks eines privaten virtuellen Netzwerks zugänglich.

    ```Dockerfile
    EXPOSE 80 2222
    ```

- Starten Sie im Startskript für Ihren Container den SSH-Server.

    ```bash
    /usr/sbin/sshd
    ```

    Sehen Sie sich z. B. an, wie der standardmäßige [Node.js 10.14-Container](https://github.com/Azure-App-Service/node/blob/master/10.14/startup/init_container.sh) den SSH-Server startet.

## <a name="access-diagnostic-logs"></a>Zugreifen auf Diagnoseprotokolle

[!INCLUDE [Access diagnostic logs](../../includes/app-service-web-logs-access-linux-no-h.md)]

## <a name="change-the-docker-image-of-a-custom-container"></a>Ändern des Docker-Images eines benutzerdefinierten Containers

Wenn Sie eine vorhandene benutzerdefinierte Container-App vom aktuellen Docker-Image auf ein neues Image umstellen möchten, verwenden Sie den folgenden Befehl:

```azurecli-interactive
az webapp config container set --name <app-name> --resource-group <group-name> --docker-custom-image-name <docker-hub-repo>/<image>
```

## <a name="use-an-image-from-a-private-registry"></a>Verwenden eines Images aus einer privaten Registrierung

Führen Sie den folgenden Befehl aus, um ein Image aus einer privaten Registrierung wie Azure Container Registry zu verwenden:

```azurecli-interactive
az webapp config container set --name <app-name> --resource-group <group-name> --docker-custom-image-name <image-name> --docker-registry-server-url <private-repo-url> --docker-registry-server-user <username> --docker-registry-server-password <password>
```

Geben Sie für *\<username>* und *\<password>* die Anmeldeinformationen für das Konto Ihrer privaten Registrierung an.

## <a name="configure-multi-container-apps"></a>Konfigurieren von Apps mit mehreren Containern

- [Verwenden von beständigem Speicher in Docker Compose](#use-persistent-storage-in-docker-compose)
- [Einschränkungen der Vorschau](#preview-limitations)
- [Optionen von Docker Compose](#docker-compose-options)

### <a name="use-persistent-storage-in-docker-compose"></a>Verwenden von beständigem Speicher in Docker Compose

Apps mit mehreren Containern wie WordPress benötigen einen beständigen Speicher, um ordnungsgemäß zu funktionieren. Um dies zu ermöglichen, muss Ihre Docker Compose-Konfiguration auf einen Speicherort *außerhalb* Ihres Containers verweisen. Speicherorte in Ihrem Container bleiben nach dem Neustart der App nicht mehr unverändert.

Aktivieren Sie den beständigen Speicher, indem Sie die App-Einstellung `WEBSITES_ENABLE_APP_SERVICE_STORAGE` mit dem Befehl [az webapp config appsettings set](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) in der Cloud Shell festlegen.

```azurecli-interactive
az webapp config appsettings set --resource-group <resource-group-name> --name <app-name> --settings WEBSITES_ENABLE_APP_SERVICE_STORAGE=TRUE
```

Ordnen Sie in Ihrer Datei *docker-compose.yml* die `volumes`-Option zu `${WEBAPP_STORAGE_HOME}` zu. 

`WEBAPP_STORAGE_HOME` ist eine Umgebungsvariable in App Service, die dem beständigen Speicher für Ihre App zugeordnet wird. Beispiel:

```yaml
wordpress:
  image: wordpress:latest
  volumes:
  - ${WEBAPP_STORAGE_HOME}/site/wwwroot:/var/www/html
  - ${WEBAPP_STORAGE_HOME}/phpmyadmin:/var/www/phpmyadmin
  - ${WEBAPP_STORAGE_HOME}/LogFiles:/var/log
```

### <a name="preview-limitations"></a>Einschränkungen der Vorschau

Apps mit mehreren Containern befinden sich derzeit in der Vorschauphase. Die folgenden Features der App Service-Plattform werden nicht unterstützt:

- Authentifizierung/Autorisierung
- Verwaltete Identitäten

### <a name="docker-compose-options"></a>Optionen von Docker Compose

In den folgenden Listen werden unterstützte und nicht unterstützte Docker Compose-Konfigurationsoptionen angezeigt:

#### <a name="supported-options"></a>Unterstützte Optionen

- command
- entrypoint
- Environment
- image
- ports
- restart
- services
- volumes

#### <a name="unsupported-options"></a>Nicht unterstützte Optionen

- build (unzulässig)
- depends_on (ignoriert)
- networks (ignoriert)
- secrets (ignoriert)
- andere Ports als 80 und 8080 (ignoriert)

> [!NOTE]
> Alle weiteren Optionen, die nicht ausdrücklich aufgeführt sind, werden in der Public Preview ignoriert.

[!INCLUDE [robots933456](../../includes/app-service-web-configure-robots933456.md)]

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Tutorial: Bereitstellen aus privatem Containerrepository](tutorial-custom-container.md?pivots=container-linux)

> [!div class="nextstepaction"]
> [Tutorial: WordPress-App mit mehreren Containern](tutorial-multi-container-app.md)

---
title: Einrichten der Azure Service Fabric Mesh-CLI
description: Die Service Fabric Mesh-Befehlszeilenschnittstelle (Command Line Interface, CLI) ist zum Bereitstellen und Verwalten von Ressourcen lokal und in Azure Service Fabric Mesh erforderlich. Die Einrichtung erfolgt folgendermaßen.
author: dkkapur
ms.author: dekapur
ms.date: 11/28/2018
ms.topic: conceptual
ms.openlocfilehash: 3ce2dbcefa63cc55a66e712664d1436ca24d3a1d
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2020
ms.locfileid: "86248508"
---
# <a name="set-up-service-fabric-mesh-cli"></a>Einrichten der Service Fabric Mesh-CLI
Die Service Fabric Mesh-Befehlszeilenschnittstelle (Command Line Interface, CLI) ist zum Bereitstellen und Verwalten von Ressourcen lokal und in Azure Service Fabric Mesh erforderlich. Die Einrichtung erfolgt folgendermaßen.

Es gibt drei Arten der CLI, die Sie verwenden können. Sie sind unten in der Tabelle zusammengefasst.

| CLI-Modul | Zielumgebung |  BESCHREIBUNG | 
|---|---|---|
| az mesh | Azure Service Fabric Mesh | Die primäre CLI, mit der Sie Ihre Anwendungen bereitstellen und Ressourcen für die Azure Service Fabric Mesh-Umgebung verwalten können. 
| sfctl | Lokale Cluster | Die Service Fabric-CLI, die die Bereitstellung und das Testen von Service Fabric-Ressourcen für lokale Cluster ermöglicht.  
| Maven-CLI | Lokale Cluster und Azure Service Fabric Mesh | Ein Wrapper für `az mesh` und `sfctl`, mit dem Java-Entwickler für die Entwicklung lokal und in Azure eine vertraute Befehlszeilenoberfläche nutzen können.  

Für die Vorschauversion wird die Azure Service Fabric Mesh CLI als eine Erweiterung der Azure CLI bereitgestellt. Sie können sie in der Azure Cloud Shell oder in einer lokalen Installation der Azure CLI installieren. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)] 

## <a name="install-the-azure-service-fabric-mesh-cli"></a>Installieren der Azure Service Fabric Mesh-CLI
1. Sie müssen Azure CLI 2.0.67 oder höher installieren. Führen Sie `az --version` aus, um die Version zu ermitteln. Informationen zur Installation und zu einem Upgrade auf die neueste Version der CLI finden Sie unter [Installieren der Azure CLI][azure-cli-install].

2. Installieren Sie das Azure Service Fabric Mesh-CLI-Erweiterungsmodul mit dem folgenden Befehl. 

    ```azurecli-interactive
    az extension add --name mesh
    ```

3. Verwenden Sie zum Aktualisieren eines vorhandenen Azure Service Fabric Mesh-CLI-Moduls den folgenden Befehl.

    ```azurecli-interactive
    az extension update --name mesh
    ```

## <a name="install-the-service-fabric-cli-sfctl"></a>Installieren der Service Fabric-CLI (sfctl) 

Befolgen Sie die Anleitung unter [Azure Service Fabric CLI](../service-fabric/service-fabric-cli.md). Das Modul **sfctl** kann für die Bereitstellung von Anwendungen basierend auf dem Ressourcenmodell für Service Fabric-Cluster auf Ihrem lokalen Computer verwendet werden. 

## <a name="install-the-maven-cli"></a>Installieren der Maven-CLI 

Auf Ihrem Computer muss Folgendes installiert sein, damit Sie die Maven-CLI verwenden können: 

* [Java](https://www.azul.com/downloads/zulu/)
* [Maven](https://maven.apache.org/download.cgi)
* [Git-Client](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
* Azure Mesh-CLI (az mesh): Für Azure Service Fabric Mesh 
* SFCTL (sfctl): Für lokale Cluster 

Die Maven-CLI für Service Fabric befindet sich noch in der Vorschauphase. 

Fügen Sie Ihrer Datei „pom.xml“ den folgenden Codeausschnitt hinzu, um das Maven-Plug-In in Ihrer Maven-Java-App zu nutzen:

```XML
<project>
  ...
  <build>
    ...
    <plugins>
      ...
      <plugin>
        <groupId>com.microsoft.azure</groupId>
          <artifactId>azure-sfmesh-maven-plugin</artifactId>
          <version>0.1.0</version>
          <configuration>
            ...
          </configuration>
      </plugin>
    </plugins>
  </build>
</project>
```

Lesen Sie den Abschnitt mit der [Referenz zur Maven-CLI](service-fabric-mesh-reference-maven.md), um ausführliche Informationen zur Nutzung zu erhalten.

## <a name="next-steps"></a>Nächste Schritte

Sie können auch Ihre [Windows-Entwicklungsumgebung](service-fabric-mesh-howto-setup-developer-environment-sdk.md) einrichten.

[Hier](service-fabric-mesh-faq.md) finden Sie Antworten auf häufig gestellte Fragen sowie Informationen zu Problemen.

[azure-cli-install]: /cli/azure/install-azure-cli

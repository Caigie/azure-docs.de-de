---
title: (VERALTET) Bereitstellen von Containern mit Helm in Azure Kubernetes
description: Verwenden Sie das Helm-Verpackungstool zum Bereitstellen von Containern in einem Kubernetes-Cluster in Azure Container Service.
author: sauryadas
ms.service: container-service
ms.topic: conceptual
ms.date: 04/10/2017
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: 2a3423b908ac5e089faa30b19914d3b74ae9a547
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "82735418"
---
# <a name="deprecated-use-helm-to-deploy-containers-on-a-kubernetes-cluster"></a>(VERALTET) Verwenden von Helm zum Bereitstellen von Containern in einem Kubernetes-Cluster

> [!TIP]
> Die aktualisierte Version dieses Artikels, wo die Verwendung von Azure Kubernetes Service berücksichtigt wird, finden Sie unter [Installieren von Anwendungen mit Helm in Azure Kubernetes Service (AKS)](../../aks/kubernetes-helm.md).

[!INCLUDE [ACS deprecation](../../../includes/container-service-kubernetes-deprecation.md)]

[Helm](https://github.com/kubernetes/helm/) ist ein Open Source-Verpackungstool, das Ihnen dabei hilft, Kubernetes-Anwendungen zu installieren und ihren Lebenszyklus zu verwalten. Ähnlich wie Linux-Paket-Manager (z.B. Apt-get und Yum) wird Helm zur Verwaltung von Kubernetes-Diagrammen verwendet, bei denen es sich um Pakete aus vorkonfigurierten Kubernetes-Ressourcen handelt. Dieser Artikel veranschaulicht die Verwendung von Helm für einen Kubernetes-Cluster, der in Azure Container Service bereitgestellt wird.

Helm besteht aus zwei Komponenten: 
* Die **Helm-CLI** ist ein Client, der auf dem Computer lokal oder in der Cloud ausgeführt wird.  

* **iller** ist ein Server, der im Kubernetes-Cluster ausgeführt wird und den Lebenszyklus der Kubernetes-Anwendungen verwaltet. 
 
## <a name="prerequisites"></a>Voraussetzungen

* [Erstellen eines Kubernetes-Clusters](container-service-kubernetes-walkthrough.md) in Azure Container Service

* [Installieren und Konfigurieren von `kubectl`](../container-service-connect.md) auf einem lokalen Computer

* [Installieren von Helm](https://helm.sh/docs/intro/quickstart/) auf einem lokalen Computer

## <a name="helm-basics"></a>Grundlagen von Helm 

Um Informationen über den Kubernetes-Cluster anzuzeigen, in dem Sie Tiller installieren und die Anwendungen bereitstellen, geben Sie den folgenden Befehl ein:

```bash
kubectl cluster-info 
```
![kubectl cluster-info](./media/container-service-kubernetes-helm/clusterinfo.png)
 
Installieren Sie nach der Installation von Helm Tiller im Kubernetes-Cluster, indem Sie den folgenden Befehl eingeben:

```bash
helm init --upgrade
```
Wenn der Vorgang erfolgreich abgeschlossen wurde, erhalten Sie eine Ausgabe wie die folgende:

![Tiller-Installation](./media/container-service-kubernetes-helm/tiller-install.png)
 
 
 
 
Um alle im Repository verfügbaren Helm-Diagramme anzuzeigen, geben Sie den folgenden Befehl ein:

```bash 
helm search 
```

Ihnen sollte eine Ausgabe wie die folgende angezeigt werden:

![Helm-Suche](./media/container-service-kubernetes-helm/helm-search.png)
 
Um die Diagramme auf die neuesten Versionen zu aktualisieren, geben Sie Folgendes ein:

```bash 
helm repo update 
```
## <a name="deploy-an-nginx-ingress-controller-chart"></a>Bereitstellen eines Nginx-Eingangscontroller-Diagramms 
 
Geben Sie zum Bereitstellen eines Nginx-Eingangscontroller-Diagramms einen einzelnen Befehl ein:

```bash
helm install stable/nginx-ingress 
```
![Eingangscontroller bereitstellen](./media/container-service-kubernetes-helm/nginx-ingress.png)

Wenn Sie `kubectl get svc` eingeben, um alle im Cluster ausgeführten Dienste anzuzeigen, sehen Sie, dass dem Eingangscontroller eine IP-Adresse zugewiesen wurde. (Während die Zuweisung ausgeführt wird, sehen Sie `<pending>`. Dieser Vorgang nimmt einige Minuten in Anspruch.) 

Nachdem die IP-Adresse zugewiesen wurde, navigieren Sie zum Wert der externen IP-Adresse, um die Ausführung des Nginx-Back-Ends anzuzeigen. 
 
![Eingangs-IP-Adresse](./media/container-service-kubernetes-helm/ingress-ip-address.png)


Um eine Liste der im Cluster installierten Diagramme anzuzeigen, geben Sie Folgendes ein:

```bash
helm list 
```

Sie können den Befehl wie folgt abkürzen: `helm ls`.
 
 
 
 
## <a name="deploy-a-mariadb-chart-and-client"></a>Bereitstellen eines MariaDB-Diagramms und eines Clients

Stellen Sie nun ein MariaDB-Diagramm und einen MariaDB-Client bereit, um eine Verbindung mit der Datenbank herzustellen.

Geben Sie zum Bereitstellen des MariaDB-Diagramms den folgenden Befehl ein:

```bash
helm install --name v1 stable/mariadb
```

wobei `--name` ein für Releases verwendetes Tag ist.

> [!TIP]
> Sollte die Bereitstellung fehlschlagen, führen Sie `helm repo update` aus, und versuchen Sie es erneut.
>
 
 
Um alle im Cluster bereitgestellten Diagramme anzuzeigen, geben Sie Folgendes ein:

```bash 
helm list
```
 
Um alle im Cluster ausgeführten Bereitstellungen anzuzeigen, geben Sie Folgendes ein:

```bash
kubectl get deployments 
``` 
 
 
Geben Sie schließlich Folgendes ein, um ein Pod für den Zugriff auf den Client auszuführen:

```bash
kubectl run v1-mariadb-client --rm --tty -i --image bitnami/mariadb --command -- bash  
``` 
 
 
Um eine Verbindung mit dem Client herzustellen, geben Sie den folgenden Befehl ein, und ersetzen Sie dabei `v1-mariadb` durch den Namen Ihrer Bereitstellung:

```bash
sudo mysql –h v1-mariadb
```
 
 
Nun können Sie SQL-Standardbefehle verwenden, um Datenbanken, Tabellen usw. zu erstellen. Mit `Create DATABASE testdb1;` erstellen Sie z.B. eine leere Datenbank. 
 
 
 
## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zum Verwalten von Kubernetes-Diagrammen finden Sie in der [Helm-Dokumentation](https://helm.sh/docs/). 


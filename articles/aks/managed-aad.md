---
title: Verwenden von Azure AD in Azure Kubernetes Service
description: Erfahren Sie, wie Sie Azure AD in Azure Kubernetes Service (AKS) verwenden.
services: container-service
manager: gwallace
ms.topic: article
ms.date: 05/11/2020
ms.openlocfilehash: 67f5f707ad2971551e3c9623dd5c07ad880afcf2
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2020
ms.locfileid: "83204377"
---
# <a name="integrate-azure-ad-in-azure-kubernetes-service-preview"></a>Integrieren von Azure AD in Azure Kubernetes Service (Vorschau)

> [!Note]
> Für bestehende AKS-Cluster mit Azure Active Directory-Integration (AAD) ist die neue, von AKS verwaltete AAD-Integration nicht verfügbar.

Die Azure AD-Integration mit einer von AKS verwalteten AAD-Bereitstellung wird die Azure AD-Integration vereinfacht, in der Benutzer früher eine Client-App und eine Server-App erstellen und dem Azure AD-Mandanten Leseberechtigungen für Verzeichnisse erteilen mussten. In der neuen Version verwaltet der AKS-Ressourcenanbieter die Client- und Server-App.

## <a name="limitations"></a>Einschränkungen

* Bestehende, mit AAD integrierte AKS-Cluster lassen sich zurzeit nicht auf die neue, von AKS verwaltete AAD-Version upgraden.

> [!IMPORTANT]
> AKS-Previewfunktionen stehen gemäß dem Self-Service- und Aktivierungsprinzip zur Verfügung. Vorschauversionen werden „wie besehen“ und „wie verfügbar“ bereitgestellt und sind von den Vereinbarungen zum Service Level und der eingeschränkten Garantie ausgeschlossen. AKS-Vorschauversionen werden teilweise vom Kundensupport auf Grundlage der bestmöglichen Leistung abgedeckt. Daher sind diese Funktionen nicht für die Verwendung in der Produktion vorgesehen. Weitere Informationen finden Sie in den folgenden Supportartikeln:
>
> - [Unterstützungsrichtlinien für Azure Kubernetes Service](support-policies.md)
> - [Häufig gestellte Fragen zum Azure-Support](faq.md)

## <a name="before-you-begin"></a>Voraussetzungen

> [!Important]
> Sie müssen Kubectl mit der Mindestversion 1.18 verwenden.

Die folgenden Ressourcen müssen installiert sein:

- Die Azure CLI mit Version 2.5.1 oder höher
- Die aks-preview 0.4.38-Erweiterung
- Kubectl mit der Mindestversion [1.18](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.18.md#v1180)

Verwenden Sie zum Installieren/Aktualisieren der aks-preview-Erweiterung oder einer höheren Version die folgenden Azure CLI-Befehle:

```azurecli
az extension add --name aks-preview
az extension list
```

```azurecli
az extension update --name aks-preview
az extension list
```

Verwenden Sie zum Installieren von kubectl Folgendes:

```azurecli
sudo az aks install-cli
kubectl version --client
```

Verwenden Sie für andere Betriebssysteme [diese Anweisungen](https://kubernetes.io/docs/tasks/tools/install-kubectl/).

> [!CAUTION]
> Nachdem Sie ein Feature für ein Abonnement registriert haben, können Sie die Registrierung dieses Features momentan nicht mehr aufheben. Wenn Sie einige Previewfunktionen aktivieren, können Standardwerte für alle AKS-Cluster verwendet werden, die anschließend im Abonnement erstellt werden. Aktivieren Sie keine Vorschaufeatures für Produktionsabonnements. Verwenden Sie stattdessen ein separates Abonnement, um Previewfunktionen zu testen und Feedback zu erfassen.

```azurecli-interactive
az feature register --name AAD-V2 --namespace Microsoft.ContainerService
```

Es kann einige Minuten dauern, bis der Status als **Registriert** angezeigt wird. Sie können den Registrierungsstatus mithilfe des Befehls [az feature list](https://docs.microsoft.com/cli/azure/feature?view=azure-cli-latest#az-feature-list) überprüfen:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/AAD-V2')].{Name:name,State:properties.state}"
```

Wenn der Status als registriert angezeigt wird, können Sie die Registrierung des `Microsoft.ContainerService`-Ressourcenanbieters mit dem Befehl [az provider register](https://docs.microsoft.com/cli/azure/provider?view=azure-cli-latest#az-provider-register) aktualisieren:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

## <a name="create-an-aks-cluster-with-azure-ad-enabled"></a>Erstellen eines AKS-Clusters mit aktiviertem Azure AD

Sie können jetzt mit den folgenden CLI-Befehlen einen AKS-Cluster erstellen.

Erstellen Sie als Erstes eine Azure-Ressourcengruppe:

```azurecli-interactive
# Create an Azure resource group
az group create --name myResourceGroup --location centralus
```

Erstellen Sie anschließend einen AKS-Cluster:

```azurecli-interactive
az aks create -g MyResourceGroup -n MyManagedCluster --enable-aad
```
Mit dem obigen Befehl wird ein AKS-Cluster mit drei Knoten erstellt. Der Benutzer, der den Cluster erstellt hat, ist standardmäßig jedoch kein Mitglied einer Gruppe, die Zugriff auf diesen Cluster hat. Dieser Benutzer muss eine Azure AD-Gruppe erstellen, sich selbst als Mitglied der Gruppe hinzufügen und anschließend den Cluster aktualisieren, wie unten gezeigt. Befolgen Sie die [hier](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-groups-create-azure-portal) aufgeführten Anweisungen.

Nachdem Sie eine Gruppe erstellt und sich selbst (und andere) als Mitglied hinzugefügt haben, können Sie den Cluster mit dem folgenden Befehl aktualisieren:

```azurecli-interactive
az aks update -g MyResourceGroup -n MyManagedCluster [--aad-admin-group-object-ids <id>] [--aad-tenant-id <id>]
```
Wenn Sie zunächst eine Gruppe erstellen und Mitglieder hinzufügen, können Sie mit dem folgenden Befehl die Azure AD-Gruppe zum Zeitpunkt der Erstellung aktivieren:

```azurecli-interactive
az aks create -g MyResourceGroup -n MyManagedCluster --enable-aad [--aad-admin-group-object-ids <id>] [--aad-tenant-id <id>]
```

Eine erfolgreiche Erstellung eines von AKS verwalteten Azure AD-Clusters wird im Antwortkörper mit dem folgenden Abschnitt angegeben:
```
"Azure ADProfile": {
    "adminGroupObjectIds": null,
    "clientAppId": null,
    "managed": true,
    "serverAppId": null,
    "serverAppSecret": null,
    "tenantId": "72f9****-****-****-****-****d011db47"
  }
```

Der Cluster wird in wenigen Minuten erstellt.

## <a name="access-an-azure-ad-enabled-cluster"></a>Zugreifen auf einen Azure AD-fähigen Cluster
So rufen Sie die Administratoranmeldeinformationen für den Zugriff auf den Cluster ab:

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name MyManagedCluster --admin
```
Verwenden Sie jetzt den Befehl „kubectl get nodes“, um die Knoten im Cluster anzuzeigen:

```azurecli-interactive
kubectl get nodes

NAME                       STATUS   ROLES   AGE    VERSION
aks-nodepool1-15306047-0   Ready    agent   102m   v1.15.10
aks-nodepool1-15306047-1   Ready    agent   102m   v1.15.10
aks-nodepool1-15306047-2   Ready    agent   102m   v1.15.10
```

So rufen Sie die Benutzeranmeldeinformationen für den Zugriff auf den Cluster ab:
 
```azurecli-interactive
 az aks get-credentials --resource-group myResourceGroup --name MyManagedCluster
```
Befolgen Sie die Anweisungen zum Anmelden.

Sie erhalten die folgende Meldung: **You must be logged in to the server (Unauthorized)** (Sie müssen beim Server angemeldet sein (keine Berechtigung).)

Der obige Benutzer erhält eine Fehlermeldung, da er nicht Mitglied einer Gruppe ist, die Zugriff auf den Cluster hat.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zur [rollenbasierten Zugriffssteuerung in Azure AD][azure-ad-rbac]
* Verwenden Sie [kubelogin](https://github.com/Azure/kubelogin), um auf Features der Azure-Authentifizierung zuzugreifen, die in kubectl nicht verfügbar sind.

<!-- LINKS - Internal -->
[azure-ad-rbac]: azure-ad-rbac.md

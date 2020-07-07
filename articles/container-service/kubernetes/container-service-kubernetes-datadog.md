---
title: (VERALTET) Überwachen von Azure-Kubernetes-Clustern mit Datadog
description: Überwachen eines Kubernetes-Clusters in Azure Container Service mithilfe von Datadog
author: bburns
ms.service: container-service
ms.topic: conceptual
ms.date: 12/09/2016
ms.author: bburns
ms.custom: mvc
ms.openlocfilehash: 1f3f70c30ab397bd549a2f3305a738274ee4f64f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79371170"
---
# <a name="deprecated-monitor-an-azure-container-service-cluster-with-datadog"></a>(VERALTET) Überwachen eines Azure Container Service-Clusters mit DataDog

[!INCLUDE [ACS deprecation](../../../includes/container-service-kubernetes-deprecation.md)]

## <a name="prerequisites"></a>Voraussetzungen
In dieser exemplarischen Vorgehensweise wird davon ausgegangen, dass Sie mit [Azure Container Service einen Kubernetes-Cluster erstellt haben](container-service-kubernetes-walkthrough.md).

Außerdem wird angenommen, dass die Tools `az` (Azure-Befehlszeilenschnittstelle) und `kubectl` installiert sind.

Führen Sie Folgendes aus, um zu prüfen, ob das Tool `az` installiert ist:

```azurecli
az --version
```

Wenn das Tool `az` nicht installiert ist, finden Sie [hier](https://github.com/azure/azure-cli#installation) Anweisungen.

Führen Sie Folgendes aus, um zu prüfen, ob das Tool `kubectl` installiert ist:

```console
kubectl version
```

Wenn `kubectl` nicht installiert ist, können Sie folgenden Befehl ausführen:

```azurecli
az acs kubernetes install-cli
```

## <a name="datadog"></a>Datadog
Datadog ist ein Überwachungsdienst, der Überwachungsdaten von den Containern innerhalb Ihres Azure-Containerdienstclusters sammelt. Datadog hat ein „Docker Integration Dashboard“, das bestimmte Metriken in Ihrem Container anzeigt. Aus Ihren Containern gesammelte Metriken werden nach CPU, Arbeitsspeicher, Netzwerk und E/A organisiert. Datadog teilt Metriken in Container und Images auf.

Sie müssen zuerst [ein Konto erstellen](https://www.datadoghq.com/lpg/).

## <a name="installing-the-datadog-agent-with-a-daemonset"></a>Installieren des Datadog-Agents mit einem DaemonSet
DaemonSets werden von Kubernetes verwendet, um auf jedem Host im Cluster eine einzelne Instanz eines Containers auszuführen.
Sie eignen sich perfekt zum Ausführen von Überwachungs-Agents.

Nachdem Sie sich bei Datadog angemeldet haben, können Sie gemäß der [Datadog-Anleitung](https://app.datadoghq.com/account/settings#agent/kubernetes) vorgehen, um Datadog-Agents mithilfe eines DaemonSets in Ihrem Cluster zu installieren.

## <a name="conclusion"></a>Zusammenfassung
Das ist alles! Wenn die Agents ausgeführt werden, sollten innerhalb weniger Minuten Daten in der Konsole angezeigt werden. Im integrierten [Kubernetes-Dashboard](https://app.datadoghq.com/screen/integration/kubernetes) finden Sie eine Zusammenfassung Ihres Clusters.

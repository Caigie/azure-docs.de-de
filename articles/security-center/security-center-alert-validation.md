---
title: Warnungsüberprüfung (EICAR-Testdatei) in Azure Security Center | Microsoft-Dokumentation
description: In diesem Dokument erfahren Sie, wie Sie die Sicherheitswarnungen in Azure Security Center überprüfen.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: f8f17a55-e672-4d86-8ba9-6c3ce2e71a57
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/04/2019
ms.author: memildin
ms.openlocfilehash: cf73b3949b0a0dc1e76ebdebb191af0a33ce22ff
ms.sourcegitcommit: 3fb5e772f8f4068cc6d91d9cde253065a7f265d6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89180472"
---
# <a name="alert-validation-in-azure-security-center"></a>Warnungsüberprüfung in Azure Security Center
In diesem Dokument erfahren Sie, wie Sie überprüfen, ob Ihr System ordnungsgemäß für Azure Security Center-Warnungen konfiguriert ist.

## <a name="what-are-security-alerts"></a>Was sind Sicherheitswarnungen?
Warnungen sind die Benachrichtigungen, die Security Center generiert, wenn Bedrohungen für Ihre Ressourcen erkannt werden. Security Center priorisiert die Warnungen und listet sie zusammen mit den Informationen auf, die erforderlich sind, um das Problem schnell zu untersuchen. Security Center stellt außerdem Empfehlungen zur Reaktion auf einen Angriff bereit.
Weitere Informationen finden Sie unter [Sicherheitswarnungen in Azure Security Center](security-center-alerts-overview.md) und [Verwalten von und Reagieren auf Sicherheitswarnungen](security-center-managing-and-responding-alerts.md).

## <a name="alert-validation"></a>Warnungsüberprüfung

* [Windows](#validate-windows)
* [Linux](#validate-linux)
* [Kubernetes](#validate-kubernetes)

## <a name="validate-alerts-on-windows-vms"></a>Überprüfen von Warnungen auf virtuellen Windows-Computern <a name="validate-windows"></a>

Nach der Installation des Security Center-Agents auf Ihrem Computer führen Sie auf dem Computer, auf dem sich die angegriffene Ressource für die Warnung befinden soll, die folgenden Schritte aus:

1. Kopieren Sie eine ausführbare Datei (beispielsweise **calc.exe**) auf den Desktop des Computers oder in ein beliebiges anderes Verzeichnis, und benennen Sie sie in **ASC_AlertTest_662jfi039N.exe** um.
1. Öffnen Sie die Eingabeaufforderung, und führen Sie die Datei mit einem Argument (beliebiger Pseudoname) aus. Beispiel: ```ASC_AlertTest_662jfi039N.exe -foo```
1. Warten Sie fünf bis zehn Minuten, und öffnen Sie die Security Center-Warnungen. Eine Warnung sollte angezeigt werden.

> [!NOTE]
> Vergewissern Sie sich bei der Prüfung dieser Testwarnung für Windows, dass **Arguments Auditing Enabled** (Argumentüberprüfung aktiviert) auf **TRUE** festgelegt ist. Sollte die Option auf **FALSE** festgelegt sein, müssen Sie die Überprüfung von Befehlszeilenargumenten aktivieren. Verwenden Sie den folgenden Befehl, um die Einstellung zu aktivieren:
>
>```reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\policies\system\Audit" /f /v "ProcessCreationIncludeCmdLine_Enabled"```

## <a name="validate-alerts-on-linux-vms"></a>Überprüfen von Warnungen auf virtuellen Linux-Computern <a name="validate-linux"></a>

Nach der Installation des Security Center-Agents auf Ihrem Computer führen Sie auf dem Computer, auf dem sich die angegriffene Ressource für die Warnung befinden soll, die folgenden Schritte aus:
1. Kopieren Sie eine ausführbare Datei an einem geeigneten Speicherort, und benennen Sie sie in **./asc_alerttest_662jfi039n** um. Beispiel:

    ```cp /bin/echo ./asc_alerttest_662jfi039n```

1. Öffnen Sie die Eingabeaufforderung, und führen Sie diese Datei aus:

    ```./asc_alerttest_662jfi039n testing eicar pipe```

1. Warten Sie fünf bis zehn Minuten, und öffnen Sie die Security Center-Warnungen. Eine Warnung sollte angezeigt werden.


## <a name="validate-alerts-on-kubernetes"></a>Überprüfen von Warnungen unter Kubernetes <a name="validate-kubernetes"></a>

Wenn Sie die Security Center-Previewfunktion der Integration von Azure Kubernetes Service verwenden, führen Sie den folgenden kubectl-Befehl aus, um zu testen, ob Ihre Warnungen funktionieren:

```kubectl get pods --namespace=asc-alerttest-662jfi039n```

Weitere Informationen zur Integration von Azure Kubernetes Service und Azure Security Center finden Sie in [diesem Artikel](azure-kubernetes-service-integration.md).

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel wurden Sie in den Prozess der Warnungsüberprüfung eingeführt. Nachdem Sie sich mit dieser Überprüfung vertraut gemacht haben, können Sie mit den folgenden Artikeln fortfahren:

* [Überprüfen der Azure Key Vault-Bedrohungserkennung in Azure Security Center](https://techcommunity.microsoft.com/t5/azure-security-center/validating-azure-key-vault-threat-detection-in-azure-security/ba-p/1220336)
* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts): Erfahren Sie, wie Sie Warnungen verwalten und auf Sicherheitsvorfälle in Security Center reagieren.
* [Überwachen der Sicherheitsintegrität in Azure Security Center:](security-center-monitoring.md) Erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
* [Verstehen der Sicherheitswarnungen in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type): Hier finden Sie Informationen zu den unterschiedlichen Arten von Sicherheitswarnungen.

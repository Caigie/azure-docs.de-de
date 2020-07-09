---
title: Häufige Ursachen für Zustandswechsel bei Clouddienstrollen | Microsoft Docs
description: Eine Clouddienstrolle, die plötzlich zyklisch ausgeführt wird, kann zu erheblichen Ausfallzeiten führen. Hier sind einige allgemeine Probleme aufgeführt, die zum zyklischen Ausführen von Rollen führen. Diese können Ihnen dabei helfen, Ausfallzeiten zu verringern.
services: cloud-services
documentationcenter: ''
author: simonxjx
manager: dcscontentpm
editor: ''
tags: top-support-issue
ms.assetid: 533930d1-8035-4402-b16a-cf887b2c4f85
ms.service: cloud-services
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 06/15/2018
ms.author: v-six
ms.openlocfilehash: 61f555dc8f24ce303934187d36ee994b25b31920
ms.sourcegitcommit: dee7b84104741ddf74b660c3c0a291adf11ed349
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85920083"
---
# <a name="common-issues-that-cause-roles-to-recycle"></a>Allgemeine Probleme, durch die Rollen zyklisch ausgeführt werden
In diesem Artikel sind einige der allgemeinen Ursachen für Bereitstellungsprobleme sowie Tipps zur Problembehandlung aufgeführt, mit denen Sie diese Probleme beheben können. Wenn die Rolleninstanz nicht gestartet wird oder zwischen den Zuständen „Initialisieren“, „Ausgelastet“ und „Beenden“ wechselt, weist dies auf ein Problem mit einer Anwendung hin.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="missing-runtime-dependencies"></a>Fehlende Laufzeitabhängigkeiten
Wenn eine Rolle in Ihrer Anwendung auf einer Assembly basiert, die nicht Teil von .NET Framework oder der von Azure verwalteten Bibliothek ist, müssen Sie diese Assembly explizit in das Anwendungspaket einschließen. Bedenken Sie, dass andere Microsoft-Frameworks nicht standardmäßig in Azure verfügbar sind. Wenn Ihre Rolle auf solch einem Framework basiert, müssen Sie dem Anwendungspaket diese Assemblys hinzufügen.

Überprüfen Sie vor dem Erstellen und Packen der Anwendung Folgendes:

* Vergewissern Sie sich bei der Verwendung von Visual Studio, dass für jede referenzierte Assembly im Projekt, die nicht Teil von Azure SDK oder .NET Framework ist, die Eigenschaft **Lokale Kopie** auf **True** festgelegt ist.
* Stellen Sie sicher, dass die Datei „web.config“ nicht auf nicht verwendete Assemblys im Element „compilation“ verweist.
* Der **Buildvorgang** jeder CSHTML-Datei ist auf **Inhalt** festgelegt. Dadurch wird sichergestellt, dass die Dateien im Paket ordnungsgemäß angezeigt werden, und das Anzeigen anderer referenzierter Dateien im Paket ermöglicht.

## <a name="assembly-targets-wrong-platform"></a>Die Assembly ist auf eine falsche Plattform ausgerichtet.
Azure ist eine 64-Bit-Umgebung. Aus diesem Grund funktionieren für eine 32-Bit-Umgebung kompilierte .NET-Assemblys nicht in Azure.

## <a name="role-throws-unhandled-exceptions-while-initializing-or-stopping"></a>Eine Rolle löst beim Initialisieren oder Beenden Ausnahmefehler aus.
Von den Methoden der [RoleEntryPoint]-Klasse ausgelöste Ausnahmen sind Ausnahmefehler. Zu diesen Methoden zählen [OnStart], [OnStop] und [Ausführen]. Wenn ein Ausnahmefehler in einer der folgenden Methoden auftritt, wird die Rolle zyklisch ausgeführt. Wenn die Rolle zyklisch ausgeführt wird, wird bei jedem versuchten Start möglicherweise ein Ausnahmefehler ausgelöst.

## <a name="role-returns-from-run-method"></a>Die Rolle wird von der Run-Methode reaktiviert.
Die Methode [Ausführen] ist für die Ausführung auf unbestimmte Zeit ausgelegt. Wenn Ihr Code die [Ausführen] -Methode außer Kraft setzt, sollte sie sich für unbegrenzte Zeit im Ruhezustand befinden. Wird die [Ausführen] -Methode jedoch reaktiviert, wird die Rolle zyklisch ausgeführt.

## <a name="incorrect-diagnosticsconnectionstring-setting"></a>Falsche DiagnosticsConnectionString-Einstellung
Wenn die Anwendung die Azure-Diagnose verwendet, muss Ihre Dienstkonfigurationsdatei die Konfigurationseinstellung `DiagnosticsConnectionString` angeben. Diese Einstellung muss eine HTTPS-Verbindung zum Speicherkonto in Azure angeben.

Überprüfen Sie vor der Bereitstellung des Anwendungspakets in Azure Folgendes, um sicherzustellen, dass die Einstellung `DiagnosticsConnectionString` korrekt ist:  

* Die Einstellung `DiagnosticsConnectionString` verweist auf ein gültiges Speicherkonto in Azure.  
  Standardmäßig verweist diese Einstellung auf das emulierte Speicherkonto. Daher müssen Sie diese Einstellung explizit ändern, bevor Sie das Anwendungspaket bereitstellen. Wenn Sie diese Einstellung nicht ändern, wird eine Ausnahme ausgelöst, wenn die Rolleninstanz versucht, den Diagnosemonitor zu starten. Dies kann dazu führen, dass die Rolleninstanz unbegrenzt zyklisch ausgeführt wird.
* Die Verbindungszeichenfolge wird in folgendem [Format](../storage/common/storage-configure-connection-string.md) angegeben. (Das Protokoll muss als HTTPS angegeben werden.) Ersetzen Sie *MyAccountName* durch den Namen Ihres Speicherkontos und *MyAccountKey* durch den Zugriffsschlüssel:    

```console
DefaultEndpointsProtocol=https;AccountName=MyAccountName;AccountKey=MyAccountKey
```

  Wenn Sie Ihre Anwendung mit den Azure-Tools für Microsoft Visual Studio entwickeln, können Sie diesen Wert mithilfe der Eigenschaftenseiten festlegen.

## <a name="exported-certificate-does-not-include-private-key"></a>Das exportierte Zertifikat enthält keinen privaten Schlüssel.
Um eine Webrolle unter TLS auszuführen, müssen Sie sicherstellen, dass das exportierte Verwaltungszertifikat den privaten Schlüssel enthält. Wenn Sie zum Exportieren des Zertifikats den *Windows-Zertifikat-Manager* verwenden, aktivieren Sie für die Option **Privaten Schlüssel exportieren** unbedingt **Ja**. Das Zertifikat muss im PFX-Format exportiert werden. Dies ist das einzige derzeit unterstützte Format.

## <a name="next-steps"></a>Nächste Schritte
Sehen Sie sich weitere [Artikel zur Problembehandlung](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) für Clouddienste an.

Informieren Sie sich in der [Blogreihe von Kevin Williamson](https://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx)über weitere Szenarien mit zyklischer Ausführung von Rollen.

[RoleEntryPoint]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.aspx
[OnStart]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx
[OnStop]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx
[Ausführen]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx

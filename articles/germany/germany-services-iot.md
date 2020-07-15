---
title: IoT-Dienste von Azure Deutschland | Microsoft-Dokumentation
description: Dieser Artikel bietet einen Einstiegspunkt in die Azure IoT Suite für Azure Deutschland.
services: germany
cloud: na
documentationcenter: na
author: gitralf
manager: rainerst
ms.assetid: na
ms.service: germany
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/12/2019
ms.author: ralfwi
ms.openlocfilehash: 369b33895d0b31802b33272f6cc479f54e15cb5d
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2020
ms.locfileid: "86207237"
---
# <a name="azure-germany-iot-services"></a>IoT-Dienste von Azure Deutschland

> [!IMPORTANT]
> Seit [August 2018](https://news.microsoft.com/europe/2018/08/31/microsoft-to-deliver-cloud-services-from-new-datacentres-in-germany-in-2019-to-meet-evolving-customer-needs/) haben wir keine neuen Kunden akzeptiert und keine neuen Features und Dienste an den ursprünglichen Microsoft Cloud Deutschland-Standorten bereitgestellt.
>
> Aufgrund der Weiterentwicklung der Kundenbedürfnisse haben wir vor Kurzem zwei neue Rechenzentrumsregionen in Deutschland [gestartet](https://azure.microsoft.com/blog/microsoft-azure-available-from-new-cloud-regions-in-germany/), die Datenresidenz für Kundendaten, umfassende Konnektivität mit dem globalen Cloudnetzwerk von Microsoft sowie wettbewerbsfähige Preise bieten. 
>
> Profitieren Sie von der Vielfalt der Funktionen, Sicherheit auf Unternehmensniveau und den umfangreichen Features, die in unseren neuen deutschen Rechenzentrumsregionen zur Verfügung stehen, und [migrieren](germany-migration-main.md) Sie noch heute.

## <a name="iot-solution-accelerators"></a>IoT Solution Accelerators
Alle Dienste der Azure IoT Suite sowie IoT Hub, Stream Analysis und Event Hub sind in Azure Deutschland verfügbar. 

### <a name="variations"></a>Abweichungen
Die Homepage für die Azure IoT Suite in Azure Deutschland unterscheidet sich von der Seite in der globalen Azure-Umgebung.

## <a name="solution-accelerators"></a>Solution Accelerators
Es empfiehlt sich, mit einem der folgenden Solution Accelerators zu beginnen. 

### <a name="remote-monitoring"></a>Remoteüberwachung
Der Solution Accelerator für die Remoteüberwachung ist eine Implementierung einer End-to-End-Überwachungslösung für mehrere Computer, die an Remotestandorten ausgeführt werden. In der Lösung sind wichtige Azure-Dienste kombiniert, um eine generische Implementierung des Geschäftsszenarios zu erzielen. Sie können sie als Einstiegspunkt für Ihre Implementierung verwenden und dann an Ihre speziellen Geschäftsanforderungen anpassen.

### <a name="predictive-maintenance"></a>Predictive Maintenance
Der Solution Accelerator für Predictive Maintenance ist eine End-to-End-Lösung für ein Geschäftsszenario, mit der der Zeitpunkt prognostiziert wird, zu dem voraussichtlich ein Fehler auftritt. Sie können diese Lösung beispielsweise zur Optimierung von Wartungsroutinen nutzen. Bei dieser Lösung werden zentrale Azure IoT Suite-Dienste wie Azure IoT Hub, Stream Analytics und ein Machine Learning-Arbeitsbereich kombiniert. Der Arbeitsbereich enthält ein Modell zum Vorhersagen der Restlebensdauer (Remaining Useful Life, RUL) eines Flugzeugtriebwerks auf der Grundlage eines öffentlichen Datasets mit Beispielwerten. Bei der Lösung wird das IoT-Geschäftsszenario vollständig als Ausgangspunkt implementiert, damit Sie eine Lösung planen und implementieren können, die Ihre speziellen Geschäftsanforderungen erfüllt.


## <a name="deploying-the-solution-accelerator"></a>Bereitstellen des Solution Accelerators

Beide Lösungen können auf zwei Arten bereitgestellt werden: über die Website oder über PowerShell.

### <a name="deploy-via-website"></a>Bereitstellen über die Website

Befolgen Sie die Anweisungen im [Tutorial für die vorkonfigurierten Lösungen](../iot-accelerators/iot-accelerators-remote-monitoring-explore.md), das über die zuvor erwähnte Homepage abgerufen werden kann.

### <a name="deploy-via-powershell"></a>Bereitstellen über PowerShell

Für die *Remoteüberwachungslösung* wird eine Vollversion (mit Azure Resource Manager-Vorlagen und Visual Studio) benötigt. Laden Sie sich das [Azure IoT-Remoteüberwachungs-Repository von GitHub](https://github.com/Azure/azure-iot-remote-monitoring) herunter. Die PowerShell-Bereitstellung ist für andere Umgebungen wie Azure Deutschland einsatzbereit. Geben Sie den *Umgebungsparameter* „AzureGermanCloud“ nach folgendem Muster ein:

```powershell
build.cmd cloud debug AzureGermanCloud
```

Bing Karten ist derzeit nicht in Azure Deutschland verfügbar und kann aus diesem Grund nicht automatisch abonniert werden. Sie können dieses Problem beheben, indem Sie den Dienst in der globalen Azure-Umgebung abonnieren und auch dort verwenden. 

> [!NOTE]
> Bei der Nutzung von Bing Karten gemäß der hier beschriebenen Art und Weise verlassen Sie die Azure Deutschland-Umgebung.

Gehen Sie hierzu wie folgt vor:

1. Erstellen Sie eine Bing Karten-API im globalen Azure-Portal, indem Sie auf **+ Neu** klicken, nach **Bing Karten-API für Unternehmen** suchen und die Anweisungen befolgen.
2. Rufen Sie Ihren Bing Karten-API für Unternehmen-Schlüssel aus dem globalen Azure-Portal ab: 
    1. Navigieren Sie zur Ressourcengruppe, in der sich Ihre Bing Karten-API für Unternehmen im globalen Azure-Portal befindet.
    2. Klicken Sie auf **Alle Einstellungen** > **Schlüsselverwaltung**. 
    3. Sie sehen zwei Schlüssel: „MasterKey“ und „QueryKey“. Kopieren Sie den Wert für „QueryKey“.
3. Besorgen Sie sich den aktuellen Code aus dem [Azure IoT-Remoteüberwachungs-Repository auf GitHub](https://github.com/Azure/azure-iot-remote-monitoring).
4. Führen Sie eine Cloudbereitstellung in Ihrer Umgebung aus, indem Sie die Anleitung zur Befehlszeilenbereitstellung im Repositoryordner `/docs/` befolgen. 
5. Wenn Sie die Bereitstellung ausgeführt haben, suchen Sie im Stammordner nach der Datei **.user.config**, die während der Bereitstellung erstellt wurde. Öffnen Sie diese Datei in einem Texteditor. 
6. Ändern Sie die folgende Zeile so ab, dass sie den oben kopierten „QueryKey“-Wert enthält: `<setting name="MapApiQueryKey" value="" />`
7. Stellen Sie die Lösung erneut bereit, indem Sie Schritt 4 wiederholen.
 


## <a name="next-steps"></a>Nächste Schritte
Abonnieren Sie den [Azure Deutschland-Blog](https://blogs.msdn.microsoft.com/azuregermany/), um weitere Informationen und Updates zu erhalten.

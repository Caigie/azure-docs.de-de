---
title: Microsoft Threat Modeling Tool, Release 11.02.2020 – Azure
description: Dokumentation der Versionshinweise für das Threat Modeling Tool
author: jegeib
ms.author: jegeib
ms.service: security
ms.topic: article
ms.date: 02/25/2020
ms.openlocfilehash: da4e61d6c89e62c3598570b30ce749390915ca1b
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2020
ms.locfileid: "86259337"
---
# <a name="threat-modeling-tool-update-release-73002061---02112020"></a>Threat Modeling Tool, Updaterelease 7.3.00206.1 – 11.02.2020

Version 7.3.00206.1 für das Microsoft Threat Modeling Tool (TMT) wurde am 11. Februar 2020 veröffentlicht und umfasst die folgenden Änderungen:

- Behebung von Programmfehlern

## <a name="notable-bug-fixes"></a>Wichtige Programmfehlerbehebungen

### <a name="errors-related-to-priority-values-outside-of-the-expected-ranges"></a>Fehler im Zusammenhang mit Prioritätswerten außerhalb der erwarteten Bereiche

Einige Kunden berichteten, dass beim Öffnen von Dateien, die in „Threat Modeling Tool 2016“ oder mit benutzerdefinierten Vorlagen erstellt wurden, folgende Fehlermeldung angezeigt wurde:

```output
System.InvalidOperationException: Invalid Priority value. Accepted values are [0..4] and 'High', 'Medium', 'Low' at ThreatModeling.Model.Threat.get_Priority()

System.ArgumentOutOfRangeException: Accepted values are 'High', 'Medium', and 'Low' Parameter name: value Actual value was 5.6. at ThreatModeling.Model.Threat.set_Priority(String value)
```

Das Problem wurde in diesem Release gelöst.

## <a name="system-requirements"></a>Systemanforderungen

- Unterstützte Betriebssysteme
  - [Windows 10 Anniversary Update](https://blogs.windows.com/windowsexperience/2016/08/02/how-to-get-the-windows-10-anniversary-update/#HTkoK5Zdv0g2F2Zq.97) oder höher
- Erforderliche .NET-Version
  - [.NET 4.7.1](https://go.microsoft.com/fwlink/?LinkId=863262) oder höher
- Zusätzliche Anforderungen
  - Eine Internetverbindung ist erforderlich, um Updates für das Tool sowie Vorlagen zu erhalten.

## <a name="documentation-and-feedback"></a>Dokumentation und Feedback

- Dokumentation für das Threat Modeling Tool befindet sich auf [docs.microsoft.com](https://docs.microsoft.com/azure/security/azure-security-threat-modeling-tool) und umfasst Informationen [zur Verwendung des Tools](https://docs.microsoft.com/azure/security/azure-security-threat-modeling-tool-getting-started).

## <a name="next-steps"></a>Nächste Schritte

Laden Sie die aktuelle Version des [Microsoft Threat Modeling Tools](https://aka.ms/threatmodelingtool) herunter.

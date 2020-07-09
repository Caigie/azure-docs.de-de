---
title: include file
description: include file
services: iot-hub
ms.service: iot-hub
author: dominicbetts
ms.topic: include
ms.date: 02/17/2019
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 2514325c93f0f72aa979f871e86e465093479402
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "69558836"
---
## <a name="customize-and-extend-the-device-management-actions"></a>Anpassen und Erweitern der Geräteverwaltungsaktionen

Ihre IoT-Lösungen können die festgelegten Geräteverwaltungsmuster erweitern oder benutzerdefinierte Muster ermöglichen. Dazu werden die Grundtypen für die Gerätezwillings- oder die C2D-Methode verwendet. Andere Beispiele für Geräteverwaltungsaktionen sind das Zurücksetzen auf die Werkseinstellungen, Firmware- und Softwareaktualisierungen, Energieverwaltung, Netzwerk- und Konnektivitätsverwaltung und Datenverschlüsselung.

## <a name="device-maintenance-windows"></a>Gerätewartungsfenster

In der Regel konfigurieren Sie die Ausführung von Aktionen für Geräte so, dass Unterbrechungen und Ausfallzeiten auf ein Minimum beschränkt sind. Bei Gerätewartungsfenstern handelt es sich um ein häufig verwendetes Muster zum Festlegen des Zeitpunkts, zu dem ein Gerät seine Konfiguration aktualisieren soll. Ihre Back-End-Lösungen können die gewünschten Eigenschaften des Gerätezwillings verwenden, um auf Ihrem Gerät eine Richtlinie zur Aktivierung eines Wartungsfensters festzulegen und zu aktivieren. Wenn ein Gerät die Wartungsfensterrichtlinie erhält, kann es mithilfe der gemeldeten Eigenschaft des Gerätezwillings den Richtlinienstatus melden. Die Back-End-App kann dann mithilfe von Gerätezwillingsabfragen die Konformität von Geräten und den einzelnen Richtlinien sicherstellen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie eine direkte Methode zum Auslösen eines Remoteneustarts auf einem Gerät verwendet. Sie haben die gemeldeten Eigenschaften zum Melden des letzten Neustartzeitpunkts des Geräts verwendet. Darüber hinaus haben Sie den Gerätezwilling abgefragt, um den letzten Neustartzeitpunkt des Geräts aus der Cloud zu ermitteln.

Informationen zu den weiteren Schritten mit IoT Hub und Geräteverwaltungsmustern, z. B. drahtloses Firmware-Remoteupdate, finden Sie unter [Durchführen eines Firmwareupdates](../articles/iot-hub/tutorial-firmware-update.md).

Unter [Planen und Übertragen von Aufträgen](../articles/iot-hub/iot-hub-node-node-schedule-jobs.md) erfahren Sie, wie Sie Ihre IoT-Lösung erweitern und Methodenaufrufe für mehrere Geräte planen.
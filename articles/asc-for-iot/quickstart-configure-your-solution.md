---
title: 'Schnellstart: Konfigurieren Ihrer Lösung'
description: In diesem Schnellstart erfahren Sie, wie Sie Ihre End-to-End-IoT-Lösung mithilfe von Azure Security Center für IoT konfigurieren.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: ae2207e8-ac5b-4793-8efc-0517f4661222
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/06/2020
ms.author: mlottner
ms.openlocfilehash: e6ab713715cacc799d2b980c2bce2a2a15b76887
ms.sourcegitcommit: 59ea8436d7f23bee75e04a84ee6ec24702fb2e61
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/07/2020
ms.locfileid: "89505453"
---
# <a name="quickstart-configure-your-iot-solution"></a>Schnellstart: Konfigurieren Ihrer IoT-Lösung

In diesem Artikel wird die Erstkonfiguration Ihrer IoT-Sicherheitslösung mithilfe von Azure Security Center für IoT erläutert.

## <a name="azure-security-center-for-iot"></a>Azure Security Center für IoT

Azure Security Center für IoT bietet umfassende End-to-End-Sicherheit für Azure-basierte IoT-Lösungen.

Mit Azure Security Center für IoT können Sie Ihre gesamte IoT-Lösung über ein zentrales Dashboard überwachen, auf dem alle Ihre IoT-Geräte, IoT-Plattformen und Back-End-Ressourcen in Azure angezeigt werden.

Nach der Aktivierung in Ihrer IoT Hub-Instanz erkennt Azure Security Center für IoT automatisch andere Azure-Dienste, die ebenfalls mit Ihrer IoT Hub-Instanz verbunden sind und mit Ihrer IoT-Lösung zusammenhängen.

Zusätzlich zur automatischen Beziehungserkennung können Sie auch weitere Azure-Ressourcengruppen als Teil Ihrer IoT-Lösung kennzeichnen.

Dadurch können Sie vollständige Abonnements, Ressourcengruppen oder auch einzelne Ressourcen hinzufügen.

Nachdem Sie alle Ressourcenbeziehungen definiert haben, stellt Azure Security Center für IoT mithilfe von Azure Security Center Sicherheitsempfehlungen und Warnungen für diese Ressourcen bereit.

## <a name="add-azure-resources-to-your-iot-solution"></a>Hinzufügen von Azure-Ressourcen zu Ihrer IoT-Lösung

Gehen Sie wie folgt vor, um Ihrer IoT-Lösung neue Ressourcen hinzuzufügen:

1. Öffnen Sie im Azure-Portal Ihre Instanz von **IoT Hub**.
1. Wählen Sie im linken Menü im Abschnitt **Sicherheit** die Option **Einstellungen** aus, und öffnen Sie sie. Wählen Sie anschließend **Überwachte Ressourcen** aus.
1. Wählen Sie **Bearbeiten** und dann die überwachten Ressourcen aus, die zu ihrer IoT-Lösung gehören.
1. Klicken Sie auf **Hinzufügen**.

Glückwunsch! Sie haben Ihrer IoT-Lösung eine neue Ressourcengruppe hinzugefügt.

Azure Security Center für IoT überwacht nun die neu hinzugefügten Ressourcengruppen und zeigt relevante Sicherheitsempfehlungen und Warnungen im Rahmen Ihrer IoT-Lösung an.

## <a name="next-steps"></a>Nächste Schritte

Im nächsten Artikel erfahren Sie, wie Sie Sicherheitsmodule erstellen:

> [!div class="nextstepaction"]
> [Quickstart: Create an azureiotsecurity module twin](quickstart-create-security-twin.md) (Schnellstart: Erstellen eines azureiotsecurity-Modulzwillings)

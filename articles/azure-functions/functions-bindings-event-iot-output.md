---
title: Azure IoT Hub-Ausgabebindung für Azure Functions
description: Erfahren Sie, wie Sie mithilfe von Azure Functions Nachrichten in Azure IoT Hubs-Datenströme schreiben.
author: craigshoemaker
ms.topic: reference
ms.date: 02/21/2020
ms.author: cshoe
ms.openlocfilehash: d4dbf43fb5684d829e581be29832e94ad46b2936
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79235146"
---
# <a name="azure-iot-hub-output-binding-for-azure-functions"></a>Azure IoT Hub-Ausgabebindung für Azure Functions

Dieser Artikel erläutert das Arbeiten mit Azure Functions-Ausgabebindungen für IoT Hub. Die IoT Hub-Unterstützung basiert auf der [Azure Event Hubs-Bindung](functions-bindings-event-hubs.md).

Informationen zu Setup- und Konfigurationsdetails finden Sie in der [Übersicht](functions-bindings-event-iot.md).

> [!IMPORTANT]
> Die folgenden Codebeispiele verwenden zwar die Event Hub-API, aber die Syntax gilt auch für IoT Hub-Funktionen.

[!INCLUDE [functions-bindings-event-hubs](../../includes/functions-bindings-event-hubs-output.md)]

## <a name="next-steps"></a>Nächste Schritte

- [Reagieren auf Ereignisse, die an einen Event Hub-Ereignisdatenstrom gesendet werden (Trigger)](./functions-bindings-event-iot-trigger.md)

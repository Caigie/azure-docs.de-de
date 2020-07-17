---
title: Verknüpfen von Azure ATP-Daten mit Azure Sentinel | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Protokolle von Azure Advanced Threat Protection (ATP) mit einem einzigen Klick nach Azure Sentinel streamen können.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/30/2019
ms.author: yelevin
ms.openlocfilehash: f58c38ccfa234752a80c05c300d245c6c9e97cf0
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85559172"
---
# <a name="connect-data-from-azure-advanced-threat-protection-atp"></a>Verknüpfen von Daten aus Azure Advanced Threat Protection (ATP)

> [!IMPORTANT]
> Der Azure Advanced Threat Protection-Datenconnector in Azure Sentinel ist derzeit als Public Preview verfügbar.
> Dieses Feature wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Sie können Protokolle aus [Azure Advanced Threat Protection](https://docs.microsoft.com/azure-advanced-threat-protection/what-is-atp) mit einem einzigen Klick nach Azure Sentinel streamen.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Benutzer mit globalen Administrator- oder Sicherheitsadministratorberechtigungen.
- Sie müssen ein Vorschaukunde von Azure ATP sein und die Integration zwischen Azure ATP und Microsoft Cloud App Security aktivieren. Weitere Informationen finden Sie unter [Integration in Azure Advanced Threat Protection](https://docs.microsoft.com/cloud-app-security/aatp-integration).

## <a name="connect-to-azure-atp"></a>Herstellen einer Verbindung mit Azure ATP

Stellen Sie sicher, dass die Vorschauversion von Azure ATP [in Ihrem Netzwerk aktiviert](https://docs.microsoft.com/azure-advanced-threat-protection/install-atp-step1) ist.
Wenn Azure ATP bereitgestellt ist und Daten erfasst, können die verdächtigen Warnungen problemlos an Azure Sentinel gestreamt werden. Es kann bis zu 24 Stunden dauern, bis mit dem Streamen der Warnungen an Azure Sentinel begonnen wird.


1. Zum Herstellen einer Verbindung zwischen Azure ATP und Azure Sentinel müssen Sie zunächst die Integration zwischen Azure ATP und Microsoft Cloud App Security aktivieren. Weitere Informationen hierzu finden Sie unter [Integration der Azure Advanced Threat Protection](https://docs.microsoft.com/cloud-app-security/aatp-integration).

1. Wählen Sie in Azure Sentinel die Option **Data connectors** (Datenconnectors) aus, und klicken Sie dann auf die Kachel **Azure Advanced Threat Protection (Vorschauversion)** .

1. Sie können auswählen, ob die Warnungen von Azure ATP automatisch Incidents in Azure Sentinel generieren sollen. Wählen Sie unter **Incidents erstellen** die Option **Aktivieren** aus, um die standardmäßige Analyseregel zu aktivieren, die automatisch Incidents aus im verbundenen Sicherheitsdienst generierten Warnungen erstellt. Anschließend können Sie diese Regel unter **Analytics** und dann unter **Aktive Regeln** bearbeiten.

1. Klicken Sie auf **Verbinden**.

1. Um das relevante Schema für die Azure ATP-Warnungen in Log Analytics zu verwenden, suchen Sie nach **SecurityAlert**.

> [!NOTE]
> Wenn die Warnungen größer als 30 KB sind, zeigt Azure Sentinel das Feld für Entitäten in den Warnungen nicht mehr an.

## <a name="next-steps"></a>Nächste Schritte
In diesem Dokument haben Sie erfahren, wie Sie Azure Advanced Threat Protection mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats-built-in.md).


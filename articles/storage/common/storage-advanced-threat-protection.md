---
title: Konfigurieren von Advanced Threat Protection
titleSuffix: Azure Storage
description: Konfigurieren Sie Advanced Threat Protection für Azure Storage, um Anomalien in Bezug auf die Kontoaktivität zu erkennen und Benachrichtigungen über potenziell schädliche Zugriffsversuche auf Ihr Konto zu erhalten.
services: storage
author: tamram
ms.service: storage
ms.subservice: common
ms.topic: conceptual
ms.date: 07/07/2020
ms.author: tamram
ms.reviewer: ozgun
ms.openlocfilehash: 3069ee020d5f127eb0bdb8cbaf251cd3f3cef8d9
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86118414"
---
# <a name="configure-advanced-threat-protection-for-azure-storage"></a>Konfigurieren von Advanced Threat Protection für Azure Storage

Advanced Threat Protection für Azure Storage ermöglicht die Nutzung intelligenter Sicherheitsfunktionen zur Erkennung von ungewöhnlichen und möglicherweise schädlichen Versuchen, auf Speicherkonten zuzugreifen oder diese unbefugt zu nutzen. Aufgrund dieser Schutzebene können Sie Bedrohungen begegnen, ohne ein Sicherheitsexperte zu sein oder Systeme für die Überwachung der Sicherheit verwalten zu müssen.

Bei Anomalien im Rahmen von Aktivitäten werden Sicherheitswarnungen ausgelöst. Diese Sicherheitswarnungen sind in [Azure Security Center](https://azure.microsoft.com/services/security-center/) integriert und werden mit Informationen zu verdächtigen Aktivitäten und Empfehlungen zur Untersuchung und Beseitigung von Bedrohungen auch per E-Mail an Abonnementadministratoren gesendet.

Der Dienst erfasst Ressourcenprotokolle von an Blob Storage und Azure Files (Vorschauversion) gesendete Lese-, Schreib- und Löschanforderungen, um Bedrohungen zu erkennen. Wenn Sie die Warnungen von Advanced Threat Protection untersuchen möchten, können Sie mithilfe von Storage Analytics Logging entsprechende Speicheraktivitäten anzeigen. Weitere Informationen finden Sie unter **Konfigurieren der Protokollierung** in [Überwachen eines Speicherkontos im Azure-Portal](storage-monitor-storage-account.md#configure-logging).

## <a name="availability"></a>Verfügbarkeit

Advanced Threat Protection für Azure Storage ist derzeit für Blob Storage, Azure Files (Vorschau) und Azure Data Lake Storage Gen2 (Vorschau) verfügbar. Zu den Kontotypen, die Advanced Threat Protection unterstützen, gehören Konten vom Typ „Allgemein v2“ sowie Blockblob- und Blob Storage-Konten. Advanced Threat Protection ist in allen öffentlichen Clouds und Clouds der US-Regierungsbehörden verfügbar, nicht aber in anderen Sovereign Cloud- oder Azure Government-Cloudregionen.

Konten mit für Data Lake Storage aktivierten hierarchischen Namespaces unterstützen Transaktionen, die sowohl die Azure Blob Storage-APIs als auch die Data Lake Storage-APIs verwenden. Azure-Dateifreigaben unterstützen Transaktionen über SMB.

Preisdetails, einschließlich einer kostenlosen 30-Tage-Testversion, finden Sie auf der [Seite mit der Preisübersicht zu Azure Security Center](https://azure.microsoft.com/pricing/details/security-center/).

## <a name="set-up-advanced-threat-protection"></a>Einrichten von Advanced Threat Protection

Sie können Advanced Threat Protection auf verschiedene Arten konfigurieren, die in den folgenden Abschnitten beschrieben werden.

### <a name="azure-security-center"></a>[Azure Security Center](#tab/azure-security-center)

Wenn Sie den Standard-Tarif in Azure Security Center abonnieren, wird Advanced Threat Protection automatisch in all Ihren Speicherkonten eingerichtet. Sie können Advanced Threat Protection für Ihre Speicherkonten in einem bestimmten Abonnement folgendermaßen aktivieren oder deaktivieren:

1. Starten Sie **Azure Security Center** im [Azure-Portal](https://portal.azure.com).
1. Klicken Sie im Hauptmenü auf **Preise & Einstellungen**.
1. Klicken Sie auf das Abonnement, in dem Sie Threat Protection für die zugehörigen Speicherkonten aktivieren oder deaktivieren möchten.

    ![Auswählen des Abonnements](./media/storage-advanced-threat-protection/storage-advanced-threat-protection-subscription.png)

1. Klicken Sie auf **Tarif**.
1. Klicken Sie im Abschnitt **Tarif nach Ressourcentyp auswählen** in der Zeile **Speicherkonten** auf **Aktiviert** oder **Deaktiviert**.

    ![Aktivieren von ATP in Security Center](./media/storage-advanced-threat-protection/storage-advanced-threat-protection-pricing2.png)
1. Klicken Sie auf **Speichern**.

### <a name="portal"></a>[Portal](#tab/azure-portal)

1. Starten Sie das [Azure-Portal](https://portal.azure.com/).
1. Navigieren Sie zu Ihrem Azure Storage-Konto. Wählen Sie unter **Einstellungen** die Option **Erweiterte Sicherheit** aus.
1. Wählen Sie den Link **Einstellungen** auf der Seite für die Konfiguration der erweiterten Sicherheit aus.
1. Legen Sie für **Erweiterte Sicherheit** den Wert **EIN** fest.
1. Klicken Sie auf **Speichern**, um die neue oder aktualisierte Richtlinie zu speichern.

    ![Aktivieren von Azure Storage Advanced Threat Protection](./media/storage-advanced-threat-protection/storage-advanced-threat-protection-turn-on.png)

### <a name="template"></a>[Vorlage](#tab/template)

Verwenden Sie zur Bereitstellung eines Azure Storage-Kontos, bei dem Advanced Threat Protection aktiviert ist, eine Azure Resource Manager-Vorlage. Weitere Informationen finden Sie unter [Storage-Konto mit Advanced Threat Protection](https://azure.microsoft.com/resources/templates/201-storage-advanced-threat-protection-create/).

### <a name="azure-policy"></a>[Azure Policy](#tab/azure-policy)

Verwenden Sie eine Azure Policy-Instanz zum Aktivieren von Advanced Threat Protection für Speicherkonten unter einem bestimmten Abonnement oder einer bestimmten Ressourcengruppe.

1. Starten Sie die Azure-Seite **Richtlinie – Definitionen**.

1. Suchen Sie nach der Richtlinie **Advanced Threat Protection für Speicherkonten bereitstellen**.

     ![Richtlinie suchen](./media/storage-advanced-threat-protection/storage-atp-policy-definitions.png)

1. Wählen Sie ein Azure-Abonnement oder eine Ressourcengruppe aus.

    ![Wählen Sie „Abonnement“ oder „Gruppe“ aus.](./media/storage-advanced-threat-protection/storage-atp-policy2.png)

1. Weisen Sie die Richtlinie zu.

    ![Seite „Richtliniendefinitionen“](./media/storage-advanced-threat-protection/storage-atp-policy1.png)

### <a name="rest-api"></a>[REST-API](#tab/rest-api)

Verwenden Sie REST-API-Befehle, um die Advanced Threat Protection-Einstellung für ein bestimmtes Speicherkonto zu erstellen, zu aktualisieren oder abzurufen.

* [Advanced Threat Protection – Create (Erstellen)](https://docs.microsoft.com/rest/api/securitycenter/advancedthreatprotection/create)
* [Advanced Threat Protection – Get (Abrufen)](https://docs.microsoft.com/rest/api/securitycenter/advancedthreatprotection/get)

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Verwenden Sie die folgenden PowerShell-Cmdlets:

* [Enable Advanced Threat Protection (Advanced Threat Protection aktivieren)](https://docs.microsoft.com/powershell/module/az.security/enable-azsecurityadvancedthreatprotection)
* [Get Advanced Threat Protection (Advanced Threat Protection abrufen)](https://docs.microsoft.com/powershell/module/az.security/get-azsecurityadvancedthreatprotection)
* [Disable Advanced Threat Protection (Advanced Threat Protection deaktivieren)](https://docs.microsoft.com/powershell/module/az.security/disable-azsecurityadvancedthreatprotection)

---

## <a name="explore-security-anomalies"></a>Untersuchen von sicherheitsrelevanten Anomalien

Wenn für die Speicheraktivität Anomalien auftreten, erhalten Sie eine E-Mail-Benachrichtigung mit Informationen zum verdächtigen Sicherheitsereignis. Einzelheiten des Ereignisses sind:

* Art der Anomalie
* Speicherkontoname
* Ereigniszeit
* Speichertyp
* Mögliche Ursachen
* Untersuchungsschritte
* Schritte zur Bereinigung

Die E-Mail enthält auch Details zu möglichen Ursachen und empfohlenen Aktionen zur Untersuchung und Eindämmung der potenziellen Bedrohung.

![Azure Storage Advanced Threat Protection – Warnungs-E-Mail](./media/storage-advanced-threat-protection/storage-advanced-threat-protection-alert-email.png)

Sie können Ihre aktuellen Sicherheitswarnungen in Azure Security Center über die [Kachel Sicherheitswarnungen](../../security-center/security-center-managing-and-responding-alerts.md) anzeigen und verwalten. Durch Klicken auf eine bestimmte Warnung werden Details und Aktionen zum Untersuchen der aktuellen Bedrohung und zum Begegnen zukünftiger Bedrohungen bereitgestellt.

![Azure Storage Advanced Threat Protection – Warnungs-E-Mail](./media/storage-advanced-threat-protection/storage-advanced-threat-protection-alert.png)

## <a name="security-alerts"></a>Sicherheitswarnungen

Warnungen werden bei ungewöhnlichen und potenziell schädlichen Zugriffsversuchen oder Exploit-Vorgängen für Speicherkonten generiert. Eine Liste der Warnungen für Azure Storage finden Sie im Abschnitt **Speicher** unter [Threat Protection für Datendienste in Azure Security Center](https://docs.microsoft.com/azure/security-center/alerts-reference#alerts-azurestorage).

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu [Protokollen in Azure Storage-Konten](/rest/api/storageservices/About-Storage-Analytics-Logging)
* Weitere Informationen zu [Azure Security Center](../../security-center/security-center-intro.md)

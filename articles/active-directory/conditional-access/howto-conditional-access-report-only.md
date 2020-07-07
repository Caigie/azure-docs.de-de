---
title: Konfigurieren einer Richtlinie für bedingten Zugriff im reinen Berichtsmodus – Azure Active Directory
description: Verwenden des reinen Berichtsmodus des bedingten Zugriffs zur Unterstützung der Einführung
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: how-to
ms.date: 05/26/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: dawoo
ms.collection: M365-identity-device-management
ms.openlocfilehash: 46a00d55c58992be1009da1de5441ebe4e589a70
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "83994968"
---
# <a name="configure-a-conditional-access-policy-in-report-only-mode"></a>Konfigurieren einer Richtlinie für bedingten Zugriff im reinen Berichtsmodus

Gehen Sie wie folgt vor, um eine Richtlinie für bedingten Zugriff im reinen Berichtsmodus zu konfigurieren:

> [!IMPORTANT]
> Wenn dies in Ihrer Organisation noch nicht erfolgt ist, [richten Sie die Integration von Azure Monitor in Azure AD ein](#set-up-azure-monitor-integration-with-azure-ad). Dies ist erforderlich, damit Daten zur Überprüfung verfügbar sind.

1. Melden Sie sich als Administrator für bedingten Zugriff, Sicherheitsadministrator oder globaler Administrator beim **Azure-Portal** an.
1. Navigieren Sie zu **Azure Active Directory** > **Sicherheit** > **Bedingter Zugriff**.
1. Wählen Sie **Neue Richtlinie**.
1. Konfigurieren Sie die Richtlinienbedingungen und die erforderlichen Steuerelemente zur Rechteerteilung nach Bedarf.
1. Legen Sie unter **Richtlinie aktivieren** die Umschaltfläche auf den Modus **Nur Bericht** fest.
1. Wählen Sie **Speichern** aus.

> [!TIP]
> Sie können den Status **Richtlinie aktivieren** einer vorhandenen Richtlinie von **Ein** in **Nur Bericht** ändern, aber dadurch wird die Richtlinienerzwingung deaktiviert. 

Sie können das Ergebnis von „Nur Bericht“ in Azure AD-Anmeldeprotokollen anzeigen.

Gehen Sie wie folgt vor, um das Ergebnis einer Richtlinie im reinen Berichtsmodus für eine bestimmte Anmeldung anzuzeigen:

1. Melden Sie sich als Berichtleseberechtigter, Benutzer mit Leseberechtigung für Sicherheitsfunktionen, Sicherheitsadministrator oder globaler Administrator beim **Azure-Portal** an.
1. Navigieren Sie zu **Azure Active Directory** > **Anmeldungen**.
1. Wählen Sie eine Anmeldung aus, oder fügen Sie Filter hinzu, um die Ergebnisse einzugrenzen.
1. Wählen Sie im Drawer **Details** die Registerkarte **Nur Bericht** aus, um die bei der Anmeldung ausgewerteten Richtlinien anzuzeigen.

> [!NOTE]
> Wählen Sie beim Herunterladen der Anmeldeprotokolle das JSON-Format aus, um nur berichtsspezifische Ergebnisdaten für den bedingten Zugriff einzubeziehen.

## <a name="set-up-azure-monitor-integration-with-azure-ad"></a>Einrichten der Integration von Azure Monitor in Azure AD

Damit Sie die aggregierten Auswirkungen von Richtlinien für bedingten Zugriff mithilfe der neuen Arbeitsmappe für Erkenntnisse zum bedingten Zugriff anzeigen können, müssen Sie Azure Monitor in Azure AD integrieren und die Anmeldeprotokolle exportieren. Zum Einrichten dieser Integration müssen Sie zwei Schritte ausführen: 

1. [Registrieren Sie sich für ein Azure Monitor-Abonnement, und erstellen Sie einen Arbeitsbereich](/azure/azure-monitor/learn/quick-create-workspace).
1. [Exportieren Sie die Anmeldeprotokolle aus Azure AD in Azure Monitor](/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics).

Weitere Informationen zur Preisgestaltung für Azure Monitor finden Sie auf der Seite [Azure Monitor – Preise](https://azure.microsoft.com/pricing/details/monitor/). Ressourcen zum Schätzen von Kosten, zum Festlegen eines Tageslimits oder zum Anpassen des Datenaufbewahrungszeitraums finden Sie im Artikel [Verwalten von Nutzung und Kosten mit Azure Monitor-Protokollen](../../azure-monitor/platform/manage-cost-storage.md#estimating-the-costs-to-manage-your-environment).

## <a name="view-conditional-access-insights-workbook"></a>Anzeigen der Arbeitsmappe für Erkenntnisse zum bedingten Zugriff

Nachdem Sie die Azure AD-Protokolle in Azure Monitor integriert haben, können Sie die Auswirkungen von Richtlinien für bedingten Zugriff mithilfe der neuen Arbeitsmappen für Erkenntnisse zum bedingten Zugriff überwachen.

1. Melden Sie sich als Sicherheitsadministrator oder globaler Administrator beim **Azure-Portal** an.
1. Navigieren Sie zu **Azure Active Directory** > **Arbeitsmappen**.
1. Wählen Sie **Erkenntnisse zum bedingten Zugriff** aus.
1. Wählen Sie in der Dropdownliste **Richtlinien für bedingten Zugriff** eine oder mehrere Richtlinien aus. Standardmäßig sind alle aktivierten Richtlinien ausgewählt.
1. Wählen Sie einen Zeitbereich aus (wenn der Zeitbereich das verfügbare Dataset überschreitet, werden im Bericht alle verfügbaren Daten angezeigt). Nachdem Sie die **Richtlinie für bedingten Zugriff** und die Parameter für den **Zeitbereich** festgelegt haben, wird der Bericht geladen.
   1. Suchen Sie optional nach einzelnen **Benutzern** oder **Apps**, um den Bereich des Berichts einzugrenzen.
1. Wählen Sie für die Anzeige der Daten im Zeitbereich zwischen der Anzahl von Benutzern oder der Anzahl von Anmeldungen aus.
1. Abhängig von der **Datenansicht** zeigt die **Zusammenfassung der Auswirkungen** die Anzahl von Benutzern oder Anmeldungen im Bereich der ausgewählten Parameter gruppiert nach Gesamtanzahl, **Erfolg**, **Fehler**, **Benutzeraktion erforderlich** und **Nicht angewendet** an. Wählen Sie eine Kachel aus, um die Anmeldungen eines bestimmten Ergebnistyps zu untersuchen. 
   1. Wenn Sie die Arbeitsmappenparameter geändert haben, können Sie eine Kopie zur späteren Verwendung speichern. Wählen Sie oben im Bericht das Symbol „Speichern“ aus, und geben Sie einen Namen und den Speicherort für die Speicherung an.
1. Scrollen Sie nach unten, um die Aufschlüsselung der Anmeldungen für jede Bedingung anzuzeigen.
1. Sehen Sie sich im unteren Bereich des Berichts die **Anmeldeinformationen** an, um einzelne, nach den oben aufgeführten Auswahlkriterien gefilterte Anmeldeereignisse zu untersuchen.

> [!TIP] 
> Müssen Sie einen Drilldown für eine bestimmte Abfrage ausführen oder die Anmeldeinformationen exportieren? Wählen Sie die Schaltfläche rechts neben einer beliebigen Abfrage aus, um die entsprechende Abfrage in Log Analytics zu öffnen. Wählen Sie „Exportieren“ aus, um die Daten in eine CSV-Datei oder nach Power BI zu exportieren.

## <a name="common-problems-and-solutions"></a>Häufige Probleme und Lösungen

### <a name="why-are-the-queries-in-the-workbook-failing"></a>Warum können die Abfragen in der Arbeitsmappe nicht ausgeführt werden?

Kunden haben festgestellt, dass Abfragen manchmal nicht ausgeführt werden, wenn der Arbeitsmappe falsche oder mehrere Arbeitsbereiche zugeordnet sind. Um dieses Problem zu beheben, klicken Sie oben in der Arbeitsmappe auf **Bearbeiten**, und klicken Sie dann auf das Zahnradsymbol „Einstellungen“. Wählen Sie nicht der Arbeitsmappe zugeordnete Arbeitsbereiche aus, und entfernen Sie diese Arbeitsbereiche. Jeder Arbeitsmappe sollte nur ein Arbeitsbereich zugeordnet sein.

### <a name="why-doesnt-the-conditional-access-policies-dropdown-parameter-contain-my-policies"></a>Warum sind meine Richtlinien nicht im Dropdownparameter „Richtlinien für bedingten Zugriff“ enthalten?

Die Dropdownliste „Richtlinien für bedingten Zugriff“ wird durch Abfragen der letzten Anmeldungen über einen Zeitraum von 4 Stunden aufgefüllt. Wenn bei einem Mandanten in den letzten vier Stunden keine Anmeldungen erfolgt sind, ist es möglich, dass die Dropdownliste leer ist. Wenn es sich bei dieser Verzögerung um ein dauerhaftes Problem handelt, beispielsweise bei kleinen Mandanten mit seltenen Anmeldungen, können Administratoren die Abfrage für die Dropdownliste „Richtlinien für bedingten Zugriff“ bearbeiten und den Zeitraum für die Abfrage auf eine Dauer von mehr als 4 Stunden verlängern.

## <a name="next-steps"></a>Nächste Schritte

[Allgemeine Richtlinien für bedingten Zugriff](concept-conditional-access-policy-common.md)

Weitere Informationen zu Azure AD-Arbeitsmappen finden Sie im Artikel [Verwenden von Azure Monitor-Arbeitsmappen für Azure Active Directory-Berichte](../reports-monitoring/howto-use-azure-monitor-workbooks.md).

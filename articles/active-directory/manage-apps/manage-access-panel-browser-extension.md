---
title: Behandeln von Problemen mit der Azure-Zugriffsbereichserweiterung für IE | Microsoft-Dokumentation
description: So stellen Sie das Internet Explorer-Add-On für das Portal "Meine Apps" mithilfe von Gruppenrichtlinien bereit
services: active-directory
documentationcenter: ''
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/11/2019
ms.author: kenwith
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 16abfbeacd972ee8b0ab55f09945e687c95f0093
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84763260"
---
# <a name="troubleshoot-the-access-panel-extension-for-internet-explorer"></a>Behandeln von Problemen mit der Zugriffsbereichserweiterung für Internet Explorer

Dieser Artikel hilft Ihnen bei der Behandlung der folgenden Probleme:

* Während der Verwendung von Internet Explorer ist nicht möglich, über das Portal "Meine Apps" auf Ihre Apps zuzugreifen.
* Die Meldung „Software installieren“ wird angezeigt, obwohl Sie die Software bereits installiert haben.

Wenn Sie Administrator sind, lesen Sie [Bereitstellen der Zugriffsbereichserweiterung für Internet Explorer mithilfe von Gruppenrichtlinien](deploy-access-panel-browser-extension.md).

## <a name="run-the-diagnostic-tool"></a>Ausführen des Diagnosetools

Sie können Probleme bei der Installation der Zugriffsbereichserweiterung diagnostizieren, indem Sie das Diagnosetool für den Zugriffsbereich herunterladen und ausführen. 

Gehen Sie wie folgt vor, um das Diagnosetool herunterzuladen und zu installieren:

1. [Wählen Sie diesen Link zum Herunterladen des Diagnosetools aus.](https://account.activedirectory.windowsazure.com/applications/AccessPanelExtensionDiagnosticTool/AccessPanelExtensionDiagnosticTool.zip)
1. Öffnen Sie die Datei, und extrahieren Sie den Inhalt auf Ihrem Computer.
1. Klicken Sie zum Ausführen des Tools mit der rechten Maustaste auf die Datei mit dem Namen *AccessPanelExtensionDiagnosticTool.js*, und wählen Sie dann **Öffnen mit** > **Microsoft Windows Script Host** aus.

    ![Öffnen mit > Microsoft Windows Script Host](./media/manage-access-panel-browser-extension/open-access-panel-extension-diagnostic-tool.png)

1. Überprüfen Sie die angezeigten Diagnoseergebnisse, und wählen Sie **Ja** aus, um die Probleme zu beheben. Das Dialogfeld **Ergebnisse überprüfen** wird mit Informationen zu Maßnahmen angezeigt, die Sie ergreifen können, falls die Erweiterung nicht funktioniert.  
1. Lesen Sie die Meldung, und wählen Sie **OK** aus.

## <a name="check-that-the-access-panel-extension-is-enabled"></a>Überprüfen, ob die Zugriffsbereichserweiterung aktiviert ist

Gehen Sie wie folgt vor, um zu überprüfen, ob die Zugriffsbereichserweiterung in Internet Explorer aktiviert ist:

1. Wählen Sie in Internet Explorer in der oberen rechten Ecke des Fensters das **Zahnradsymbol** aus, und wählen Sie dann **Internetoptionen** aus.
1. Wechseln Sie zur Registerkarte **Programme**, und wählen Sie **Add-Ons verwalten** aus.
1. Wählen Sie im Abschnitt **Microsoft Corporation** die Option **Zugriffsbereichserweiterung** aus, und wählen Sie dann **Aktivieren** aus.
1. Schließen Sie zum Speichern der Änderungen alle geöffneten Internet Explorer-Browserfenster. Die Änderung wird beim nächsten Öffnen von Internet Explorer wirksam.

## <a name="enable-extensions-for-inprivate-browsing"></a>Aktivieren von Erweiterungen für InPrivate-Browsen

Gehen Sie wie folgt vor, um Erweiterungen für InPrivate-Browsen zu aktivieren:

1. Wählen Sie in Internet Explorer in der oberen rechten Ecke des Fensters das **Zahnradsymbol** aus, und wählen Sie dann **Internetoptionen** aus.
1. Wechseln Sie zur Registerkarte **Datenschutz**, und vergewissern Sie sich, dass das Kontrollkästchen **Symbolleisten und Erweiterungen beim Starten des InPrivate-Browsens deaktivieren** deaktiviert ist.
1. Schließen Sie zum Speichern der Änderungen alle geöffneten Internet Explorer-Browserfenster. Die Änderung wird beim nächsten Öffnen von Internet Explorer wirksam.

## <a name="uninstall-the-access-panel-extension"></a>Deinstallieren der Zugriffsbereichserweiterung

Gehen Sie wie folgt vor, um die Zugriffsbereichserweiterung auf Ihrem Computer zu deinstallieren:

1. Suchen Sie in der Systemsteuerung nach *deinstallieren*.
1. Wählen Sie in den Suchergebnissen den Eintrag **Programm deinstallieren** aus.

    ![Auswahl der Option „Programm deinstallieren“ in den Einstellungen](./media/manage-access-panel-browser-extension/uninstall-program-control-panel.png)

1. Wählen Sie in der Liste den Eintrag **Zugriffsbereichserweiterung** aus, und wählen Sie dann **Deinstallieren** aus.

    ![Deinstallieren der Zugriffsbereichserweiterung](./media/manage-access-panel-browser-extension/uninstall-access-panel-extension.png)

1. Sie können dann versuchen, die Erweiterung erneut zu installieren, um festzustellen, ob das Problem behoben wurde.

Wenn beim Deinstallieren der Erweiterung Probleme auftreten, können Sie die Erweiterung auch mithilfe des Tools [Microsoft Fix It](https://go.microsoft.com/?linkid=9779673) entfernen.

## <a name="related-articles"></a>Verwandte Artikel

* [Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory](what-is-single-sign-on.md)
* [Bereitstellen der Zugriffsbereichserweiterung für Internet Explorer mithilfe von Gruppenrichtlinien](deploy-access-panel-browser-extension.md)

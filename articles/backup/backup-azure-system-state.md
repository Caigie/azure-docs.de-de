---
title: Sichern des Windows-Systemstatus in Azure
description: Erfahren Sie, wie Sie den Systemstatus von Windows Server und/oder Windows-Computern in Azure sichern.
ms.topic: conceptual
ms.date: 05/23/2018
ms.openlocfilehash: ea38b76d9a8b7b8ccc1898ed9450177da2cb2458
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87003844"
---
# <a name="back-up-windows-system-state-to-azure"></a>Sichern des Windows-Systemstatus in Azure

Dieser Artikel beschreibt, wie Sie den Systemstatus von Windows Server in Azure sichern. Hier werden die Grundlagen ausführlich beschrieben.

Falls Sie weitere Informationen zu Azure Backup erhalten möchten, können Sie diese [Übersicht](backup-overview.md)lesen.

Falls Sie noch nicht über ein Azure-Abonnement verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, mit dem Sie auf alle Azure-Dienste zugreifen können.

[!INCLUDE [How to create a Recovery Services vault](../../includes/backup-create-rs-vault.md)]

## <a name="set-storage-redundancy-for-the-vault"></a>Festlegen der Speicherredundanz für den Tresor

Vergewissern Sie sich beim Erstellen eines Recovery Services-Tresors, dass die Speicherredundanz wie gewünscht konfiguriert ist.

1. Klicken Sie auf dem Blatt **Recovery Services-Tresore** auf den neuen Tresor.

    ![Auswählen des neuen Tresors in der Liste mit Recovery Services-Tresoren](./media/backup-try-azure-backup-in-10-mins/rs-vault-list.png)

    Wenn Sie den Tresor auswählen, wird das Blatt **Recovery Services-Tresor** schmaler, und das Blatt mit den Einstellungen (*auf dem oben der Name des Tresors angegeben ist*) sowie das Blatt mit den Tresordetails werden geöffnet.

    ![Anzeigen der Speicherkonfiguration für den neuen Tresor](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration-2.png)
2. Scrollen Sie auf dem Blatt mit den Einstellungen des neuen Tresors mithilfe des vertikalen Schiebereglers nach unten zum Verwaltungsabschnitt, und klicken Sie auf **Sicherungsinfrastruktur**.
    Das Blatt der Sicherungsinfrastruktur wird geöffnet.
3. Klicken Sie auf dem Blatt der Sicherungsinfrastruktur auf **Sicherungskonfiguration**, um das Blatt **Sicherungskonfiguration** zu öffnen.

    ![Speicherkonfiguration für neuen Tresor festlegen](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration.png)
4. Wählen Sie die passende Speicherreplikationsoption für Ihren Tresor aus.

    ![Speicherkonfigurationsoptionen](./media/backup-try-azure-backup-in-10-mins/choose-storage-configuration.png)

    Standardmäßig verfügt Ihr Tresor über einen georedundanten Speicher. Wenn Sie Azure als primären Speicherendpunkt für die Sicherung verwenden, verwenden Sie weiterhin die Option **Georedundant**. Wenn Sie Azure nicht als primären Speicherendpunkt für die Sicherung verwenden, wählen Sie **Lokal redundant** aus. Dadurch verringern sich die Kosten für Azure-Speicher. Weitere Informationen zu den Optionen für [georedundanten](../storage/common/storage-redundancy.md) und [lokal redundanten](../storage/common/storage-redundancy.md) Speicher finden Sie in [dieser Übersicht über Speicherredundanz](../storage/common/storage-redundancy.md).

Sie haben einen Tresor erstellt und können ihn nun für das Sichern des Windows-Systemstatus konfigurieren.

## <a name="configure-the-vault"></a>Konfigurieren des Tresors

1. Klicken Sie auf dem Blatt „Recovery Services-Tresor“ (für den gerade erstellten Tresor) im Abschnitt „Erste Schritte“ auf **Backup**. Wählen Sie dann auf dem Blatt **Erste Schritte mit der Sicherung** die Option **Sicherungsziel** aus.

    ![Öffnen des Blatts „Backup Goal“ (Sicherungsziel)](./media/backup-try-azure-backup-in-10-mins/open-backup-settings.png)

    Das Blatt **Sicherungsziel** wird geöffnet.

    ![Öffnen des Blatts „Backup Goal“ (Sicherungsziel)](./media/backup-try-azure-backup-in-10-mins/backup-goal-blade.png)

2. Wählen Sie im Dropdownmenü **Wo wird Ihre Workload ausgeführt?** die Option **Lokal** aus.

    Sie wählen **Lokal**, da Ihr Windows Server- oder Windows-Computer ein physischer Computer ist, der sich nicht in Azure befindet.

3. Wählen Sie im Menü **Welche Daten möchten Sie sichern?** die Option **Systemstatus** aus, und klicken Sie auf **OK**.

    ![Konfigurieren von Dateien und Ordnern](./media/backup-azure-system-state/backup-goal-system-state.png)

    Nach dem Klicken auf „OK“ erscheint ein Häkchen neben **Sicherungsziel**, und das Blatt **Vorbereiten der Infrastruktur** wird geöffnet.

    ![Sicherungsziel konfiguriert, dann Vorbereiten der Infrastruktur](./media/backup-try-azure-backup-in-10-mins/backup-goal-configed.png)

4. Klicken Sie auf dem Blatt **Vorbereiten der Infrastruktur** auf **Agent für Windows Server oder Windows-Client herunterladen**.

    ![Agent für Windows Server oder Windows-Client herunterladen](./media/backup-try-azure-backup-in-10-mins/choose-agent-for-server-client.png)

    Wenn Sie Windows Server Essential verwenden, laden Sie den Agent für Windows Server Essential herunter. Sie werden in einem Popupmenü aufgefordert, „MARSAgentInstaller.exe“ auszuführen oder zu speichern.

    ![Dialogfeld „MARSAgentInstaller“](./media/backup-try-azure-backup-in-10-mins/mars-installer-run-save.png)

5. Klicken Sie im Downloadpopupmenü auf **Speichern**.

    Die Datei **MARSagentinstaller.exe** wird standardmäßig in Ihrem Downloadordner gespeichert. Nachdem das Installationsprogramm heruntergeladen wurde, erscheint ein Popup mit der Frage, ob Sie das Installationsprogramm ausführen oder den Ordner öffnen möchten.

    ![Agent für Windows Server oder Windows-Client herunterladen](./media/backup-try-azure-backup-in-10-mins/mars-installer-complete.png)

    Sie müssen den Agent noch nicht installieren. Sie können den Agent installieren, nachdem Sie die Anmeldeinformationen für den Tresor heruntergeladen haben.

6. Klicken Sie auf dem Blatt **Vorbereiten der Infrastruktur** auf **Herunterladen**.

    ![Herunterladen der Tresoranmeldeinformationen](./media/backup-try-azure-backup-in-10-mins/download-vault-credentials.png)

    Die Anmeldeinformationen für den Tresor werden in den Ordner „Downloads“ heruntergeladen. Nach dem Herunterladen der Anmeldeinformationen für den Tresor erscheint ein Popup mit der Frage, ob Sie die Anmeldeinformationen öffnen oder speichern möchten. Klicken Sie auf **Speichern**. Wenn Sie versehentlich auf **Öffnen** klicken, brechen Sie den Vorgang zum Öffnen der Anmeldeinformationen für den Tresor ab. Sie können die Anmeldeinformationen für den Tresor nicht öffnen. Fahren Sie mit dem nächsten Schritt fort. Die Anmeldeinformationen für den Tresor befinden sich im Ordner „Downloads“.

    ![Herunterladen der Tresoranmeldeinformationen abgeschlossen](./media/backup-try-azure-backup-in-10-mins/vault-credentials-downloaded.png)
   > [!NOTE]
   > Die Tresoranmeldeinformationen dürfen nur an einem für die Windows Server-Instanz lokalen Speicherort gespeichert werden, in der Sie den Agent verwenden möchten.
   >

[!INCLUDE [backup-upgrade-mars-agent.md](../../includes/backup-upgrade-mars-agent.md)]

## <a name="install-and-register-the-agent"></a>Installieren und Registrieren des Agents

> [!NOTE]
> Die Sicherung kann noch nicht über das Azure-Portal aktiviert werden. Verwenden Sie den Microsoft Azure Recovery Services-Agent, um den Systemstatus von Windows Server zu sichern.
>

1. Doppelklicken Sie im Downloadordner (bzw. am entsprechenden Speicherort) auf die Datei **MARSagentinstaller.exe**.

    Das Installationsprogramm sendet eine Reihe von Nachrichten, während es den Recovery Services-Agent extrahiert, installiert und registriert.

    ![Anmeldeinformationen zum Ausführen des Installationsprogramms für den Recovery Services-Agent](./media/backup-try-azure-backup-in-10-mins/mars-installer-registration.png)

2. Führen Sie die Schritte im Setup-Assistenten für den Microsoft Azure Recovery Services Agent aus. Zum Abschließen des Assistenten sind folgende Schritte erforderlich:

   * Wählen Sie einen Speicherort für den Installations- und Cacheordner aus.
   * Geben Sie Ihre Proxyserverinformationen an, wenn Sie einen Proxyserver verwenden, um eine Internetverbindung herzustellen.
   * Geben Sie die Informationen zum Benutzernamen und zum Kennwort ein, wenn Sie einen authentifizierten Proxy verwenden.
   * Angeben der heruntergeladenen Tresoranmeldeinformationen
   * Speichern Sie die Verschlüsselungspassphrase an einem sicheren Ort.

     > [!NOTE]
     > Wenn Sie die Passphrase verlieren oder vergessen, kann Microsoft Ihnen bei der Wiederherstellung der Sicherungsdaten nicht behilflich sein. Speichern Sie die Datei daher an einem sicheren Ort. Sie ist erforderlich, um eine Sicherung wiederherzustellen.
     >
     >

Der Agent wurde jetzt installiert, und Ihr Computer wurde im Tresor registriert. Sie können die Sicherung jetzt konfigurieren und planen.

## <a name="back-up-windows-server-system-state"></a>Sichern des Systemstatus von Windows Server

Die erste Sicherung umfasst zwei Aufgaben:

* Planen der Sicherung
* Erstmaliges Sichern des Systemstatus

Für die erste Sicherung verwenden Sie den Microsoft Azure Recovery Services-Agent.

> [!NOTE]
> Sie können den Systemstatus unter Windows Server 2008 R2 bis Windows Server 2016 sichern. Das Sichern des Systemstatus wird auf Client-SKUs nicht unterstützt. Der Systemstatus wird für Windows-Clients oder Computer mit Windows Server 2008 SP2 nicht als Option angezeigt.
>
>

### <a name="to-schedule-the-backup-job"></a>So planen Sie den Sicherungsauftrag

1. Öffnen Sie den Microsoft Azure Recovery Services-Agent. Den Agent finden Sie, indem Sie auf Ihrem Computer nach **Microsoft Azure Backup**suchen.

    ![Starten des Azure Recovery Services-Agents](./media/backup-try-azure-backup-in-10-mins/snap-in-search.png)

2. Klicken Sie im der Recovery Services-Agent auf **Sicherung planen**.

    ![Planen einer Windows Server-Sicherung](./media/backup-try-azure-backup-in-10-mins/schedule-first-backup.png)

3. Klicken Sie im Assistenten zum Planen der Sicherung auf der Seite mit den ersten Schritten auf **Weiter**.

4. Klicken Sie auf der Seite „Elemente für Sicherung auswählen“ auf **Elemente hinzufügen**.

5. Wählen Sie **Systemstatus** aus, und klicken Sie anschließend auf **OK**.

6. Klicken Sie auf **Weiter**.

7. Wählen Sie auf den nachfolgenden Seiten die erforderliche Sicherungshäufigkeit und Aufbewahrungsrichtlinie für die Systemstatussicherungen.

8. Lesen Sie sich die Informationen auf der Seite „Bestätigung“ durch, und klicken Sie dann auf **Fertig stellen**.

9. Klicken Sie auf **Schließen**, nachdem der Assistent die Erstellung des Sicherungszeitplans abgeschlossen hat.

### <a name="to-back-up-windows-server-system-state-for-the-first-time"></a>So sichern Sie den Windows Server-Systemstatus zum ersten Mal

1. Stellen Sie sicher, dass es keine ausstehenden Updates für Windows Server gibt, die einen Neustart erfordern.

2. Klicken Sie im Recovery Services-Agent auf **Jetzt sichern** , um das anfängliche Seeding über das Netzwerk abzuschließen.

    ![Windows Server jetzt sichern](./media/backup-try-azure-backup-in-10-mins/backup-now.png)

3. Wählen Sie auf dem angezeigten Bildschirm **Sicherungselement auswählen** die Option **Systemstatus**, und klicken Sie auf **Weiter**.

4. Überprüfen Sie auf der Seite „Bestätigung“ die Einstellungen, die vom Assistenten für die sofortige Sicherung zum Sichern des Computers verwendet werden. Klicken Sie dann auf **Sichern**.

5. Klicken Sie auf **Schließen** , um den Assistenten zu schließen. Wenn Sie den Assistenten schließen, bevor der Sicherungsvorgang abgeschlossen ist, wird der Assistent im Hintergrund weiter ausgeführt.
    > [!NOTE]
    > Der MARS-Agent löst SFC/verifyonly als Teil der Vorabüberprüfungen vor jeder Systemstatussicherung aus. Dadurch wird sichergestellt, dass Dateien, die im Rahmen des Systemstatus gesichert werden, die richtigen Versionen entsprechend der Windows-Version aufweisen. Weitere Informationen zu System File Checker (SFC) finden Sie in [diesem Artikel](/windows-server/administration/windows-commands/sfc).
    >

Nach Abschluss des ersten Backups wird der Status des Auftrags in der Backup-Konsole als **Auftrag abgeschlossen** angezeigt.

  ![Abgeschlossen](./media/backup-try-azure-backup-in-10-mins/ircomplete.png)

## <a name="questions"></a>Fragen?

Wenn Sie Fragen haben oder Anregungen zu gewünschten Funktionen mitteilen möchten, [senden Sie uns Ihr Feedback](https://feedback.azure.com/forums/258995-azure-backup).

## <a name="next-steps"></a>Nächste Schritte

* Informieren Sie sich ausführlicher über das [Sichern von Windows-Computern](backup-windows-with-mars-agent.md).
* Nachdem Sie nun Ihren Windows Server-Systemstatus gesichert haben, können Sie Ihre [Tresore und Server verwalten](backup-azure-manage-windows-server.md).
* Informationen zum Wiederherstellen einer Sicherung finden Sie im Artikel zum [Wiederherstellen von Dateien auf einem Windows-Computer](backup-azure-restore-windows-server.md).

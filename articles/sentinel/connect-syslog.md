---
title: Verbinden von Syslog-Daten mit Azure Sentinel | Microsoft-Dokumentation
description: Verbinden Sie eine lokale Appliance mit Syslog-Unterstützung mit Azure Sentinel, indem Sie einen Agent auf einem Linux-Computer zwischen Appliance und Sentinel verwenden. 
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
ms.openlocfilehash: 65c4e5d9e0752379541063c8a80a4316196ad7c3
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85565376"
---
# <a name="connect-your-external-solution-using-syslog"></a>Verbinden Sie Ihre externe Lösung mithilfe von Syslog

Sie können jede lokale Appliance, die Syslog unterstützt, mit Azure Sentinel verbinden. Hierzu wird ein Agent verwendet, der sich auf einem Linux-Computer befindet, der sich zwischen der Appliance und Azure Sentinel befindet. Ist Ihr Linux-Computer in Azure angeordnet, können Sie die Protokolle von Ihrer Appliance oder Anwendung an einen dedizierten Arbeitsbereich streamen, den Sie in Azure erstellt und verbunden haben. Ist Ihr Linux-Computer nicht in Azure angeordnet, können Sie die Protokolle von Ihrer Appliance an einen dedizierten lokalen virtuellen Computer oder Computer streamen, auf dem Sie den Agent für Linux installiert haben. 

> [!NOTE]
> Wenn Ihre Appliance Syslog CEF unterstützt, ist die Verbindung vollständiger, und Sie sollten Sie diese Option wählen die Anweisungen unter [Verbinden von Daten aus CEF](connect-common-event-format.md) befolgen.

## <a name="how-it-works"></a>Funktionsweise

Syslog ist ein gängiges Protokoll zur Ereignisprotokollierung für Linux. Anwendungen senden Nachrichten, die auf dem lokalen Computer gespeichert oder an einen Syslog-Sammler übermittelt werden können. Wenn der Log Analytics-Agent für Linux installiert ist, konfiguriert er den lokalen Syslog-Daemon zum Weiterleiten von Nachrichten an den Agent. Der Agent sendet die Nachricht dann an Azure Monitor, wo ein entsprechender Datensatz erstellt wird.

Weitere Informationen finden Sie unter [Syslog-Datenquellen in Azure Monitor](../azure-monitor/platform/data-sources-syslog.md).

> [!NOTE]
> - Der Agent kann Protokolle von mehreren Quellen erfassen, muss aber auf einem dedizierten Proxycomputer installiert sein.
> - Wenn Sie Connectors für CEF und Syslog auf derselben VM unterstützen möchten, führen Sie die folgenden Schritte aus, um Datenduplizierung zu vermeiden:
>    1. Befolgen Sie die Anweisungen zum [Verbinden Ihres CEF](connect-common-event-format.md).
>    2. Um die Syslog-Daten zu verbinden, wechseln Sie zu **Einstellungen** > **Arbeitsbereichseinstellungen** > **Erweiterte Einstellungen** > **Daten** > **Syslog**, und legen Sie die Einrichtungen mit ihren Prioritäten fest, sodass sie nicht mit den Einrichtungen und Eigenschaften übereinstimmen, die Sie in der CEF-Konfiguration verwendet haben. <br></br>Wenn Sie **Nachstehende Konfiguration auf meine Computer anwenden** auswählen, werden diese Einstellungen auf alle VMs angewandt, die mit diesem Arbeitsbereich verbunden sind.


## <a name="connect-your-syslog-appliance"></a>Verbinden Ihrer Syslog-Appliance

1. Wählen Sie in Azure Sentinel die Option **Datenconnectors**, und wählen Sie dann den Connector **Syslog** aus.

2. Wählen Sie auf dem Blatt **Syslog** die Option **Connectorseite öffnen** aus.

3. Installieren des Linux Agent:
    
    - Wenn sich Ihr virtueller Linux-Computer in Azure befindet, wählen Sie **Download and install agent on Azure Linux virtual machine** (Agent herunterladen und auf virtuellem Linux-Computer in Azure installieren) aus. Wählen Sie auf dem Blatt **Virtuelle Computer** die virtuellen Computer aus, auf denen der Agent installiert werden soll, und klicken Sie dann auf **Verbinden**.
    - Befindet sich Ihr Linux-Computer nicht in Azure, wählen Sie **Download and install agent on Linux non-Azure machine** (Agent herunterladen und auf Azure-fremdem Linux-Computer installieren) aus. Kopieren Sie im Blatt **Direkt-Agent** den Befehl für **AGENT FÜR LINUX HERUNTERLADEN UND INTEGRIEREN**, und führen Sie ihn auf Ihrem Computer aus. 
    
   > [!NOTE]
   > Konfigurieren Sie die Sicherheitseinstellungen für diese Computer entsprechend der Sicherheitsrichtlinie Ihrer Organisation. Beispielsweise können Sie die Netzwerkeinstellungen so konfigurieren, dass sie der Sicherheitsrichtlinie für das Netzwerk der Organisation entsprechen, und die Ports und Protokolle im Daemon Ihren Sicherheitsanforderungen entsprechend ändern.

4. Wählen Sie **Konfiguration der erweiterten Einstellungen für den Arbeitsbereich öffnen** aus.

5. Wählen Sie auf dem Blatt **Erweiterte Einstellungen** die Option **Daten** > **Syslog** aus. Fügen Sie dann die Einrichtungen hinzu, die der Connector erfassen soll.
    
    Fügen Sie die Einrichtungen hinzu, die Ihre Syslog-Appliance in ihren Protokollheadern enthält. Sie können diese Konfiguration in der Syslog-Appliance in **Syslog-d** im Ordner `/etc/rsyslog.d/security-config-omsagent.conf` und in **r-Syslog** aus `/etc/syslog-ng/security-config-omsagent.conf` anzeigen.
    
    Wenn Sie für die zu sammelnden Daten die Erkennung ungewöhnlicher SSH-Anmeldungen verwenden möchten, fügen Sie **auth** und **authpriv** hinzu. Im [folgenden Abschnitt](#configure-the-syslog-connector-for-anomalous-ssh-login-detection) finden Sie weitere Details.

6. Wenn Sie alle zu überwachenden Einrichtungen hinzugefügt und alle Schweregradoptionen für jede Einrichtung angepasst haben, aktivieren Sie das Kontrollkästchen **Nachstehende Konfiguration auf meine Computer anwenden**.

7. Wählen Sie **Speichern** aus. 

8. Stellen Sie in der Syslog-Appliance sicher, dass die von Ihnen angegebenen Einrichtungen gesendet werden.

9. Um das entsprechende Schema in Azure Monitor für die Syslog-Protokolle zu verwenden, suchen Sie nach **Syslog**.

10. Sie können mit der in [Verwenden von Funktionen in Azure Monitor-Protokollabfragen](../azure-monitor/log-query/functions.md) beschriebenen Kusto-Funktion die Syslog-Nachrichten analysieren. Sie können sie dann als neue Log Analytics Funktion speichern, um sie als neuen Datentyp zu verwenden.

### <a name="configure-the-syslog-connector-for-anomalous-ssh-login-detection"></a>Konfigurieren des Syslog-Connectors für die Erkennung ungewöhnlicher SSH-Anmeldungen

> [!IMPORTANT]
> Die Erkennung ungewöhnlicher SSH-Anmeldungen befindet sich derzeit in der öffentlichen Vorschauphase.
> Dieses Feature wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Azure Sentinel kann Machine Learning (ML) auf die Syslog-Daten anwenden, um ungewöhnliche SSH-Anmeldungen (Secure Shell) zu identifizieren. Mögliche Szenarien:

- Unmöglicher Ortswechsel – wenn zwei erfolgreiche Anmeldeereignisse von zwei Standorten aus auftreten, die innerhalb des Zeitraums zwischen den beiden Anmeldeereignissen nicht erreichbar sind.
- Unerwarteter Standort – der Standort, von dem aus eine erfolgreiche Anmeldung durchgeführt wurde, ist verdächtig. Beispielsweise ist der Standort in jüngster Zeit nicht aufgetreten.
 
Diese Erkennung erfordert eine bestimmte Konfiguration des Syslog-Datenconnectors: 

1. Stellen Sie in Schritt 5 des vorherigen Verfahrens sicher, dass sowohl **auth** als auch **authpriv** als zu überwachende Einrichtungen ausgewählt sind. Übernehmen Sie die Standardeinstellungen für die Schweregradoptionen, sodass sie alle ausgewählt sind. Beispiel:
    
    > [!div class="mx-imgBorder"]
    > ![Einrichtungen, die für die Erkennung ungewöhnlicher SSH-Anmeldungen erforderlich sind](./media/connect-syslog/facilities-ssh-detection.png)

2. Lassen Sie ausreichend Zeit zum Sammeln der Syslog-Informationen. Navigieren Sie dann zu **Azure Sentinel – Protokolle**, kopieren Sie die folgende Abfrage, und fügen Sie sie ein:
    
        Syslog |  where Facility in ("authpriv","auth")| extend c = extract( "Accepted\\s(publickey|password|keyboard-interactive/pam)\\sfor ([^\\s]+)",1,SyslogMessage)| where isnotempty(c) | count 
    
    Ändern Sie bei Bedarf den **Zeitbereich**, und wählen Sie **Ausführen** aus.
    
    Wenn die resultierende Anzahl 0 (null) ist, überprüfen Sie die Konfiguration des Connectors, und überprüfen Sie, ob in dem Zeitraum, den Sie für die Abfrage angegeben haben, auf den überwachten Computern erfolgreiche Anmeldungen durchgeführt wurden.
    
    Wenn die resultierende Anzahl größer als 0 (null) ist, eignen sich die Syslog-Daten für die Erkennung ungewöhnlicher SSH-Anmeldungen. Sie aktivieren diese Erkennung über **Analyse** >  **Rule templates** (Regelvorlagen)  >  **(Preview) Anomalous SSH Login Detection** ((Vorschau) Erkennung ungewöhnlicher SSH-Anmeldungen).

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie gelernt, wie Sie lokale Syslog-Appliances mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats-built-in.md).
- [Verwenden Sie Arbeitsmappen](tutorial-monitor-your-data.md), um Ihre Daten zu überwachen.


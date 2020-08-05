---
title: Erfassen von Daten in Azure Data Lake Storage Gen2
description: Hier erfahren Sie, wie Daten in Azure Data Lake Storage Gen2 in Azure Synapse Analytics erfasst werden.
services: synapse-analytics
author: djpmsft
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/15/2020
ms.author: daperlov
ms.reviewer: jrasnick
ms.openlocfilehash: 8307f01e690536a71d98d5d5ca99f8f7a77a433f
ms.sourcegitcommit: 5b8fb60a5ded05c5b7281094d18cf8ae15cb1d55
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2020
ms.locfileid: "87383918"
---
# <a name="ingest-data-into-azure-data-lake-storage-gen2"></a>Erfassen von Daten in Azure Data Lake Storage Gen2 

In diesem Artikel erfahren Sie, wie Sie mithilfe von Azure Synapse Analytics Daten aus einem Speicherort an einem anderen Speicherort in einem Azure Data Lake Gen 2-Speicherkonto erfassen.

## <a name="prerequisites"></a>Voraussetzungen

* **Azure-Abonnement**: Wenn Sie über kein Azure-Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.
* **Azure Storage-Konto**: Sie verwenden Azure Data Lake Gen 2 als *Quelldatenspeicher*. Wenn Sie kein Speicherkonto haben, finden Sie unter [Erstellen eines Azure Storage-Kontos](../../storage/blobs/data-lake-storage-quickstart-create-account.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) die erforderlichen Schritte zum Erstellen eines solchen Kontos.

## <a name="create-linked-services"></a>Erstellen von verknüpften Diensten

In Azure Synapse Analytics definieren Sie in einem verknüpften Dienst Ihre Verbindungsinformationen für andere Dienste. In diesem Abschnitt fügen Sie Azure Synapse Analytics und Azure Data Lake Gen 2 als verknüpfte Dienste hinzu.

1. Öffnen Sie die Benutzeroberfläche für Azure Synapse Analytics, und wechseln Sie zur Registerkarte **Verwalten**.
1. Wählen Sie unter **Externe Verbindungen** die Option **Verknüpfte Dienste** aus.
1. Wenn Sie einen verknüpften Dienst hinzufügen möchten, klicken Sie auf **Neu**.
1. Wählen Sie in der Liste die Kachel „Azure Data Lake Storage Gen2“ aus, und klicken Sie auf **Weiter**.
1. Geben Sie Ihre Anmeldeinformationen für die Authentifizierung ein. Derzeit werden die Authentifizierungstypen „Kontoschlüssel“, „Dienstprinzipal“ und „Verwaltete Identität“ unterstützt. Klicken Sie auf „Verbindung testen“, um die Richtigkeit Ihrer Anmeldeinformationen zu überprüfen. 
1. Klicken Sie auf **Erstellen**, nachdem der Vorgang abgeschlossen wurde.

## <a name="create-pipeline"></a>Erstellen der Pipeline

Eine Pipeline enthält den logischen Ablauf für die Ausführung einer Aktivitätenmenge. In diesem Abschnitt erstellen Sie eine Pipeline mit einer Kopieraktivität, die Daten aus Azure Data Lake Gen 2 in einem SQL-Pool erfasst.

1. Wechseln Sie zur Registerkarte **Orchestrieren**. Klicken Sie neben dem Header „Pipelines“ auf das Pluszeichen, und wählen Sie **Pipeline**aus.
1. Ziehen Sie im Bereich „Aktivitäten“ unter **Verschieben und transformieren** den Befehl **Daten kopieren** auf die Pipelinecanvas.
1. Klicken Sie auf die Kopieraktivität, und wechseln Sie zur Registerkarte **Quelle**. Klicken Sie auf **Neu**, um ein neues Quelldataset zu erstellen.
1. Wählen Sie „Azure Data Lake Storage Gen2“ als Datenspeicher aus, und klicken Sie auf „Weiter“.
1. Wählen Sie „DelimitedText“ als Format aus, und klicken Sie auf „Weiter“.
1. Wählen Sie im Bereich „Eigenschaften festlegen“ den von Ihnen erstellten verknüpften ADLS-Dienst aus. Geben Sie den Dateipfad Ihrer Quelldaten und dann an, ob die erste Zeile einen Header hat. Sie können das Schema aus dem Dateispeicher oder einer Beispieldatei importieren. Klicken Sie auf „OK“, wenn der Vorgang abgeschlossen ist.
1. Wechseln Sie zur Registerkarte **Senke**. Klicken Sie auf **Neu**, um ein neues Senkendataset zu erstellen.
1. Wählen Sie „Azure Data Lake Storage Gen 2“ als Datenspeicher aus, und klicken Sie auf „Weiter“.
1. Wählen Sie „DelimitedText“ als Format aus, und klicken Sie auf „Weiter“.
1. Wählen Sie im Bereich „Eigenschaften festlegen“ den von Ihnen erstellten verknüpften ADLS-Dienst aus. Geben Sie den Pfad des Ordners an, in den Daten geschrieben werden sollen. Klicken Sie auf „OK“, wenn der Vorgang abgeschlossen ist.

## <a name="debug-and-publish-pipeline"></a>Debuggen und Veröffentlichen der Pipeline

Nach Abschluss der Konfiguration Ihrer Pipeline können Sie einen Debuglauf durchführen, bevor Sie Ihre Artefakte zur Überprüfung veröffentlichen, ob alles einwandfrei ist.

1. Klicken Sie auf der Symbolleiste auf **Debuggen**, um die Pipeline zu debuggen. Der Status der Pipelineausführung wird unten im Fenster auf der Registerkarte **Ausgabe** angezeigt. 
1. Wenn die Pipeline ausgeführt wird, klicken Sie auf der oberen Symbolleiste auf **Alle veröffentlichen**. Mit dieser Aktion werden erstellte Entitäten (Datasets und Pipelines) im Synapse Analytics-Dienst veröffentlicht.
1. Warten Sie, bis die Meldung **Erfolgreich veröffentlicht** angezeigt wird. Wenn Sie Benachrichtigungsmeldungen anzeigen möchten, klicken Sie oben rechts auf die Schaltfläche mit der Glocke. 


## <a name="trigger-and-monitor-the-pipeline"></a>Auslösen und Überwachen der Pipeline

In diesem Schritt lösen Sie die im vorherigen Schritt veröffentlichte Pipeline manuell aus. 

1. Wählen Sie in der Symbolleiste die Option **Trigger hinzufügen** und dann **Jetzt auslösen**. Klicken Sie auf der Seite **Pipeline Run** (Pipelineausführung) auf **Fertig stellen**.  
1. Wechseln Sie in der linken Randleiste zur Registerkarte **Monitor**. Sie sehen eine Pipelineausführung, die von einem manuellen Trigger ausgelöst wird. Sie können über Links in der Spalte **Aktionen** Aktivitätsdetails anzeigen und die Pipeline erneut ausführen.
1. Klicken Sie in der Spalte **Aktionen** auf den Link **View Activity Runs** (Aktivitätsausführungen anzeigen), um mit der Pipelineausführung verknüpfte Aktivitätsausführungen anzuzeigen. Da in diesem Beispiel nur eine Aktivität vorhanden ist, wird in der Liste nur ein Eintrag angezeigt. Wenn Sie Details zum Kopiervorgang anzeigen möchten, klicken Sie auf den Link **Details** (Brillensymbol) in der Spalte **Aktionen**. Wählen Sie oben **Pipelineausführungen** aus, um zurück zur Ansicht mit den Pipelineausführungen zu wechseln. Klicken Sie zum Aktualisieren der Ansicht auf **Aktualisieren**.
1. Überprüfen Sie, ob Ihre Daten im SQL-Pool richtig geschrieben werden.


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Datenintegration bei Synapse Analytics finden Sie im Artikel [Erfassen von Daten in einem SQL-Pool](data-integration-sql-pool.md).

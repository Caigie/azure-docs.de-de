---
title: Anpassen der Tasks der Microsoft-Sicherheitscodeanalyse
titleSuffix: Azure
description: In diesem Artikel wird beschrieben, wie die Tasks in der Erweiterung „Microsoft-Sicherheitscodeanalyse“ angepasst werden.
author: sukhans
manager: sukhans
ms.author: terrylan
ms.date: 07/31/2019
ms.topic: article
ms.service: security
services: azure
ms.assetid: 521180dc-2cc9-43f1-ae87-2701de7ca6b8
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.openlocfilehash: 8d074c12f28abdc61f4d70356c2a7aa264deb44c
ms.sourcegitcommit: f57297af0ea729ab76081c98da2243d6b1f6fa63
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2020
ms.locfileid: "82871899"
---
# <a name="configure-and-customize-the-build-tasks"></a>Konfigurieren und Anpassen der Buildtasks

In diesem Artikel werden die in den einzelnen Buildtasks verfügbaren Konfigurationsoptionen ausführlich beschrieben. Zunächst werden in diesem Artikel die Tasks für die Tools der Sicherheitscodeanalyse erläutert. Zum Schluss werden die Tasks für die Nachbearbeitung beschrieben.

## <a name="anti-malware-scanner-task"></a>Schadsoftwarescanner-Task

>[!NOTE]
> Der Schadsoftwarescanner-Buildtask erfordert einen Build-Agent mit aktiviertem Windows Defender. Ein solcher Agent ist ab Hosted Visual Studio 2017 verfügbar. Der Buildtask kann nicht unter Verwendung des gehosteten Agents von Visual Studio 2015 ausgeführt werden.
>
> Obwohl Signaturen für diese Agents nicht aktualisiert werden können, sollten sie immer weniger als drei Stunden alt sein.

Details zur Taskkonfiguration finden Sie im folgenden Screenshot und Text.

![Konfigurieren des Schadsoftwarescanner-Buildtasks](./media/security-tools/5-add-anti-malware-task600.png)

Im Screenshot ist im Listenfeld **Type** (Typ) die Option **Basic** (Standard) ausgewählt. Wählen Sie **Custom** (Benutzerdefiniert) aus, um Befehlszeilenargumente zur Anpassung der Überprüfung anzugeben.

Windows Defender verwendet den Windows Update-Client, um Signaturen herunterzuladen und zu installieren. Wenn beim Signaturupdate für den Build-Agent ein Fehler auftritt, wird der **HRESULT**-Fehlercode wahrscheinlich durch Windows Update generiert.

Weitere Informationen zu Windows Update-Fehlern und dazu, wie Sie sie vermeiden, finden Sie unter [Windows Update-Fehlercodes nach Komponente](https://docs.microsoft.com/windows/deployment/update/windows-update-error-reference) und im TechNet-Artikel zu [Fehlercodes des Windows Update-Agents](https://social.technet.microsoft.com/wiki/contents/articles/15260.windows-update-agent-error-codes.aspx).

Weitere Informationen zur YAML-Konfiguration für diese Aufgabe finden Sie in den [YAML-Optionen für die Antischadsoftware](yaml-configuration.md#anti-malware-scanner-task).

## <a name="binskim-task"></a>BinSkim-Task

> [!NOTE]
> Bevor Sie den BinSkim-Task ausführen können, muss Ihr Build eine der folgenden Bedingungen erfüllen:
>  - Der Build erstellt binäre Artefakte aus verwaltetem Code.
>  - Sie haben binäre Artefakte committet, die Sie mit BinSkim analysieren möchten.

Details zur Taskkonfiguration finden Sie im folgenden Screenshot und in der Liste.

![Konfigurieren des BinSkim-Buildtasks](./media/security-tools/7-binskim-task-details.png)

- Legen Sie die Buildkonfiguration auf „Debug“ (Debuggen) fest, um PDB-Debugdateien zu erzeugen. BinSkim verwendet diese Dateien, um Probleme in den Ausgabebinärdateien wieder dem Quellcode zuzuordnen.
- So müssen Sie keine eigene Befehlszeile entwickeln und erstellen:
     - Wählen Sie in der Liste **Type** (Typ) die Option **Basic** (Standard) aus.
     - Wählen Sie in der Liste **Function** (Funktion) die Option **Analyze** (Analysieren) aus.
- Geben Sie unter **Target** (Ziel) mindestens einen Spezifizierer für ein Datei-, Verzeichnis- oder Filtermuster ein. Die Spezifizierer ergeben mindestens eine zu analysierende Binärdatei:
    - Mehrere Ziele müssen durch ein Semikolon (;) getrennt werden.
    - Ein Spezifizierer kann aus einer einzelnen Datei bestehen oder Platzhalter enthalten.
    - Verzeichnisangaben müssen immer mit „\\*“ enden.
    - Beispiele:

           *.dll;*.exe
           $(BUILD_STAGINGDIRECTORY)\*
           $(BUILD_STAGINGDIRECTORY)\*.dll;$(BUILD_STAGINGDIRECTORY)\*.exe;

- Wenn Sie in der Liste **Type** (Typ) die Option **Command Line** (Befehlszeile) auswählen, müssen Sie „binskim.exe“ ausführen:
     - Stellen Sie sicher, dass es sich beim ersten Argument für „binskim.exe“ um das Verb **analyze** gefolgt von mindestens einer Pfadangabe handelt. Jeder Pfad kann entweder ein vollständiger Pfad oder ein Pfad relativ zum Quellverzeichnis sein.
     - Mehrere Zielpfade müssen durch ein Leerzeichen getrennt werden.
     - Sie können die Option **/o** oder **/output** weglassen. Der Ausgabewert wird für Sie hinzugefügt oder ersetzt.
     - Standardmäßige Befehlszeilenkonfigurationen sehen wie folgt aus.

           analyze $(Build.StagingDirectory)\* --recurse --verbose
           analyze *.dll *.exe --recurse --verbose

          > [!NOTE]
          > Die nachstehenden Zeichen „\\*“ sind wichtig, wenn Sie Verzeichnisse für das Ziel angeben.

Weitere Informationen zu BinSkim-Befehlszeilenargumenten, Regeln nach ID oder Exitcodes finden Sie im [BinSkim-Benutzerhandbuch](https://github.com/Microsoft/binskim/blob/master/docs/UserGuide.md).

Weitere Informationen zur YAML-Konfiguration für diese Aufgabe finden Sie in den [YAML-Optionen für BinSkim](yaml-configuration.md#binskim-task).

## <a name="credential-scanner-task"></a>Credential Scanner-Task

Details zur Taskkonfiguration finden Sie im folgenden Screenshot und in der Liste.

![Konfigurieren des Credential Scanner-Buildtasks](./media/security-tools/3-taskdetails.png)

Folgende Optionen sind verfügbar:
  - **Anzeigename**: Name des Azure DevOps-Tasks. Der Standardwert ist „Credential Scanner ausführen“.
  - **Hauptversion des Tools**: Verfügbare Werte sind **CredScan V2**, **CredScan V1**. Wir empfehlen Kunden, die Version **CredScan V2** zu verwenden.
  - **Output Format** (Ausgabeformat): Zu den verfügbaren Werten gehören **TSV**, **CSV**, **SARIF** und **PREfast**.
  - **Tool Version** (Toolversion): Es wird empfohlen, **Latest** (Neueste) auszuwählen.
  - **Scan Folder** (Überprüfungsordner): Der zu überprüfende Repositoryordner.
  - **Searchers File Type** (Suchroutinen-Dateityp): Die Optionen zum Suchen der für die Überprüfung verwendeten Suchroutinendatei.
  - **Suppressions File** (Unterdrückungsdatei): Eine [JSON-Datei](https://json.org/) kann zum Unterdrücken von Problemen im Ausgabeprotokoll verwendet werden. Weitere Informationen zu Unterdrückungsszenarien finden Sie im Abschnitt „FAQ“ dieses Artikels.
  - **Verbose Output** (Ausführliche Ausgabe): Selbsterklärend
  - **Batch Size** (Batchgröße): Die Anzahl gleichzeitiger Threads, die zum Ausführen von Credential Scanner verwendet werden. Der Standardwert lautet 20. Mögliche Werte reichen von 1 bis 2.147.483.647.
  - **Match Timeout** (Übereinstimmungstimeout): Gibt an, wie viele Sekunden die Suchroutine versucht, eine Übereinstimmung zu finden, bevor die Überprüfung abgebrochen wird.
  - **File Scan Read Buffer Size** (Lesepuffergröße der Dateiüberprüfung): Die Größe des beim Lesen von Inhalt verwendeten Puffers in Bytes. Der Standardwert lautet 524.288.  
  - **Maximum File Scan Read Bytes** (Maximale Anzahl gelesener Bytes der Dateiüberprüfung): Die maximale Anzahl von Bytes, die während der Inhaltsanalyse aus einer Datei gelesen werden. Der Standardwert lautet 104.857.600.
  - **Control Options (Steuerungsoptionen)**  > **Run this task (Diesen Task ausführen)** : Gibt an, wann der Task ausgeführt wird. Wählen Sie **Custom Conditions** (Benutzerdefinierte Bedingungen) aus, um komplexere Bedingungen festzulegen.
  - **Version**: Die Buildtaskversion in Azure DevOps. Diese Option wird selten verwendet.

Weitere Informationen zur YAML-Konfiguration für diese Aufgabe finden Sie in den [YAML-Optionen für Credential Scanner](yaml-configuration.md#credential-scanner-task).

## <a name="microsoft-security-risk-detection-task"></a>Microsoft Security Risk Detection (MSRD)-Task

> [!NOTE]
> Vor Verwendung des MSRD-Tasks müssen Sie ein Konto mit dem MSRD (Microsoft Security Risk Detection)-Dienst erstellen und konfigurieren. Dieser Dienst erfordert einen separaten Onboardingprozess. Im Gegensatz zu den meisten anderen Tasks in dieser Erweiterung erfordert dieser Task ein separates Abonnement mit MSRD.
>
> Anleitungen dazu finden Sie unter [Microsoft-Sicherheitsrisikoerkennung](https://aka.ms/msrddocs) und [Microsoft-Sicherheitsrisikoerkennung: Vorgehensweise](https://docs.microsoft.com/security-risk-detection/how-to/).

Details zum Konfigurieren dieses Tasks finden Sie in der folgenden Liste. Um Hilfe zu einem Benutzeroberflächenelement zu erhalten, können Sie darauf zeigen.

   - **Azure DevOps-Dienstendpunktname für MSRD**: Ihre integrierte MSRD-Instanz-URL und Ihr REST-API-Zugriffstoken werden von einem generischen Typ des Azure DevOps-Dienstendpunkts gespeichert. Wenn Sie einen solchen Endpunkt erstellt haben, können Sie ihn hier angeben. Wählen Sie andernfalls den Link **Manage** (Verwalten) aus, um einen neuen Dienstendpunkt für diesen MSRD-Task zu erstellen und zu konfigurieren.
   - **Account ID (Konto-ID)** : Eine GUID, die aus der URL des MSRD-Kontos abgerufen werden kann.
   - **URLs to Binaries (URLs für Binärdateien)** : Eine durch Semikolons getrennte Liste öffentlich verfügbarer URLs. Der Fuzzingcomputer verwendet diese URLs, um die Binärdateien herunterzuladen.
   - **URLs of the Seed Files (URLs der Seeddateien)** : Eine durch Semikolons getrennte Liste öffentlich verfügbarer URLs. Der Fuzzingcomputer verwendet diese URLs, um die Seeds herunterzuladen. Dieser Wert ist optional, wenn die Seeddateien zusammen mit den Binärdateien heruntergeladen werden.
   - **OS Platform Type (Typ der Betriebssystemplattform)** : Die Betriebssystemplattform der Computer, auf denen der Fuzzingauftrag ausgeführt wird. Verfügbare Werte sind **Windows** und **Linux**.
   - **Windows Edition/Linux Edition**: Die Betriebssystemedition der Computer, auf denen der Fuzzingauftrag ausgeführt wird. Sie können den Standardwert überschreiben, wenn Ihre Computer eine andere Betriebssystemedition aufweisen.
   - **Package Installation Script (Paketinstallationsskript)** : Ihr Skript, das auf einem Testcomputer ausgeführt werden soll. Mit diesem Skript werden das Testzielprogramm und dessen Abhängigkeiten installiert, bevor der Fuzzingauftrag übermittelt wird.
   - **Job Submission Parameters (Parameter für die Auftragsübermittlung)** :
       - **Seed Directory (Seedverzeichnis)** : Der Pfad zum Verzeichnis auf dem Fuzzingcomputer, in dem die Seeds enthalten sind.
       - **Seed Extension (Seederweiterung)** : Die Dateinamenerweiterung der Seeds.
       - **Test Driver Executable (Ausführbare Testtreiberdatei)** : Der Pfad zur ausführbaren Zieldatei auf dem Fuzzingcomputer.
       - **Test Driver Executable Architecture (Architektur der ausführbaren Testtreiberdatei)** : Die Architektur der ausführbaren Zieldatei. Verfügbare Werte sind **x86** und **amd64**.
       - **Test Driver Arguments (Testtreiberargumente)** : Die Befehlszeilenargumente, die an die ausführbare Testdatei übergeben werden. Das Argument "%testfile%" wird (einschließlich der Anführungszeichen) automatisch durch den vollständigen Pfad zur Zieldatei ersetzt. Diese Datei wird vom Testtreiber analysiert und ist erforderlich.
       - **Test Driver Process Exits Upon Test Completion (Testtreiberprozess wird nach Abschluss des Tests beendet)** : Aktivieren Sie dieses Kontrollkästchen, wenn der Testtreiber nach Abschluss beendet werden soll. Deaktivieren Sie es, wenn das Beenden des Testtreibers erzwungen werden muss.
       - **Maximum Duration (in seconds) (Maximale Dauer (in Sekunden))** : Eine sinnvolle Schätzung der maximal erwarteten Zeit, die das Zielprogramm zum Analysieren einer Eingabedatei benötigt. Je genauer diese Schätzung ist, desto effizienter wird die Fuzzing-App ausgeführt.
       - **Test Driver Can Be Run Repeatedly (Testtreiber kann wiederholt ausgeführt werden)** : Aktivieren Sie dieses Kontrollkästchen, wenn der Testtreiber unabhängig von einem beständigen oder gemeinsamen globalen Zustand wiederholt ausgeführt werden kann.
       - **Test Driver Can Be Renamed (Testtreiber kann umbenannt werden)** : Aktivieren Sie dieses Kontrollkästchen, wenn die ausführbare Datei des Testtreibers umbenannt werden kann und trotzdem weiterhin ordnungsgemäß funktioniert.
       - **The Fuzzing Application Runs as a Single OS Process (Die Fuzzinganwendung wird als einzelner Betriebssystemprozess ausgeführt)** : Aktivieren Sie dieses Kontrollkästchen, wenn der Testtreiber unter einem einzelnen Betriebssystemprozess ausgeführt wird. Deaktivieren Sie es, wenn der Testtreiber zusätzliche Prozesse erzeugt.

Weitere Informationen zur YAML-Konfiguration für diese Aufgabe finden Sie in den [YAML-Optionen für Microsoft Security Risk Detection](yaml-configuration.md#microsoft-security-risk-detection-task).

## <a name="roslyn-analyzers-task"></a>Roslyn Analyzers-Task

> [!NOTE]
> Ihr Build muss folgende Bedingungen erfüllen, bevor Sie den Roslyn Analyzers-Task ausführen können:
>
> - Ihre Builddefinition enthält den integrierten MSBuild- oder VSBuild-Buildtask zum Kompilieren von C#- oder Visual Basic-Code. Der Analyzers-Task nutzt die Eingabe und Ausgabe des integrierten Tasks, um die MSBuild-Kompilierung mit aktivierten Roslyn Analyzers auszuführen.
> - Für den zur Ausführung dieses Buildtasks verwendeten Build-Agent ist Visual Studio 2017, Version 15.5 oder höher, installiert, sodass mindestens Compilerversion 2.6 genutzt wird.

Details zur Taskkonfiguration finden Sie in der folgenden Liste und im Hinweis.

Folgende Optionen sind verfügbar:

- **Ruleset** (Regelsatz): Die Werte sind **SDL Required** (SDL erforderlich), **SDL Recommended** (SDL empfohlen) oder Ihr eigener benutzerdefinierter Regelsatz.
- **Analyzers Version** (Analyzers-Version): Es wird empfohlen, **Latest** (Neueste) auszuwählen.
- **Compiler Warnings Suppressions File** (Unterdrückungsdatei für Compilerwarnungen): Eine Textdatei mit einer Liste von Warnungs-IDs, die unterdrückt werden.
- **Control Options (Steuerungsoptionen)**  > **Run this task (Diesen Task ausführen)** : Gibt an, wann der Task ausgeführt wird. Wählen Sie **Custom conditions** (Benutzerdefinierte Bedingungen) aus, um komplexere Bedingungen festzulegen.

> [!NOTE]
>
> - Roslyn Analyzers sind in den Compiler integriert und können nur im Rahmen der Kompilierung von „csc.exe“ ausgeführt werden. Daher muss für diesen Task der Compilerbefehl, der zuvor im Build ausgeführt wurde, wiedergegeben oder erneut ausgeführt werden. Bei der Wiedergabe oder Ausführung werden die Protokolle des MSBuild-Buildtasks von Visual Studio Team Services (VSTS) abgefragt.
>
>   Es gibt keine andere Möglichkeit für den Task, die Befehlszeile für die MSBuild-Kompilierung zuverlässig aus der Builddefinition abzurufen. Wir haben überlegt, ein Freiform-Textfeld hinzuzufügen, in dem Benutzer ihre Befehlszeilen eingeben können. Dann wäre es allerdings schwierig, diese Befehlszeilen auf dem neuesten Stand und synchron mit dem Hauptbuild zu halten.
>
>   Bei benutzerdefinierten Builds muss der gesamte Befehlssatz wiedergegeben werden, nicht nur Compilerbefehle. In diesen Fällen ist die Aktivierung von Roslyn Analyzers weder einfach noch zuverlässig.
>
> - Roslyn Analyzers sind in den Compiler integriert. Roslyn Analyzers müssen kompiliert werden, damit sie aufgerufen werden können.
>
>   Der neue Buildtask wird implementiert, indem C#-Projekte, die bereits erstellt wurden, erneut kompiliert werden. Der neue Task verwendet nur den MSBuild-und VSBuild-Buildtask im selben Build oder in derselben Builddefinition wie der ursprüngliche Task. In diesem Fall werden sie vom neuen Task jedoch mit aktivierten Roslyn Analyzers verwendet.
>
>   Wenn der neue Task auf dem selben Agent wie der ursprüngliche Task ausgeführt wird, wird die Ausgabe des ursprünglichen Tasks im Quellordner *s* durch die Ausgabe des neuen Tasks überschrieben. Obwohl die Buildausgabe identisch ist, wird empfohlen, dass Sie MSBuild ausführen, die Ausgabe in das Artefaktstagingverzeichnis kopieren und dann Roslyn Analyzers ausführen.

Weitere Ressourcen für den Roslyn Analyzers-Task finden Sie unter [Die auf Roslyn basierenden Analysetools](https://docs.microsoft.com/dotnet/standard/analyzers/) in der Microsoft-Dokumentation.

Sie finden das Analyzer-Paket, das von diesem Buildtask installiert und verwendet wird, auf der NuGet-Seite [Microsoft.CodeAnalysis.FxCopAnalyzers](https://www.nuget.org/packages/Microsoft.CodeAnalysis.FxCopAnalyzers).

Weitere Informationen zur YAML-Konfiguration für diese Aufgabe finden Sie in den [YAML-Optionen für Roslyn Analyzers](yaml-configuration.md#roslyn-analyzers-task).

## <a name="tslint-task"></a>TSLint-Task

Weitere Informationen zu TSLint finden Sie im [TSLint GitHub-Repository](https://github.com/palantir/tslint).

>[!NOTE] 
>Wie Sie vielleicht wissen, ist auf der Homepage des [TSLint GitHub-Repositorys](https://github.com/palantir/tslint) angegeben, dass TSLint im Laufe des Jahres 2019 als veraltet eingestuft wird. Microsoft prüft, ob [ESLint](https://github.com/eslint/eslint) als alternativer Task infrage kommt.

Weitere Informationen zur YAML-Konfiguration für diese Aufgabe finden Sie in den [YAML-Optionen für TSLint](yaml-configuration.md#tslint-task).

## <a name="publish-security-analysis-logs-task"></a>Task zum Veröffentlichen von Sicherheitsanalyseprotokollen

Details zur Taskkonfiguration finden Sie im folgenden Screenshot und in der Liste.

![Konfigurieren des Buildtasks zum Veröffentlichen von Sicherheitsanalyseprotokollen](./media/security-tools/9-publish-security-analsis-logs600.png)  

- **Artifact Name** (Artefaktname): Eine beliebige Zeichenfolgen-ID.
- **Artifact Type** (Artefakttyp): Je nach Auswahl können Sie Protokolle auf Ihrer Azure DevOps Server-Instanz oder in einer freigegebenen Datei veröffentlichen, die für den Build-Agent zugänglich ist.
- **Tools:** Sie können auswählen, ob Sie Protokolle für spezifische Tools beibehalten möchten, oder **All Tools** (Alle Tools) auswählen, um alle Protokolle beizubehalten.

Weitere Informationen zur YAML-Konfiguration für diese Aufgabe finden Sie in den [YAML-Optionen zum Veröffentlichen von Sicherheitsprotokollen](yaml-configuration.md#publish-security-analysis-logs-task).

## <a name="security-report-task"></a>Sicherheitsberichts-Task

Details zur Konfiguration von Sicherheitsberichten finden Sie im folgenden Screenshot und in der Liste.

![Konfigurieren des Sicherheitsberichts-Buildtasks](./media/security-tools/4-createsecurityanalysisreport600.png)

- **Reports** (Berichte): Wählen Sie das Format **Pipeline Console** (Pipelinekonsole), **TSV File** (TSV-Datei) oder **Html File** (HTML-Datei) aus. Für jedes ausgewählte Format wird eine Berichtsdatei erstellt.
- **Tools:** Wählen Sie die Tools in der Builddefinition aus, für die eine Zusammenfassung der erkannten Probleme angezeigt werden soll. Für jedes ausgewählte Tool können Sie u. U. auswählen, ob im Zusammenfassungsbericht nur Fehler oder sowohl Fehler als auch Warnungen angezeigt werden sollen.
- **Advanced Options** (Erweiterte Optionen): Wenn für eines der ausgewählten Tools keine Protokolle vorhanden sind, können Sie auswählen, ob eine Warnung oder ein Fehler protokolliert werden soll. Wenn Sie einen Fehler protokollieren, verursacht der Task einen Fehler.
- **Base Logs Folder** (Basisprotokollordner): Sie können den Ordner für Basisprotokolle anpassen, in dem die Protokolle gespeichert werden. Diese Option wird jedoch in der Regel nicht verwendet.

Weitere Informationen zur YAML-Konfiguration für diese Aufgabe finden Sie in den [YAML-Optionen für Sicherheitsberichte](yaml-configuration.md#security-report-task).

## <a name="post-analysis-task"></a>Nachanalyse-Task

Details zur Taskkonfiguration finden Sie im folgenden Screenshot und in der Liste.

![Konfigurieren des Nachanalyse-Buildtasks](./media/security-tools/a-post-analysis600.png)

- **Tools:** Wählen Sie die Tools in der Builddefinition aus, für die Sie bedingt eine Buildunterbrechung einfügen möchten. Für jedes ausgewählte Tool können Sie u. U. auswählen, ob nur bei Fehlern oder sowohl bei Fehlern als auch bei Warnungen eine Unterbrechung auftreten soll.
- **Report** (Bericht): Die Ergebnisse, die die Buildunterbrechung verursachen, können optional ausgegeben werden. Die Ergebnisse werden in das Azure DevOps-Konsolenfenster und in die Protokolldatei geschrieben.
- **Advanced Options** (Erweiterte Optionen): Wenn für eines der ausgewählten Tools keine Protokolle vorhanden sind, können Sie auswählen, ob eine Warnung oder ein Fehler protokolliert werden soll. Wenn Sie einen Fehler protokollieren, verursacht der Task einen Fehler.

Weitere Informationen zur YAML-Konfiguration für diese Aufgabe finden Sie in den [YAML-Optionen für die Nachanalyse](yaml-configuration.md#post-analysis-task).

## <a name="next-steps"></a>Nächste Schritte

Informationen zur YAML-basierten Konfiguration finden Sie im [YAML-Konfigurationshandbuch](yaml-configuration.md).

Wenn Sie weitere Fragen zur Erweiterung „Sicherheitscodeanalyse“ und zu den angebotenen Tools haben, lesen Sie unsere [Seite mit FAQs](security-code-analysis-faq.md).
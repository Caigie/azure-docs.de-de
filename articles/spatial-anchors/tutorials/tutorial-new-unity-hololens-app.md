---
title: 'Tutorial: Erstellen einer neuen HoloLens-Unity-App'
description: In diesem Tutorial wird beschrieben, wie Sie mit Azure Spatial Anchors eine neue HoloLens Unity-App erstellen.
author: craigktreasure
manager: vriveras
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 07/05/2019
ms.topic: tutorial
ms.service: azure-spatial-anchors
ms.openlocfilehash: a3a81cdfa9e3c6659c870b184cda4ebe6f7f805a
ms.sourcegitcommit: b55d1d1e336c1bcd1c1a71695b2fd0ca62f9d625
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2020
ms.locfileid: "84429664"
---
# <a name="tutorial-step-by-step-instructions-to-create-a-new-hololens-unity-app-using-azure-spatial-anchors"></a>Tutorial: Schritt-für-Schritt-Anleitung zum Erstellen einer neuen HoloLens Unity-App mit Azure Spatial Anchors

In diesem Tutorial wird gezeigt, wie Sie mit Azure Spatial Anchors eine neue HoloLens Unity-App erstellen.

## <a name="prerequisites"></a>Voraussetzungen

Damit Sie dieses Tutorial ausführen können, benötigen Sie folgende Komponenten:

1. Einen Windows-Computer mit Installation von <a href="https://www.visualstudio.com/downloads/" target="_blank">Visual Studio 2017 oder höher</a> mit der Workload **Entwicklung für die universelle Windows-Plattform** und der Komponente **Windows 10 SDK (10.0.18362.0 oder höher)** und <a href="https://git-scm.com/download/win" target="_blank">Git für Windows</a>.
2. Die [C++/WinRT-Visual Studio-Erweiterung (VSIX)](https://aka.ms/cppwinrt/vsix) für Visual Studio sollte über den [Visual Studio Marketplace](https://marketplace.visualstudio.com/) installiert werden.
3. Ein HoloLens-Gerät mit aktiviertem [Entwicklermodus](https://docs.microsoft.com/windows/mixed-reality/using-visual-studio). Für diesen Artikel wird ein HoloLens-Gerät mit dem [Windows 10-Update von Oktober 2018](https://docs.microsoft.com/windows/mixed-reality/release-notes-october-2018 ) (auch als RS5 bezeichnet) benötigt. Öffnen Sie zum Aktualisieren auf das neueste HoloLens-Release die App **Einstellungen**, navigieren Sie zu **Update und Sicherheit**, und wählen Sie anschließend die Schaltfläche **Nach Updates suchen**.

## <a name="getting-started"></a>Erste Schritte

Wir werden zuerst unser Projekt und die Unity-Szene einrichten:
1. Starten Sie Unity.
2. Wählen Sie **Neu**aus.
4. Vergewissern Sie sich, dass **3D** ausgewählt ist.
5. Benennen Sie Ihr Projekt, und geben Sie einen **Speicherort** ein.
6. Klicken Sie auf **Projekt erstellen**.
7. Speichern Sie die leere Standardszene in einer neuen Datei mithilfe von: **Datei** > **Speichern unter**.
8. Nennen Sie die neue Szene **Main** (Haupt), und drücken Sie auf die Schaltfläche **Speichern**.

**Einrichten der Projekteinstellungen**

Jetzt legen wir einige Unity-Projekteinstellungen fest, die uns dabei helfen, das Windows Holographic SDK für die Entwicklung zu verwenden.

Zuerst legen wir Qualitätseinstellungen für unsere Anwendung fest.
1. Wählen Sie **Bearbeiten** > **Projekteinstellungen** > **Qualität** aus.
2. Klicken Sie in der Spalte unter dem **Windows Store**-Logo auf den Pfeil in der Zeile **Standard**, und wählen Sie **Sehr niedrig** aus. Sie wissen, dass die Einstellung ordnungsgemäß angewendet wurde, wenn das Feld in der Spalte **Windows Store** und der Zeile **Sehr niedrig** grün ist.

Wir müssen Unity mitteilen, dass die App, die wir exportieren möchten, eine plastische Ansicht anstelle einer 2D-Ansicht erstellen soll. Wir erstellen eine plastische Ansicht durch Aktivieren der Unterstützung für virtuelle Realität in Unity mit dem Windows 10 SDK als Ziel.

1. Wechseln Sie zu **Bearbeiten** > **Projekteinstellungen** > **Player**.
2. Wählen Sie im **Inspector-Bereich** für **Playereinstellungen**das **Windows Store**-Symbol aus.
3. Erweitern Sie die Gruppe **XR-Einstellungen**.
4. Aktivieren Sie im Abschnitt **Rendering** das Kontrollkästchen **Virtuelle Realität unterstützt**, um eine neue Liste mit **Virtual Reality SDKs** hinzuzufügen.
5. Überprüfen Sie, ob in der Liste **Windows Mixed Reality** angezeigt wird. Wenn nicht, wählen Sie die Schaltfläche **+** am Ende der Liste aus, und wählen Sie **Windows Mixed Reality** aus.

> [!NOTE]
> Wenn das Windows Store-Symbol nicht angezeigt wird, vergewissern Sie sich erneut, dass Sie das Windows Store .NET-Skript-Back-End vor der Installation ausgewählt haben. Wenn nicht, müssen Sie Unity eventuell mit der richtigen Windows-Installation neu installieren.

**Überprüfen der Konfiguration des Skript-Back-Ends**
1. Wechseln Sie zu **Bearbeiten** > **Projekteinstellungen** > **Player** (eventuell haben Sie **Player** noch aus dem vorherigen Schritt geöffnet).
2. Wählen Sie im **Inspector-Bereich** für **Playereinstellungen**das **Windows Store**-Symbol aus.
3. Stellen Sie im Konfigurationsabschnitt **Weitere Einstellungen** sicher, dass **Skript-Back-End-** auf **IL2CPP** festgelegt ist.

**Festlegen von Funktionen**
1. Wechseln Sie zu **Bearbeiten** > **Projekteinstellungen** > **Player** (eventuell haben Sie **Player** noch aus dem vorherigen Schritt geöffnet).
2. Wählen Sie im **Inspector-Bereich** für **Playereinstellungen**das **Windows Store**-Symbol aus.
3. Aktivieren Sie Konfigurationsabschnitt **Veröffentlichungseinstellungen** die Kontrollkästchen **InternetClientServer** und **SpatialPerception**.

**Einrichten der virtuellen Hauptkamera**
1. Wählen Sie im **Hierarchiebereich** die Option **Hauptkamera** aus.
2. Legen Sie im **Inspector** die Transformationsposition auf **0,0,0** fest.
3. Suchen Sie die Eigenschaft **Flags löschen** und ändern Sie das Dropdown von **Skybox** in **Volltonfarbe**.
4. Klicken Sie auf das Feld **Hintergrund**, um eine Farbauswahl zu öffnen.
5. Legen Sie **R, G, B und A** auf **0** fest.
6. Wählen Sie **Komponente hinzufügen** aus, und fügen Sie **Spatial Mapping Collider** hinzu.

**Erstellen unseres Skripts**
1. Erstellen Sie im Bereich **Projekt** einen neuen Ordner namens **Skripts** unter dem Ordner **Assets** (Ressourcen).
2. Klicken Sie mit der rechten Maustaste auf den Ordner, und wählen Sie dann **Erstellen >** , **C#-Skript** aus. Benennen Sie es mit **AzureSpatialAnchorsScript**.
3. Wechseln Sie zu **GameObject** -> **Leer erstellen**.
4. Wählen Sie es aus, und benennen Sie es im **Inspector** von **GameObject** in **MixedRealityCloud** um. Wählen Sie **Komponente hinzufügen** aus, suchen Sie nach **AzureSpatialAnchorsScript**, und fügen Sie es hinzu.

**Erstellen des Prefabs für die Kugel**
1. Wechseln Sie zu **GameObject** -> **3D-Objekt** -> **Kugel**.
2. Legen Sie das Größenverhältnis im **Inspector** auf **0,25, 0,25, 0,25** fest.
3. Suchen Sie das Objekt **Kugel** im Bereich **Hierarchie**. Klicken Sie darauf, und ziehen Sie es im Bereich **Projekt** in den Ordner **Ressourcen**.
4. Klicken Sie mit der rechten Maustaste in den Bereich **Hierarchie**, und **löschen** Sie die ursprüngliche Kugel, die Sie erstellt haben.

Nun verfügen Sie über ein Prefab für die Kugel im Bereich **Projekt**.

## <a name="trying-it-out"></a>Ausprobieren
Um zu testen, dass alles funktioniert, erstellen Sie Ihre App in **Unity**, und stellen Sie sie aus **Visual Studio** bereit. Gehen Sie hierzu wie in Kapitel 6 des [**MR-Grundlagen 100: Erste Schritte mit Unity**-Kurses](https://docs.microsoft.com/windows/mixed-reality/holograms-100#chapter-6---build-and-deploy-to-device-from-visual-studio) vor. Es sollte der Unity-Startbildschirm und anschließend eine leere Anzeige angezeigt werden.

## <a name="place-an-object-in-the-real-world"></a>Platzieren eines Objekts in der realen Welt
Wir erstellen über Ihre App jetzt ein Objekt und platzieren es. Öffnen Sie die Visual Studio-Projektmappe, die wir beim [Bereitstellen unserer App](#trying-it-out) erstellt haben.

Fügen Sie zunächst die folgenden Importe für `Assembly-CSharp (Universal Windows)\Scripts\AzureSpatialAnchorsScript.cs` hinzu:

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=19-24)]

Fügen Sie anschließend die folgenden Membervariablen in Ihrer `AzureSpatialAnchorsScript`-Klasse hinzu:

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=26-42,48-52,60-79)]

Bevor wir fortfahren, müssen wir das Kugel-Prefab festlegen, das wir in der spherePrefab-Membervariable erstellt haben. Wechseln Sie zurück zu **Unity**.
1. Wechseln Sie in **Unity** im Bereich **Hierarchy** das Objekt **MixedRealityCloud** aus.
2. Klicken Sie auf das Prefab **Kugel**, das Sie im Bereich **Projekt** gespeichert haben. Ziehen Sie die **Kugel** in den Bereich für das **Kugel-Prefab** im Bereich **Inspector** unter **Azure Spatial Anchors-Skript (Skript)** .

Jetzt ist die **Kugel** als Prefab in Ihrem Skript festgelegt. Kompilieren Sie aus **Unity** heraus, und öffnen Sie dann die resultierende **Visual Studio**-Projektmappe erneut, wie unter [Ausprobieren](#trying-it-out) erläutert.

Öffnen Sie in **Visual Studio** die Datei `AzureSpatialAnchorsScript.cs` erneut. Fügen Sie den folgenden Code zu Ihrer `Start()`-Methode hinzu. Dieser Code bindet `GestureRecognizer` ein, der erkennt, wenn in die Luft getippt wird, und dann `HandleTap` aufruft.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=81-90,93&highlight=4-10)]

Wir müssen nun die folgende `HandleTap()`-Methode unterhalb von `Update()` hinzufügen. Sie führt ein Raycasting aus und erhält einen Trefferpunkt (Hit Point), an dem eine Kugel platziert wird.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=267-277,299-300,304-312)]

Diese Kugel müssen wir nun erstellen. Die Kugel wird anfänglich weiß sein, aber dieser Wert wird später noch angepasst. Fügen Sie die folgende `CreateAndSaveSphere()`-Methode hinzu:

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=314-325,390)]

Führen Sie Ihre App aus **Visual Studio** heraus aus, um sie noch einmal zu überprüfen. Tippen Sie diesmal auf den Bildschirm, um Ihre weiße Kugel zu erstellen und auf der Oberfläche Ihrer Wahl zu platzieren.

## <a name="set-up-the-dispatcher-pattern"></a>Einrichten des Dispatcher-Musters

Bei der Arbeit mit Unity müssen alle Unity-APIs, z. B. APIs, die Sie für Aktualisierungen der Benutzeroberfläche verwenden, im Hauptthread ausgeführt werden. In dem Code, den wir schreiben, erhalten wir Rückrufe an andere Threads. In diesen Rückrufen möchten wir die Benutzeroberfläche aktualisieren, weshalb wir eine Möglichkeit benötigen, um einem Nebenthread zum Hauptthread zu gelangen. Um Code aus einem Nebenthread im Hauptthread auszuführen, verwenden wir das Dispatcher-Muster.

Fügen wir eine Membervariable namens „dispatchQueue“ hinzu, bei der es sich um eine Warteschlange mit Aktionen handelt. Wir pushen Aktionen in die Warteschlange, und entfernen sie dann im Hauptthread aus der Warteschlange und führen sie aus.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=38-51&highlight=6-9)]

Als Nächstes fügen wir eine Möglichkeit hinzu, um der Warteschlange eine Aktion hinzuzufügen. Fügen Sie direkt hinter `Update()``QueueOnUpdate()` hinzu:

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=107-117)]

Nun verwenden wir die Update()-Schleife, um zu überprüfen, ob eine Aktion in der Warteschlange vorhanden ist. Wenn ja, entfernen wir die Aktion aus der Warteschlange und führen sie aus.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=95-105&highlight=4-10)]

## <a name="get-the-azure-spatial-anchors-sdk"></a>Abrufen des Azure Spatial Anchors SDK

## <a name="via-unity-package"></a>[Über das Unity-Paket](#tab/UnityPackage)

Jetzt laden wir das Azure Spatial Anchors SDK herunter. Wechseln Sie zur Seite [Azure Spatial Anchors GitHub Releases](https://github.com/Azure/azure-spatial-anchors-samples/releases). Laden Sie unter „Assets“ (Ressourcen) **AzureSpatialAnchors.unitypackage** herunter. Wechseln Sie in Unity zu **Assets** (Ressourcen), und klicken Sie auf **Import Package (Paket importieren)**  > **Custom Package... (Benutzerdefiniertes Paket...)** . Navigieren Sie zu dem Paket, und wählen Sie **Öffnen** aus.

Deaktivieren Sie im neu geöffneten Fenster **Import Unity Package** (Unity-Paket importieren) die Option **Plugins**, und klicken Sie dann in der unteren rechten Ecke auf **Import** (Importieren).

Nun müssen wir Nuget-Pakete wiederherstellen, um das Azure Spatial Anchors SDK zu erhalten. Kompilieren Sie aus **Unity** heraus, und öffnen Sie dann die sich ergebende **Visual Studio**-Projektmappe, und erstellen Sie diese erneut, wie unter [Ausprobieren](#trying-it-out) dargelegt.

## <a name="via-nugetforunity"></a>[Über NuGetForUnity](#tab/NuGetForUnity)

Zuerst müssen wir NuGetForUnity installieren. Wechseln Sie zur Seite der [NuGetForUnity-Releases auf GitHub](https://github.com/GlitchEnzo/NuGetForUnity/releases). Laden Sie unter „Assets“ (Ressourcen) das neueste **NuGetForUnity.unitypackage** herunter. Wechseln Sie in Unity zu **Assets** (Ressourcen), und klicken Sie auf **Import Package (Paket importieren)**  > **Custom Package... (Benutzerdefiniertes Paket...)** . Navigieren Sie zu dem Paket, und wählen Sie **Öffnen** aus. Nun wird NugetForUnity von Unity installiert. Wenn in Unity keine neue **NuGet**-Dropdownliste angezeigt wird, müssen Sie möglicherweise mit der rechten Maustaste auf **Projects** (Projekte) > **Assets** (Ressourcen) klicken. Wählen Sie dann **Reimport All** (Alle erneut importieren) aus.

Nachdem Sie NuGetForUnity installiert haben, wählen Sie **NuGet** > **Manage NuGet Packages** (NuGet-Pakete verwalten) aus. Suchen Sie dann nach Microsoft.Azure.SpatialAnchors.Unity, und wählen Sie **Install** (Installieren) aus.

Wir müssen jetzt kompilieren, um das tatsächliche Azure Spatial Anchors-SDK zu erhalten, da das soeben heruntergeladene NuGet-Paket nur Hilfsskripts enthält. Kompilieren Sie aus **Unity** heraus, und öffnen Sie dann die sich ergebende **Visual Studio**-Projektmappe, und erstellen Sie diese erneut, wie unter [Ausprobieren](#trying-it-out) dargelegt.

---

Fügen Sie in Ihrer **Visual Studio**-Projektmappe den folgenden Import in Ihrem `<ProjectName>\Assets\Scripts\AzureSpatialAnchorsScript.cs` hinzu:

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=18-21&highlight=1)]

Fügen Sie anschließend die folgenden Membervariablen in Ihrer `AzureSpatialAnchorsScript`-Klasse hinzu:

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=48-63&highlight=6-11)]

## <a name="attach-a-local-azure-spatial-anchor-to-the-local-anchor"></a>Anfügen eines lokalen Azure-Raumankers an den lokalen Anker

Richten wir die CloudSpatialAnchorSession des Azure-Raumankers ein. Wir beginnen damit, dass wir die folgende `InitializeSession()`-Methode in Ihrer `AzureSpatialAnchorsScript`-Klasse hinzufügen. Nach dem Aufrufen wird sichergestellt, dass eine Azure Spatial Anchors-Sitzung erstellt und beim Starten Ihrer App richtig initialisiert wird.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=174-202,205-209)]

Nun müssen wir Code schreiben, um Delegataufrufe zu verarbeiten. Im weiteren Verlauf werden wir diesen noch mehr hinzufügen.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=211-226)]

Nun binden wir Ihre `initializeSession()`-Methode in Ihre `Start()`-Methode ein.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=81-93&highlight=12)]

Fügen Sie abschließend Ihrer `CreateAndSaveSphere()`-Methode den folgenden Code hinzu. Er fügt der Kugel, die wir in der realen Welt platzieren, einen lokalen Azure-Raumanker an.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=314-338,390&highlight=14-25)]

Vor dem Fortfahren müssen Sie einen Azure Spatial Anchors-Kontobezeichner und -schlüssel erstellen, falls Sie nicht bereits darüber verfügen. Befolgen Sie die Anleitung im folgenden Abschnitt, um diese Angaben zu erhalten.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="upload-your-local-anchor-into-the-cloud"></a>Hochladen Ihres lokalen Ankers in die Cloud

Wenn Ihnen der Azure Spatial Anchors-Kontobezeichner und -schlüssel vorliegen, fügen Sie die `Account Id` in `SpatialAnchorsAccountId` und `Account Key` in `SpatialAnchorsAccountKey` ein.

Im letzten Schritt werden alle Komponenten zusammengefügt. Fügen Sie in Ihrer `SpawnNewAnchoredObject()`-Methode den folgenden Code hinzu. Er ruft die `CreateAnchorAsync()`-Methode auf, sobald Ihre Kugel erstellt wurde. Nach der Methodenrückgabe wird die Kugel mit dem unten angegebenen Code noch ein letztes Mal aktualisiert, indem ihre Farbe in Blau geändert wird.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=314-391&highlight=26-77)]

Führen Sie Ihre App ein weiteres Mal aus **Visual Studio** heraus aus. Bewegen Sie Ihren Kopf, und tippen Sie dann in die Luft, um Ihre Kugel zu platzieren. Nachdem genügend Frames vorhanden sind, ändert sich die Farbe der Kugel in Gelb, und der Upload in die Cloud beginnt. Nach Abschluss des Uploadvorgangs ändert sich die Farbe der Kugel in Blau. Optional könnten Sie auch das Ausgabefenster innerhalb von **Visual Studio** verwenden, um die Protokollmeldungen zu überwachen, die von Ihrer App gesendet werden. Sie können den empfohlenen Erstellungsstatus beobachten sowie den Ankerbezeichner, den die Cloud nach Abschluss des Uploadvorgangs zurückgibt.

> [!NOTE]
> Wenn Sie „DllNotFoundException: Die DLL 'AzureSpatialAnchors' kann nicht geladen werden: Das angegebene Modul wurde nicht gefunden.“ erhalten, sollten Sie Ihre Projektmappe **Bereinigen** und erneut **Erstellen**.

## <a name="locate-your-cloud-spatial-anchor"></a>Suchen nach Ihrem Raumanker für die Cloud

Nachdem Ihr Anker in die Cloud hochgeladen wurde, können wir erneut versuchen, danach zu suchen. Fügen wir nun den folgenden Code in Ihrer `HandleTap()`-Methode hinzu. Dieser Code bewirkt Folgendes:

* Er ruft `ResetSession()` auf, wodurch die `CloudSpatialAnchorSession` beendet und unsere vorhandene blaue Kugel vom Bildschirm entfernt wird.
* Er initialisiert `CloudSpatialAnchorSession` erneut. Dies machen wir, damit sichergestellt ist, dass der zu suchende Anker aus der Cloud stammt und es sich nicht um den von uns erstellten lokalen Anker handelt.
* Er erstellt einen **Watcher**, der nach dem Anker sucht, den wir in Azure Spatial Anchors hochgeladen haben.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=267-305&highlight=13-31,35-36)]

Nun fügen wir unserer `ResetSession()`- und `CleanupObjects()`-Methoden hinzu. Sie können sie unterhalb von `QueueOnUpdate()` einfügen.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=119-172)]

Nun müssen wir Code einbinden, der aufgerufen wird, wenn der von uns abgefragte Anker gefunden wird. Fügen Sie innerhalb von `InitializeSession()` die folgenden Rückrufe hinzu:

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=200-206&highlight=4-5)]


Jetzt fügen wir Code hinzu, der eine grüne Kugel erstellt und platziert, nachdem der CloudSpatialAnchor gefunden wurde. Auch das Tippen auf den Bildschirm wird wieder aktiviert, sodass Sie das gesamte Szenario wiederholen können: weiteren lokalen Anker erstellen, Upload durchführen und anschließend danach suchen.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=228-265)]

Das ist alles! Führen Sie Ihre App ein letztes Mal aus **Visual Studio** heraus aus, um das gesamte Szenario von Anfang bis Ende auszuprobieren. Bewegen Sie Ihr Gerät, und platzieren Sie Ihre weiße Kugel. Bewegen Sie Ihren Kopf dann weiter, um Umgebungsdaten zu erfassen, bis sich die Farbe der Kugel in Gelb ändert. Ihr lokaler Anker wird hochgeladen, und Ihre Kugel nimmt die blaue Farbe an. Tippen Sie abschließend noch einmal auf Ihren Bildschirm, damit Ihr lokaler Anker entfernt wird. Anschließend führen wir eine Abfrage nach der Entsprechung in der Cloud durch. Fahren Sie mit dem Umherbewegen Ihres Geräts fort, bis Ihr Raumanker für die Cloud gefunden wurde. Eine grüne Kugel sollte an der richtigen Position angezeigt werden, und Sie können das gesamte Szenario noch einmal durchführen.

[!INCLUDE [AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md)]
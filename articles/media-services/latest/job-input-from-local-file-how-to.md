---
title: Erstellen einer Azure Media Services-Auftragseingabe aus einer lokalen Datei | Microsoft-Dokumentation
description: In diesem Artikel wird gezeigt, wie Sie eine Azure Media Services-Auftragseingabe aus einer lokalen Datei erstellen.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 03/26/2020
ms.author: juliako
ms.openlocfilehash: a635a75a64db59fc56c6890f79af967a58735853
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87001127"
---
# <a name="create-a-job-input-from-a-local-file"></a>Erstellen einer Auftragseingabe aus einer lokalen Datei

Wenn Sie in Media Services v3 Aufträge zur Verarbeitung Ihrer Videos übermitteln, müssen Sie Media Services mitteilen, wo sich das Eingabevideo befindet. Das Eingabevideo kann als Media Service-Medienobjekt gespeichert werden. In diesem Fall erstellen Sie ein Eingabemedienobjekt basierend auf einer Datei (die lokal oder in Azure Blob Storage gespeichert ist). In diesem Thema wird gezeigt, wie Sie eine Auftragseingabe aus einer lokalen Datei erstellen. Ein vollständiges Beispiel finden Sie in diesem [GitHub-Beispiel](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs).

## <a name="prerequisites"></a>Voraussetzungen 

* [Erstellen Sie ein Media Services-Konto.](./create-account-howto.md)
* Informieren Sie sich über das [Verwalten von Ressourcen](manage-asset-concept.md).

## <a name="net-sample"></a>.NET-Beispiel

Der folgende Code zeigt, wie Sie ein Eingabemedienobjekt erstellen und es als Eingabe für den Auftrag nutzen können. Die „CreateInputAsset“-Funktion führt die folgenden Aktionen aus:

* Das Medienobjekt erstellen
* Abrufen einer nicht schreibgeschützten [SAS-URL](../../storage/common/storage-sas-overview.md) für den [Container](../../storage/blobs/storage-quickstart-blobs-dotnet.md#upload-blobs-to-a-container) des Objekts im Speicher
* Die Datei in den Container im Speicher mithilfe der SAS-URL hochladen

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#CreateInputAsset)]

Der folgende Codeausschnitt erzeugt ein Ausgabemedienobjekt, wenn es nicht bereits vorhanden ist:

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#CreateOutputAsset)]

Der folgende Codeausschnitt übermittelt einen Codierungsauftrag:

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#SubmitJob)]

## <a name="job-error-codes"></a>Auftragsfehlercodes

Weitere Informationen finden Sie unter [Fehlercodes](/rest/api/media/jobs/get#joberrorcode).

## <a name="next-steps"></a>Nächste Schritte

[Erstellen einer Auftragseingabe aus einer HTTPS-URL](job-input-from-http-how-to.md)

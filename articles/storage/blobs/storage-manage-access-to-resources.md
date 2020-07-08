---
title: Verwalten des öffentlichen Lesezugriffs für Container und Blobs
titleSuffix: Azure Storage
description: Erfahren Sie, wie Sie Container und Blobs für den anonymen Zugriff verfügbar machen und darauf programmgesteuert zugreifen.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 05/05/2020
ms.author: tamram
ms.reviewer: ozgun
ms.openlocfilehash: 61f27d81cda2e57eb5730016dcc22252508fdd01
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84809002"
---
# <a name="manage-anonymous-read-access-to-containers-and-blobs"></a>Verwalten des anonymen Lesezugriffs auf Container und Blobs

Sie können anonymen öffentlichen Lesezugriff auf einen Container und dessen Blobs in Azure Blob Storage aktivieren. Auf diese Weise können Sie schreibgeschützten Zugriff auf diese Ressourcen gewähren, ohne Ihren Kontoschlüssel freizugeben und eine Shared Access Signature (SAS) zu erfordern.

Öffentlicher Lesezugriff ist am besten für Szenarien, in denen bestimmte Blobs stets für anonymen Lesezugriff zur Verfügung stehen sollen. Für eine feiner abgestufte Zugriffssteuerung können Sie eine Shared Access Signature erstellen. Shared Access Signatures ermöglichen es Ihnen, eingeschränkten Zugriff mit unterschiedlichen Berechtigungen für einen bestimmten Zeitraum zu gewähren. Weitere Informationen zum Erstellen von Shared Access Signatures finden Sie unter [Verwenden von Shared Access Signatures (SAS) in Azure Storage](../common/storage-sas-overview.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

## <a name="grant-anonymous-users-permissions-to-containers-and-blobs"></a>Erteilen von anonymen Benutzerberechtigungen für Container und Blobs

In der Standardeinstellung kann nur ein Benutzer mit den entsprechenden Berechtigungen auf einen Container und auf darin enthaltene Blobs zugreifen. Wenn anonyme Benutzer Lesezugriff auf den Container und auf die zugehörigen Blobs erhalten sollen, können Sie für den Container die öffentliche Zugriffsebene festlegen. Diese Benutzer müssen die Anforderung dann nicht autorisieren.

Sie können einen Container mit den folgenden Berechtigungen konfigurieren:

- **No public read access** (Kein öffentlicher Lesezugriff): Nur der Speicherkontobesitzer kann auf den Container und dessen Blobs zugreifen. Dies ist die Standardeinstellung für alle neuen Container.
- **Public read access for blobs only** (Öffentlicher Lesezugriff nur für Blobs): Blobs innerhalb des Containers können über anonyme Anforderungen gelesen werden, Containerdaten sind aber nicht verfügbar. Anonyme Clients können die Blobs im Container nicht aufzählen.
- **Public read access for container and its blobs** (Öffentlicher Lesezugriff für Container und zugehörige Blobs): Alle Container- und Blobdaten können über anonyme Anforderungen gelesen werden. Clients können Blobs im Container über anonyme Anforderungen aufzählen, können aber keine Container im Speicherkonto aufzählen.

### <a name="set-container-public-access-level-in-the-azure-portal"></a>Festlegen der öffentlichen Zugriffsebene für einen Container im Azure-Portal

Im [Azure-Portal](https://portal.azure.com) können Sie die öffentliche Zugriffsebene für einen oder mehrere Container ändern:

1. Navigieren Sie zu Ihrer Speicherkontoübersicht im Azure-Portal.
1. Wählen Sie auf dem Menüblatt unter **Blob-Dienst** die Option **Blobs** aus.
1. Wählen Sie die Container aus, für die Sie die öffentliche Zugriffsebene festlegen möchten.
1. Klicken Sie auf **Zugriffsebene ändern**, um die Einstellungen für den öffentlichen Zugriff aufzurufen.
1. Wählen Sie aus der Dropdownliste **Öffentliche Zugriffsebene** den gewünschten Eintrag aus, und klicken Sie auf „OK“, damit die Änderungen auf die ausgewählten Container angewendet werden.

Auf dem folgenden Screenshot wird gezeigt, wie Sie die öffentliche Zugriffsebene für die ausgewählten Container ändern.

![Screenshot: Festlegen der öffentlichen Zugriffsebene im Azure-Portal](./media/storage-manage-access-to-resources/storage-manage-access-to-resources-0.png)

> [!NOTE]
> Sie können die öffentliche Zugriffsebene nicht für einzelne Blobs ändern, da diese ausschließlich auf Containerebene festgelegt wird.

### <a name="set-container-public-access-level-with-net"></a>Festlegen der öffentlichen Zugriffsebene für den Container mit .NET

# <a name="net-v12-sdk"></a>[\.NET v12 SDK](#tab/dotnet)

Um die Berechtigungen für einen Container festzulegen, rufen Sie die [BlobContainerClient.SetAccessPolicy](https://docs.microsoft.com/dotnet/api/azure.storage.blobs.blobcontainerclient.setaccesspolicy?view=azure-dotnet)-Methode auf. 

Im folgenden Beispiel werden die Berechtigungen des Containers auf vollständigen öffentlichen Lesezugriff festgelegt. Wenn Sie Berechtigungen für den öffentlichen Lesezugriff nur für Blobs festlegen möchten, übergeben Sie das Feld **PublicAccessType.Blob** an die [BlobContainerClient.SetAccessPolicy](https://docs.microsoft.com/dotnet/api/azure.storage.blobs.blobcontainerclient.setaccesspolicy?view=azure-dotnet)-Methode. Um alle Berechtigungen für anonyme Benutzer zu entfernen, verwenden Sie das Feld **BlobContainerPublicAccessType.None**.

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/Security.cs" id="Snippet_SetPublicContainerPermissions":::

# <a name="net-v11-sdk"></a>[\.NET v11 SDK](#tab/dotnet11)

Um die Berechtigungen für einen Container mit der Azure Storage-Clientbibliothek für .NET festzulegen, rufen Sie zunächst die vorhandenen Berechtigungen des Containers ab, indem Sie eine der folgenden Methoden aufrufen:

- [GetPermissions](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.getpermissions)
- [GetPermissionsAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.getpermissionsasync)

Legen Sie als Nächstes die Eigenschaft **PublicAccess** für das Objekt [BlobContainerPermissions](/dotnet/api/microsoft.azure.storage.blob.blobcontainerpermissions) fest, das von der **GetPermissions**-Methode zurückgegeben wird.

Rufen Sie schließlich eine der folgenden Methoden auf, um die Berechtigungen für den Container zu aktualisieren:

- [SetPermissions](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.setpermissions)
- [SetPermissionsAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.setpermissionsasync)

Im folgenden Beispiel werden die Berechtigungen des Containers auf vollständigen öffentlichen Lesezugriff festgelegt. Legen Sie zum Festlegen von Berechtigungen auf den öffentlichen Lesezugriff nur für Blobs die **PublicAccess**-Eigenschaft auf **BlobContainerPublicAccessType.Blob** fest. Um alle Berechtigungen für anonyme Benutzer zu entfernen, legen Sie die Eigenschaft auf **BlobContainerPublicAccessType.Off**fest.

```csharp
private static async Task SetPublicContainerPermissions(CloudBlobContainer container)
{
    BlobContainerPermissions permissions = await container.GetPermissionsAsync();
    permissions.PublicAccess = BlobContainerPublicAccessType.Container;
    await container.SetPermissionsAsync(permissions);

    Console.WriteLine("Container {0} - permissions set to {1}", container.Name, permissions.PublicAccess);
}
```

---

## <a name="access-containers-and-blobs-anonymously"></a>Anonymer Zugriff auf Container und Blobs

Ein Client, der auf Container und Blobs anonym zugreift, kann Konstruktoren verwenden, für die keine Anmeldeinformationen erforderlich sind. In den folgenden Beispielen werden verschiedene Möglichkeiten zum anonymen Verweisen auf Container und Blobs gezeigt.

### <a name="create-an-anonymous-client-object"></a>Erstellen eines anonymen Clientobjekts

Sie können ein neues Dienstclientobjekt für den anonymen Zugriff durch Bereitstellen des Blob Storage-Endpunkts für das Konto erstellen. Allerdings müssen Sie auch den Namen eines Containers in diesem Konto kennen, der für den anonymen Zugriff verfügbar ist.

# <a name="net-v12-sdk"></a>[\.NET v12 SDK](#tab/dotnet)

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/Security.cs" id="Snippet_CreateAnonymousBlobClient":::

# <a name="net-v11-sdk"></a>[\.NET v11 SDK](#tab/dotnet11)

```csharp
public static void CreateAnonymousBlobClient()
{
    // Create the client object using the Blob storage endpoint for your account.
    CloudBlobClient blobClient = new CloudBlobClient(
        new Uri(@"https://storagesamples.blob.core.windows.net"));

    // Get a reference to a container that's available for anonymous access.
    CloudBlobContainer container = blobClient.GetContainerReference("sample-container");

    // Read the container's properties. 
    // Note this is only possible when the container supports full public read access.
    container.FetchAttributes();
    Console.WriteLine(container.Properties.LastModified);
    Console.WriteLine(container.Properties.ETag);
}
``` 

---

### <a name="reference-a-container-anonymously"></a>Anonymes Verweisen auf einen Container

Wenn Sie über die URL zu einem Container verfügen, der anonym verfügbar ist, können Sie damit direkt auf den Container verweisen.

# <a name="net-v12-sdk"></a>[\.NET v12 SDK](#tab/dotnet)

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/Security.cs" id="Snippet_ListBlobsAnonymously":::

# <a name="net-v11-sdk"></a>[\.NET v11 SDK](#tab/dotnet11)

```csharp
public static void ListBlobsAnonymously()
{
    // Get a reference to a container that's available for anonymous access.
    CloudBlobContainer container = new CloudBlobContainer(
        new Uri(@"https://storagesamples.blob.core.windows.net/sample-container"));

    // List blobs in the container.
    // Note this is only possible when the container supports full public read access.
    foreach (IListBlobItem blobItem in container.ListBlobs())
    {
        Console.WriteLine(blobItem.Uri);
    }
}
``` 

---

### <a name="reference-a-blob-anonymously"></a>Anonymes Verweisen auf ein Blob

Wenn Sie über die URL zu einem Blob verfügen, das für den anonymen Zugriff verfügbar ist, können Sie damit über diese URL direkt auf das Blob verweisen:

# <a name="net-v12-sdk"></a>[\.NET v12 SDK](#tab/dotnet)

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/Security.cs" id="Snippet_DownloadBlobAnonymously":::

# <a name="net-v11-sdk"></a>[\.NET v11 SDK](#tab/dotnet11)

```csharp
public static void DownloadBlobAnonymously()
{
    CloudBlockBlob blob = new CloudBlockBlob(
        new Uri(@"https://storagesamples.blob.core.windows.net/sample-container/logfile.txt"));
    blob.DownloadToFile(@"C:\Temp\logfile.txt", FileMode.Create);
}
``` 

---

## <a name="next-steps"></a>Nächste Schritte

- [Autorisierung des Zugriffs auf Azure Storage](../common/storage-auth.md)
- [Gewähren von eingeschränktem Zugriff auf Azure Storage-Ressourcen mithilfe von SAS (Shared Access Signature)](../common/storage-sas-overview.md)
- [REST-API des Blob-Diensts](/rest/api/storageservices/blob-service-rest-api)

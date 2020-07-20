---
title: Verschlüsselung ruhender Daten mit kundenseitig verwalteten Schlüsseln
titleSuffix: Azure Cognitive Search
description: Ergänzung der serverseitigen Verschlüsselung über Indizes und Synonymzuordnungen in Azure Cognitive Search durch Schlüssel, die Sie in Azure Key Vault erstellen und verwalten.
manager: nitinme
author: NatiNimni
ms.author: natinimn
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 01/08/2020
ms.openlocfilehash: f6bda61960efd9a5e176f8792601e315ba96bcca
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85553290"
---
# <a name="encryption-at-rest-of-content-in-azure-cognitive-search-using-customer-managed-keys-in-azure-key-vault"></a>Verschlüsselung ruhender Inhalte in Azure Cognitive Search mit von Kunden verwalteten Schlüsseln in Azure Key Vault

Standardmäßig werden in Azure Cognitive Search ruhende indizierte Inhalte mit [dienstseitig verwalteten Schlüsseln](https://docs.microsoft.com/azure/security/fundamentals/encryption-atrest#data-encryption-models) verschlüsselt. Sie können die Standardverschlüsselung durch eine zusätzliche Verschlüsselungsebene ergänzen, indem Sie Schlüssel verwenden, die Sie in Azure Key Vault erstellen und verwalten. In diesem Artikel werden die entsprechenden Schritte beschrieben.

Die serverseitige Verschlüsselung wird durch die Integration in [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-overview) unterstützt. Sie können Ihre eigenen Verschlüsselungsschlüssel erstellen und in einem Schlüsseltresor speichern oder mit Azure Key Vault-APIs Verschlüsselungsschlüssel generieren. Mit Azure Key Vault können Sie auch die Schlüsselverwendung überwachen. 

Die Verschlüsselung mit von Kunden verwalteten Schlüsseln wird auf Ebene von Indizes oder Synonymzuordnungen konfiguriert, sofern diese Objekte erstellt werden, und nicht auf Ebene des Suchdiensts. Bereits vorhandene Inhalte können nicht verschlüsselt werden. 

Die Schlüssel müssen sich nicht alle in derselben Key Vault-Instanz befinden. Ein einzelner Suchdienst kann mehrere verschlüsselte Indizes oder Synonymzuordnungen hosten, die jeweils mit ihren eigenen kundenseitig verwalteten Verschlüsselungsschlüsseln verschlüsselt werden, die in verschiedenen Key Vault-Instanzen gespeichert sind.  Im gleichen Dienst können auch Indizes und Synonymzuordnungen enthalten sein, die nicht mit kundenseitig verwalteten Schlüsseln verschlüsselt wurden. 

> [!IMPORTANT] 
> Dieses Feature ist in der [REST-API](https://docs.microsoft.com/rest/api/searchservice/) und der [.NET SDK-Version 8.0-preview](search-dotnet-sdk-migration-version-9.md) verfügbar. Derzeit wird die Konfiguration von kundenseitig verwalteten Verschlüsselungsschlüsseln im Azure-Portal nicht unterstützt. Der Suchdienst muss nach Januar 2019 erstellt worden sein und darf kein kostenloser (gemeinsam genutzter) Dienst sein.

## <a name="prerequisites"></a>Voraussetzungen

In diesem Beispiel werden die folgenden Dienste verwendet. 

+ [Erstellen Sie einen Dienst für die kognitive Azure-Suche](search-create-service-portal.md), oder [suchen Sie nach einem vorhandenen Dienst](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) in Ihrem aktuellen Abonnement. 

+ [Erstellen einer Azure Key Vault-Ressource](https://docs.microsoft.com/azure/key-vault/quick-create-portal#create-a-vault) oder Suchen eines vorhandenen Tresors in Ihrem Abonnement.

+ Für Konfigurationsaufgaben wird [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) oder die [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/install-azure-cli) verwendet.

+ Die REST-API kann mithilfe von [Postman](search-get-started-postman.md), [Azure PowerShell](search-create-index-rest-api.md) und [.NET SDK preview](https://aka.ms/search-sdk-preview) aufgerufen werden. Aktuell wird die von Kunden verwaltete Verschlüsselung im Portal nicht unterstützt.

>[!Note]
> Aufgrund der Art der Funktion zur Verschlüsselung mit kundenseitig verwalteten Schlüsseln können Ihre Daten in Azure Cognitive Search nicht abgerufen werden, wenn der Azure Key Vault-Schlüssel gelöscht wird. Um Datenverluste aufgrund versehentlich gelöschter Key Vault-Schlüssel zu vermeiden, **müssen** Sie in Key Vault die Optionen „Vorläufiges Löschen“ und „Bereinigungsschutz“ aktivieren, damit der Dienst verwendet werden kann. Weitere Informationen finden Sie unter [Übersicht über die Azure Key Vault-Funktion für vorläufiges Löschen](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete).   

## <a name="1---enable-key-recovery"></a>1: Aktivieren der Schlüsselwiederherstellung

Aktivieren Sie nach dem Erstellen der Azure Key Vault-Ressource die Optionen **Vorläufiges Löschen** und **Bereinigungsschutz** im ausgewählten Schlüsseltresor durch Ausführen der folgenden PowerShell- oder Azure CLI-Befehle:   

```powershell
$resource = Get-AzResource -ResourceId (Get-AzKeyVault -VaultName "<vault_name>").ResourceId

$resource.Properties | Add-Member -MemberType NoteProperty -Name "enableSoftDelete" -Value 'true'

$resource.Properties | Add-Member -MemberType NoteProperty -Name "enablePurgeProtection" -Value 'true'

Set-AzResource -resourceid $resource.ResourceId -Properties $resource.Properties
```

```azurecli-interactive
az keyvault update -n <vault_name> -g <resource_group> --enable-soft-delete --enable-purge-protection
```

## <a name="2---create-a-new-key"></a>2: Erstellen eines neuen Schlüssels

Wenn Sie Inhalte der kognitiven Azure-Suche mit einem vorhandenen Schlüssel verschlüsseln, können Sie diesen Schritt überspringen.

1. [Melden Sie sich beim Azure-Portal an](https://portal.azure.com), und wechseln Sie zum Key Vault-Dashboard.

1. Wählen Sie im linken Navigationsbereich die Einstellung **Schlüssel** aus, und klicken Sie auf **+ Generieren/importieren**.

1. Wählen Sie im Bereich **Schlüssel erstellen** in der Liste mit den **Optionen** die gewünschte Schlüsselerstellungsmethode aus. Sie können einen neuen Schlüssel **generieren**, einen vorhandenen Schlüssel **hochladen** oder **Sicherung wiederherstellen** verwenden, um eine Sicherung eines Schlüssels auszuwählen.

1. Geben Sie einen **Namen** für den Schlüssel ein, und wählen Sie optional andere Schlüsseleigenschaften aus.

1. Klicken Sie auf die Schaltfläche **Erstellen**, um die Bereitstellung zu starten.

Notieren Sie sich die Schlüsselkennung – diese setzt sich aus dem **Schlüsselwert-URI**, dem **Schlüsselnamen** und der **Schlüsselversion** zusammen. Diese Angaben benötigen Sie, um einen verschlüsselten Index in der kognitiven Azure-Suche zu definieren.
 
![Erstellen eines neuen Key Vault-Schlüssels](./media/search-manage-encryption-keys/create-new-key-vault-key.png "Erstellen eines neuen Key Vault-Schlüssels")

## <a name="3---create-a-service-identity"></a>3: Erstellen einer Dienstidentität

Durch Zuweisen einer Identität zu dem Suchdienst können Sie Key Vault-Zugriffsberechtigungen für den Suchdienst erteilen. Im Suchdienst wird die Authentifizierung bei Azure Key Vault mit dieser Identität durchgeführt.

Bei der kognitiven Azure-Suche werden zwei Methoden zum Zuweisen einer Identität unterstützt: eine verwaltete Identität oder eine extern verwaltete Azure Active Directory-Anwendung. 

Verwenden Sie möglichst eine verwaltete Identität. Dies ist die einfachste Möglichkeit zum Zuweisen einer Identität zu Ihrem Suchdienst und sollte in den meisten Szenarien funktionieren. Wenn Sie mehrere Schlüssel für Indizes und Synonymzuordnungen verwenden oder wenn die Lösung in einer verteilten Architektur verwendet wird, die eine identitätsbasierte Authentifizierung ausschließt, verwenden Sie die erweiterte Methode einer [extern verwalteten Azure Active Directory-Instanz](#aad-app), die am Ende dieses Artikels beschrieben wird.

 Im Allgemeinen kann der Suchdienst über eine verwaltete Identität bei Azure Key Vault authentifiziert werden, ohne dass Anmeldeinformationen im Code gespeichert werden. Der Lebenszyklus dieses Typs einer verwalteten Identität ist an den Lebenszyklus des Suchdiensts gebunden, der nur eine verwaltete Identität enthalten kann. [Weitere Informationen zu verwalteten Identitäten](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview).

1. Melden Sie sich zum Erstellen einer verwalteten Identität [beim Azure-Portal an](https://portal.azure.com), und öffnen Sie das Dashboard Ihres Suchdiensts. 

1. Klicken Sie im linken Navigationsbereich auf **Identität**, um den Status in **Ein** zu ändern, und klicken Sie dann auf **Speichern**.

![Aktivieren einer verwalteten Identität](./media/search-enable-msi/enable-identity-portal.png "Aktivieren einer verwalteten Identität")

## <a name="4---grant-key-access-permissions"></a>4: Erteilen von Zugriffsberechtigungen für den Schlüssel

Um den Suchdienst zur Verwendung des Key Vault-Schlüssels zu aktivieren, müssen Sie bestimmte Zugriffsberechtigungen für den Suchdienst erteilen.

Die Zugriffsberechtigungen können jederzeit aufgehoben werden. Nach dem Aufheben können alle Indizes oder Synonymzuordnungen des Suchdiensts, die diese Key Vault-Instanz verwenden, nicht mehr verwendet werden. Durch Wiederherstellen der Key Vault-Zugriffsberechtigungen zu einem späteren Zeitpunkt wird der Zugriff auf die Indizes oder Synonymzuordnungen wiederhergestellt. Weitere Informationen finden Sie unter [Sicherer Zugriff auf einen Schlüsseltresor](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault).

1. [Melden Sie sich beim Azure-Portal an](https://portal.azure.com), und öffnen Sie die Übersichtsseite Ihrer Key Vault-Instanz. 

1. Wählen Sie im linken Navigationsbereich die Einstellung **Zugriffsrichtlinien** aus, und klicken Sie auf **+ Neu hinzufügen**.

   ![Hinzufügen einer neuen Key Vault-Zugriffsrichtlinie](./media/search-manage-encryption-keys/add-new-key-vault-access-policy.png "Hinzufügen einer neuen Key Vault-Zugriffsrichtlinie")

1. Klicken Sie auf **Prinzipal auswählen**, und wählen Sie Ihren Dienst für die kognitive Azure-Suche aus. Sie können den Dienst anhand des Namens oder der Objekt-ID suchen, die nach dem Aktivieren der verwalteten Identität angezeigt wurde.

   ![Auswählen des Prinzipals für die Key Vault-Zugriffsrichtlinie](./media/search-manage-encryption-keys/select-key-vault-access-policy-principal.png "Auswählen des Prinzipals für die Key Vault-Zugriffsrichtlinie")

1. Klicken Sie auf **Schlüsselberechtigungen**, und aktivieren Sie *Abrufen*, *Schlüssel entpacken* und *Schlüssel packen*. Sie können die Vorlage von *Azure Data Lake Storage oder Azure Storage* verwenden, um die gewünschten Berechtigungen schnell auszuwählen.

   Der kognitiven Azure-Suche müssen die folgenden [Zugriffsberechtigungen](https://docs.microsoft.com/azure/key-vault/about-keys-secrets-and-certificates#key-operations) erteilt werden:

   * *Abrufen:* ermöglicht im Suchdienst das Abrufen des öffentlichen Teils Ihres Schlüssels in einem Key Vault.
   * *Schlüssel packen:* ermöglicht im Suchdienst die Verwendung Ihres Schlüssels zum Schutz des internen Verschlüsselungsschlüssels.
   * *Schlüssel entpacken:* ermöglicht im Suchdienst die Verwendung Ihres Schlüssels zum Entpacken des internen Verschlüsselungsschlüssels.

   ![Auswählen der Schlüsselberechtigungen für die Key Vault-Zugriffsrichtlinie](./media/search-manage-encryption-keys/select-key-vault-access-policy-key-permissions.png "Auswählen der Schlüsselberechtigungen für die Key Vault-Zugriffsrichtlinie")

1. Klicken Sie auf **OK**, und **speichern** Sie die Änderungen an der Zugriffsrichtlinie.

> [!Important]
> Verschlüsselte Inhalte in der kognitiven Azure-Suche sind zur Verwendung eines bestimmten Azure Key Vault-Schlüssels mit einer bestimmten **Version** konfiguriert. Wenn Sie den Schlüssel oder die Version ändern, muss der Index oder die Synonymzuordnung zur Verwendung des neuen Schlüssels oder der neuen Version geändert werden, **bevor** der vorherige Schlüssel bzw. die vorherige Version gelöscht wird. Andernfalls kann der Index oder die Synonymzuordnung nicht mehr verwendet werden, da Sie die Inhalte nicht mehr entschlüsseln können, wenn der Schlüsselzugriff verloren geht.   

## <a name="5---encrypt-content"></a>5: Verschlüsseln von Inhalten

Indizes oder Synonymzuordnungen, die mit einem von Kunden verwalteten Schlüssel verschlüsselt sind, können noch nicht über das Azure-Portal erstellt werden. Solche Indizes oder Synonymzuordnungen können Sie mit der REST-API für die kognitive Azure-Suche erstellen.

Indizes und Synonymzuordnungen unterstützen die neue allgemeine Eigenschaft **encryptionKey**, mit der der Schlüssel angegeben wird. 

Mithilfe des **Schlüsseltresor-URI**, des **Schlüsselnamens** und der **Schlüsselversion** Ihres Key Vault-Schlüssels kann eine **encryptionKey**-Definition erstellt werden:

```json
{
  "encryptionKey": {
    "keyVaultUri": "https://demokeyvault.vault.azure.net",
    "keyVaultKeyName": "myEncryptionKey",
    "keyVaultKeyVersion": "eaab6a663d59439ebb95ce2fe7d5f660"
  }
}
```
> [!Note] 
> Keine dieser Key Vault-Details gelten als geheim. Diese Angaben können durch Navigieren zu der entsprechenden Seite des Azure Key Vault-Schlüssels im Azure-Portal einfach abgerufen werden.

Wenn Sie eine AAD-Anwendung für die Key Vault-Authentifizierung anstelle einer verwalteten Identität verwenden, fügen Sie Ihrem Verschlüsselungsschlüssel die **Zugriffsanmeldeinformationen** der AAD-Anwendung hinzu: 
```json
{
  "encryptionKey": {
    "keyVaultUri": "https://demokeyvault.vault.azure.net",
    "keyVaultKeyName": "myEncryptionKey",
    "keyVaultKeyVersion": "eaab6a663d59439ebb95ce2fe7d5f660",
    "accessCredentials": {
      "applicationId": "00000000-0000-0000-0000-000000000000",
      "applicationSecret": "myApplicationSecret"
    }
  }
}
```

## <a name="example-index-encryption"></a>Beispiel: Indexverschlüsselung
Die Angaben zum Erstellen eines neuen Index über die REST-API finden Sie unter [Index erstellen (REST-API für die kognitive Azure-Suche)](https://docs.microsoft.com/rest/api/searchservice/create-index). Der einzige Unterschied besteht hier darin, dass die Angaben des Verschlüsselungsschlüssels als Teil der Indexdefinition angegeben werden: 

```json
{
 "name": "hotels",  
 "fields": [
  {"name": "HotelId", "type": "Edm.String", "key": true, "filterable": true},
  {"name": "HotelName", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": true, "facetable": false},
  {"name": "Description", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false, "analyzer": "en.lucene"},
  {"name": "Description_fr", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
  {"name": "Category", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true},
  {"name": "Tags", "type": "Collection(Edm.String)", "searchable": true, "filterable": true, "sortable": false, "facetable": true},
  {"name": "ParkingIncluded", "type": "Edm.Boolean", "filterable": true, "sortable": true, "facetable": true},
  {"name": "LastRenovationDate", "type": "Edm.DateTimeOffset", "filterable": true, "sortable": true, "facetable": true},
  {"name": "Rating", "type": "Edm.Double", "filterable": true, "sortable": true, "facetable": true},
  {"name": "Location", "type": "Edm.GeographyPoint", "filterable": true, "sortable": true},
 ],
 "encryptionKey": {
   "keyVaultUri": "https://demokeyvault.vault.azure.net",
   "keyVaultKeyName": "myEncryptionKey",
   "keyVaultKeyVersion": "eaab6a663d59439ebb95ce2fe7d5f660"
 }
}
```
Dann können Sie die Anforderung zur Indexerstellung senden und anschließend den Index normal verwenden.

## <a name="example-synonym-map-encryption"></a>Beispiel: Verschlüsselung einer Synonymzuordnung

Die Angaben zum Erstellen einer neuen Synonymzuordnung über die REST-API finden Sie unter [Synonymzuordnung erstellen (REST-API für die kognitive Azure-Suche)](https://docs.microsoft.com/rest/api/searchservice/create-synonym-map). Der einzige Unterschied besteht hier darin, dass die Angaben des Verschlüsselungsschlüssels als Teil der Definition der Synonymzuordnung angegeben werden: 

```json
{   
  "name" : "synonymmap1",  
  "format" : "solr",  
  "synonyms" : "United States, United States of America, USA\n
  Washington, Wash. => WA",
  "encryptionKey": {
    "keyVaultUri": "https://demokeyvault.vault.azure.net",
    "keyVaultKeyName": "myEncryptionKey",
    "keyVaultKeyVersion": "eaab6a663d59439ebb95ce2fe7d5f660"
  }
}
```
Dann können Sie die Anforderung zum Erstellen der Synonymzuordnung senden und anschließend die Synonymzuordnung normal verwenden.

>[!Important] 
> Obwohl die **encryptionKey**-Eigenschaft nicht vorhandenen Indizes oder Synonymzuordnungen in der kognitiven Azure-Suche hinzugefügt werden kann, kann sie aktualisiert werden, indem unterschiedliche Werte für eines der drei Key Vault-Details angegeben werden (z.B. Aktualisierung der Schlüsselversion). Wenn Sie einen neuen Key Vault-Schlüssel oder eine neue Schlüsselversion angeben, müssen alle Indizes oder Synonymzuordnungen in der kognitiven Azure-Suche, die den Schlüssel verwenden, zunächst für die Verwendung des neuen Schlüssels oder der neuen Version geändert werden, **bevor** der vorherige Schlüssel bzw. die vorherige Version gelöscht wird. Andernfalls kann der Index oder die Synonymzuordnung nicht mehr verwendet werden, da Sie die Inhalte nicht mehr entschlüsseln können, wenn der Schlüsselzugriff verloren geht.   
> Durch Wiederherstellen der Key Vault-Zugriffsberechtigungen zu einem späteren Zeitpunkt wird der Zugriff auf die Inhalte wiederhergestellt.

## <a name="advanced-use-an-externally-managed-azure-active-directory-application"></a><a name="aad-app"></a> Erweitert: Verwenden einer extern verwalteten Azure Active Directory-Anwendung

Wenn die Verwendung einer verwalteten Identität nicht möglich ist, können Sie eine Azure Active Directory-Anwendung mit einem Sicherheitsprinzipal für Ihren Dienst für die kognitive Azure-Suche erstellen. Insbesondere unter den folgenden Bedingungen eignet sich eine verwaltete Identität nicht:

* Sie können für Ihren Suchdienst nicht direkt Zugriffsberechtigungen für die Key Vault-Instanz erteilen (z. B. wenn der Suchdienst sich in einem anderen Active Directory-Mandanten befindet als die Azure Key Vault-Instanz).

* Ein einzelner Suchdienst ist erforderlich, um mehrere verschlüsselte Indizes oder Synonymzuordnungen zu hosten, die jeweils einen anderen Schlüssel aus einem anderen Schlüsselspeicher verwenden, wobei jeder Schlüsselspeicher **eine andere Identität** zur Authentifizierung verwenden muss. Wenn die Verwendung einer jeweils anderen Identität zum Verwalten verschiedener Schlüsseltresore nicht erforderlich ist, können Sie die oben beschriebene Option der verwalteten Identität verwenden.  

Für diese Topologien unterstützt die kognitive Azure-Suche die Verwendung von Azure Active Directory-Anwendungen (AAD-Anwendungen) zur Authentifizierung zwischen dem Suchdienst und der Key Vault-Instanz.    
So erstellen Sie eine AAD-Anwendung im Portal

1. [Erstellen Sie eine Azure Active Directory-Anwendung.](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#create-an-azure-active-directory-application)

1. [Rufen Sie die Anwendungs-ID und den Authentifizierungsschlüssel ab](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in), da diese zum Erstellen eines verschlüsselten Index benötigt werden. Zu den Werten, die Sie angeben müssen, gehören auch die **Anwendungs-ID** und der **Authentifizierungsschlüssel**.

>[!Important]
> Berücksichtigen Sie bei der Entscheidung, eine AAD-Anwendung zur Authentifizierung anstelle einer verwalteten Identität zu verwenden, auch, dass die AAD-Anwendung von der kognitiven Azure-Suche nicht in Ihrem Namen verwaltet werden kann und Sie selbst die AAD-Anwendung verwalten müssen, beispielsweise die regelmäßige Rotation des Anwendungsauthentifizierungsschlüssels.
> Wenn Sie eine AAD-Anwendung oder den zugehörigen Authentifizierungsschlüssel ändern, müssen alle Indizes oder Synonymzuordnungen in der kognitiven Azure-Suche, die die Anwendung verwenden, zunächst für die Verwendung der neuen Anwendungs-ID oder des neuen Schlüssels geändert werden, **bevor** die vorherige Anwendung oder der zugehörige Authentifizierungsschlüssel geändert wird und bevor der Key Vault-Zugriff darauf widerrufen wird.
> Andernfalls kann der Index oder die Synonymzuordnung nicht mehr verwendet werden, da Sie die Inhalte nicht mehr entschlüsseln können, wenn der Schlüsselzugriff verloren geht.   

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie nicht mit der Azure-Sicherheitsarchitektur vertraut sind, finden Sie entsprechende Informationen in der [Dokumentation zur Azure-Sicherheit](https://docs.microsoft.com/azure/security/) und insbesondere in folgendem Artikel:

> [!div class="nextstepaction"]
> [Verschlüsselung ruhender Daten](https://docs.microsoft.com/azure/security/fundamentals/encryption-atrest)

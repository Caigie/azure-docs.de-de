---
title: Serverseitige Verschlüsselung von Azure Managed Disks – PowerShell
description: Azure Storage schützt Ihre Daten, indem der Dienst diese im Ruhezustand verschlüsselt, bevor diese auf Storage-Clustern gespeichert werden. Sie können von Microsoft verwaltete Schlüssel für die Verschlüsselung Ihrer verwalteten Datenträger nutzen, oder Sie können mit vom Kunden verwalteten Schlüsseln die Verschlüsselung mit Ihren eigenen Schlüsseln verwalten.
author: roygara
ms.date: 04/21/2020
ms.topic: conceptual
ms.author: rogarana
ms.service: virtual-machines-windows
ms.subservice: disks
ms.openlocfilehash: c3a73028350054d54c6714107bfdfa7ead3ee4a3
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85610433"
---
# <a name="server-side-encryption-of-azure-managed-disks"></a>Serverseitige Verschlüsselung von verwalteten Azure-Datenträgern

Verwaltete Azure-Datenträger verschlüsseln Daten standardmäßig automatisch, wenn die Daten in der Cloud gespeichert werden. Die serverseitige Verschlüsselung (Server-side Encryption, SSE) schützt Ihre Daten und unterstützt Sie beim Einhalten der Sicherheits- und Complianceanforderungen Ihrer Organisation.

Daten in verwalteten Azure-Datenträgern werden transparent mit 256-Bit-[AES-Verschlüsselung](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) – einer der stärksten verfügbaren Blockverschlüsselungen – FIPS 140-2-konform ver- und entschlüsselt. Weitere Informationen zu den kryptografischen Modulen, die verwalteten Azure-Datenträgern zugrunde liegen, finden Sie unter [Kryptografie-API: Die nächste Generation](https://docs.microsoft.com/windows/desktop/seccng/cng-portal).

Die Verschlüsselung wirkt sich nicht auf die Leistung verwalteter Datenträger aus, und es fallen keine zusätzlichen Kosten für die Verschlüsselung an. 

> [!NOTE]
> Temporäre Datenträger sind keine verwalteten Datenträger und werden nicht von SSE verschlüsselt. Weitere Informationen zu temporären Datenträgern finden Sie unter [Übersicht über verwaltete Datenträger: Datenträgerrollen](managed-disks-overview.md#disk-roles).

## <a name="about-encryption-key-management"></a>Informationen zur Verwaltung von Verschlüsselungsschlüsseln

Sie können von der Plattform verwaltete Schlüssel für die Verschlüsselung der verwalteten Datenträger verwenden oder die Verschlüsselung mit eigenen Schlüsseln verwalten. Wenn Sie die Verschlüsselung mit eigenen Schlüsseln verwalten möchten, können Sie einen *vom Kunden verwalteten Schlüssel* angeben, der zum Verschlüsseln und Entschlüsseln aller Daten in verwalteten Datenträgern verwendet werden soll. 

In den folgenden Abschnitten werden die einzelnen Optionen für die Schlüsselverwaltung ausführlicher beschrieben.

## <a name="platform-managed-keys"></a>Von der Plattform verwaltete Schlüssel

Verwaltete Datenträger verwenden standardmäßig von der Plattform verwaltete Verschlüsselungsschlüssel. Seit 10. Juni 2017 werden alle neuen verwalteten Datenträger, Momentaufnahmen, Images und neuen Daten, die auf vorhandene verwaltete Datenträger geschrieben wurden, im Ruhezustand automatisch mit von der Plattform verwalteten Schlüsseln verschlüsselt.

## <a name="customer-managed-keys"></a>Vom Kunden verwaltete Schlüssel

Sie können die Verschlüsselung auf der Ebene verwalteter Datenträger mit eigenen Schlüsseln verwalten. Die serverseitige Verschlüsselung für verwaltete Datenträger mit vom Kunden verwalteten Schlüsseln bietet eine integrierte Benutzerfunktionalität mit Azure Key Vault. Sie können entweder [ihre RSA-Schlüssel](../../key-vault/keys/hsm-protected-keys.md) in den Schlüsseltresor importieren oder neue RSA-Schlüssel in Azure Key Vault generieren. 

Die Verschlüsselung und Entschlüsselung von verwalteten Azure-Datenträgern erfolgt durch [Umschlagverschlüsselung](../../storage/common/storage-client-side-encryption.md#encryption-and-decryption-via-the-envelope-technique) vollständig transparent. Daten werden mithilfe eines [AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)-256 basierten Datenverschlüsselungsschlüssels (DEK) verschlüsselt, der wiederum mit Ihren Schlüsseln geschützt wird. Der Speicherdienst generiert Datenverschlüsselungsschlüssel und verschlüsselt sie mit vom Kunden verwalteten Schlüsseln unter Verwendung der RSA-Verschlüsselung. Mithilfe der Umschlagverschlüsselung können Sie Ihre Schlüssel gemäß Ihren Kompatibilitätsrichtlinien regelmäßig rotieren (ändern), ohne Ihre VMs zu beeinträchtigen. Wenn Sie Ihre Schlüssel rotieren, verschlüsselt der Speicherdienst die Datenverschlüsselungsschlüssel mit den neuen, vom Kunden verwalteten Schlüsseln neu. 

Sie müssen den Zugriff auf verwaltete Datenträger in Ihrem Schlüsseltresor gewähren, damit Sie Ihre Schlüssel zum Verschlüsseln und Entschlüsseln des DEK verwenden können. Dies ermöglicht eine umfassende Kontrolle über Ihre Daten und Schlüssel. Sie können Ihre Schlüssel jederzeit deaktivieren oder den Zugriff auf verwaltete Datenträger widerrufen. Sie können auch die Verwendung von Verschlüsselungsschlüsseln mithilfe der Azure Key Vault-Überwachung überwachen, um sicherzustellen, dass nur verwaltete Datenträger oder andere vertrauenswürdige Azure-Dienste auf Ihre Schlüssel zugreifen.

Für SSD Premium, SSD Standard und HDD Standard gilt Folgendes: Wenn Sie Ihren Schlüssel deaktivieren oder löschen, werden alle virtuellen Computer mit Datenträgern, die diesen Schlüssel verwenden, automatisch heruntergefahren. Danach können die virtuellen Computer nur verwendet werden, wenn der Schlüssel erneut aktiviert wird oder wenn Sie einen neuen Schlüssel zuweisen.

Wenn Sie bei Ultra-Datenträgern einen Schlüssel deaktivieren oder löschen, werden virtuelle Computer mit Ultra-Datenträgern, die den Schlüssel verwenden, nicht automatisch heruntergefahren. Nachdem die Zuordnung der virtuellen Computer aufgehoben wurde und die virtuellen Computer neu gestartet wurden, wird der Schlüssel nicht mehr von den Datenträgern verwendet, und die virtuellen Computer werden nicht wieder online geschaltet. Um die virtuellen Computer wieder online zu schalten, müssen Sie einen neuen Schlüssel zuweisen oder den vorhandenen Schlüssel aktivieren.

Das folgende Diagramm zeigt, wie verwaltete Datenträger Azure Active Directory und Azure Key Vault verwenden, um Anforderungen mit dem vom Kunden verwalteten Schlüssel zu senden:

![Workflow für verwaltete Datenträger und vom Kunden verwaltete Schlüssel. Ein Administrator erstellt eine Azure Key Vault-Instanz, dann einen Datenträgerverschlüsselungssatz und richtet den Datenträgerverschlüsselungssatz ein. Der Satz wird einem virtuellen Computer zugeordnet, sodass der Datenträger Azure AD zur Authentifizierung verwenden kann.](media/disk-storage-encryption/customer-managed-keys-sse-managed-disks-workflow.png)


In der folgenden Liste wird das Diagramm ausführlicher erläutert:

1. Ein Azure Key Vault-Administrator erstellt Schlüsseltresorressourcen.
1. Der Schlüsseltresoradministrator importiert die jeweiligen RSA-Schlüssel in Key Vault oder generiert neue RSA-Schlüssel in Key Vault.
1. Dieser Administrator erstellt eine Instanz der Datenträgerverschlüsselungssatz-Ressource und gibt eine Azure Key Vault-ID sowie eine Schlüssel-URL an. Der Datenträgerverschlüsselungssatz ist eine neue Ressource, die zur Vereinfachung der Schlüsselverwaltung für verwaltete Datenträger eingeführt wurde. 
1. Beim Erstellen eines Datenträgerverschlüsselungssatzes wird eine [vom System zugewiesene verwaltete Identität](../../active-directory/managed-identities-azure-resources/overview.md) in Azure Active Directory (AD) erstellt und mit dem Datenträgerverschlüsselungssatz verknüpft. 
1. Der Azure Key Vault-Administrator erteilt dann der verwalteten Identität Berechtigungen zum Durchführen von Vorgängen im Schlüsseltresor.
1. Ein VM-Benutzer erstellt Datenträger, indem er sie dem Datenträgerverschlüsselungssatz zuordnet. Der VM-Benutzer kann auch die serverseitige Verschlüsselung mit vom Kunden verwalteten Schlüsseln für vorhandene Ressourcen aktivieren, indem er diese dem Datenträgerverschlüsselungssatz zuordnet. 
1. Verwaltete Datenträger verwenden die verwaltete Identität, um Anforderungen an den Azure Key Vault zu senden.
1. Zum Lesen oder Schreiben von Daten senden verwaltete Datenträger Anforderungen an Azure Key Vault, um den Datenverschlüsselungsschlüssel zu verschlüsseln (wrap) und zu entschlüsseln (unwrap), um die Daten zu verschlüsseln und zu entschlüsseln. 

Informationen zum Widerrufen von Kunden verwalteter Schlüsseln finden Sie in den Artikeln zu [Azure Key Vault-PowerShell](https://docs.microsoft.com/powershell/module/azurerm.keyvault/) und zur [Azure Key Vault-CLI](https://docs.microsoft.com/cli/azure/keyvault). Durch das Widerrufen des Zugriffs wird der Zugriff auf alle Daten im Speicherkonto blockiert, da Azure Storage keinen Zugriff mehr auf den Verschlüsselungsschlüssel hat.

### <a name="supported-regions"></a>Unterstützte Regionen

[!INCLUDE [virtual-machines-disks-encryption-regions](../../../includes/virtual-machines-disks-encryption-regions.md)]

### <a name="restrictions"></a>Beschränkungen

Vorerst gelten für vom Kunden verwaltete Schlüssel die folgenden Einschränkungen:

- Wenn dieses Feature für Ihren Datenträger aktiviert ist, können Sie es nicht deaktivieren.
    Bei Bedarf müssen Sie [alle Daten auf einen anderen verwalteten Datenträger ohne kundenseitig verwaltete Schlüssel kopieren](disks-upload-vhd-to-managed-disk-powershell.md#copy-a-managed-disk).
- Es werden ausschließlich [Software- und HSM RSA-Schlüssel](../../key-vault/keys/about-keys.md) der Größe 2080 unterstützt, keine anderen Schlüssel oder Größen.
- Datenträger, die aus benutzerdefinierten Images erstellt werden, die mit serverseitiger Verschlüsselung und vom Kunden verwalteten Schlüsseln verschlüsselt wurden, müssen mit denselben vom Kunden verwalteten Schlüsseln verschlüsselt werden und sich im selben Abonnement befinden.
- Momentaufnahmen, die von Datenträgern erstellt werden, die mit serverseitiger Verschlüsselung und vom Kunden verwalteten Schlüsseln verschlüsselt wurden, müssen mit denselben vom Kunden verwalteten Schlüsseln verschlüsselt werden.
- Alle Ressourcen, die sich auf Ihre vom Kunden verwalteten Schlüssel (Azure Key Vaults, Datenträgerverschlüsselungssätze, VMs, Datenträger und Momentaufnahmen) beziehen, müssen sich im selben Abonnement und in derselben Region befinden.
- Mit vom Kunden verwalteten Schlüsseln verschlüsselte Datenträger, Momentaufnahmen und Images können nicht in ein anderes Abonnement verschoben werden.
- Verwaltete Datenträger, die mithilfe von serverseitiger Verschlüsselung und mit kundenseitig verwalteten Schlüsseln verschlüsselt werden, können nicht außerdem mit Azure Disk Encryption verschlüsselt werden und umgekehrt.
- Informationen über die Verwendung von kundenseitig verwalteter Schlüssel mit Shared Image Gallerys finden Sie unter [Vorschau: Verwenden von kundenseitig verwalteten Schlüsseln zum Verschlüsseln von Images](../image-version-encryption.md).

### <a name="powershell"></a>PowerShell

#### <a name="setting-up-your-azure-key-vault-and-diskencryptionset"></a>Einrichten von Azure Key Vault und DiskEncryptionSet

[!INCLUDE [virtual-machines-disks-encryption-create-key-vault-powershell](../../../includes/virtual-machines-disks-encryption-create-key-vault-powershell.md)]

#### <a name="create-a-vm-using-a-marketplace-image-encrypting-the-os-and-data-disks-with-customer-managed-keys"></a>Erstellen einer VM mit einem Marketplace-Image, Verschlüsseln der Datenträger für Betriebssystem und Daten mit vom Kunden verwalteten Schlüsseln

```powershell
$VMLocalAdminUser = "yourVMLocalAdminUserName"
$VMLocalAdminSecurePassword = ConvertTo-SecureString <password> -AsPlainText -Force
$LocationName = "westcentralus"
$ResourceGroupName = "yourResourceGroupName"
$ComputerName = "yourComputerName"
$VMName = "yourVMName"
$VMSize = "Standard_DS3_v2"
$diskEncryptionSetName="yourdiskEncryptionSetName"
    
$NetworkName = "yourNetworkName"
$NICName = "yourNICName"
$SubnetName = "yourSubnetName"
$SubnetAddressPrefix = "10.0.0.0/24"
$VnetAddressPrefix = "10.0.0.0/16"
    
$SingleSubnet = New-AzVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $SubnetAddressPrefix
$Vnet = New-AzVirtualNetwork -Name $NetworkName -ResourceGroupName $ResourceGroupName -Location $LocationName -AddressPrefix $VnetAddressPrefix -Subnet $SingleSubnet
$NIC = New-AzNetworkInterface -Name $NICName -ResourceGroupName $ResourceGroupName -Location $LocationName -SubnetId $Vnet.Subnets[0].Id
    
$Credential = New-Object System.Management.Automation.PSCredential ($VMLocalAdminUser, $VMLocalAdminSecurePassword);
    
$VirtualMachine = New-AzVMConfig -VMName $VMName -VMSize $VMSize
$VirtualMachine = Set-AzVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate
$VirtualMachine = Add-AzVMNetworkInterface -VM $VirtualMachine -Id $NIC.Id
$VirtualMachine = Set-AzVMSourceImage -VM $VirtualMachine -PublisherName 'MicrosoftWindowsServer' -Offer 'WindowsServer' -Skus '2012-R2-Datacenter' -Version latest

$diskEncryptionSet=Get-AzDiskEncryptionSet -ResourceGroupName $ResourceGroupName -Name $diskEncryptionSetName

$VirtualMachine = Set-AzVMOSDisk -VM $VirtualMachine -Name $($VMName +"_OSDisk") -DiskEncryptionSetId $diskEncryptionSet.Id -CreateOption FromImage

$VirtualMachine = Add-AzVMDataDisk -VM $VirtualMachine -Name $($VMName +"DataDisk1") -DiskSizeInGB 128 -StorageAccountType Premium_LRS -CreateOption Empty -Lun 0 -DiskEncryptionSetId $diskEncryptionSet.Id 
    
New-AzVM -ResourceGroupName $ResourceGroupName -Location $LocationName -VM $VirtualMachine -Verbose
```

#### <a name="create-an-empty-disk-encrypted-using-server-side-encryption-with-customer-managed-keys-and-attach-it-to-a-vm"></a>Erstellen eines leeren Datenträgers, der mit serverseitiger Verschlüsselung und vom Kunden verwalteten Schlüsseln verschlüsselt wird, und Anfügen an eine VM

```PowerShell
$vmName = "yourVMName"
$LocationName = "westcentralus"
$ResourceGroupName = "yourResourceGroupName"
$diskName = "yourDiskName"
$diskSKU = "Premium_LRS"
$diskSizeinGiB = 30
$diskLUN = 1
$diskEncryptionSetName="yourDiskEncryptionSetName"


$vm = Get-AzVM -Name $vmName -ResourceGroupName $ResourceGroupName 

$diskEncryptionSet=Get-AzDiskEncryptionSet -ResourceGroupName $ResourceGroupName -Name $diskEncryptionSetName

$vm = Add-AzVMDataDisk -VM $vm -Name $diskName -CreateOption Empty -DiskSizeInGB $diskSizeinGiB -StorageAccountType $diskSKU -Lun $diskLUN -DiskEncryptionSetId $diskEncryptionSet.Id 

Update-AzVM -ResourceGroupName $ResourceGroupName -VM $vm

```

#### <a name="encrypt-existing-managed-disks"></a>Verschlüsseln vorhandener verwalteter Datenträger 

Die vorhandenen Datenträger dürfen nicht an einen laufenden virtuellen Computer angefügt werden, damit Sie sie mithilfe des folgenden Skripts verschlüsseln können:

```PowerShell
$rgName = "yourResourceGroupName"
$diskName = "yourDiskName"
$diskEncryptionSetName = "yourDiskEncryptionSetName"
 
$diskEncryptionSet = Get-AzDiskEncryptionSet -ResourceGroupName $rgName -Name $diskEncryptionSetName
 
New-AzDiskUpdateConfig -EncryptionType "EncryptionAtRestWithCustomerKey" -DiskEncryptionSetId $diskEncryptionSet.Id | Update-AzDisk -ResourceGroupName $rgName -DiskName $diskName
```

#### <a name="create-a-virtual-machine-scale-set-using-a-marketplace-image-encrypting-the-os-and-data-disks-with-customer-managed-keys"></a>Erstellen einer VM-Skalierungsgruppe mit einem Marketplace-Image, Verschlüsseln der Datenträger für Betriebssystem und Daten mit vom Kunden verwalteten Schlüsseln

```PowerShell
$VMLocalAdminUser = "yourLocalAdminUser"
$VMLocalAdminSecurePassword = ConvertTo-SecureString Password@123 -AsPlainText -Force
$LocationName = "westcentralus"
$ResourceGroupName = "yourResourceGroupName"
$ComputerNamePrefix = "yourComputerNamePrefix"
$VMScaleSetName = "yourVMSSName"
$VMSize = "Standard_DS3_v2"
$diskEncryptionSetName="yourDiskEncryptionSetName"
    
$NetworkName = "yourVNETName"
$SubnetName = "yourSubnetName"
$SubnetAddressPrefix = "10.0.0.0/24"
$VnetAddressPrefix = "10.0.0.0/16"
    
$SingleSubnet = New-AzVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $SubnetAddressPrefix

$Vnet = New-AzVirtualNetwork -Name $NetworkName -ResourceGroupName $ResourceGroupName -Location $LocationName -AddressPrefix $VnetAddressPrefix -Subnet $SingleSubnet

$ipConfig = New-AzVmssIpConfig -Name "myIPConfig" -SubnetId $Vnet.Subnets[0].Id 

$VMSS = New-AzVmssConfig -Location $LocationName -SkuCapacity 2 -SkuName $VMSize -UpgradePolicyMode 'Automatic'

$VMSS = Add-AzVmssNetworkInterfaceConfiguration -Name "myVMSSNetworkConfig" -VirtualMachineScaleSet $VMSS -Primary $true -IpConfiguration $ipConfig

$diskEncryptionSet=Get-AzDiskEncryptionSet -ResourceGroupName $ResourceGroupName -Name $diskEncryptionSetName

# Enable encryption at rest with customer managed keys for OS disk by setting DiskEncryptionSetId property 

$VMSS = Set-AzVmssStorageProfile $VMSS -OsDiskCreateOption "FromImage" -DiskEncryptionSetId $diskEncryptionSet.Id -ImageReferenceOffer 'WindowsServer' -ImageReferenceSku '2012-R2-Datacenter' -ImageReferenceVersion latest -ImageReferencePublisher 'MicrosoftWindowsServer'

$VMSS = Set-AzVmssOsProfile $VMSS -ComputerNamePrefix $ComputerNamePrefix -AdminUsername $VMLocalAdminUser -AdminPassword $VMLocalAdminSecurePassword

# Add a data disk encrypted at rest with customer managed keys by setting DiskEncryptionSetId property 

$VMSS = Add-AzVmssDataDisk -VirtualMachineScaleSet $VMSS -CreateOption Empty -Lun 1 -DiskSizeGB 128 -StorageAccountType Premium_LRS -DiskEncryptionSetId $diskEncryptionSet.Id

$Credential = New-Object System.Management.Automation.PSCredential ($VMLocalAdminUser, $VMLocalAdminSecurePassword);

New-AzVmss -VirtualMachineScaleSet $VMSS -ResourceGroupName $ResourceGroupName -VMScaleSetName $VMScaleSetName
```

#### <a name="change-the-key-of-a-diskencryptionset-to-rotate-the-key-for-all-the-resources-referencing-the-diskencryptionset"></a>Ändern des Schlüssels eines DiskEncryptionSet, um den Schlüssel für alle Ressourcen, die auf das DiskEncryptionSet verweisen, zu rotieren

```PowerShell
$ResourceGroupName="yourResourceGroupName"
$keyVaultName="yourKeyVaultName"
$keyName="yourKeyName"
$diskEncryptionSetName="yourDiskEncryptionSetName"

$keyVault = Get-AzKeyVault -VaultName $keyVaultName -ResourceGroupName $ResourceGroupName

$keyVaultKey = Get-AzKeyVaultKey -VaultName $keyVaultName -Name $keyName

Update-AzDiskEncryptionSet -Name $diskEncryptionSetName -ResourceGroupName $ResourceGroupName -SourceVaultId $keyVault.ResourceId -KeyUrl $keyVaultKey.Id
```

#### <a name="find-the-status-of-server-side-encryption-of-a-disk"></a>Ermitteln des Status der serverseitigen Verschlüsselung eines Datenträgers

[!INCLUDE [virtual-machines-disks-encryption-status-powershell](../../../includes/virtual-machines-disks-encryption-status-powershell.md)]

> [!IMPORTANT]
> Von Kunden verwaltete Schlüssel basieren auf verwalteten Identitäten für Azure-Ressourcen, einem Feature von Azure Active Directory (Azure AD). Wenn Sie vom Kunden verwaltete Schlüssel konfigurieren, wird Ihren Ressourcen im Hintergrund automatisch eine verwaltete Identität zugewiesen. Wenn Sie anschließend das Abonnement, die Ressourcengruppe oder den verwalteten Datenträger von einem Azure AD-Verzeichnis in ein anderes Verzeichnis verschieben, wird die den verwalteten Datenträgern zugeordnete verwaltete Identität nicht an den neuen Mandanten übertragen, sodass vom Kunden verwaltete Schlüssel möglicherweise nicht mehr funktionieren. Weitere Informationen finden Sie unter [Übertragen eines Abonnements zwischen Azure AD-Verzeichnissen](../../active-directory/managed-identities-azure-resources/known-issues.md#transferring-a-subscription-between-azure-ad-directories).

[!INCLUDE [virtual-machines-disks-encryption-portal](../../../includes/virtual-machines-disks-encryption-portal.md)]

> [!IMPORTANT]
> Von Kunden verwaltete Schlüssel basieren auf verwalteten Identitäten für Azure-Ressourcen, einem Feature von Azure Active Directory (Azure AD). Wenn Sie vom Kunden verwaltete Schlüssel konfigurieren, wird Ihren Ressourcen im Hintergrund automatisch eine verwaltete Identität zugewiesen. Wenn Sie anschließend das Abonnement, die Ressourcengruppe oder den verwalteten Datenträger von einem Azure AD-Verzeichnis in ein anderes Verzeichnis verschieben, wird die den verwalteten Datenträgern zugeordnete verwaltete Identität nicht an den neuen Mandanten übertragen, sodass vom Kunden verwaltete Schlüssel möglicherweise nicht mehr funktionieren. Weitere Informationen finden Sie unter [Übertragen eines Abonnements zwischen Azure AD-Verzeichnissen](../../active-directory/managed-identities-azure-resources/known-issues.md#transferring-a-subscription-between-azure-ad-directories).

## <a name="server-side-encryption-versus-azure-disk-encryption"></a>Vergleich: Serverseitige Verschlüsselung und Azure-Datenträgerverschlüsselung

[Azure Disk Encryption](../../security/fundamentals/azure-disk-encryption-vms-vmss.md) nutzt das [BitLocker](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-overview)-Feature von Windows zum Verschlüsseln verwalteter Datenträger mit vom Kunden verwalteten Schlüsseln innerhalb der Gast-VM.  Die serverseitige Verschlüsselung mit vom Kunden verwalteten Schlüsseln bewirkt eine ADE-Verbesserung, indem Sie beliebige Betriebssystemtypen und Images für Ihre VMs verwenden können, indem Daten im Speicherdienst verschlüsselt werden.

## <a name="next-steps"></a>Nächste Schritte

- [Untersuchen der Azure Resource Manager-Vorlagen zum Erstellen verschlüsselter Datenträger mit vom Kunden verwalteten Schlüsseln](https://github.com/ramankumarlive/manageddiskscmkpreview)
- [Was ist der Azure-Schlüsseltresor?](../../key-vault/general/overview.md)
- [Replizieren von Computern mit Datenträgern, die für kundenseitig verwaltete Schlüssel aktiviert sind](../../site-recovery/azure-to-azure-how-to-enable-replication-cmk-disks.md)
- [Einrichten der Notfallwiederherstellung von virtuellen VMware-Computern in Azure mithilfe von PowerShell](../../site-recovery/vmware-azure-disaster-recovery-powershell.md#replicate-vmware-vms)
- [Einrichten der Notfallwiederherstellung in Azure für Hyper-V-VMs mithilfe von PowerShell und Azure Resource Manager](../../site-recovery/hyper-v-azure-powershell-resource-manager.md#step-7-enable-vm-protection)

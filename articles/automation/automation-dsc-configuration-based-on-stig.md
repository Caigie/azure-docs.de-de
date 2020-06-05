---
title: Konfigurieren von Daten basierend auf STIG in Azure Automation State Configuration
description: In diesem Artikel erfahren Sie, wie Sie Daten basierend auf STIG für Azure Automation State Configuration konfigurieren.
keywords: DSC,PowerShell,Konfiguration,Setup,Einrichtung
services: automation
ms.service: automation
ms.subservice: dsc
author: mgreenegit
ms.author: migreene
ms.date: 08/08/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: d1b05f9e77d3530f3e883aa3f9d98de09c8f54c2
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2020
ms.locfileid: "83836990"
---
# <a name="configure-data-based-on-stig"></a>Konfigurieren von Daten basierend auf STIG

> Gilt für: Windows PowerShell 5.1

Das erstmalige Erstellen von Konfigurationsinhalten kann eine Herausforderung darstellen.
In vielen Fällen besteht das Ziel darin, die Konfiguration von Servern gemäß gewissen Grundwerten („Baseline“) zu automatisieren, die hoffentlich eine Branchenempfehlung erfüllen.

> [!NOTE]
> Dieser Artikel bezieht sich auf eine Lösung, die von der Open Source-Community verwaltet wird.
> Support ist nur in Form der GitHub-Zusammenarbeit erhältlich, nicht von Microsoft.

## <a name="community-project-powerstig"></a>Communityprojekt: PowerSTIG

Ein Communityprojekt namens [PowerSTIG](https://github.com/microsoft/powerstig) zielt darauf ab, dieses Problem zu beheben, indem DSC-Inhalte basierend auf [öffentlichen Informationen](https://public.cyber.mil/stigs/) über STIG (Security Technical Implementation Guide, Sicherheitstechnisches Implementierungshandbuch) generiert werden.

Die Handhabung von Baselines ist komplizierter, als es klingt.
Viele Organisationen müssen [Ausnahmen von Regeln dokumentieren](https://github.com/microsoft/powerstig#powerstigdata) und diese Daten skaliert verwalten.
PowerSTIG löst dieses Problem, indem es [zusammengesetzte Ressourcen](https://github.com/microsoft/powerstig#powerstigdsc) bereitstellt, die jeden Bereich der Konfiguration abdecken, anstatt zu versuchen, den gesamten Bereich von Einstellungen in einer großen Datei zu erfassen.

Sobald die Konfigurationen einmal generiert sind, können Sie die [DSC-Konfigurationsskripts](/powershell/scripting/dsc/configurations/configurations) verwenden, um MOF-Dateien zu generieren und die [MOF-Dateien in Azure Automation hochzuladen](/azure/automation/tutorial-configure-servers-desired-state#create-and-upload-a-configuration-to-azure-automation).
Danach registrieren Sie Ihre Server entweder von [lokal](/azure/automation/automation-dsc-onboarding#onboarding-physicalvirtual-windows-machines-on-premises-or-in-a-cloud-other-than-azure-including-aws-ec2-instances) oder [in Azure](/azure/automation/automation-dsc-onboarding#onboarding-azure-vms) um Konfigurationen abzurufen.

Um PowerSTIG zu testen, besuchen Sie den [PowerShell-Katalog](https://www.powershellgallery.com), und laden Sie die Lösung herunter, oder klicken Sie auf „Projektwebsite“, um die [Dokumentation](https://github.com/microsoft/powerstig) anzuzeigen.

## <a name="next-steps"></a>Nächste Schritte

- Eine Einführung in PowerShell DSC finden Sie unter [Windows PowerShell DSC – Übersicht](/powershell/scripting/dsc/overview/overview).
- Erfahren Sie mehr über PowerShell DSC-Ressourcen in [DSC-Ressourcen](/powershell/scripting/dsc/resources/resources).
- Weitere Informationen zur Konfiguration des lokalen Konfigurations-Managers finden Sie unter [Konfigurieren des lokalen Konfigurations-Managers](/powershell/scripting/dsc/managing-nodes/metaconfig).

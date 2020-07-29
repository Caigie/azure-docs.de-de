---
title: Verwenden des aggregierten Azure HPC Cache-Namespace
description: Planen des virtuellen Namespace für Azure HPC Cache
author: ekpgh
ms.service: hpc-cache
ms.topic: how-to
ms.date: 10/30/2019
ms.author: v-erkel
ms.openlocfilehash: c16d2f9e9c94603361d9a096f33d559105f2d28d
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86497028"
---
# <a name="plan-the-aggregated-namespace"></a>Planen des aggregierten Namespace

Azure HPC Cache ermöglicht Clients den Zugriff auf eine Vielzahl von Speichersystemen über einen virtuellen Namespace, der die Details des Back-End-Speichersystems verbirgt.

Beim Hinzufügen eines Speicherziels legen Sie den clientseitigen Dateipfad fest. Clientcomputer binden diesen Dateipfad ein und können Dateileseanforderungen an den Cache stellen, anstatt das Speichersystem direkt einzubinden.

Da Azure HPC Cache dieses virtuelle Dateisystem verwaltet, können Sie das Speicherziel ändern, ohne den clientseitigen Pfad zu ändern. Beispielsweise können Sie ein Hardware-Speichersystem durch Cloudspeicher ersetzen, ohne die clientseitigen Prozeduren erneut generieren zu müssen.

## <a name="aggregated-namespace-example"></a>Beispiel zum aggregierten Namespace

Planen Sie Ihren aggregierten Namespace so, dass die Clientcomputer komfortabel an die benötigten Informationen gelangen und Administratoren und Workflowtechniker die Pfade leicht unterscheiden können.

Ziehen Sie beispielsweise ein System in Erwägung, in dem eine Instanz von Azure HPC Cache zum Verarbeiten der im Azure-Blob gespeicherten Daten verwendet wird. Für die Analyse sind Vorlagendateien erforderlich, die in einem lokalen Rechenzentrum gespeichert sind.

Die Vorlagendaten sind in einem Rechenzentrum gespeichert, und die für diesen Auftrag erforderlichen Informationen sind in diesen Unterverzeichnissen gespeichert:

* */goldline/templates/acme2017/sku798*
* */goldline/templates/acme2017/sku980*

Das Speichersystem des Rechenzentrums macht diese Exporte verfügbar:

* */*
* */goldline*
* */goldline/templates*

Die zu analysierenden Daten wurden mithilfe des [CLFSLoad-Hilfsprogramms](hpc-cache-ingest.md#pre-load-data-in-blob-storage-with-clfsload) in einen Azure-Blobspeichercontainer mit dem Namen „sourcecollection“ kopiert.

Um den komfortablen Zugriff über den Cache zu ermöglichen, sollten Sie das Erstellen von Speicherzielen mit diesen virtuellen Namespacepfaden in Erwägung ziehen:

| Back-End-Speichersystem <br/> (NFS-Dateipfad oder Blobcontainer) | Pfad des virtuellen Namespace |
|-----------------------------------------|------------------------|
| /goldline/templates/acme2017/sku798     | /templates/sku798      |
| /goldline/templates/acme2017/sku980     | /templates/sku980      |
| sourcecollection                        | /source/               |

Ein NFS-Speicherziel kann mehrere virtuelle Namespacepfade aufweisen, sofern jeder von ihnen auf einen eindeutigen Exportpfad verweist.

Da es sich bei den NFS-Quellpfaden um Unterverzeichnisse des gleichen Exports handelt, müssen Sie für das gleiche Speicherziel mehrere Namespacepfade definieren.

| Hostname des Speicherziels  | NFS-Exportpfad     | Unterverzeichnispfad | Namespacepfad    |
|--------------------------|---------------------|-------------------|-------------------|
| *IP-Adresse oder Hostname* | /goldline/templates | acme2017/sku798   | /templates/sku798 |
| *IP-Adresse oder Hostname* | /goldline/templates | acme2017/sku980   | /templates/sku980 |

Eine Clientanwendung kann den Cache einbinden und komfortabel auf die Dateipfade ``/source``, ``/templates/sku798`` und ``/templates/sku980`` des aggregierten Namespace zugreifen.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich dafür entschieden haben, wie Sie Ihr virtuelles Dateisystem einrichten möchten, [erstellen Sie Speicherziele](hpc-cache-add-storage.md), um Ihren Back-End-Speicher Ihren clientseitigen virtuellen Dateipfaden zuzuordnen.

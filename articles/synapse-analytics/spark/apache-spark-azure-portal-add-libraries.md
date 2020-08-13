---
title: Hinzufügen und Verwalten von Bibliotheken für Apache Spark
description: Erfahren Sie, wie Sie Bibliotheken hinzufügen und verwalten, die von Apache Spark in Azure Synapse Analytics verwendet werden.
services: synapse-analytics
author: euangMS
ms.service: synapse-analytics
ms.topic: conceptual
ms.date: 04/15/2020
ms.author: euang
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: c0d34d80df77b5c6fcdefc39b3bc3b1619a93705
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2020
ms.locfileid: "87496252"
---
# <a name="add-and-manage-libraries-for-apache-spark-in-azure-synapse-analytics"></a>Hinzufügen und Verwalten von Bibliotheken für Apache Spark in Azure Synapse Analytics

Apache Spark basiert bei der Bereitstellung seiner Funktionen auf einer Vielzahl von Bibliotheken. Diese Bibliotheken können durch zusätzliche Bibliotheken oder durch aktualisierte Versionen älterer Versionen erweitert oder ersetzt werden.

Python-Pakete können auf Spark-Poolebene (Vorschau) hinzugefügt werden, und JAR-basierte Pakete können auf Spark-Auftragsdefinitionsebene hinzugefügt werden.

## <a name="add-or-update-python-libraries"></a>Hinzufügen oder Aktualisieren von Python-Bibliotheken

Apache Spark in Azure Synapse Analytics verfügt über eine vollständige Anacondas-Installation und zusätzliche Bibliotheken. Die vollständige Liste der Bibliotheken finden Sie unter [Apache Spark-Versionsunterstützung](apache-spark-version-support.md).

Wenn eine Spark-Instanz gestartet wird, wird eine neue virtuelle Umgebung erstellt, die diese Installation als Basis verwendet. Zusätzlich kann eine *requirements.txt*-Datei (Ausgabe des `pip freeze`-Befehls) zum verwendet werden, um die virtuelle Umgebung zu aktualisieren. Die in dieser Datei für Installation oder Upgrade aufgeführten Pakete werden zum Zeitpunkt des Clusterstarts von PyPi heruntergeladen. Diese Anforderungsdatei wird jedes Mal verwendet, wenn eine Spark-Instanz aus diesem Spark-Pool erstellt wird.

> [!IMPORTANT]
>
> - Wenn das Paket, das Sie installieren, groß ist oder seine Installation lange dauert, wirkt sich dies auf die Startzeit der Spark-Instanz aus.
> - Pakete, die zur Installationszeit Compilerunterstützung erfordern, z. B. GCC, werden nicht unterstützt.
> - Pakete können nicht herabgestuft, sondern nur hinzugefügt oder aktualisiert werden.

### <a name="requirements-format"></a>Anforderungsformat

Der folgende Codeausschnitt zeigt das Format für die Anforderungsdatei. Der Name des PyPi-Pakets wird zusammen mit einer exakten Version aufgeführt. Diese Datei hält das Format ein, das in der Referenzdokumentation zu [pip freeze](https://pip.pypa.io/en/stable/reference/pip_freeze/) beschrieben wird. Dieses Beispiel verwendet eine bestimmte Version. 

```
absl-py==0.7.0
adal==1.2.1
alabaster==0.7.10
```

### <a name="python-library-user-interface"></a>Benutzeroberfläche der Python-Bibliothek

Die Benutzeroberfläche zum Hinzufügen von Bibliotheken befindet sich auf der Registerkarte **Zusätzliche Einstellungen** der Seite **Apache Spark-Pool erstellen** im Azure-Portal.

Laden Sie die Umgebungskonfigurationsdatei mithilfe der Dateiauswahl im Abschnitt **Pakete** auf der Seite hoch.

![Hinzufügen von Python-Bibliotheken](./media/apache-spark-azure-portal-add-libraries/add-python-libraries.png "Hinzufügen von Python-Bibliotheken")

### <a name="verify-installed-libraries"></a>Überprüfen von installierten Bibliotheken

Führen Sie den folgenden Code aus, um zu überprüfen, ob die richtigen Versionen der richtigen Bibliotheken installiert sind

```python
import pip #needed to use the pip functions
for i in pip.get_installed_distributions(local_only=True):
    print(i)
```

## <a name="next-steps"></a>Nächste Schritte

- [Azure Synapse Analytics](https://docs.microsoft.com/azure/synapse-analytics)
- [Apache Spark-Dokumentation](https://spark.apache.org/docs/2.4.4/)

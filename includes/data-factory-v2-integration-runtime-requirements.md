---
author: linda33wj
ms.service: data-factory
ms.topic: include
ms.date: 08/12/2019
ms.author: jingwang
ms.openlocfilehash: 2e90d218aa6dc90746ba0e928fb3393f0bdb5e5a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "68966359"
---
<!--
    Separate the generic requirement on Self-hosted Integration Runtime set-up from connector articles.
-->
Wurde Ihr Datenspeicher mit einer der folgenden Methoden konfiguriert, müssen Sie eine [selbstgehostete Integration Runtime](../articles/data-factory/create-self-hosted-integration-runtime.md) einrichten, um eine Verbindung mit diesem Datenspeicher herstellen zu können:

- Der Datenspeicher befindet sich in einem lokalen Netzwerk, in Azure Virtual Network oder innerhalb der virtuellen privaten Amazon-Cloud.
- Der Datenspeicher ist ein verwalteter Clouddatendienst, bei dem der Zugriff auf IP-Adressen beschränkt ist, die in der Whiteliste der Firewallregeln enthalten sind.

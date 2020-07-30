---
title: Automatisieren der Dienstregistrierung und -ermittlung
description: Erfahren Sie, wie Sie die Dienstermittlung und -registrierung mithilfe der Spring Cloud-Dienstregistrierung automatisieren.
author: bmitchell287
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 10/05/2019
ms.author: brendm
ms.custom: devx-track-java
ms.openlocfilehash: c3e26b157630df6004292c93a0a0a47307d5949a
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87071025"
---
# <a name="discover-and-register-your-spring-cloud-services"></a>Ermitteln und Registrieren Ihrer Spring Cloud-Dienste

Die Dienstermittlung ist eine wichtige Voraussetzung für eine auf Microservices basierende Architektur.  Das manuelle Konfigurieren der einzelnen Clients ist zeitaufwendig und kann menschliche Fehler verursachen.  Dieses Problem wird durch die Azure Spring Cloud-Dienstregistrierung gelöst.  Nach der Konfiguration steuert ein Dienstregistrierungsserver die Dienstregistrierung und -ermittlung für die Microservices Ihrer Anwendung. Der Dienstregistrierungsserver verwaltet eine Registrierung der bereitgestellten Microservices, aktiviert den clientseitigen Lastenausgleich und entkoppelt die Dienstanbieter von den Clients, ohne dafür DNS zu benötigen.

## <a name="register-your-application-using-spring-cloud-service-registry"></a>Registrieren Ihrer Anwendung mithilfe der Spring Cloud-Dienstregistrierung

Bevor Ihre Anwendung die Dienstregistrierung und -ermittlung mithilfe der Spring Cloud-Dienstregistrierung verwalten kann, müssen mehrere Abhängigkeiten in der Datei *pom.xml* der Anwendung hinzugefügt werden.
Fügen Sie in der Datei *pom.xml* Abhängigkeiten für *spring-cloud-starter-netflix-eureka-client* und *spring-cloud-starter-azure-spring-cloud-client* hinzu.

```xml
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>spring-cloud-starter-azure-spring-cloud-client</artifactId>
        <version>2.1.0</version>
    </dependency>
```

## <a name="update-the-top-level-class"></a>Aktualisieren der Klasse der obersten Ebene

Abschließend fügen Sie der Klasse auf oberster Ebene Ihrer Anwendung eine Anmerkung hinzu.

 ```java
    package foo.bar;

    import org.springframework.boot.SpringApplication;
    import org.springframework.cloud.client.SpringCloudApplication;

    @SpringCloudApplication
    public class DemoApplication {
        public static void main(String... args) {
            SpringApplication.run(DemoApplication.class, args);
        }
    }
 ```

Der Serverendpunkt der Spring Cloud-Dienstregistrierung wird als Umgebungsvariable in Ihre Anwendung eingefügt.  Microservices können sich damit selbst beim Dienstregistrierungsserver registrieren und andere abhängige Microservices ermitteln.

Beachten Sie, dass es einige Minuten dauern kann, bis die Änderungen vom Server an alle Microservices weitergegeben wurden.

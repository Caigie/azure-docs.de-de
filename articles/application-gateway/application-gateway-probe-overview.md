---
title: Übersicht über die Systemüberwachung für Azure Application Gateway
description: Azure Application Gateway überwacht die Integrität aller Ressourcen in seinem Back-End-Pool und entfernt automatisch alle als fehlerhaft geltenden Ressourcen aus dem Pool.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 02/20/2020
ms.author: victorh
ms.openlocfilehash: e1afc389508eb75313d046b759bcc9c03a50daad
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "83648406"
---
# <a name="application-gateway-health-monitoring-overview"></a>Systemüberwachung des Application Gateways – Übersicht

Azure Application Gateway überwacht standardmäßig die Integrität aller Ressourcen in seinem Back-End-Pool und entfernt automatisch alle als fehlerhaft geltenden Ressourcen aus dem Pool. Application Gateway überwacht die fehlerhaften Instanzen weiterhin und fügt sie dem fehlerfreien Back-End-Pool hinzu, sobald sie verfügbar sind und auf Zustandsüberprüfungen reagieren. Application Gateway sendet die Integritätstests über denselben Port, der in den HTTP-Einstellungen des Back-Ends festgelegt wurde. Durch diese Konfiguration wird sichergestellt, dass derselbe Port getestet wird, den der Kunde für die Verbindung mit dem Back-End verwenden würde. 

Die Quell-IP-Adresse, die Application Gateway für Integritätstests verwendet, hängt vom Back-End-Pool ab:
 
- Wenn der Back-End-Pool ein öffentlicher Endpunkt ist, ist die Quelladresse die öffentliche IP-Adresse des Application Gateway-Front-Ends.
- Wenn der Back-End-Pool ein privater Endpunkt ist, stammt die Quell-IP-Adresse aus dem privaten IP-Adressbereich des Application Gateway-Subnetzes.


![Beispiel für einen Application Gateway-Test][1]

Zusätzlich zur Nutzung der standardmäßigen Überwachung der Integritätsüberprüfung können Sie die Integritätsüberprüfung auch an die Anforderungen Ihrer Anwendung anpassen. In diesem Artikel werden sowohl standardmäßige als auch benutzerdefinierte Integritätstests behandelt.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="default-health-probe"></a>Standardmäßige Integritätsüberprüfung

Ein Anwendungsgateway konfiguriert automatisch eine standardmäßige Integritätsüberprüfung, wenn Sie keine benutzerdefinierte Überprüfungskonfiguration einrichten. Das Verhalten der Überwachung funktioniert durch das Erstellen einer HTTP-Anforderung an die für den Back-End-Pool konfigurierten IP-Adressen. Bei Standardüberprüfungen wird auch HTTPS für den Integritätstest der Back-Ends verwendet, wenn die HTTP-Einstellungen des Back-Ends für HTTPS konfiguriert sind.

Beispiel: Sie konfigurieren Ihr Application Gateway für die Verwendung der Back-End-Server A, B und C zum Empfang von HTTP-Netzwerkdatenverkehr an Port 80. Die standardmäßige Integritätsüberwachung testet die drei Server alle 30 Sekunden auf eine fehlerfreie HTTP-Antwort. Eine fehlerfreie HTTP-Antwort weist einen [Statuscode](https://msdn.microsoft.com/library/aa287675.aspx) zwischen 200 und 399 auf.

Wenn die Standardüberprüfung für Server A fehlschlägt, entfernt das Application Gateway sie aus dem Back-End-Pool, und der Netzwerkdatenverkehr an diesen Server wird angehalten. Die Standardüberprüfung führt weiterhin alle 30 Sekunden eine Überprüfung für Server A aus. Wenn Server A erfolgreich auf eine Anforderung einer standardmäßigen Integritätsüberprüfung antwortet, wird er dem Back-End-Pool wieder als fehlerfrei hinzugefügt, und der Datenverkehr an den Server startet erneut.

### <a name="probe-matching"></a>Testabgleich

Standardmäßig gilt eine HTTP(S)-Antwort mit einem Statuscode zwischen 200 und 399 als fehlerfrei. Benutzerdefinierte Integritätstests unterstützen außerdem zwei Abgleichskriterien. Abgleichskriterien können verwendet werden, um optional die Standardinterpretation einer fehlerfreien Antwort zu ändern.

Abgleichskriterien: 

- **Abgleich des Statuscodes der HTTP-Antwort**: Testabgleichskriterium zum Akzeptieren der vom Benutzer angegebenen HTTP-Antwortcodes oder -Antwortcodebereiche. Einzelne kommagetrennte Antwortstatuscodes und ein Bereich von Statuscodes werden unterstützt.
- **Abgleich des HTTP-Antworttexts**: Testabgleichskriterium, das den HTTP-Antworttext heranzieht und ihn mit einer vom Benutzer angegebenen Zeichenfolge abgleicht. Beim Abgleich wird lediglich auf das Vorhandensein der vom Benutzer angegebenen Zeichenfolge im Antworttext geachtet. Es findet kein Abgleich mit einem vollständigen regulären Ausdruck statt.

Abgleichskriterien können mithilfe des Cmdlets `New-AzApplicationGatewayProbeHealthResponseMatch` angegeben werden.

Beispiel:

```azurepowershell
$match = New-AzApplicationGatewayProbeHealthResponseMatch -StatusCode 200-399
$match = New-AzApplicationGatewayProbeHealthResponseMatch -Body "Healthy"
```
Nachdem Sie die Abgleichskriterien angegeben haben, können sie der Testkonfiguration mit einem `-Match`-Parameter in PowerShell angefügt werden.

### <a name="default-health-probe-settings"></a>Einstellungen für die standardmäßige Integritätsüberprüfung

| Überprüfungseigenschaft | Wert | BESCHREIBUNG |
| --- | --- | --- |
| Überprüfungs-URL |http://127.0.0.1:\<port\>/ |URL-Pfad |
| Intervall |30 |Wartezeitraum in Sekunden, bevor der nächste Integritätstest gesendet wird.|
| Zeitüberschreitung |30 |Gibt in Sekunden an, wie lange das Anwendungsgateway auf eine Testantwort wartet, bevor der Test als fehlerhaft gekennzeichnet wird. Wenn ein Test als fehlerfrei zurückgegeben wird, wird das entsprechende Back-End sofort als fehlerfrei gekennzeichnet.|
| Fehlerhafter Schwellenwert |3 |Steuert, wie viele Tests gesendet werden sollen, falls beim regulären Integritätstest ein Fehler auftritt. Diese zusätzlichen Integritätstests werden in kurzen Abständen gesendet, um die Back-End-Integrität schnell zu ermitteln und nicht auf das Testintervall zu warten. Dieses Verhalten weist nur die v1-SKU auf. Bei der v2-SKU warten die Integritätstests auf das Intervall. Der Back-End-Server wird als außer Betrieb markiert, nachdem die Anzahl der aufeinanderfolgenden fehlgeschlagenen Überprüfungen den fehlerhaften Schwellenwert erreicht. |

> [!NOTE]
> Dieser Port stimmt mit dem Port in den HTTP-Einstellungen des Back-Ends überein.

Der Standardtest untersucht nur „http:\//127.0.0.1:\<port\>“, um den Integritätsstatus zu bestimmen. Wenn Sie die Integritätsüberprüfung für eine benutzerdefinierte URL konfigurieren oder andere Einstellungen ändern möchten, müssen Sie benutzerdefinierte Überprüfungen verwenden. Weitere Informationen zu HTTP-Tests finden Sie unter [Übersicht über TLS-Beendigung und End-to-End-TLS mit Application Gateway](ssl-overview.md#for-probe-traffic).

### <a name="probe-intervals"></a>Testintervalle

Alle Application Gateway-Instanzen testen das Back-End unabhängig voneinander. Für jede Application Gateway-Instanz gilt die gleiche Testkonfiguration. Wenn die Testkonfiguration beispielsweise vorgibt, dass alle 30 Sekunden Integritätstests gesendet werden sollen, und das Anwendungsgateway über zwei Instanzen verfügt, senden beide Instanzen den Integritätstest alle 30 Sekunden.

Falls mehrere Listener vorhanden sind, testen die einzelnen Listener das Back-End unabhängig voneinander. Wenn beispielsweise zwei Listener auf denselben Back-End-Pool über zwei unterschiedliche Ports verweisen (über zwei unterschiedliche Back-End-HTTP-Einstellungen konfiguriert), testen die einzelnen Listener dasselbe Back-End unabhängig voneinander. In diesem Fall sind zwei Tests von jeder Anwendungsgatewayinstanz für die beiden Listener vorhanden. Wenn bei diesem Szenario zwei Instanzen des Anwendungsgateways vorhanden sind, werden für den virtuellen Back-End-Computer vier Tests pro konfiguriertem Testintervall durchgeführt.

## <a name="custom-health-probe"></a>Benutzerdefinierte Integritätsüberprüfung

Benutzerdefinierte Überprüfungen ermöglichen Ihnen eine präzisere Kontrolle über die Überwachung des Systemzustands. Bei Verwendung von benutzerdefinierten Überprüfungen können Sie das Überprüfungsintervall, die URL und den zu überprüfenden Pfad konfigurieren und festlegen, wie viele fehlerhafte Antworten akzeptiert werden, bevor die Back-End-Pool-Instanz als fehlerhaft gekennzeichnet wird.

### <a name="custom-health-probe-settings"></a>Einstellungen für die benutzerdefinierte Integritätsüberprüfung

Die folgende Tabelle enthält Definitionen der Eigenschaften eines benutzerdefinierten Integritätstests.

| Überprüfungseigenschaft | BESCHREIBUNG |
| --- | --- |
| Name |Name der Überprüfung. Dieser Name wird verwendet, um in den Back-End-HTTP-Einstellungen auf die Überprüfung zu verweisen. |
| Protocol |Das zum Senden der Überprüfung verwendete Protokoll. Für die Überprüfung wird das in den HTTP-Einstellungen des Back-Ends festgelegte Protokoll verwendet. |
| Host |Hostname zum Senden der Überprüfung Nur relevant, wenn in Application Gateway mehrere Standorte konfiguriert sind. Andernfalls verwenden Sie 127.0.0.1. Dieser Wert entspricht nicht dem Hostnamen des virtuellen Computers. |
| `Path` |Relativer Pfad der Überprüfung. Der gültige Pfad beginnt mit „/“. |
| Intervall |Überprüfungsintervall in Sekunden Dieser Wert ist das Zeitintervall zwischen zwei aufeinanderfolgenden Überprüfungen. |
| Zeitüberschreitung |Zeitüberschreitung der Überprüfung in Sekunden. Die Überprüfung wird als fehlerhaft markiert, wenn innerhalb des Zeitraums für die Zeitüberschreitung keine gültige Antwort empfangen wird.  |
| Fehlerhafter Schwellenwert |Anzahl der Wiederholungsversuche der Überprüfung Der Back-End-Server wird als außer Betrieb markiert, nachdem die Anzahl der aufeinanderfolgenden fehlgeschlagenen Überprüfungen den fehlerhaften Schwellenwert erreicht. |

> [!IMPORTANT]
> Falls Application Gateway für einen einzelnen Standort konfiguriert ist, muss der Hostname standardmäßig als „127.0.0.1“ angegeben werden, sofern in der benutzerdefinierten Überprüfung nichts anderes konfiguriert ist.
> Als Verweis wird ein Test an „\<protocol\>://\<host\>:\<port\>\<path\>“ gesendet. Dieser Port ist immer der gleiche Port, der in den HTTP-Einstellungen des Back-Ends definiert ist.

## <a name="nsg-considerations"></a>NSG-Aspekte

Sie müssen eingehenden Internetdatenverkehr über die TCP-Ports 65503–65534 (für die Application Gateway v1-SKU) und über die TCP-Ports 65200–65535 (für die v2-SKU) mit dem Zielsubnetz **Beliebig** und der Quelle **GatewayManager**-Diensttag zulassen. Dieser Portbereich ist für die Kommunikation mit der Azure-Infrastruktur erforderlich.

Außerdem kann die Internetkonnektivität in ausgehender Richtung nicht blockiert werden, und eingehender Datenverkehr vom **AzureLoadBalancer**-Tag muss zugelassen werden.

Weitere Informationen finden Sie unter [Application Gateway-Konfiguration: Übersicht](configuration-overview.md#network-security-groups-on-the-application-gateway-subnet).

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie sich mit der Systemüberwachung von Application Gateway vertraut gemacht haben, können Sie einen [benutzerdefinierten Integritätstest](application-gateway-create-probe-portal.md) im Azure-Portal oder einen [benutzerdefinierten Integritätstest](application-gateway-create-probe-ps.md) mit PowerShell und dem Azure Resource Manager-Bereitstellungsmodell konfigurieren.

[1]: ./media/application-gateway-probe-overview/appgatewayprobe.png

---
title: Häufig gestellte Fragen zu Azure Cache for Redis
description: In diesem Artikel erhalten Sie Antworten auf häufig gestellte Fragen sowie Informationen zu Mustern und Best Practices für Azure Cache for Redis.
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.topic: conceptual
ms.date: 04/29/2019
ms.openlocfilehash: 9a6ee4f5b18c6747796f33bc433d1d40982205a3
ms.sourcegitcommit: ec682dcc0a67eabe4bfe242fce4a7019f0a8c405
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2020
ms.locfileid: "86185006"
---
# <a name="azure-cache-for-redis-faq"></a>Häufig gestellte Fragen zu Azure Cache for Redis
In diesem Artikel erhalten Sie Antworten auf häufig gestellte Fragen sowie Informationen zu Mustern und Best Practices für Azure Cache for Redis.

## <a name="what-if-my-question-isnt-answered-here"></a>Was kann ich tun, wenn meine Frage hier nicht beantwortet wird?
Wenn Ihre Frage hier nicht aufgeführt wird, informieren Sie uns, und wir helfen Ihnen dabei, eine Antwort zu finden.

* Sie können am Ende dieser FAQ in den Kommentaren eine Frage stellen und mit dem Azure Cache-Team und anderen Mitgliedern der Community über diesen Artikel diskutieren.
* Sie können eine Frage auf der [Q&A-Seite von Microsoft zu Azure Cache](https://docs.microsoft.com/answers/topics/azure-cache-redis.html) stellen und mit dem Azure Cache-Team und anderen Mitgliedern der Community diskutieren, um eine größere Benutzergruppe zu erreichen
* Featureanfragen und Anregungen können an [Azure Cache for Redis User Voice](https://feedback.azure.com/forums/169382-cache) gesendet werden.
* Sie können auch eine E-Mail an unsere Adresse für [externes Feedback zu Azure Cache](mailto:azurecache@microsoft.com)senden.

## <a name="azure-cache-for-redis-basics"></a>Azure Cache for Redis – Grundlagen
Die häufig gestellten Fragen in diesem Abschnitt beziehen sich auf die Grundlagen von Azure Cache for Redis.

* [Was ist Azure Cache for Redis?](#what-is-azure-cache-for-redis)
* [Wie kann ich mit Azure Cache for Redis starten?](#how-can-i-get-started-with-azure-cache-for-redis)

Im Folgenden finden Sie häufig gestellte Fragen zu grundlegenden Konzepten. Fragen zu Azure Cache for Redis werden in einem der anderen Abschnitte mit häufig gestellten Fragen beantwortet.

* [Welches Azure Cache for Redis-Angebot und welche Redis Cache-Größe sollte ich verwenden?](#what-azure-cache-for-redis-offering-and-size-should-i-use)
* [Welche Azure Cache for Redis-Clients kann ich verwenden?](#what-azure-cache-for-redis-clients-can-i-use)
* [Gibt es einen lokalen Emulator für Azure Cache for Redis?](#is-there-a-local-emulator-for-azure-cache-for-redis)
* [Wie überwache ich die Integrität und Leistung meines Caches?](#how-do-i-monitor-the-health-and-performance-of-my-cache)

## <a name="planning-faqs"></a>Häufig gestellte Fragen zur Planung
* [Welches Azure Cache for Redis-Angebot und welche Redis Cache-Größe sollte ich verwenden?](#what-azure-cache-for-redis-offering-and-size-should-i-use)
* [Azure Cache for Redis – Leistung](#azure-cache-for-redis-performance)
* [Ich welcher Region sollte ich meinen Cache platzieren?](#in-what-region-should-i-locate-my-cache)
* [Wo befinden sich meine zwischengespeicherten Daten?](#where-do-my-cached-data-reside)
* [Wie wird Azure Cache for Redis abgerechnet?](#how-am-i-billed-for-azure-cache-for-redis)
* [Kann ich Azure Cache for Redis mit der Azure Government Cloud, der Azure Cloud in China oder Microsoft Azure Deutschland verwenden?](#can-i-use-azure-cache-for-redis-with-azure-government-cloud-azure-china-cloud-or-microsoft-azure-germany)

## <a name="development-faqs"></a>Häufig gestellte Fragen zur Entwicklung
* [Was bewirken die Konfigurationsoptionen für "StackExchange.Redis"?](#what-do-the-stackexchangeredis-configuration-options-do)
* [Welche Azure Cache for Redis-Clients kann ich verwenden?](#what-azure-cache-for-redis-clients-can-i-use)
* [Gibt es einen lokalen Emulator für Azure Cache for Redis?](#is-there-a-local-emulator-for-azure-cache-for-redis)
* [Wie führe ich Redis-Befehle aus?](#how-can-i-run-redis-commands)
* [Warum gibt es für Azure Cache for Redis keinen MSDN-Klassenbibliothekverweis wie für einige andere Azure-Dienste?](#why-doesnt-azure-cache-for-redis-have-an-msdn-class-library-reference-like-some-of-the-other-azure-services)
* [Kann ich Azure Cache for Redis als PHP-Sitzungscache verwenden?](#can-i-use-azure-cache-for-redis-as-a-php-session-cache)
* [Was sind Redis-Datenbanken?](#what-are-redis-databases)

## <a name="security-faqs"></a>Häufig gestellte Fragen zur Sicherheit
* [Wann sollte ich den nicht für TLS/SSL bestimmten Port für die Verbindungsherstellung mit Redis aktivieren?](#when-should-i-enable-the-non-tlsssl-port-for-connecting-to-redis)

## <a name="production-faqs"></a>Häufig gestellte Fragen zur Produktion
* [Welche Best Practices gelten für die Produktion?](#what-are-some-production-best-practices)
* [Was muss bei der Verwendung gängiger Redis-Befehle beachtet werden?](#what-are-some-of-the-considerations-when-using-common-redis-commands)
* [Wie kann ich die Leistung meines Caches messen und testen?](#how-can-i-benchmark-and-test-the-performance-of-my-cache)
* [Wichtige Details zum Threadpool-Wachstum](#important-details-about-threadpool-growth)
* [Aktivieren der Garbage Collection auf dem Server, um bei Verwenden von „StackExchange.Redis“ mehr Durchsatz auf dem Client zu erzielen](#enable-server-gc-to-get-more-throughput-on-the-client-when-using-stackexchangeredis)
* [Überlegungen zur Leistung im Zusammenhang mit Verbindungen](#performance-considerations-around-connections)

## <a name="monitoring-and-troubleshooting-faqs"></a>Häufig gestellte Fragen zur Überwachung und Problembehandlung
In den häufig gestellten Fragen in diesem Abschnitt werden allgemeine Fragen zur Überwachung und Problembehandlung behandelt. Weitere Informationen zur Überwachung und Problembehandlung Ihrer Azure Cache for Redis-Instanzen finden Sie unter [Überwachen von Azure Cache for Redis](cache-how-to-monitor.md) und in den verschiedenen Handbüchern zur Problembehandlung.

* [Wie überwache ich die Integrität und Leistung meines Caches?](#how-do-i-monitor-the-health-and-performance-of-my-cache)
* [Warum kommt es zu Timeouts?](#why-am-i-seeing-timeouts)
* [Warum wurde mein Client vom Cache getrennt?](#why-was-my-client-disconnected-from-the-cache)

## <a name="prior-cache-offering-faqs"></a>Häufig gestellte Fragen zu vorherigen Cache-Angeboten
* [Welches Azure-Cache-Angebot ist das Richtige für mich?](#which-azure-cache-offering-is-right-for-me)

### <a name="what-is-azure-cache-for-redis"></a>Was ist Azure Cache for Redis?
Azure Cache for Redis basiert auf der beliebten Open Source-Software [Redis](https://redis.io/). Sie erhalten Zugriff auf einen sicheren, dedizierten Azure Cache for Redis, der von Microsoft verwaltet wird und in Azure von einer beliebigen Anwendung aus aufgerufen werden kann. Eine ausführlichere Übersicht finden Sie auf der [Azure Cache for Redis](https://azure.microsoft.com/services/cache/)-Produktseite auf Azure.com.

### <a name="how-can-i-get-started-with-azure-cache-for-redis"></a>Wie kann ich mit Azure Cache for Redis starten?
Es gibt verschiedene Möglichkeiten, mit Azure Cache for Redis zu starten.

* Sie können sich eines unserer Tutorials ansehen, verfügbar für [.NET](cache-dotnet-how-to-use-azure-redis-cache.md), [ASP.NET](cache-web-app-howto.md), [Java](cache-java-get-started.md), [Node.js](cache-nodejs-get-started.md) und [Python](cache-python-get-started.md).
* Sie können sich das Video [How to Build High Performance Apps Using Microsoft Azure Cache for Redis](https://azure.microsoft.com/documentation/videos/how-to-build-high-performance-apps-using-microsoft-azure-cache/) (So erstellen Sie mit Microsoft Azure Cache for Redis hochleistungsfähige Apps) ansehen.
* Sie können sich die Clientdokumentation für die Clients für die jeweilige Entwicklersprache Ihres Projekts ansehen, um weitere Informationen zur Verwendung von Redis zu erhalten. Es gibt viele Redis-Clients, die mit Azure Cache for Redis verwendet werden können. Eine Liste der Redis-Clients finden Sie unter [https://redis.io/clients](https://redis.io/clients).

Wenn Sie noch kein Azure-Konto besitzen, haben Sie folgende Möglichkeiten:

* [Kostenloses Anlegen eines Azure-Kontos](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=redis_cache_hero). Sie erhalten ein Guthaben, mit dem Sie andere kostenpflichtige Azure-Dienste ausprobieren können. Auch nachdem Sie das Guthaben aufgebraucht haben, können Sie das Konto behalten und kostenlose Azure-Dienste und -Features nutzen.
* [Aktivieren Sie Visual Studio-Abonnementvorteile](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=redis_cache_hero). Ihr MSDN-Abonnement beinhaltet ein monatliches Guthaben, das Sie für zahlungspflichtige Azure-Dienste verwenden können.

<a name="cache-size"></a>

### <a name="what-azure-cache-for-redis-offering-and-size-should-i-use"></a>Welches Azure Cache for Redis-Angebot und welche Redis Cache-Größe sollte ich verwenden?
Jedes Azure Cache for Redis-Angebot umfasst unterschiedliche Optionen in Bezug auf **Größe**, **Bandbreite**, **Hochverfügbarkeit** und **SLA**.

Nachfolgend sind verschiedene Aspekte aufgeführt, die Ihnen bei der Wahl helfen können.

* **Arbeitsspeicher**: Der Basic-Tarif und der Standard-Tarif bieten 250 MB bis 53 GB. Der Premium-Tarif bietet bis zu 1,2 TB (als Cluster) oder 120 GB (nicht gruppiert). Weitere Informationen finden Sie unter [Azure Cache for Redis – Preise](https://azure.microsoft.com/pricing/details/cache/).
* **Netzwerkleistung**: Bei einer Workload, die einen hohen Durchsatz erfordert, bietet der Premium-Tarif im Vergleich zum Standard- oder Basic-Tarif eine größere Bandbreite. Zudem haben die größeren Caches aufgrund des zugrunde liegenden virtuellen Computers, der den Cache hostet, bei jedem Tarif eine höhere Bandbreite. Weitere Informationen finden Sie in der [folgenden Tabelle](#cache-performance).
* **Durchsatz**: Der Premium-Tarif bietet den maximal verfügbaren Durchsatz. Wenn Cacheserver oder -clients die Bandbreitengrenzwerte erreichen, können Timeouts auf der Clientseite auftreten. Ausführlichere Informationen finden Sie in der unten stehenden Tabelle.
* **Hochverfügbarkeit/SLA**: Azure Cache for Redis garantiert, dass ein Standard-/Premium-Cache mindestens 99,9 % der Zeit zur Verfügung steht. Weitere Informationen zu unserer SLA finden Sie unter [Azure Cache for Redis – Preise](https://azure.microsoft.com/support/legal/sla/cache/v1_0/). Die SLA deckt nur die Konnektivität zu den Cache-Endpunkten ab. Sie bezieht sich dagegen nicht auf Schutz vor Datenverlusten. Es wird empfohlen, das Redis-Feature für Datenpersistenz im Premium-Tarif zu verwenden, um den Schutz vor Datenverlusten zu erhöhen.
* **Redis-Datenpersistenz**: Der Tarif "Premium" ermöglicht die Persistenz der Cachedaten in einem Azure Storage-Konto. In einem Basic- oder Standard-Cache werden alle Daten nur im Arbeitsspeicher gespeichert. Probleme mit der zugrunde liegenden Infrastruktur können zu potenziellen Datenverlusten führen. Es wird empfohlen, das Redis-Feature für Datenpersistenz im Premium-Tarif zu verwenden, um den Schutz vor Datenverlusten zu erhöhen. Azure Cache for Redis bietet RDB- und AOF-Optionen (Vorschauversion) bei der Redis-Persistenz. Weitere Informationen finden Sie unter [Konfigurieren von Persistenz für Azure Cache for Redis vom Typ „Premium“](cache-how-to-premium-persistence.md).
* **Redis-Cluster**: Zum Erstellen von Caches mit einer Größe von über 120 GB oder zum horizontalen Partitionieren („Sharding“) von Daten über mehrere Redis-Knoten hinweg können Sie das im Premium-Tarif verfügbare Redis-Clustering verwenden. Für Hochverfügbarkeit besteht jeder Knoten aus einem Paar aus primärem Cache und Replikatcache. Weitere Informationen finden Sie unter [Konfigurieren von Clustern für Azure Cache for Redis vom Typ „Premium“](cache-how-to-premium-clustering.md).
* **Verbesserte Sicherheit und Netzwerkisolation**: Die Bereitstellung über Azure Virtual Network (VNET) bietet ein höheres Maß an Sicherheit und Isolation sowohl für Ihre Azure Cache for Redis-Instanz als auch für Ihre Subnetze. Sie profitieren außerdem von Richtlinien für die Zugriffssteuerung sowie von weiteren Features zur Begrenzung des Zugriffs. Weitere Informationen finden Sie unter [Konfigurieren der Unterstützung virtueller Netzwerke für Azure Cache for Redis vom Typ „Premium“](cache-how-to-premium-vnet.md).
* **Konfigurieren von Redis**: Sowohl im Standard- als auch im Premium-Tarif können Sie Redis für Keyspace-Benachrichtigungen konfigurieren.
* **Maximale Anzahl von Clientverbindungen**: Der Premium-Tarif bietet die maximale Anzahl von Clients, die eine Verbindung mit Redis herstellen können, mit einer größeren Anzahl an Verbindungen für größere Caches. Clustering erhöht nicht die Anzahl von Verbindungen, die für einen gruppierten Cache verfügbar sind. Weitere Informationen finden Sie unter [Azure Cache for Redis – Preise](https://azure.microsoft.com/pricing/details/cache/).
* **Dedizierter Kern für Redis-Server**: Im Premium-Tarif verfügen alle Cachegrößen über einen dedizierten Kern für Redis. Im Basic- oder Standard-Tarif verfügen alle Cachegrößen ab C1 über einen dedizierten Kern für Redis-Server.
* **Redis verwendet Single-Threading**, deshalb bietet der Einsatz von mehr als zwei Kernen keine zusätzlichen Vorteile gegenüber der Verwendung von nur zwei Kernen. Größere virtuelle Computer besitzen jedoch in der Regel mehr Bandbreite als kleinere. Wenn Cacheserver oder -client die Bandbreitengrenzwerte erreichen, kommt es zu Timeouts auf Clientseite.
* **Leistungsverbesserungen**: Caches im Premium-Tarif werden auf Hardware mit schnelleren Prozessoren bereitgestellt, die im Vergleich zu den Tarifen Basic oder Standard eine bessere Leistung bieten. Caches im Premium-Tarif erreichen einen höheren Durchsatz und geringere Wartezeiten.

<a name="cache-performance"></a>

### <a name="azure-cache-for-redis-performance"></a>Azure Cache for Redis – Leistung
In der folgenden Tabelle sind die maximalen Bandbreitenwerte beim Testen verschiedener Standard- und Premium-Cachegrößen bei Verwendung von `redis-benchmark.exe` aus einem virtuellen IaaS-Computer mit dem Azure Cache for Redis-Endpunkt angegeben. Für TLS-Durchsatz wird der Redis-Vergleichstest mit „stunnel“ für das Herstellen der Verbindung mit dem Azure Cache for Redis-Endpunkt verwendet.

>[!NOTE] 
>Diese Werte werden nicht garantiert, und es gibt keine SLA für diese Werte; sie sollten jedoch typischerweise erreicht werden. Führen Sie Auslastungstests für Ihre Anwendung durch, um die geeignete Cachegröße für Ihre Anwendung zu ermitteln.
>Diese Zahlen können sich ändern, da wir in regelmäßigen Abständen neuere Ergebnisse veröffentlichen.
>

Aus dieser Tabelle können folgende Schlussfolgerungen gezogen werden:

* Der Durchsatz für Caches gleicher Größe ist im Premium-Tarif besser als im Standard-Tarif. Beispielsweise liegt bei einem Cache mit 6 GB der Durchsatz von P1 bei 180.000 Anforderungen pro Sekunde (RPS) im Vergleich zu 100.000 RPS bei C3.
* Mit dem Redis-Clustering steigt der Durchsatz linear, je mehr Shards (Knoten) Sie im Cluster verwenden. Wenn Sie beispielsweise einen P4-Cluster mit 10 Shards erstellen, beträgt der verfügbare Durchsatz 400.000 * 10 = 4 Millionen RPS.
* Der Durchsatz für größere Schlüsselgrößen ist im Premium-Tarif höher als im Standard-Tarif.

| Tarif | Size | CPU-Kerne | Verfügbare Bandbreite | 1 KB Wertgröße | 1 KB Wertgröße |
| --- | --- | --- | --- | --- | --- |
| **Standard-Cachegröße** | | |**Megabits pro Sekunde (MBit/s)/Megabyte pro Sekunde (MB/s)** |**Anforderungen pro Sekunde (RPS), kein SLL** |**Anforderungen pro Sekunde (RPS), SLL** |
| C0 | 250 MB | Shared | 100 / 12,5  |  15.000 |   7\.500 |
| C1 |   1 GB | 1      | 500 / 62,5  |  38.000 |  20.720 |
| C2 | 2,5 GB | 2      | 500 / 62,5  |  41.000 |  37.000 |
| C3 |   6 GB | 4      | 1\.000 / 125  | 100.000 |  90.000 |
| C4 |  13 GB | 2      | 500 / 62,5  |  60.000 |  55.000 |
| C5 |  26 GB | 4      | 1\.000/125 | 102.000 |  93.000 |
| C6 |  53 GB | 8      | 2\.000/250 | 126.000 | 120.000 |
| **Premium-Cachegröße** | |**CPU-Kerne pro Shard** | **Megabits pro Sekunde (MBit/s)/Megabyte pro Sekunde (MB/s)** |**Anforderungen pro Sekunde (RPS), kein SLL, pro Shard** |**Anforderungen pro Sekunde (RPS), SLL, pro Shard** |
| P1 |   6 GB |  2 | 1\.500/187,5 | 180.000 | 172.000 |
| P2 |  13 GB |  4 | 3\.000/375   | 350.000 | 341.000 |
| P3 |  26 GB |  4 | 3\.000/375   | 350.000 | 341.000 |
| P4 |  53 GB |  8 | 6\.000/750   | 400.000 | 373.000 |
| P5 | 120 GB | 20 | 6\.000/750   | 400.000 | 373.000 |

Anweisungen zum Einrichten von stunnel oder zum Herunterladen von Redis-Tools wie `redis-benchmark.exe` finden Sie im Abschnitt [Wie führe ich Redis-Befehle aus?](#cache-commands).

<a name="cache-region"></a>

### <a name="in-what-region-should-i-locate-my-cache"></a>Ich welcher Region sollte ich meinen Cache platzieren?
Um eine optimale Leistung und die niedrigste Latenz zu erzielen, sollten Sie Ihren Azure Cache for Redis in derselben Region platzieren wie Ihre Cacheclientanwendung.

### <a name="where-do-my-cached-data-reside"></a>Wo befinden sich meine zwischengespeicherten Daten?
Azure Cache for Redis speichert Ihre Anwendungsdaten abhängig vom Tarif im RAM der VM oder der VMs, die Ihren Cache hosten. Ihre Daten bleiben strikt in der Azure-Region, die Sie standardmäßig ausgewählt haben. Es gibt zwei Fälle, in denen Ihre Daten eine Region verlassen könnten:
  1. Wenn Sie die Persistenz im Cache aktivieren, sichert Azure Cache for Redis Ihre Daten in einem Azure Storage-Konto, das Sie besitzen. Wenn sich das von Ihnen bereitgestellte Speicherkonto in einer anderen Region befindet, wird eine Kopie der Daten dort gespeichert.
  1. Wenn Sie Georeplikation einrichten und sich der sekundäre Cache in einer anderen Region befindet, was normalerweise der Fall ist, werden die Daten in dieser Region repliziert.

Sie müssen Azure Cache for Redis explizit so konfigurieren, um diese Features zu verwenden. Außerdem verfügen Sie über die komplette Kontrolle über die Region, in der sich das Speicherkonto oder der sekundäre Cache befindet.

<a name="cache-billing"></a>

### <a name="how-am-i-billed-for-azure-cache-for-redis"></a>Wie wird Azure Cache for Redis abgerechnet?
Die Preise für Azure Cache for Redis finden Sie [hier](https://azure.microsoft.com/pricing/details/cache/). Auf der Seite mit den Preisen sind die Kosten als Stundensatz ausgewiesen. Caches werden auf Minutenbasis ab dem Zeitpunkt der Erstellung des Caches bis zu dem Zeitpunkt abgerechnet, an dem ein Cache gelöscht wird. Es gibt keine Möglichkeit, die Abrechnung eines Caches zu beenden oder anzuhalten.

### <a name="can-i-use-azure-cache-for-redis-with-azure-government-cloud-azure-china-cloud-or-microsoft-azure-germany"></a>Kann ich Azure Cache for Redis mit der Azure Government Cloud, der Azure Cloud in China oder Microsoft Azure Deutschland verwenden?
Ja, Azure Cache for Redis ist in der Azure Government Cloud, der Azure 21Vianet Cloud in China und Microsoft Azure Deutschland verfügbar. Die URLs für den Zugriff auf und die Verwaltung von Azure Cache for Redis unterscheiden sich in diesen Clouds von den URLs der Azure Public Cloud.

| Cloud   | DNS-Suffix für Redis            |
|---------|---------------------------------|
| Öffentlich  | *.redis.cache.windows.net       |
| US Gov  | *.redis.cache.usgovcloudapi.net |
| Deutschland | *.redis.cache.cloudapi.de       |
| China   | *.redis.cache.chinacloudapi.cn  |

Weitere zu berücksichtigende Aspekte bei der Verwendung von Azure Cache for Redis mit anderen Clouds werden unter den folgenden Links beschrieben.

- [Azure Government-Datenbanken – Azure Cache for Redis](../azure-government/documentation-government-services-database.md#azure-cache-for-redis)
- [Azure 21Vianet Cloud in China – Azure Cache for Redis](https://www.azure.cn/home/features/redis-cache/)
- [Microsoft Azure Deutschland](https://azure.microsoft.com/overview/clouds/germany/)

Informationen zur Nutzung von Azure Cache for Redis in Verbindung mit PowerShell in der Azure Government Cloud, in der Azure 21Vianet Cloud in China und in Microsoft Azure Deutschland finden Sie unter [Herstellen einer Verbindung mit anderen Clouds – Azure Cache for Redis PowerShell](cache-how-to-manage-redis-cache-powershell.md#how-to-connect-to-other-clouds).

<a name="cache-configuration"></a>

### <a name="what-do-the-stackexchangeredis-configuration-options-do"></a>Was bewirken die Konfigurationsoptionen für "StackExchange.Redis"?
Für "StackExchange.Redis" stehen zahlreiche Optionen zur Verfügung. In diesem Abschnitt werden einige der gängigsten Optionen erläutert. Ausführlichere Informationen zu den StackExchange.Redis-Optionen finden Sie unter [StackExchange.Redis-Konfiguration](https://stackexchange.github.io/StackExchange.Redis/Configuration).

| ConfigurationOptions | BESCHREIBUNG | Empfehlung |
| --- | --- | --- |
| AbortOnConnectFail |Wenn Sie diese Option auf "true" setzen, wird die Verbindung nach einem Netzwerkausfall nicht wiederhergestellt. |Bei Festlegung auf "false" kann "StackExchange.Redis" die Verbindung automatisch wiederherstellen. |
| ConnectRetry |Die Anzahl von Versuchen für die Verbindungsherstellung bei der anfänglichen Verbindung. |Die folgenden Informationen bieten eine Orientierung. |
| ConnectTimeout |Timeout in Millisekunden für Verbindungsvorgänge. |Die folgenden Informationen bieten eine Orientierung. |

In der Regel reichen die Standardwerte des Clients aus. Sie können die Optionen basierend auf Ihrer Workload optimieren.

* **Wiederholungsversuche**
  * Für „ConnectRetry“ und „ConnectTimeout“ lautet die allgemeine Empfehlung, Fail-Fast-fähig zu sein und den Vorgang zu wiederholen. Diese Empfehlung hängt von Ihrer Workload und davon ab, wie lange es auf Ihrem Client durchschnittlich dauert, einen Redis-Befehl auszugeben und eine Antwort zu empfangen.
  * Ermöglichen Sie eine automatische Neuverbindung durch "StackExchange.Redis", statt die Prüfung des Verbindungsstatus und die erneute Verbindungsherstellung selbst zu prüfen. **Vermeiden Sie die Verwendung der ConnectionMultiplexer.IsConnected-Eigenschaft**.
  * Schneeballeffekt: Gelegentlich kann es zu Problemen kommen, bei denen Wiederholungsversuche zu nicht behebbaren, immer wiederkehrenden Problemen führen. Wenn der Schneeballeffekt auftritt, sollten Sie die Verwendung eines exponentiellen Backoff/Retry-Algorithmus in Erwägung ziehen, wie er im Artikel [Allgemeiner Leitfaden zum Wiederholen von Vorgängen](../best-practices-retry-general.md) der Microsoft Patterns & Practices-Gruppe beschrieben wird.
  
* **Timeoutwerte**
  * Berücksichtigen Sie Ihre Workload, und legen Sie die Werte entsprechend fest. Wenn Sie große Werte speichern, legen Sie die Timeouteinstellung auf einen höheren Wert fest.
  * Legen Sie `AbortOnConnectFail` auf „false“ fest, und überlassen Sie StackExchange.Redis die erneute Verbindungsherstellung.
  * Verwenden Sie eine einzelne ConnectionMultiplexer-Instanz für die Anwendung. Sie können "LazyConnection" verwenden, um eine einzelne Instanz zu erstellen, die über eine Connection-Eigenschaft zurückgegeben wird, wie gezeigt in [Verbindungsherstellung mit dem Cache unter Verwendung der ConnectionMultiplexer-Klasse](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache).
  * Legen Sie die `ConnectionMultiplexer.ClientName` -Eigenschaft zu Diagnosezwecken auf einen eindeutigen App-Instanznamen fest.
  * Verwenden Sie für benutzerdefinierte Workloads mehrere `ConnectionMultiplexer` -Instanzen.
      * Sie können diesem Modell folgen, wenn die Last in Ihrer Anwendung variiert. Beispiel:
      * Sie können einen Multiplexer zur Verarbeitung großer Schlüssel verwenden.
      * Sie können einen Multiplexer zur Verarbeitung kleiner Schlüssel verwenden.
      * Sie können verschiedene Werte für Verbindungstimeouts und eine retry-Logik für jede verwendete ConnectionMultiplexer-Instanz verwenden.
      * Legen Sie die `ClientName` -Eigenschaft für jeden Multiplexer fest, um die Diagnose zu vereinfachen.
      * Diese Empfehlung kann zu einer optimierten Latenz pro `ConnectionMultiplexer` führen.

### <a name="what-azure-cache-for-redis-clients-can-i-use"></a>Welche Azure Cache for Redis-Clients kann ich verwenden?
Einer der großen Vorteile von Redis ist, dass es viele Clients gibt, die viele verschiedene Programmiersprachen unterstützen. Eine aktuelle Liste von Clients finden Sie unter [Redis-Clients](https://redis.io/clients). Tutorials, in denen mehrere verschiedene Sprachen und Clients behandelt werden, finden Sie unter [Verwenden von Azure Redis Cache](cache-dotnet-how-to-use-azure-redis-cache.md) und in den zugehörigen Artikeln im Inhaltsverzeichnis.

[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

<a name="cache-emulator"></a>

### <a name="is-there-a-local-emulator-for-azure-cache-for-redis"></a>Gibt es einen lokalen Emulator für Azure Cache for Redis?
Für Azure Cache for Redis ist kein lokaler Emulator verfügbar. Sie können jedoch die MSOpenTech-Version von „redis-server.exe“ über die [Redis-Befehlszeilentools](https://github.com/MSOpenTech/redis/releases/) auf dem lokalen Computer ausführen und eine Verbindung herstellen, um ein ähnliches Verhalten wie bei einem lokalen Cache-Emulator zu erhalten. Dies wird im folgenden Beispiel gezeigt:

```csharp
private static Lazy<ConnectionMultiplexer>
    lazyConnection = new Lazy<ConnectionMultiplexer> (() =>
    {
        // Connect to a locally running instance of Redis to simulate
        // a local cache emulator experience.
        return ConnectionMultiplexer.Connect("127.0.0.1:6379");
    });

public static ConnectionMultiplexer Connection
{
    get
    {
        return lazyConnection.Value;
    }
}
```

Optional können Sie die Datei [redis.conf](https://redis.io/topics/config) konfigurieren, um bei Bedarf eine bessere Abstimmung mit den [standardmäßigen Cacheeinstellungen](cache-configure.md#default-redis-server-configuration) für Ihren Online-Azure Cache for Redis zu erreichen.

<a name="cache-commands"></a>

### <a name="how-can-i-run-redis-commands"></a>Wie führe ich Redis-Befehle aus?
Sie können alle aufgelisteten [Redis-Befehle](https://redis.io/commands#) mit Ausnahme der Befehle verwenden, die unter [Redis-Befehle, die in Azure Cache for Redis nicht unterstützt werden](cache-configure.md#redis-commands-not-supported-in-azure-cache-for-redis) aufgeführt sind. Für die Ausführung von Redis-Befehlen haben Sie mehrere Optionen.

* Wenn Sie über einen Standard- oder Premium-Cache verfügen, können Sie Redis-Befehle über die [Redis-Konsole](cache-configure.md#redis-console)ausführen. Die Redis-Konsole ist eine sichere Methode zum Ausführen von Redis-Befehlen im Azure-Portal.
* Sie können auch die Redis-Befehlszeilentools verwenden. Führen Sie hierzu die folgenden Schritte aus:
* Laden Sie die [Redis-Befehlszeilentools](https://github.com/MSOpenTech/redis/releases/) herunter.
* Stellen Sie über `redis-cli.exe`eine Verbindung mit dem Cache her. Übergeben Sie den Cacheendpunkt mithilfe des Schalters „-h“ und dem Schlüssel mit Verwendung von „-a“, wie im folgenden Beispiel gezeigt:
* `redis-cli -h <Azure Cache for Redis name>.redis.cache.windows.net -a <key>`

> [!NOTE]
> Die Redis-Befehlszeilentools funktionieren nicht mit dem TLS-Port, aber Sie können ein Hilfsprogramm wie `stunnel` verwenden, um eine sichere Verbindung zwischen den Tools und dem TLS-Port herzustellen. Anweisungen hierzu finden Sie unter [Verwenden des Redis-Befehlszeilentools mit Azure Cache for Redis](https://docs.microsoft.com/azure/azure-cache-for-redis/cache-how-to-redis-cli-tool).
>
>

<a name="cache-reference"></a>

### <a name="why-doesnt-azure-cache-for-redis-have-an-msdn-class-library-reference-like-some-of-the-other-azure-services"></a>Warum gibt es für Azure Cache for Redis keinen MSDN-Klassenbibliothekverweis wie für einige andere Azure-Dienste?
Microsoft Azure Cache for Redis basiert auf dem beliebten Open-Source-Speicherdienst Azure Cache for Redis. Der Zugriff kann über eine Vielzahl von [Redis-Clients](https://redis.io/clients) für zahlreiche Programmiersprachen erfolgen. Jeder Client verfügt über eine eigene API, die unter Verwendung von [Redis-Befehlen](https://redis.io/commands) Aufrufe an den Azure Cache for Redis sendet.

Da jeder Client anders ist, gibt es nicht nur einen zentralen Klassenverweis in MSDN, und jeder Client verwaltet seine eigene Verweisdokumentation. Zusätzlich zur Referenzdokumentation gibt es mehrere Tutorials, die Ihnen den Einstieg in die Verwendung von Azure Cache for Redis mit verschiedenen Sprachen und Cacheclients erleichtern. Sie können unter [Verwenden von Azure Cache for Redis](cache-dotnet-how-to-use-azure-redis-cache.md) und in den zugehörigen Artikeln im Inhaltsverzeichnis auf diese Tutorials zugreifen.

### <a name="can-i-use-azure-cache-for-redis-as-a-php-session-cache"></a>Kann ich Azure Cache for Redis als PHP-Sitzungscache verwenden?
Ja. Um Azure Cache for Redis als PHP-Sitzungscache zu verwenden, geben Sie die Verbindungszeichenfolge zu Ihrer Azure Cache for Redis-Instanz in `session.save_path` an.

> [!IMPORTANT]
> Bei der Verwendung von Azure Cache for Redis als PHP-Sitzungscache müssen Sie – wie im folgenden Beispiel dargestellt – eine URL-Codierung des Sicherheitsschlüssels durchführen, der zum Herstellen der Verbindung mit dem Cache verwendet wird:
>
> `session.save_path = "tcp://mycache.redis.cache.windows.net:6379?auth=<url encoded primary or secondary key here>";`
>
> Wurde keine URL-Codierung des Schlüssels vorgenommen, erhalten Sie ggf. eine Ausnahmemeldung ähnlich der folgenden: `Failed to parse session.save_path`
>
>

Weitere Informationen zur Nutzung von Azure Cache for Redis als PHP-Sitzungscache mit dem PhpRedis-Client finden Sie unter [PHP Session handler](https://github.com/phpredis/phpredis#php-session-handler)(PHP-Sitzungshandler).

### <a name="what-are-redis-databases"></a>Was sind Redis-Datenbanken?

Redis-Datenbanken sind einfach eine logische Trennung von Daten innerhalb der gleichen Redis-Instanz. Der Cachespeicher wird von allen Datenbanken gemeinsam genutzt, und die tatsächliche Arbeitsspeichernutzung einer bestimmten Datenbank richtet sich nach den in dieser Datenbank gespeicherten Schlüsseln bzw. Werten. Ein Beispiel: Ein C6-Cache verfügt über 53 GB Arbeitsspeicher, ein P5 über 120 GB. Sie können die Gesamtmenge von 53 GB/120 GB einer einzigen Datenbank zuweisen oder das Datenvolumen auf mehrere Datenbanken aufteilen. 

> [!NOTE]
> Wenn Sie einen Premium-Azure Cache for Redis mit aktiviertem Clustering verwenden, ist nur Datenbank 0 verfügbar. Dies ist eine intrinsische Einschränkung von Redis und nicht spezifisch für Azure Cache for Redis. Weitere Informationen finden Sie unter [Muss ich Änderungen an meiner Clientanwendung vornehmen, um Clustering verwenden zu können?](cache-how-to-premium-clustering.md#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)
> 
> 


<a name="cache-ssl"></a>

### <a name="when-should-i-enable-the-non-tlsssl-port-for-connecting-to-redis"></a>Wann sollte ich den nicht für TLS/SSL bestimmten Port für die Verbindungsherstellung mit Redis aktivieren?
Der Redis-Server bietet keine native TLS-Unterstützung, während dies für Azure Cache for Redis der Fall ist. Wenn Sie eine Verbindung mit Azure Cache for Redis herstellen und Ihr Client TLS unterstützt (z. B. StackExchange.Redis), sollten Sie TLS verwenden.

>[!NOTE]
>Bei neuen Azure Cache for Redis-Instanzen ist der nicht für TLS bestimmte Port standardmäßig deaktiviert. Wenn Ihr Client keine TLS-Unterstützung bietet, müssen Sie den nicht für TLS bestimmten Port gemäß den Anweisungen im Abschnitt [Zugriffsports](cache-configure.md#access-ports) des Artikels [Konfigurieren eines Caches in Azure Cache for Redis](cache-configure.md) aktivieren.
>
>

Redis-Tools wie `redis-cli` funktionieren nicht mit dem TLS-Port, aber Sie können ein Hilfsprogramm wie `stunnel` verwenden, um eine sichere Verbindung zwischen den Tools und dem TLS-Port herzustellen. Anweisungen hierzu finden Sie im Blogbeitrag zur [Ankündigung des ASP.NET-Sitzungsstatusanbieters für Redis-Vorschauversion](https://devblogs.microsoft.com/aspnet/announcing-asp-net-session-state-provider-for-redis-preview-release/).

Anweisungen zum Herunterladen der Redis-Tools finden Sie im Abschnitt [Wie führe ich Redis-Befehle aus?](#cache-commands) .

### <a name="what-are-some-production-best-practices"></a>Welche Best Practices gelten für die Produktion?
* [Best Practices für StackExchange.Redis](#stackexchangeredis-best-practices)
* [Konfiguration und Konzepte](#configuration-and-concepts)
* [Leistungstests](#performance-testing)

#### <a name="stackexchangeredis-best-practices"></a>Best Practices für StackExchange.Redis
* Legen Sie `AbortConnect` auf „false“ fest, und lassen Sie dann den ConnectionMultiplexer automatisch eine neue Verbindung herstellen. [Ausführliche Informationen finden Sie hier](https://gist.github.com/JonCole/36ba6f60c274e89014dd#file-se-redis-setabortconnecttofalse-md).
* Verwenden Sie den ConnectionMultiplexer wieder – erstellen Sie nicht für jede Anforderung einen neuen ConnectionMultiplexer. Sie sollten das [hier gezeigte](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache)`Lazy<ConnectionMultiplexer>`-Muster verwenden.
* Redis funktioniert am besten mit kleineren Werten, deshalb sollten Sie die Aufteilung großer Daten in mehrere Schlüssel erwägen. In [dieser Diskussion zu Redis](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ) werden 100 KB als groß betrachtet. Lesen Sie [diesen Artikel](https://gist.github.com/JonCole/db0e90bedeb3fc4823c2#large-requestresponse-size) über ein beispielhaftes Problem, das durch große Werte verursacht werden kann.
* Konfigurieren Sie Ihre [ThreadPool-Einstellungen](#important-details-about-threadpool-growth) , um Timeouts zu verhindern.
* Verwenden Sie mindestens den Standardwert von 5 Sekunden für connectTimeout. Dieses Intervall gibt StackExchange.Redis bei einer Netzwerkunterbrechung genügend Zeit zur erneuten Verbindungsherstellung.
* Berücksichtigen Sie die Leistungskosten für andere Vorgänge, die Sie ausführen. Beispielsweise ist der `KEYS` -Befehl ein O(n)-Vorgang und sollte vermieden werden. Die [redis.io-Website](https://redis.io/commands/) umfasst Details zur Zeitkomplexität für jeden unterstützten Vorgang. Klicken Sie auf jeden Befehl, um die Komplexität für jeden Vorgang anzuzeigen.

#### <a name="configuration-and-concepts"></a>Konfiguration und Konzepte
* Verwenden Sie den Standard- oder Premium-Tarif für Produktionssysteme. Der Basic-Tarif ist ein System mit einem einzelnen Knoten, ohne Datenreplikation und ohne SLA. Verwenden Sie mindestens einen C1-Cache. C0-Caches werden in der Regel für einfache Entwicklungs-/Testszenarien verwendet.
* Beachten Sie, dass Redis ein **In-Memory** -Datenspeicher ist. Lesen Sie [diesen Artikel](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md) , um zu erfahren, in welchen Szenarien es zu einem Datenverlust kommen kann.
* Entwickeln Sie Ihr System so, dass es Verbindungsunterbrechungen [aufgrund von Patching- und Failovervorgängen](https://gist.github.com/JonCole/317fe03805d5802e31cfa37e646e419d#file-azureredis-patchingexplained-md)behandeln kann.

#### <a name="performance-testing"></a>Leistungstests
* Starten Sie mit der Verwendung von `redis-benchmark.exe` , um den möglichen Durchsatz einschätzen zu können, bevor Sie Ihre eigenen Leistungstests schreiben. Weil `redis-benchmark` TLS nicht unterstützt, müssen Sie [den nicht für TLS bestimmten Port über das Azure-Portal aktivieren](cache-configure.md#access-ports), bevor Sie den Test ausführen. Beispiele finden Sie unter [Wie kann ich die Leistung meines Caches messen und testen?](#how-can-i-benchmark-and-test-the-performance-of-my-cache)
* Die für den Test verwendete Client-VM sollte sich in derselben Region befinden wie Ihre Azure Cache for Redis-Instanz.
* Es wird empfohlen, die Dv2-VM-Serie für Ihren Client zu verwenden, da sie über bessere Hardware verfügt und die besten Ergebnisse liefern dürfte.
* Stellen Sie sicher, dass die von Ihnen ausgewählte Client-VM mindestens über die gleiche Computing- und Bandbreitenkapazität verfügt wie der getestete Cache.
* Aktivieren Sie VRSS auf dem Clientcomputer, wenn Sie unter Windows arbeiten. [Ausführliche Informationen finden Sie hier](https://technet.microsoft.com/library/dn383582.aspx).
* Redis-Instanzen im Premium-Tarif verfügen über eine bessere Netzwerklatenz und einen höheren Durchsatz, weil sowohl die CPU als auch das Netzwerk auf besserer Hardware ausgeführt werden.

<a name="cache-redis-commands"></a>

### <a name="what-are-some-of-the-considerations-when-using-common-redis-commands"></a>Was muss bei der Verwendung gängiger Redis-Befehle beachtet werden?

* Vermeiden Sie die Verwendung bestimmter Redis-Befehle mit langer Ausführungszeit, es sei denn, Sie verstehen deren Auswirkungen. Führen Sie beispielsweise nicht den Befehl [KEYS](https://redis.io/commands/keys) in der Produktion aus. Die Rückgabe könnte in Abhängigkeit von der Anzahl von Schlüsseln sehr viel Zeit in Anspruch nehmen. Redis ist ein Server mit Single-Threading, d. h. Befehle werden nacheinander verarbeitet. Wenn Sie nach KEYS weitere Befehle ausgeben, werden diese erst ausgeführt, wenn Redis den KEYS-Befehl verarbeitet hat. Die [redis.io-Website](https://redis.io/commands/) umfasst Details zur Zeitkomplexität für jeden unterstützten Vorgang. Klicken Sie auf jeden Befehl, um die Komplexität für jeden Vorgang anzuzeigen.
* Schlüsselgrößen – sollte ich kleine Schlüssel/Werte oder große Schlüssel/Werte verwenden? Dies hängt vom Szenario ab. Wenn Ihre Szenarios größere Schlüssel erfordern, können Sie den ConnectionTimeout anpassen, dann die Werte erneut ausprobieren und Ihre Logik für erneute Verbindungsversuche anpassen. In Bezug auf den Redis-Server führen kleinere Werte zu einer besseren Leistung.
* Diese Überlegungen bedeuten jedoch nicht, dass Sie in Redis keine größeren Werte speichern können – Sie müssen sich lediglich der folgenden Punkte bewusst sein. Latenzen sind höher. Wenn Sie über einen größeren und einen kleineren Datensatz verfügen, können Sie mehrere ConnectionMultiplexer-Instanzen verwenden und jede mit einem anderen Satz an Werten für Timeout und erneute Verbindungsherstellung konfigurieren – wie im vorangegangenen Abschnitt [Was bewirken die StackExchange.Redis-Konfigurationsoptionen?](#cache-configuration) beschrieben.

<a name="cache-benchmarking"></a>

### <a name="how-can-i-benchmark-and-test-the-performance-of-my-cache"></a>Wie kann ich die Leistung meines Caches messen und testen?
* [Aktivieren Sie die Cachediagnose](cache-how-to-monitor.md#enable-cache-diagnostics), damit Sie die Integrität Ihres Caches [überwachen](cache-how-to-monitor.md) können. Sie können die Metriken im Azure-Portal anzeigen und anschließend mit einem Tool Ihrer Wahl [herunterladen und prüfen](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) .
* Mit "redis-benchmark.exe" können Sie Auslastungstests für Ihren Redis-Server durchführen.
* Stellen Sie sicher, dass sich Client und Azure Cache for Redis für die Auslastungstests in derselben Region befinden.
* Verwenden Sie "redis-cli.exe", und überwachen Sie den Cache unter Verwendung des INFO-Befehls.
* Wenn Ihre Datenlast zu einer hohen Arbeitsspeicherfragmentierung führt, sollten Sie eine Hochskalierung auf einen größeren Cache durchführen.
* Anweisungen zum Herunterladen der Redis-Tools finden Sie im Abschnitt [Wie führe ich Redis-Befehle aus?](#cache-commands) .

Die folgenden Befehle sind ein Beispiel für die Verwendung von „redis-benchmark.exe“. Führen Sie diese Befehle von einem virtuellen Computer in der gleichen Region wie Ihr Cache aus, um genaue Ergebnisse zu erhalten.

* Test von SET-Anforderungen in der Pipeline mithilfe einer 1-K-Nutzlast

  `redis-benchmark.exe -h **yourcache**.redis.cache.windows.net -a **yourAccesskey** -t SET -n 1000000 -d 1024 -P 50`
* Test von GET-Anforderungen in der Pipeline mithilfe einer 1-K-Nutzlast
  HINWEIS:  Führen Sie zunächst den oben gezeigten SET-Test aus, um den Cache zu füllen.

  `redis-benchmark.exe -h **yourcache**.redis.cache.windows.net -a **yourAccesskey** -t GET -n 1000000 -d 1024 -P 50`

<a name="threadpool"></a>

### <a name="important-details-about-threadpool-growth"></a>Wichtige Details zum Threadpool-Wachstum
Der CLR-Threadpool enthält zwei Typen von Threads: Workerthreads und E/A-Abschlussportthreads (IOCP, I/O Completion Port).

* Workerthreads werden für Vorgänge wie das Verarbeiten der Methoden `Task.Run(…)` oder `ThreadPool.QueueUserWorkItem(…)` verwendet. Diese Threads werden auch von verschiedenen Komponenten in der CLR verwendet, wenn Arbeitsvorgänge in einem Hintergrundthread ausgeführt werden müssen.
* IOCP-Threads werden verwendet, wenn asynchrone E/A-Vorgänge ausgeführt werden, z. B. beim Lesen aus dem Netzwerk.

Der Threadpool stellt neue Workerthreads oder E/A-Abschlussthreads nach Bedarf (und ohne Drosselung) bereit, bis die Einstellung für das Minimum des jeweiligen Threadtyps erreicht ist. Standardmäßig entspricht die minimale Anzahl von Threads der Anzahl von Prozessoren in einem System.

Wenn die Anzahl der vorhandenen (ausgelasteten) Threads die „minimale“ Anzahl von Threads erreicht, drosselt der Threadpool die Rate, mit der er neue Threads hinzufügt, auf einen Thread pro 500 Millisekunden. Wenn bei Ihrem System eine Arbeitsspitze eingeht, die einen IOCP-Thread benötigt, werden diese Arbeitsvorgänge in der Regel schnell verarbeitet. Wenn für die Arbeitsspitze jedoch mehr Threads erforderlich sind als in der konfigurierten Einstellung für das Minimum vorgesehen, tritt eine gewisse Verzögerung bei der Verarbeitung einiger Arbeitsvorgänge auf. Der Threadpool wartet darauf, dass zwei Dinge geschehen:

1. Ein vorhandener Thread wird frei, um die Arbeitsvorgänge zu verarbeiten.
2. Es wird 500 ms lang kein vorhandener Thread frei, weshalb dann ein neuer Thread erstellt wird.

Im Grunde genommen heißt dass: Wenn die Anzahl von ausgelasteten Threads größer ist als die minimale Anzahl von Threads, muss wahrscheinlich eine Verzögerung von 500 ms in Kauf genommen werden, bevor der Netzwerkdatenverkehr von der Anwendung verarbeitet wird. Außerdem gilt es zu beachten, dass ein Thread nach einem längeren Leerlauf als 15 Sekunden (soweit ich mich erinnere) bereinigt wird. Danach kann der Zyklus aus Wachstum und Schrumpfung erneut beginnen.

Betrachten wir eine Beispielfehlermeldung aus StackExchange.Redis (Build 1.0.450 oder höher). Sie sehen, dass nun Threadpool-Statistiken ausgegeben werden (siehe die Details für IOCP und WORKER unten).

```
System.TimeoutException: Timeout performing GET MyKey, inst: 2, mgr: Inactive,
queue: 6, qu: 0, qs: 6, qc: 0, wr: 0, wq: 0, in: 0, ar: 0,
IOCP: (Busy=6,Free=994,Min=4,Max=1000),
WORKER: (Busy=3,Free=997,Min=4,Max=1000)
```

Im obigen Beispiel sehen Sie, dass bei den IOCP-Threads sechs Threads ausgelastet sind, und dass das System für ein Minimum von vier Threads konfiguriert ist. In diesem Fall wird der Client wahrscheinlich zwei Verzögerungen von 500 ms hingenommen haben, weil 6 größer als 4 ist.

Beachten Sie, dass für StackExchange.Redis ein Timout eintreten kann, wenn IOCP- oder WORKER-Threads gedrosselt werden.

### <a name="recommendation"></a>Empfehlung

Vor dem Hintergrund dieser Informationen empfehlen wir dringend, als minimalen Wert für IOCP- und WORKER-Threads einen höheren Wert als den Standardwert zu konfigurieren. Wir können keine allgemeingültigen Richtlinien zur Größe dieses Werts geben, weil der richtige Wert für eine Anwendung für eine andere Anwendung wahrscheinlich zu hoch oder zu niedrig sein wird. Diese Einstellung kann sich auch auf die Leistung von anderen Teilen komplexer Anwendungen auswirken. Daher muss jeder Kunde diese Einstellung gemäß seinen eigenen Anforderungen optimieren. Ein guter Ausgangspunkt sind 200 oder 300 Threads. Testen Sie diese Einstellung, und passen Sie sie nach Bedarf an.

So konfigurieren Sie diese Einstellung:

* Wir empfehlen, diese Einstellung programmgesteuert zu ändern, indem Sie die [ThreadPool.SetMinThreads (...) ](/dotnet/api/system.threading.threadpool.setminthreads#System_Threading_ThreadPool_SetMinThreads_System_Int32_System_Int32_)-Methode in `global.asax.cs` verwenden. Beispiel:

    ```csharp
    private readonly int minThreads = 200;
    void Application_Start(object sender, EventArgs e)
    {
        // Code that runs on application startup
        AreaRegistration.RegisterAllAreas();
        RouteConfig.RegisterRoutes(RouteTable.Routes);
        BundleConfig.RegisterBundles(BundleTable.Bundles);
        ThreadPool.SetMinThreads(minThreads, minThreads);
    }
    ```

    > [!NOTE]
    > Der von dieser Methode festgelegte Wert ist eine globale Einstellung, die die gesamte AppDomain betrifft. Wenn Sie beispielsweise über einen Computer mit 4 Kernen verfügen und *minWorkerThreads* und *minIOThreads* auf 50 pro CPU während der Laufzeit festlegen möchten, verwenden Sie **ThreadPool.SetMinThreads (200, 200)** .

* Sie können außerdem die Einstellung für die Mindestanzahl von Threads mithilfe der Konfigurationseinstellungen [*minIoThreads* bzw. *minWorkerThreads*](https://msdn.microsoft.com/library/vstudio/7w2sway1(v=vs.100).aspx) unter dem Konfigurationselement `<processModel>` in `Machine.config` angeben, das sich normalerweise in `%SystemRoot%\Microsoft.NET\Framework\[versionNumber]\CONFIG\` befindet. **Es wird generell nicht empfohlen, die Mindestanzahl von Threads auf diese Weise festzulegen, weil es sich um eine systemweite Einstellung handelt.**

  > [!NOTE]
  > Der in diesem Konfigurationselement angegebene Wert ist eine Einstellung *pro Kern*. Wenn Ihr Computer z.B. über vier Kerne verfügt und Sie eine *minIoThreads*-Einstellung von 200 zur Laufzeit festlegen möchten, müssen Sie `<processModel minIoThreads="50"/>` verwenden.
  >

<a name="server-gc"></a>

### <a name="enable-server-gc-to-get-more-throughput-on-the-client-when-using-stackexchangeredis"></a>Aktivieren der Garbage Collection auf dem Server-, um bei Verwenden von „StackExchange.Redis“ mehr Durchsatz auf dem Client zu erzielen
Das Aktivieren der Garbage Collection auf dem Server kann den Client optimieren und bei Verwenden von „StackExchange.Redis“ mehr Leistung und Durchsatz ermöglichen. Weitere Informationen zur Garbage Collection auf dem Server und ihrer Aktivierung finden Sie in den folgenden Artikeln:

* [So aktivieren Sie die Garbage Collection auf dem Server](/dotnet/framework/configure-apps/file-schema/runtime/gcserver-element)
* [Grundlagen der Garbage Collection](/dotnet/standard/garbage-collection/fundamentals)
* [Garbage Collection und Leistung](/dotnet/standard/garbage-collection/performance)


### <a name="performance-considerations-around-connections"></a>Überlegungen zur Leistung im Zusammenhang mit Verbindungen

Jeder Tarif hat verschiedene Limits für Clientverbindungen, Speicher und Bandbreite. Obgleich jede Cachegröße eine *bestimmte* Anzahl von Verbindungen zulässt, fällt für jede Verbindung ein Mehraufwand an. Ein Beispiel für einen solchen Aufwand ist die CPU- und Arbeitsspeicherauslastung aufgrund der TLS-/SSL-Verschlüsselung. Das maximale Verbindungslimit für eine angegebene Cachegröße geht von einem geringfügig ausgelasteten Cache aus. Wenn die Last des Verbindungsaufwands *plus* die Last von Clientvorgängen die Systemkapazität überschreiten, können im Cache Kapazitätsprobleme entstehen, auch wenn Sie das Verbindungslimit für die aktuelle Cachegröße nicht überschritten haben.

Weitere Informationen zu den verschiedenen Verbindungsgrenzwerten für die einzelnen Ebenen finden Sie unter [Azure Cache for Redis – Preise](https://azure.microsoft.com/pricing/details/cache/). Weitere Informationen zu Verbindungen und anderen Standardkonfigurationen finden Sie unter [Standardmäßige Redis-Serverkonfiguration](cache-configure.md#default-redis-server-configuration).

<a name="cache-monitor"></a>

### <a name="how-do-i-monitor-the-health-and-performance-of-my-cache"></a>Wie überwache ich die Integrität und Leistung meines Caches?
Microsoft Azure Cache for Redis-Instanzen können im [Azure-Portal](https://portal.azure.com)überwacht werden. Sie können Metriken anzeigen, Metrikdiagramme an das Startmenü anheften, Daten- und Zeitbereiche für Überwachungsdiagramme anpassen, Metriken aus Diagrammen hinzufügen und entfernen sowie Warnungen festlegen, die ausgelöst werden, wenn bestimmte Bedingungen erfüllt sind. Weitere Informationen finden Sie unter [Überwachen von Azure Cache for Redis](cache-how-to-monitor.md).

Das **Ressourcenmenü** von Azure Cache for Redis enthält auch mehrere Tools zur Überwachung und Problembehandlung für Ihre Caches.

* **Diagnose und Problembehandlung** bietet Informationen zu häufigen Problemen sowie Strategien zu deren Behebung.
* **Ressourcenintegrität** dienen zum Überwachen Ihrer Ressource und informieren Sie darüber, ob sie wie erwartet ausgeführt wird. Weitere Informationen zum Azure Resource Health-Dienst finden Sie in der [Übersicht über Azure Resource Health](../resource-health/resource-health-overview.md).
* **Neue Supportanfrage** bietet Optionen, um eine Supportanfrage für Ihren Cache zu öffnen.

Diese Tools ermöglichen es Ihnen, die Integrität Ihrer Azure Cache for Redis-Instanzen zu überwachen, und unterstützen Sie beim Verwalten Ihrer Cachinganwendungen. Weitere Informationen finden Sie im Abschnitt „Einstellungen zu Support und Problembehandlung“ des Artikels [Gewusst wie: Konfigurieren von Azure Cache for Redis](cache-configure.md).

<a name="cache-timeouts"></a>

### <a name="why-am-i-seeing-timeouts"></a>Warum kommt es zu Timeouts?
Timeouts treten auf dem Client auf, der mit Redis kommuniziert. Wenn ein Befehl an den Redis-Server gesendet wird, wird dieser in die Warteschlange eingereiht, bis der Redis-Server den Befehl auswählt und ausführt. Auf dem Client kann es bei diesem Vorgang zu einem Timeout kommen. In diesem Fall wird auf der aufrufenden Seite eine Ausnahme ausgelöst. Weitere Informationen zur Behandlung von Timeoutproblemen finden Sie unter [Behandeln von clientseitigen Problemen](cache-troubleshoot-client.md) und [StackExchange.Redis-Timeoutausnahmen](cache-troubleshoot-timeouts.md#stackexchangeredis-timeout-exceptions).

<a name="cache-disconnect"></a>

### <a name="why-was-my-client-disconnected-from-the-cache"></a>Warum wurde mein Client vom Cache getrennt?
Nachfolgend werden einige Gründe dafür aufgeführt, warum ein Cache getrennt wird.

* Gründe auf Clientseite
  * Die Clientanwendung wurde neu bereitgestellt.
  * Die Clientanwendung hat eine Skalierung ausgeführt.
    * Bei Cloud Services oder Web-Apps kann dies aufgrund einer automatischen Skalierung der Fall sein.
  * Die Netzwerkschicht auf Clientseite wurde geändert.
  * Vorübergehende Fehler auf dem Client oder in den Netzwerkknoten zwischen Client und Server.
  * Die Bandbreitenschwellenwerte wurden erreicht.
  * Die Ausführung CPU-bezogener Vorgänge hat zu viel Zeit in Anspruch genommen.
* Gründe auf Serverseite
  * Im Standard-Cacheangebot initiiert der Azure Cache for Redis-Dienst ein Failover vom primären Knoten auf den Replikatknoten.
  * Azure hat ein Patching der Instanz durchgeführt, auf dem der Cache bereitgestellt wurde.
    * Dies kann bei Redis-Serverupdates oder im Rahmen der allgemeinen VM-Wartung der Fall sein.

### <a name="which-azure-cache-offering-is-right-for-me"></a>Welches Azure-Cache-Angebot ist das Richtige für mich?
> [!IMPORTANT]
> Gemäß einer [Ankündigung](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/) im letzten Jahr wurden die Dienste Azure Managed Cache Service und Azure In-Role Cache am 30. November 2016 **deaktiviert**. Es wird empfohlen, [Azure Cache for Redis](https://azure.microsoft.com/services/cache/) zu verwenden. Informationen zur Migration finden Sie unter [Migrieren von Managed Cache Service zu Azure Cache for Redis](cache-migrate-to-redis.md).
>
>

### <a name="azure-cache-for-redis"></a>Azure Cache for Redis
Azure Cache for Redis ist allgemein verfügbar in Größen bis zu 120 GB mit einer Verfügbarkeits-SLA von 99,9 %. Der neue [Premium-Tarif](cache-premium-tier-intro.md) bietet Größen bis zu 1,2 TB und unterstützt Clustering, VNET sowie Persistenz mit einer SLA von 99,9 %.

Mit Azure Cache for Redis erhalten Kunden Zugriff auf eine sichere, dedizierte Azure Cache for Redis-Instanz, die von Microsoft verwaltet wird. Mit diesem Angebot können Sie die umfangreichen Features und die Umgebung von Redis nutzen und dabei von zuverlässigem Hosting und Überwachung durch Microsoft profitieren.

Im Gegensatz zu traditionellen Caches, die nur mit Schlüssel-Wert-Paaren arbeiten, wird Redis besonders für seine äußerst leistungsfähigen Datentypen geschätzt. Redis unterstützt außerdem die Ausführung atomarer Vorgänge für diese Typen, z. B. das Anhängen an eine Zeichenfolge, das Erhöhen des Werts in einem Hash, das Einfügen per Push in eine Liste, das Berechnen der Schnittmenge, der Gesamtmenge und der Differenz oder das Abrufen des Elements mit der höchsten Rangfolge in einem sortierten Satz. Weitere Funktionen sind unter anderem die Unterstützung von Transaktionen, Pub/Sub, Lua-Skripts, Schlüssel mit begrenzter Lebensdauer und Konfigurationseinstellungen, mit denen sich Redis wie ein herkömmlicher Cache verhält.

Ein weiterer wichtiger Aspekt für den Erfolg von Redis ist das gesunde, lebendige Open Source-Ökosystem. Dies spiegelt sich in den verschiedenen verfügbaren Redis-Clients in mehreren Sprachen wider. Dieses Ökosystem und eine breite Palette von Clients ermöglichen, dass Azure Cache for Redis von nahezu allen Workloads verwendet werden kann, die Sie in Azure erstellen können.

Weitere Informationen zu den ersten Schritten mit Azure Cache for Redis finden Sie unter [Verwenden von Azure Cache for Redis](cache-dotnet-how-to-use-azure-redis-cache.md) und in der [Azure Cache for Redis-Dokumentation](index.yml).

### <a name="managed-cache-service"></a>Managed Cache Service
[Managed Cache Service wurde am 30. November 2016 außer Betrieb gesetzt.](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/)

Archivierte Dokumentationen finden Sie unter [Archived Managed Cache Service Documentation](/previous-versions/azure/azure-services/dn386094(v=azure.100)) (Archivierte Dokumentation zu Managed Cache Service).

### <a name="in-role-cache"></a>In-Role Cache
[In-Role Cache wurde am 30. November 2016 eingestellt.](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/)

Archivierte Dokumentationen finden Sie unter [Archived In-Role Cache Documentation](/previous-versions/azure/azure-services/dn386103(v=azure.100)) (archivierte Dokumentation zu Azure In-Role Cache).

["minIoThreads" configuration setting]: https://msdn.microsoft.com/library/vstudio/7w2sway1(v=vs.100).aspx

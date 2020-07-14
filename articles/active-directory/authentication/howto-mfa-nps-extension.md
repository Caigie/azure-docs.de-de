---
title: Bereitstellen von Azure MFA-Funktionen mit NPS – Azure Active Directory
description: Hinzufügen von cloudbasierten Funktionen für eine zweistufige Überprüfung zu Ihrer vorhandenen Authentifizierungsinfrastruktur
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.custom: has-adal-ref
ms.openlocfilehash: ca244136178c9c05f2b88a917219035451d5e391
ms.sourcegitcommit: cec9676ec235ff798d2a5cad6ee45f98a421837b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85848475"
---
# <a name="integrate-your-existing-nps-infrastructure-with-azure-multi-factor-authentication"></a>Integrieren Ihrer vorhandenen NPS-Infrastruktur in Azure Multi-Factor Authentication

Die NPS-Erweiterung (Network Policy Server, Netzwerkrichtlinienserver) für Azure MFA fügt Ihrer Authentifizierungsinfrastruktur unter Verwendung Ihrer vorhandenen Server cloudbasierte MFA-Funktionen hinzu. Mit der NPS-Erweiterung können Sie Ihrem bestehenden Authentifizierungsvorgang eine Überprüfung per Telefonanruf, SMS oder Telefon-App hinzufügen, ohne neue Server installieren, konfigurieren und verwalten zu müssen. 

Diese Erweiterung wurde für Organisationen entwickelt, die VPN-Verbindungen schützen möchten, ohne den Azure MFA-Server bereitzustellen. Die NPS-Erweiterung fungiert als Adapter zwischen RADIUS und cloudbasierter Azure MFA, um eine zweite Stufe der Authentifizierung für Verbund- oder synchronisierte Benutzer bereitzustellen.

Bei Verwenden der NPS-Erweiterung für Azure MFA umfasst der Authentifizierungsprozess die folgenden Komponenten: 

1. Der **NAS/VPN-Server** empfängt Anforderungen von VPN-Clients und wandelt sie in RADIUS-Anforderungen an NPS-Server um. 
2. Der **NPS-Server** stellt eine Verbindung mit Active Directory her, um die primäre Authentifizierung für die RADIUS-Anforderungen durchzuführen und übergibt bei Erfolg die Anforderung an installierte Erweiterungen.  
3. Die **NPS-Erweiterung** löst eine Anforderung einer sekundären Authentifizierung an Azure MFA aus. Sobald die Erweiterung die Antwort empfängt und die MFA-Abfrage erfolgreich ist, wird die Authentifizierungsanforderung abgeschlossen, indem dem NPS-Server Sicherheitstoken bereitgestellt werden, die einen von Azure STS ausgegebenen MFA-Anspruch enthalten.  
4. **Azure MFA** kommuniziert mit Azure Active Directory zum Abrufen der Details des Benutzers und führt die sekundäre Authentifizierung mithilfe einer Überprüfungsmethode aus, die für den Benutzer konfiguriert ist.

Das folgende Diagramm veranschaulicht den allgemeinen Ablauf dieser Authentifizierungsanforderung: 

![Diagramm des Authentifizierungsablaufs](./media/howto-mfa-nps-extension/auth-flow.png)

## <a name="plan-your-deployment"></a>Planen der Bereitstellung

Die NPS-Erweiterung kümmert sich automatisch um die Redundanz, sodass keine spezielle Konfiguration erforderlich ist.

Sie können beliebig viele Azure MFA-fähige NPS-Server erstellen. Wenn Sie mehrere Server installieren, sollten Sie für jeden ein anderes Clientzertifikat verwenden. Durch das Erstellen eines Zertifikats für jeden Server können Sie jedes Zertifikat einzeln aktualisieren und müssen sich so keine Gedanken über Ausfallzeiten für Ihre einzelnen Server machen.

Da VPN-Server die Authentifizierungsanforderungen weiterleiten, müssen sie über neue NPS-Server mit Azure MFA informiert werden.

## <a name="prerequisites"></a>Voraussetzungen

Die NPS-Erweiterung soll mit der vorhandenen Infrastruktur zusammenarbeiten. Stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind, bevor Sie beginnen.

### <a name="licenses"></a>Lizenzen

Die NPS-Erweiterung für Azure MFA steht Kunden mit [Lizenzen für Azure Multi-Factor Authentication](multi-factor-authentication.md) zur Verfügung (enthalten in Azure AD Premium, EMS oder einer eigenständigen MFA-Lizenz). Verbrauchsbasierte Lizenzen für Azure MFA, z.B. pro Benutzer oder pro Authentifizierungslizenz, sind mit der NPS-Erweiterung nicht kompatibel. 

### <a name="software"></a>Software

Windows Server 2008 R2 SP1 oder höher

### <a name="libraries"></a>Bibliotheken

Diese Bibliotheken werden automatisch mit der Erweiterung installiert.

- [Visual C++ Redistributable Packages für Visual Studio 2013 (x64)](https://www.microsoft.com/download/details.aspx?id=40784)
- [Microsoft Azure Active Directory-Modul für Windows PowerShell, Version 1.1.166.0](https://www.powershellgallery.com/packages/MSOnline/1.1.166.0)

Das Microsoft Azure Active Directory-Modul für Windows PowerShell wird, sofern es nicht bereits vorhanden ist, über ein Konfigurationsskript installiert, das Sie als Teil des Installationsvorgangs ausführen. Es ist nicht erforderlich, dieses Modul vorab zu installieren, wenn es nicht bereits installiert ist.

Sie müssen die folgende Bibliothek manuell installieren:

- [Visual C++ Redistributable für Visual Studio 2015](https://www.microsoft.com/download/details.aspx?id=48145)

### <a name="azure-active-directory"></a>Azure Active Directory

Alle Benutzer der NPS-Erweiterung müssen mithilfe von Azure AD Connect mit Azure Active Directory synchronisiert und für MFA registriert werden.

Wenn Sie die Erweiterung installieren, benötigen Sie die *Mandanten-ID* und die Administratoranmeldeinformationen für Ihren Azure AD-Mandanten. Führen Sie die folgenden Schritte aus, um die Mandanten-ID abzurufen:

1. Melden Sie sich im [Azure-Portal](https://portal.azure.com) als globaler Administrator des Azure-Mandanten an.
1. Suchen Sie nach **Azure Active Directory**, und wählen Sie es aus.
1. Auf der Seite **Übersicht** werden die *Mandanteninformationen* angezeigt. Wählen Sie neben der *Mandanten-ID* das Symbol **Kopieren** aus, wie im folgenden Beispielscreenshot gezeigt:

   ![Abrufen der Mandanten-ID aus dem Azure-Portal](./media/howto-mfa-nps-extension/azure-active-directory-tenant-id-portal.png)

### <a name="network-requirements"></a>Netzwerkanforderungen

Der NPS-Server muss über die Ports 80 und 443 mit den folgenden URLs kommunizieren können.

- https:\//adnotifications.windowsazure.com
- https:\//login.microsoftonline.com
- https:\//credentials.azure.com

Darüber hinaus ist eine Verbindung zu den folgenden URLs erforderlich, um die [Einrichtung des Adapters mit dem bereitgestellten PowerShell-Skript](#run-the-powershell-script) abzuschließen.

- https:\//login.microsoftonline.com
- https:\//provisioningapi.microsoftonline.com
- https:\//aadcdn.msauth.net

## <a name="prepare-your-environment"></a>Vorbereiten der Umgebung

Vor der Installation der NPS-Erweiterung müssen Sie Ihre Umgebung für die Verarbeitung des Authentifizierungsdatenverkehrs vorbereiten.

### <a name="enable-the-nps-role-on-a-domain-joined-server"></a>Aktivieren der NPS-Rolle auf einem in die Domäne eingebundenen Server

Der NPS-Server stellt eine Verbindung mit Azure Active Directory her und authentifiziert die MFA-Anforderungen. Wählen Sie einen Server für diese Rolle aus. Sie sollten einen Server auswählen, der keine Anforderungen von anderen Diensten verarbeitet, da die NPS-Erweiterung für alle Anforderungen Fehler auslöst, die keine RADIUS-Anforderungen sind. Der NPS-Server muss als primärer und sekundärer Authentifizierungsserver für Ihre Umgebung eingerichtet werden. Eine Weiterleitung von RADIUS-Anfragen an einen anderen Server als Proxy ist nicht möglich.

1. Öffnen Sie auf dem Server den **Assistenten zum Hinzufügen von Rollen und Features** über das Schnellstartmenü im Server-Manager.
2. Wählen Sie als Installationstyp **Rollenbasierte oder featurebasierte Installation** aus.
3. Wählen Sie die Serverrolle **Netzwerkrichtlinien- und Zugriffsdienste** aus. Es wird möglicherweise ein Fenster angezeigt, in dem Sie über die erforderlichen Features für die Ausführung dieser Rolle informiert werden.
4. Gehen Sie den Assistenten bis zur Bestätigungsseite durch. Wählen Sie **Installieren** aus.

Nachdem Sie einen Server für NPS festgelegt haben, sollten Sie diesen Server für die Verarbeitung eingehender RADIUS-Anforderungen von der VPN-Lösung konfigurieren.

### <a name="configure-your-vpn-solution-to-communicate-with-the-nps-server"></a>Konfigurieren der VPN-Lösung für die Kommunikation mit dem NPS-Server

Die Schritte zum Konfigurieren der RADIUS-Authentifizierungsrichtlinie variieren in Abhängigkeit von der verwendeten VPN-Lösung. Konfigurieren Sie diese Richtlinie mit einem Verweis auf Ihren RADIUS-NPS-Server.

### <a name="sync-domain-users-to-the-cloud"></a>Synchronisieren von Domänenbenutzern in der Cloud

Dieser Schritt wurde möglicherweise bereits auf Ihrem Mandanten durchgeführt. Es ist aber ratsam zu überprüfen, ob Azure AD Connect die Datenbanken kürzlich synchronisiert hat.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als Administrator an.
2. Wählen Sie **Azure Active Directory** > **Azure AD Connect** aus.
3. Vergewissern Sie sich, dass der Synchronisierungsstatus **Aktiviert** lautet und dass die letzte Synchronisierung weniger als eine Stunde zurückliegt.

Wenn Sie einen neuen Synchronisierungslauf starten möchten, gehen Sie anhand der Anweisungen in [Azure AD Connect-Synchronisierung: Scheduler](../hybrid/how-to-connect-sync-feature-scheduler.md#start-the-scheduler) vor.

### <a name="determine-which-authentication-methods-your-users-can-use"></a>Bestimmen Sie die Authentifizierungsmethoden, die Ihre Benutzer verwenden können

Zwei Faktoren haben Einfluss darauf, welche Authentifizierungsmethoden mit der Bereitstellung einer NPS-Erweiterung verfügbar sind:

1. Der Kennwortverschlüsselungsalgorithmus wird zwischen dem RADIUS-Client (VPN, NetScaler-Server oder andere) und den NPS-Servern verwendet.
   - **PAP** unterstützt alle Authentifizierungsmethoden von Azure MFA in der Cloud: Telefonanruf, unidirektionale Textnachricht, Benachrichtigung über eine mobile App, OATH-Hardwaretoken und Überprüfungscode in der mobilen App.
   - **CHAPV2** und **EAP** unterstützt Telefonanruf und Benachrichtigung über eine mobile App.

      > [!NOTE]
      > Verwenden Sie bei der Bereitstellung der NPS-Erweiterung diese Faktoren, um auszuwerten, welche Methoden für Benutzer verfügbar sind. Wenn Ihr RADIUS-Client PAP unterstützt, der Client UX jedoch über kein Eingabefeld für einen Überprüfungscode verfügt, sind der Telefonanruf und die Benachrichtigung über eine mobile App die zwei unterstützten Optionen.
      >
      > Wenn die Benutzeroberfläche Ihres VPN-Clients kein Eingabefeld unterstützt und Sie eine Netzwerkzugriffsrichtlinie konfiguriert haben, kann zwar die Authentifizierung erfolgreich sein, aber keines der in der Netzwerkrichtlinie konfigurierten RADIUS-Attribute wird auf das Netzwerkzugriffsgerät, z. B. den RRAS-Server, oder den VPN-Client angewendet. Folglich hat der VPN-Client möglicherweise mehr Zugriff als gewünscht, weniger oder gar keinen Zugriff.
      >

2. Die Eingabemethoden, die von der Clientanwendung (VPN, NetScaler-Server oder andere) verarbeitet werden kann. Beispiel: Verfügt der VPN-Client über Mittel, die es dem Benutzer erlauben, einen Überprüfungscode aus einem Text oder einer mobilen App einzugeben?

Sie können [nicht unterstützte Authentifizierungsmethoden](howto-mfa-mfasettings.md#verification-methods) in Azure deaktivieren.

### <a name="register-users-for-mfa"></a>Registrieren von Benutzern für MFA

Bevor Sie die NPS-Erweiterung bereitstellen und verwenden, müssen Benutzer, die die zweistufige Überprüfung durchführen müssen, für MFA registriert werden. Vorher benötigen Sie zum Testen der Erweiterung bei der Bereitstellung zumindest ein Testkonto, das vollständig für Multi-Factor Authentication registriert ist.

Führen Sie die folgende Schritte aus, um ein Testkonto einzurichten:

1. Melden Sie sich bei [https://aka.ms/mfasetup](https://aka.ms/mfasetup) mit einem Testkonto an.
2. Befolgen Sie die Anweisungen zum Einrichten einer Überprüfungsmethode.
3. [Erstellen Sie eine Richtlinie für bedingten Zugriff](howto-mfa-getstarted.md#create-conditional-access-policy), um für das Testkonto eine mehrstufige Authentifizierung zu erzwingen.

> [!IMPORTANT]
>
> Vergewissern Sie sich, dass Benutzer sich erfolgreich für Azure Multi-Factor Authentication registriert haben. Wenn sich Benutzer zuvor nur für die Self-Service-Kennwortzurücksetzung (Self-Service Password Reset, SSPR) registriert haben, wird für ihr Konto *StrongAuthenticationMethods* aktiviert. Azure Multi-Factor Authentication wird erzwungen, wenn *StrongAuthenticationMethods* konfiguriert ist, auch wenn sich der Benutzer nur für SSPR registriert hat.
>
> Es kann eine kombinierte Sicherheitsregistrierung aktiviert werden, die SSPR und Azure Multi-Factor Authentication gleichzeitig konfiguriert. Weitere Informationen finden Sie unter [Aktivieren der kombinierten Registrierung von Sicherheitsinformationen in Azure Active Directory](howto-registration-mfa-sspr-combined.md).
>
> Sie können auch [Benutzer zur erneuten Registrierung von Authentifizierungsmethoden](howto-mfa-userdevicesettings.md#manage-user-authentication-options) zwingen, wenn sie zuvor nur SSPR aktiviert hatten.

## <a name="install-the-nps-extension"></a>Installieren der NPS-Erweiterung

> [!IMPORTANT]
> Installieren Sie die NPS-Erweiterung auf einem anderen Server als dem VPN-Zugriffspunkt.

### <a name="download-and-install-the-nps-extension-for-azure-mfa"></a>Herunterladen und Installieren der NPS-Erweiterung für Azure MFA

1. [Laden Sie die NPS-Erweiterung aus dem Microsoft Download Center herunter](https://aka.ms/npsmfa).
2. Kopieren Sie die Binärdatei auf den Netzwerkrichtlinienserver, der konfiguriert werden soll.
3. Führen Sie *setup.exe* aus, und folgen Sie den Installationsanweisungen. Wenn Fehler auftreten, überprüfen Sie, ob die beiden Bibliotheken im Abschnitt „Voraussetzungen“ installiert wurden.

#### <a name="upgrade-the-nps-extension"></a>Aktualisieren der NPS-Erweiterung

Wenn Sie eine bestehende NPS-Erweiterung aktualisieren, führen Sie die folgenden Schritte aus, um einen Neustart des zugrunde liegenden Servers zu vermeiden:

1. Deinstallieren der vorhandenen Version
1. Ausführen des neuen Installers
1. Starten des Network Policy Server (IAS)-Diensts

### <a name="run-the-powershell-script"></a>Ausführen des PowerShell-Skripts

Das Installationsprogramm erstellt ein PowerShell-Skript an diesem Speicherort: `C:\Program Files\Microsoft\AzureMfa\Config` (wobei „C:\“ Ihr Installationslaufwerk ist). Dieses PowerShell-Skript führt bei jeder Ausführung folgende Aktionen aus:

- Erstellen eines selbstsignierten Zertifikats
- Zuordnen des öffentlichen Schlüssels des Zertifikats zum Dienstprinzipal in Azure AD
- Speichern des Zertifikats im lokalen Zertifikatspeicher
- Gewähren des Zugriffs auf den privaten Schlüssel des Zertifikats für den Netzwerkbenutzer
- Neustarten des NPS

Außer wenn Sie Ihre eigenen Zertifikate nutzen möchten (anstelle der selbstsignierten Zertifikate, die das PowerShell-Skript generiert), führen Sie zum Abschließen der Installation das PowerShell-Skript aus. Wenn Sie die Erweiterung auf mehreren Servern installieren möchten, sollte jeder über ein eigenes Zertifikat verfügen.

1. Führen Sie Windows PowerShell als Administrator aus.
2. Wechseln Sie das Verzeichnis.

   ```powershell
   cd "C:\Program Files\Microsoft\AzureMfa\Config"
   ```

3. Führen Sie das PowerShell-Skript aus, das vom Installationsprogramm erstellt wurde.

   > [!IMPORTANT]
   > Für Kunden, die die Azure Government- oder Azure China 21ViaNet-Cloud verwenden, bearbeiten Sie zunächst die `Connect-MsolService`-Cmdlets im Skript *AzureMfaNpsExtnConfigSetup.ps1* so, dass die *AzureEnvironment*-Parameter für die betreffende Cloud enthalten sind. Geben Sie z. B. *-AzureEnvironment USGovernment* oder *-AzureEnvironment AzureChinaCloud* an.
   >
   > Weitere Informationen finden Sie in der [Referenz der Connect-MsolService-Parameter](/powershell/module/msonline/connect-msolservice#parameters).

   ```powershell
   .\AzureMfaNpsExtnConfigSetup.ps1
   ```

4. Melden Sie sich bei Azure AD als Administrator an.
5. PowerShell fordert Sie zur Angabe Ihrer Mandanten-ID auf. Verwenden Sie die *Mandanten-ID*-GUID, die Sie im Abschnitt „Voraussetzungen“ aus dem Azure-Portal kopiert haben.
6. PowerShell zeigt eine Erfolgsmeldung an, wenn die Skriptausführung abgeschlossen wurde.  

Wiederholen Sie diese Schritte für alle zusätzlichen NPS-Server, die Sie für den Lastenausgleich einrichten möchten.

Wenn das vorherige Computerzertifikat abgelaufen ist und ein neues Zertifikat generiert wurde, sollten Sie alle abgelaufenen Zertifikate löschen. Bei abgelaufenen Zertifikaten können beim Start der NPS-Erweiterung Probleme auftreten.

> [!NOTE]
> Wenn Sie Ihre eigenen Zertifikate verwenden, statt diese mit dem PowerShell-Skript zu generieren, stellen Sie sicher, dass sie den NPS-Namenskonventionen entsprechen. Der Antragstellernamen muss **CN=\<TenantID\>,OU=Microsoft NPS Extension** lauten.

### <a name="microsoft-azure-government-or-azure-china-21vianet-additional-steps"></a>Zusätzliche Schritte für Microsoft Azure Government oder Azure China 21ViaNet

Für Kunden, die die Azure Government- oder Azure China 21Vianet-Cloud verwenden, sind auf jedem NPS-Server die folgenden zusätzlichen Konfigurationsschritte erforderlich.

> [!IMPORTANT]
> Konfigurieren Sie diese Registrierungseinstellungen nur, wenn Sie Azure Government- oder Azure China 21Vianet-Kunde sind.

1. Wenn Sie Azure Government- oder Azure China 21Vianet-Kunde sind, öffnen Sie den **Registrierungs-Editor** auf dem NPS-Server.
1. Navigieren Sie zu `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\AzureMfa`.
1. Legen Sie für Azure Government Kunden die folgenden Schlüsselwerte fest:

    | Registrierungsschlüssel       | Wert |
    |--------------------|-----------------------------------|
    | AZURE_MFA_HOSTNAME | adnotifications.windowsazure.us   |
    | STS_URL            | https://login.microsoftonline.us/ |

1. Legen Sie für Azure China 21ViaNet-Kunden die folgenden Schlüsselwerte fest:

    | Registrierungsschlüssel       | Wert |
    |--------------------|-----------------------------------|
    | AZURE_MFA_HOSTNAME | adnotifications.windowsazure.cn   |
    | STS_URL            | https://login.chinacloudapi.cn/   |

1. Wiederholen Sie die beiden vorherigen Schritte, um für jeden NPS-Server die Registrierungsschlüsselwerte festzulegen.
1. Starten Sie auf jedem NPS-Server den NPS-Dienst erneut.

    Nehmen Sie für minimale Auswirkung jeden NPS-Server einzeln aus der NLB-Rotation heraus, und warten Sie, bis alle Verbindungen beendet sind.

### <a name="certificate-rollover"></a>Zertifikatrollover

Ab Release 1.0.1.32 der NPS-Erweiterung wird das Lesen von mehreren Zertifikaten unterstützt. Diese Funktion vereinfacht das Aktualisieren von Zertifikaten vor deren Ablauf. Wenn in Ihrer Organisation eine ältere Version der NPS-Erweiterung ausgeführt wird, sollten Sie ein Upgrade auf Version 1.0.1.32 oder höher durchführen.

Vom `AzureMfaNpsExtnConfigSetup.ps1`-Skript erstellte Zertifikate sind 2 Jahre gültig. IT-Organisationen sollten das Ablaufdatum von Zertifikaten überwachen. Zertifikate für die NPS-Erweiterung werden im Zertifikatspeicher des lokalen Computers unter „Persönlich“ gespeichert und für die Mandanten-ID ausgestellt, die für das Skript bereitgestellt wird.

Wenn sich das Ablaufdatum eines Zertifikats nähert, sollten Sie ein neues Zertifikat erstellen, um es zu ersetzen.  Dies können Sie durch erneutes Ausführen von `AzureMfaNpsExtnConfigSetup.ps1` erreichen. Behalten Sie dazu die Mandanten-ID bei, wenn Sie dazu aufgefordert werden. Dieser Vorgang sollte auf jedem NPS-Server in Ihrer Umgebung wiederholt werden.

## <a name="configure-your-nps-extension"></a>Konfigurieren der NPS-Erweiterung

Dieser Abschnitt enthält Überlegungen zum Entwurf und Vorschläge für erfolgreiche Bereitstellungen der NPS-Erweiterung.

### <a name="configuration-limitations"></a>Einschränkungen der Konfiguration

- Die NPS-Erweiterung bietet für Azure MFA keine Tools zum Migrieren von Benutzern und Einstellungen vom MFA-Server in die Cloud. Aus diesem Grund wird die Verwendung der Erweiterung für neue Bereitstellungen statt vorhandener Bereitstellung empfohlen. Wenn Sie die Erweiterung für eine vorhandene Bereitstellung verwenden, müssen die Benutzer die Bestätigung erneut ausführen, um ihre MFA-Details in der Cloud anzugeben.  
- Die NPS-Erweiterung verwendet für die Durchführung der sekundären Authentifizierung den Benutzerprinzipalnamen (User Principal Name, UPN) aus dem lokalen Active Directory zur Identifizierung des Benutzers in Azure MFA. Die Erweiterung kann für die Verwendung eines anderen Bezeichners wie einer alternativen Anmelde-ID oder eines benutzerdefinierten Active Directory-Felde (außer dem UPN) konfiguriert werden. Weitere Informationen finden Sie im Artikel [Erweiterte Konfigurationsoptionen für die NPS-Erweiterung für Multi-Factor Authentication](howto-mfa-nps-extension-advanced.md).
- Nicht alle Verschlüsselungsprotokolle unterstützen alle Überprüfungsmethoden.
   - **PAP** unterstützt Telefonanruf, unidirektionale Textnachricht, Benachrichtigung über eine mobile App und Überprüfungscode in der mobilen App
   - **CHAPV2** und **EAP** unterstützt Telefonanruf und Benachrichtigung über eine mobile App

### <a name="control-radius-clients-that-require-mfa"></a>Steuern von RADIUS-Clients, die MFA erfordern

Nachdem Sie MFA für einen RADIUS-Client mithilfe der NPS-Erweiterung aktiviert haben, muss bei allen Authentifizierungen für diesen Client MFA erfolgen. Wenn Sie MFA nur für einige RADIUS-Clients aktivieren möchten, für andere dagegen nicht, können Sie zwei NPS-Server konfigurieren und die Erweiterung nur auf einem installieren. Verweisen Sie RADIUS-Clients, die MFA zum Senden von Anforderungen auffordern sollen, an den mit der Erweiterung konfigurierten NPS-Server und andere RADIUS-Clients an den NPS-Server, der nicht mit der Erweiterung konfiguriert ist.

### <a name="prepare-for-users-that-arent-enrolled-for-mfa"></a>Vorbereiten von Benutzern, die nicht für MFA registriert sind

Wenn es Benutzer gibt, die nicht für MFA registriert sind, können Sie bestimmen, was geschieht, wenn sie versuchen, sie zu authentifizieren. Verwenden Sie die Registrierungseinstellung *REQUIRE_USER_MATCH* im Registrierungspfad *HKLM\Software\Microsoft\AzureMFA* zum Steuern des Verhaltens des Features. Diese Einstellung hat eine einzelne Konfigurationsoption:

| Schlüssel | Wert | Standard |
| --- | ----- | ------- |
| REQUIRE_USER_MATCH | TRUE/FALSE | Nicht festgelegt (gleichwertig mit TRUE) |

Der Zweck dieser Einstellung besteht darin, die Vorgehensweise für den Fall festzulegen, dass ein Benutzer nicht für die MFA registriert ist. Wenn der Schlüssel nicht vorhanden, nicht festgelegt oder auf TRUE festgelegt ist und der Benutzer nicht registriert ist, erfüllt die Erweiterung die MFA-Abfrage nicht. Wenn der Schlüssel auf FALSE festgelegt und der Benutzer nicht registriert ist, wird die Authentifizierung ohne MFA fortgesetzt. Wenn ein Benutzer für MFA registriert ist, muss er sich mit MFA authentifizieren, auch wenn REQUIRE_USER_MATCH auf FALSE festgelegt ist.

Sie können diesen Schlüssel erstellen und auf FALSE festlegen, während Ihre Benutzer eingebunden werden und möglicherweise noch nicht alle für Azure MFA registriert sind. Da das Festlegen des Schlüssels Benutzern, die nicht für MFA registriert sind, die Anmeldung erlaubt, müssen Sie diesen Schlüssel vor dem Wechsel in die Produktionsumgebung entfernen.

## <a name="troubleshooting"></a>Problembehandlung

### <a name="nps-extension-health-check-script"></a>Skript für die Integritätsprüfung der NPS-Erweiterung

Zum Ausführen der Schritte der grundlegenden Integritätsprüfung bei der Problembehandlung der NPS-Erweiterung steht das folgende Skript zur Verfügung.

[MFA_NPS_Troubleshooter.ps1](https://docs.microsoft.com/samples/azure-samples/azure-mfa-nps-extension-health-check/azure-mfa-nps-extension-health-check/)

---

### <a name="how-do-i-verify-that-the-client-cert-is-installed-as-expected"></a>Wie überprüfe ich, ob das Clientzertifikat wie erwartet installiert ist?

Suchen Sie das vom Installationsprogramm erstellte selbstsignierte Zertifikat im Zertifikatspeicher, und prüfen Sie, ob der private Schlüssel dem Benutzer **NETZWERKDIENST** Berechtigungen erteilt. Das Zertifikat hat den Antragstellernamen **CN \<tenantid\>, OU = Microsoft NPS Extension**.

Selbstsignierte Zertifikate, die mit dem Skript *AzureMfaNpsExtnConfigSetup.ps1* generiert werden, haben eine Gültigkeitsdauer von zwei Jahren. Wenn Sie überprüfen, ob das Zertifikat installiert ist, sollten Sie auch überprüfen, ob das Zertifikat nicht abgelaufen ist.

---

### <a name="how-can-i-verify-that-my-client-cert-is-associated-to-my-tenant-in-azure-active-directory"></a>Wie kann ich überprüfen, ob mein Clientzertifikat meinem Mandanten in Azure Active Directory zugeordnet ist?

Öffnen Sie eine PowerShell-Eingabeaufforderung, und führen Sie die folgenden Befehle aus:

```powershell
import-module MSOnline
Connect-MsolService
Get-MsolServicePrincipalCredential -AppPrincipalId "981f26a1-7f43-403b-a875-f8b09b8cd720" -ReturnKeyValues 1
```

Diese Befehle geben alle Zertifikate aus, die Ihren Mandanten Ihrer Instanz der NPS-Erweiterung in der PowerShell-Sitzung zuordnen. Suchen Sie Ihr Zertifikat, indem Sie Ihr Clientzertifikat als Datei des Typs „Base64-codiert X.509 (.CER)“ ohne den privaten Schlüssel exportieren und es mit der Liste aus PowerShell vergleichen.

Der folgende Befehl erstellt eine Datei namens „npscertificate“ im CER-Format auf dem Laufwerk „C:“.

```powershell
import-module MSOnline
Connect-MsolService
Get-MsolServicePrincipalCredential -AppPrincipalId "981f26a1-7f43-403b-a875-f8b09b8cd720" -ReturnKeyValues 1 | select -ExpandProperty "value" | out-file c:\npscertificate.cer
```

Wenn Sie diesen Befehl ausgeführt haben, rufen Sie Laufwerk „C:“ auf, suchen Sie die Datei, und doppelklicken Sie darauf. Öffnen Sie die Details und scrollen Sie nach unten zu „Fingerabdruck“, vergleichen Sie den Fingerabdruck des auf dem Server installierten Zertifikats mit diesem. Der Zertifikatfingerabdruck muss übereinstimmen.

Die Zeitstempel „Valid-From“ und „Valid-Until“ in einem vom Menschen lesbaren Format dienen zum Herausfiltern offensichtlicher Fehler, wenn der Befehl mehrere Zertifikate zurückgibt.

---

### <a name="why-cannot-i-sign-in"></a>Warum kann ich mich nicht anmelden?

Überprüfen Sie, ob Ihr Kennwort nicht abgelaufen ist. Die NPS-Erweiterung unterstützt nicht das Ändern von Kennwörtern im Rahmen des Anmeldeworkflows. Wenden Sie sich an das IT-Team Ihrer Organisation, um weitere Unterstützung zu erhalten.

---

### <a name="why-are-my-requests-failing-with-adal-token-error"></a>Warum misslingen meine Anforderungen mit ADAL-Tokenfehler?

Dieser Fehler kann verschiedene Gründe haben. Gehen Sie zur Problembehandlung folgendermaßen vor:

1. Starten Sie den NPS-Server neu.
2. Vergewissern Sie sich, dass das Clientzertifikat wie erwartet installiert wurde.
3. Überprüfen Sie, ob das Zertifikat Ihrem Mandanten in Azure AD zugeordnet ist.
4. Überprüfen Sie auf dem Server, auf dem die Erweiterung ausgeführt wird, ob auf `https://login.microsoftonline.com/` zugegriffen werden kann.

---

### <a name="why-does-authentication-fail-with-an-error-in-http-logs-stating-that-the-user-is-not-found"></a>Warum misslingt die Authentifizierung mit einem Fehler in den HTTP-Protokollen, der besagt, dass der Benutzer nicht gefunden wurde?

Überprüfen Sie, ob AD Connect ausgeführt wird und der Benutzer in Windows Active Directory und Azure Active Directory vorhanden ist.

---

### <a name="why-do-i-see-http-connect-errors-in-logs-with-all-my-authentications-failing"></a>Warum enthalten meine Protokolle HTTP-Verbindungs- und Authentifizierungsfehler?

Überprüfen Sie auf dem Server, auf dem die NPS-Erweiterung ausgeführt wird, ob auf https://adnotifications.windowsazure.com zugegriffen werden kann.

---

### <a name="why-is-authentication-not-working-despite-a-valid-certificate-being-present"></a>Warum funktioniert die Authentifizierung trotz eines gültigen Zertifikats nicht?

Wenn das vorherige Computerzertifikat abgelaufen ist und ein neues Zertifikat generiert wurde, sollten Sie alle abgelaufenen Zertifikate löschen. Bei abgelaufenen Zertifikaten können beim Start der NPS-Erweiterung Probleme auftreten.

Um festzustellen, ob ein gültiges Zertifikat vorhanden ist, überprüfen Sie über MMC den Zertifikatspeicher des lokalen Computerkontos, und stellen Sie sicher, dass das Ablaufdatum des Zertifikats nicht überschritten wurde. Um ein neues gültiges Zertifikat zu generieren, führen Sie die Schritte im Abschnitt [Ausführen des PowerShell-Skripts](#run-the-powershell-script) erneut aus.

## <a name="managing-the-tlsssl-protocols-and-cipher-suites"></a>Verwalten von TLS/SSL-Protokollen und Verschlüsselungssammlungen

Es empfiehlt sich, ältere und weniger leistungsstarke Verschlüsselungssammlungen zu deaktivieren oder zu entfernen, sofern diese nicht von Ihrer Organisation benötigt werden. Informationen zur Durchführung dieser Aufgabe finden Sie im Artikel [Verwalten von SSL/TLS-Protokollen und Verschlüsselungssammlungen für AD FS](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/manage-ssl-protocols-in-ad-fs).

### <a name="additional-troubleshooting"></a>Weitere Informationen zur Problembehandlung

Zusätzliche Anleitungen zur Problembehandlung und mögliche Lösungen finden Sie im Artikel [Auflösen von Fehlermeldungen in der NPS-Erweiterung für Azure Multi-Factor Authentication](howto-mfa-nps-extension-errors.md).

## <a name="next-steps"></a>Nächste Schritte

- [Übersicht und Konfiguration des Netzwerkrichtlinienservers unter Windows Server](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-top)

- Konfigurieren Sie alternative IDs für die Anmeldung, oder richten Sie unter [Erweiterte Konfigurationsoptionen für die NPS-Erweiterung für Multi-Factor Authentication](howto-mfa-nps-extension-advanced.md) eine Ausnahmeliste für IP-Adressen ein, welche die zweistufige Überprüfung nicht ausführen müssen.

- Erfahren Sie, wie Sie [Remotedesktopgateway](howto-mfa-nps-extension-rdg.md) und [VPN-Server](howto-mfa-nps-extension-vpn.md) mithilfe der NPS-Erweiterung integrieren.

- [Auflösen von Fehlermeldungen in der NPS-Erweiterung für Azure Multi-Factor Authentication](howto-mfa-nps-extension-errors.md)

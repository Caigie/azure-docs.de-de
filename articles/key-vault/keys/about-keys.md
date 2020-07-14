---
title: Informationen zu Azure Key Vault-Schlüsseln – Azure Key Vault
description: Hier finden Sie eine Übersicht über die Azure Key Vault-REST-Schnittstelle sowie Informationen für Entwickler zu Schlüsseln.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: keys
ms.topic: overview
ms.date: 09/04/2019
ms.author: mbaldwin
ms.openlocfilehash: b9803726bf3a54eb31d3c2ebaddce11fb96472be
ms.sourcegitcommit: fdaad48994bdb9e35cdd445c31b4bac0dd006294
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/26/2020
ms.locfileid: "85413722"
---
# <a name="about-azure-key-vault-keys"></a>Informationen zu Azure Key Vault-Schlüsseln

Azure Key Vault unterstützt mehrere Schlüsseltypen und Algorithmen und ermöglicht die Verwendung von Hardwaresicherheitsmodulen (Hardware Security Modules, HSM) für Schlüssel von hohem Wert.

Kryptografische Schlüssel in Key Vault werden als JSON Web Key-Objekte (JWK) dargestellt. Die Spezifikationen von JavaScript Object Notation (JSON) und JavaScript Object Signing and Encryption (JOSE) lauten wie folgt:

-   [JSON Web Key (JWK)](https://tools.ietf.org/html/draft-ietf-jose-json-web-key)  
-   [JSON Web Encryption (JWE)](http://tools.ietf.org/html/draft-ietf-jose-json-web-encryption)  
-   [JSON Web Algorithms (JWA)](http://tools.ietf.org/html/draft-ietf-jose-json-web-algorithms)  
-   [JSON Web Signature (JWS)](https://tools.ietf.org/html/draft-ietf-jose-json-web-signature) 

Die grundlegenden JWK/JWA-Spezifikationen wurden erweitert, um Schlüsseltypen zu ermöglichen, die für die Key Vault-Implementierung eindeutig sind. Das Importieren von Schlüsseln mit anbieterspezifischer HSM-Paketerstellung ermöglicht einen sicheren Transport von Schlüsseln, die nur in Key Vault-HSMs verwendet werden dürfen. 

Azure Key Vault unterstützt sowohl softwaregeschützte als auch HSM-geschützte Schlüssel:

- **Softwaregeschützte Schlüssel**: Schlüssel, die in der Software von Key Vault verarbeitet, aber im Ruhezustand unter Verwendung eines Systemschlüssels, der sich in einem HSM befindet, verschlüsselt werden. Clients können einen vorhandenen RSA- oder EC-Schlüssel (Elliptic Curve, elliptische Kurve) importieren oder anfordern, dass Key Vault einen solchen Schlüssel generiert.
- **HSM-geschützte Schlüssel**: Schlüssel, die in einem HSM (Hardwaresicherheitsmodul) verarbeitet werden. Diese Schlüssel werden in einer der HSM Security Worlds von Key Vault geschützt (es gibt in jeder geografischen Region eine Security World, um die Isolation aufrechtzuerhalten). Clients können einen RSA- oder EC-Schlüssel importieren, entweder in softwaregeschützter Form oder durch Exportieren von einem kompatiblen HSM-Gerät. Clients können auch anfordern, dass Key Vault einen Schlüssel generiert. Dieser Schlüsseltyp fügt dem JWK das key_hsm-Attribut hinzu, um das HSM-Schlüsselmaterial zu tragen.

Weitere Informationen zu geografischen Grenzen finden Sie unter [Datenschutz](https://azure.microsoft.com/support/trust-center/privacy/).  

## <a name="cryptographic-protection"></a>Kryptografischer Schutz

Key Vault unterstützt nur RSA- und Elliptic Curve-Schlüssel. 

-   **EC**: Softwaregeschützter Elliptic Curve-Schlüssel.
-   **EC-HSM**: „Hard“-Elliptic Curve-Schlüssel.
-   **RSA**: Softwaregeschützter RSA-Schlüssel.
-   **RSA-HSM**: „Hard“-RSA-Schlüssel.

Key Vault unterstützt RSA-Schlüssel der Größen 2048, 3072 und 4096. Key Vault unterstützt die Elliptic Curve-Schlüsseltypen P-256, P-384, P-521 und P-256K (SECP256K1).

Die kryptografischen Module, die Key Vault verwendet – sowohl HSM als auch Software – sind FIPS-geprüft (Federal Information Processing Standards). Für die Ausführung im FIPS-Modus müssen Sie keine besonderen Maßnahmen ergreifen. Schlüssel, die als HSM-geschützt **erstellt** oder **importiert** wurden, werden in einem HSM verarbeitet und gemäß FIPS 140-2 Level 2 geprüft. Schlüssel, die als softwaregeschützt **erstellt** oder **importiert** wurden, werden in kryptografischen Modulen verarbeitet und gemäß FIPS 140-2 Level 1 geprüft.

###  <a name="ec-algorithms"></a>EC-Algorithmen
 Die folgenden Algorithmusbezeichner werden mit EC- und EC-HSM-Schlüsseln in Key Vault unterstützt. 

#### <a name="curve-types"></a>Kurventypen

-   **P-256**: Die NIST-Kurve P-256, definiert unter [DSS FIPS PUB 186-4](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.186-4.pdf).
-   **P-256K**: Die SEC-Kurve SECP256K1, definiert unter [SEC 2: Recommended Elliptic Curve Domain Parameters (SEC 2: Empfohlene Domänenparameter für elliptische Kurven)](https://www.secg.org/sec2-v2.pdf).
-   **P-384**: Die NIST-Kurve P-384, definiert unter [DSS FIPS PUB 186-4](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.186-4.pdf).
-   **P-521**: Die NIST-Kurve P-521, definiert unter [DSS FIPS PUB 186-4](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.186-4.pdf).

#### <a name="signverify"></a>SIGN/VERIFY

-   **ES256** – ECDSA für SHA-256-Hashs und -Schlüssel, erstellt mit P-256-Kurve. Dieser Algorithmus wird im [RFC 7518](https://tools.ietf.org/html/rfc7518) beschrieben.
-   **ES256K** – ECDSA für SHA-256-Hashs und -Schlüssel, erstellt mit P-256K-Kurve. Bei diesem Algorithmus steht die Standardisierung noch aus.
-   **ES384** – ECDSA für SHA-384-Hashs und -Schlüssel, erstellt mit P-384-Kurve. Dieser Algorithmus wird im [RFC 7518](https://tools.ietf.org/html/rfc7518) beschrieben.
-   **ES512** – ECDSA für SHA-512-Hashs und -Schlüssel, erstellt mit P-521-Kurve. Dieser Algorithmus wird im [RFC 7518](https://tools.ietf.org/html/rfc7518) beschrieben.

###  <a name="rsa-algorithms"></a>RSA-Algorithmen  
 Die folgenden Algorithmusbezeichner werden mit RSA- und RSA-HSM-Schlüsseln in Key Vault unterstützt.  

#### <a name="wrapkeyunwrapkey-encryptdecrypt"></a>WRAPKEY/UNWRAPKEY, ENCRYPT/DECRYPT

-   **RSA1_5**: RSAES-PKCS1-V1_5 [RFC3447] Schlüsselverschlüsselung  
-   **RSA-OAEP**: RSAES unter Verwendung von Optimal Asymmetric Encryption Padding (OAEP) [RFC3447], wobei die Standardparameter durch RFC 3447 in Abschnitt A.2.1 angegeben werden. Diese Standardparameter verwenden eine Hashfunktion von SHA-1 und eine Maskengenerierungsfunktion von MGF1 mit SHA-1.  

#### <a name="signverify"></a>SIGN/VERIFY

-   **PS256**: RSASSA-PSS unter Verwendung von SHA-256 und MGF1 mit SHA-256, wie in [RFC7518](https://tools.ietf.org/html/rfc7518) beschrieben.
-   **PS384**: RSASSA-PSS unter Verwendung von SHA-384 und MGF1 mit SHA-384, wie in [RFC7518](https://tools.ietf.org/html/rfc7518) beschrieben.
-   **PS512**: RSASSA-PSS unter Verwendung von SHA-512 und MGF1 mit SHA-512, wie in [RFC7518](https://tools.ietf.org/html/rfc7518) beschrieben.
-   **RS256**: RSASSA-PKCS-v1_5 mithilfe von SHA-256. Der von der Anwendung bereitgestellte Zusammenfassungswert muss mithilfe von SHA-256 berechnet werden und 32 Byte lang sein.  
-   **RS384**: RSASSA-PKCS-v1_5 mithilfe von SHA-384. Der von der Anwendung bereitgestellte Zusammenfassungswert muss mithilfe von SHA-384 berechnet werden und 48 Byte lang sein.  
-   **RS512**: RSASSA-PKCS-v1_5 mithilfe von SHA-512. Der von der Anwendung bereitgestellte Zusammenfassungswert muss mithilfe von SHA-512 berechnet werden und 64 Byte lang sein.  
-   **RSNULL**: Siehe [RFC2437], ein spezialisierter Anwendungsfall, um bestimmte TLS-Szenarios zu ermöglichen.  

##  <a name="key-operations"></a>Schlüsselvorgänge

Key Vault unterstützt die folgenden Vorgänge bei Schlüsselobjekten:  

-   **Erstellen**: Ermöglicht einem Client, einen Schlüssel in Key Vault zu erstellen. Der Wert des Schlüssels wird von Key Vault generiert und gespeichert und nicht für den Client freigegeben. In Key Vault können asymmetrische Schlüssel erstellt werden.  
-   **Import**: Ermöglicht einem Client, einen vorhandenen Schlüssel in Key Vault zu importieren. Asymmetrische Schlüssel können mithilfe einer Reihe unterschiedlicher Paketerstellungsmethoden in einem JWK-Konstrukt in Key Vault importiert werden. 
-   **Update** (Aktualisieren): Ermöglicht einem Client mit ausreichenden Berechtigungen, die Metadaten (Schlüsselattribute) zu ändern, die einem zuvor in Key Vault gespeicherten Schlüssel zugeordnet sind.  
-   **Löschen:** Ermöglicht einem Client mit ausreichenden Berechtigungen das Löschen eines Schlüssels aus Key Vault.  
-   **List** (Auflisten): Ermöglicht einem Client das Auflisten aller Schlüssel in einem bestimmten Schlüsseltresor.  
-   **List versions** (Versionen auflisten): Ermöglicht einem Client das Auflisten aller Versionen eines bestimmten Schlüssels in einem bestimmten Key Vault.  
-   **Get** (Abrufen): Ermöglicht einem Client das Abrufen des öffentlichen Teils eines bestimmten Schlüssels in einem Schlüsseltresor.  
-   **Backup** (Sichern): Exportiert einen Schlüssel in einer geschützten Form.  
-   **Restore** (Wiederherstellen): Importiert einen zuvor gesicherten Schlüssel.  

Weitere Informationen finden Sie unter [Schlüsselvorgänge in der Referenz zur REST-API für Azure Key Vault](/rest/api/keyvault).  

Nachdem ein Schlüssel in Key Vault erstellt wurde, können die folgenden kryptografischen Vorgänge mit dem Schlüssel ausgeführt werden:  

-   **Sign an Verify** (Signieren und überprüfen): Streng genommen handelt es sich hierbei um den Vorgang „sign hash“ (Hash signieren) oder „verify hash“ (Hash überprüfen), da Key Vault das Hashing von Inhalten im Rahmen der Signaturerstellung nicht unterstützt. Anwendungen sollten ein Hashing der Daten durchführen, die lokal signiert werden sollen, und dann anfordern, dass Key Vault den Hash signiert. Die Überprüfung signierter Hashes wird aus Gründen der Vereinfachung für Anwendungen unterstützt, die möglicherweise keinen Zugriff auf (öffentliches) Schlüsselmaterial haben. Um eine optimale Anwendungsleistung zu erzielen, sollten VERIFY-Vorgänge lokal ausgeführt werden.  
-   **Key Encryption / Wrapping** (Verschlüsselung mit Schlüssel / Umschließen): Ein in Key Vault gespeicherter Schlüssel kann zum Schutz eines anderen Schlüssels verwendet werden. In der Regel handelt es sich dabei um einen symmetrischen Inhaltsverschlüsselungsschlüssel (Content Encryption Key, CEK). Wenn der Schlüssel in Key Vault asymmetrisch ist, wird die Schlüsselverschlüsselung verwendet. RSA-OAEP und die WRAPKEY/UNWRAPKEY-Vorgänge sind beispielsweise äquivalent zu ENCRYPT/DECRYPT. Wenn der Schlüssel in Key Vault symmetrisch ist, wird die Schlüsselumschließung verwendet. Beispiel: AES-KW. Der WRAPKEY-Vorgang wird aus Gründen der Vereinfachung für Anwendungen unterstützt, die möglicherweise keinen Zugriff auf (öffentliches) Schlüsselmaterial haben. Um eine optimale Anwendungsleistung zu erzielen, sollten WRAPKEY-Vorgänge lokal ausgeführt werden.  
-   **Encrypt and Decrypt** (Verschlüsseln und Entschlüsseln): Ein in Key Vault gespeicherter Schlüssel kann zum Verschlüsseln oder Entschlüsseln eines einzelnen Datenblocks verwendet werden. Die Größe des Blocks wird durch den Schlüsseltyp und den ausgewählten Verschlüsselungsalgorithmus bestimmt. Der ENCRYPT-Vorgang wird aus Gründen der Vereinfachung für Anwendungen unterstützt, die möglicherweise keinen Zugriff auf (öffentliches) Schlüsselmaterial haben. Um eine optimale Anwendungsleistung zu erzielen, sollten ENCRYPT-Vorgänge lokal ausgeführt werden.  

Obwohl WRAPKEY/UNWRAPKEY mit asymmetrischen Schlüsseln überflüssig erscheinen mögen (da die Vorgänge äquivalent zu ENCRYPT/DECRYPT sind), ist die Verwendung unterschiedlicher Vorgänge wichtig. Die Unterscheidung sorgt für eine semantische und autorisierungsbezogene Trennung dieser Vorgänge sowie für Konsistenz, wenn andere Schlüsseltypen vom Dienst unterstützt werden.  

Key Vault unterstützt keine EXPORT-Vorgänge. Sobald ein Schlüssel im System bereitgestellt ist, kann er weder extrahiert noch sein Schlüsselmaterial geändert werden. Möglicherweise benötigen Key Vault-Benutzer ihren Schlüssel jedoch für andere Anwendungsfälle, beispielsweise nachdem er gelöscht wurde. In diesem Fall können die Vorgänge BACKUP und RESTORE verwendet werden, um den Schlüssel in geschützter Form zu exportieren und zu importieren. Vom BACKUP-Vorgang erstellte Schlüssel können außerhalb von Key Vault nicht verwendet werden. Alternativ dazu kann der IMPORT-Vorgang für mehrere Key Vault-Instanzen verwendet werden.  

Benutzer können die kryptografischen Vorgänge, die Key Vault pro Schlüssel unterstützt, mithilfe der key_ops-Eigenschaft des JWK-Objekts einschränken.  

Weitere Informationen zu JWK-Objekten finden Sie unter [JSON Web Key (JWK)](https://tools.ietf.org/html/draft-ietf-jose-json-web-key-41).  

## <a name="key-attributes"></a>Schlüsselattribute

Zusätzlich zum Schlüsselmaterial können die folgenden Attribute angegeben werden. In einer JSON-Anforderung sind das Schlüsselwort „attributes“ und die Klammern „{“ „}“ auch dann erforderlich, wenn keine Attribute angegeben werden.  

- *enabled*: Boolesch, optional, Standardwert ist **true**. Gibt an, ob der Schlüssel aktiviert und für kryptografische Vorgänge verwendbar ist. Das Attribut *enabled* wird in Verbindung mit *nbf* und *exp* verwendet. Wenn ein Vorgang zwischen *nbf* und *exp* auftritt, wird er nur zugelassen, wenn *enabled* auf **true** gesetzt ist. Vorgänge außerhalb des *nbf* / *exp*-Fensters sind automatisch nicht zugelassen, mit Ausnahme bestimmter Vorgangstypen unter [besonderen Bedingungen](#date-time-controlled-operations).
- *nbf*: IntDate, optional, Standardwert ist „now“. Das Attribut *nbf* („not before“, nicht vor) gibt den Zeitpunkt an, vor dem der Schlüssel NICHT für kryptografische Vorgänge verwendet werden darf, mit Ausnahme bestimmter Vorgangstypen unter [besonderen Bedingungen](#date-time-controlled-operations). Die Verarbeitung des *nbf*-Attributs erfordert, dass das aktuelle Datum / die aktuelle Uhrzeit mindestens dem Wert des *nbf*-Attributs entsprechen MUSS. Key Vault KANN einen kleinen Spielraum von in der Regel nicht mehr als einigen wenigen Minuten einräumen, um Abweichungen der Uhr zu berücksichtigen. Der Wert MUSS eine Zahl sein, die einen IntDate-Wert enthält.  
- *exp*: IntDate, optional, Standardwert ist „forever“. Das Attribut *exp* („expiration time“, Ablaufzeit) gibt die Ablaufzeit an, nach deren Verstreichen der Schlüssel NICHT für kryptografische Vorgänge verwendet werden darf, mit Ausnahme bestimmter Vorgangstypen unter [besonderen Bedingungen](#date-time-controlled-operations). Die Verarbeitung des *exp*-Attributs erfordert, dass das aktuelle Datum / die aktuelle Uhrzeit vor dem Wert des *exp*-Attributs liegen MUSS. Key Vault KANN einen kleinen Spielraum von in der Regel nicht mehr als einigen wenigen Minuten einräumen, um Abweichungen der Uhr zu berücksichtigen. Der Wert MUSS eine Zahl sein, die einen IntDate-Wert enthält.  

Es gibt zusätzliche schreibgeschützte Attribute, die in alle Antworten einbezogen werden, die Schlüsselattribute enthalten:  

- *created*: IntDate, optional. Das *created*-Attribut gibt an, wann diese Version des Schlüssels erstellt wurde. Der Wert ist NULL für Schlüssel, die vor dem Hinzufügen dieses Attributs erstellt wurden. Der Wert MUSS eine Zahl sein, die einen IntDate-Wert enthält.  
- *updated*: IntDate, optional. Das *updated*-Attribut gibt an, wann diese Version des Schlüssels aktualisiert wurde. Der Wert ist NULL für Schlüssel, die vor dem Hinzufügen dieses Attributs zuletzt aktualisiert wurden. Der Wert MUSS eine Zahl sein, die einen IntDate-Wert enthält.  

Weitere Informationen zu IntDate und anderen Datentypen finden Sie unter „Informationen zu Schlüsseln, Geheimnissen und Zertifikaten“ im Abschnitt [Datentypen](../general/about-keys-secrets-certificates.md#data-types).

### <a name="date-time-controlled-operations"></a>Durch Datum und Uhrzeit gesteuerte Vorgänge

Noch nicht gültige und abgelaufene Schlüssel, die außerhalb des *nbf* / *exp*-Fensters liegen, sind in **decrypt**-, **unwrap**- und **verify**-Vorgängen verwendbar (keine Rückgabe von „403 – verboten“). Grund für die Verwendung des „Noch-nicht-gültig“-Status ist, den Test eines Schlüssels vor der Verwendung in der Produktion zu ermöglichen. Grund für die Verwendung des abgelaufenen Zustands ist, Wiederherstellungsvorgänge bei Daten zu ermöglichen, die erstellt wurden, als der Schlüssel gültig war. Sie können den Zugriff auf einen Schlüssel auch mittels Key Vault-Richtlinien oder durch Aktualisieren des *enabled*-Schlüsselattributs mit **false** deaktivieren.

Weitere Informationen zu Datentypen finden Sie unter [Datentypen](../general/about-keys-secrets-certificates.md#data-types).

Weitere Informationen zu anderen möglichen Attributen finden Sie unter [JSON Web Key (JWK)](https://tools.ietf.org/html/draft-ietf-jose-json-web-key-41).

## <a name="key-tags"></a>Schlüsseltags

Sie können zusätzliche anwendungsspezifische Metadaten in Form von Tags angeben. Key Vault unterstützt bis zu 15 Tags, von denen jedes einen 256 Zeichen langen Namen und einen Wert von 256 Zeichen aufweisen kann.  

>[!Note]
>Tags sind für Aufrufer lesbar, die über die *list*- oder *get*-Berechtigung für diesen Objekttyp (Schlüssel, Geheimnisse oder Zertifikate) verfügen.

##  <a name="key-access-control"></a>Schlüsselzugriffssteuerung

Die Zugriffssteuerung für Schlüssel, die von Key Vault verwaltet werden, ist auf der Ebene eines Schlüsseltresors möglich, der als Schlüsselcontainer fungiert. Die Zugriffssteuerungsrichtlinie für Schlüssel unterscheidet sich von der Zugriffssteuerungsrichtlinie für Geheimnisse im selben Key Vault. Benutzer können einen oder mehrere Tresore zum Speichern von Schlüsseln erstellen und müssen für eine dem Szenario entsprechende Segmentierung und Verwaltung von Schlüsseln sorgen. Die Zugriffssteuerung für Schlüssel ist unabhängig von der Zugriffssteuerung für Geheimnisse.  

Die folgenden Berechtigungen können auf Benutzer-/Dienstprinzipalbasis im Schlüsselzugriffssteuerungs-Eintrag für einen Tresor erteilt werden. Diese Berechtigungen spiegeln die für ein Schlüsselobjekt zulässigen Vorgänge wider.  Das Gewähren des Zugriffs auf einen Dienstprinzipal im Schlüsseltresor ist ein einmaliger Vorgang und bleibt für alle Azure-Abonnements gleich. Sie können damit beliebig viele Zertifikate bereitstellen. 

- Berechtigungen für Schlüsselverwaltungsvorgänge
  - *get*: Lesen des öffentlichen Teils eines Schlüssels sowie seiner Attribute
  - *list*: Auflisten der Schlüssel oder Versionen eines in einem Schlüsseltresor gespeicherten Schlüssels
  - *update*: Aktualisieren der Attribute für einen Schlüssel
  - *create*: Erstellen neuer Schlüssel
  - *import*: Importieren eines Schlüssels in einen Schlüsseltresor
  - *delete*: Löschen des Schlüsselobjekts
  - *recover*: Wiederherstellen eines gelöschten Schlüssels
  - *backup*: Sichern eines Schlüssels in einem Schlüsseltresor
  - *restore*: Wiederherstellen eines gesicherten Schlüssels in einem Schlüsseltresor

- Berechtigungen für kryptografische Vorgänge
  - *decrypt*: Verwenden des Schlüssels zum Aufheben des Schutzes einer Folge von Bytes
  - *encrypt*: Verwenden des Schlüssels zum Schützen einer beliebigen Folge von Bytes
  - *unwrapKey*: Verwenden des Schlüssels zum Aufheben des Schutzes eines umschlossenen symmetrischen Schlüssels
  - *wrapKey*: Verwenden des Schlüssels zum Schützen eines symmetrischen Schlüssels
  - *verify*: Verwenden des Schlüssels zum Überprüfen von Digests  
  - *sign*: Verwenden des Schlüssels zum Signieren von Digests
    
- Berechtigungen für privilegierte Vorgänge
  - *purge*: Bereinigen (dauerhaftes Löschen) eines gelöschten Schlüssels

Weitere Informationen zum Arbeiten mit Schlüsseln finden Sie in der [REST-API-Referenz für Key Vault](/rest/api/keyvault). Informationen zum Einrichten von Berechtigungen finden Sie unter [Tresore – Erstellen oder Aktualisieren](/rest/api/keyvault/vaults/createorupdate) und [Vaults – Aktualisieren der Zugriffsrichtlinie](/rest/api/keyvault/vaults/updateaccesspolicy). 

## <a name="next-steps"></a>Nächste Schritte

- [Informationen zu Key Vault](../general/overview.md)
- [Informationen zu Schlüsseln, Geheimnissen und Zertifikaten](../general/about-keys-secrets-certificates.md)
- [Informationen zu Geheimnissen](../secrets/about-secrets.md)
- [Informationen zu Zertifikaten](../certificates/about-certificates.md)
- [Authentifizierung, Anforderungen und Antworten](../general/authentication-requests-and-responses.md)
- [Entwicklerhandbuch für Key Vault](../general/developers-guide.md)

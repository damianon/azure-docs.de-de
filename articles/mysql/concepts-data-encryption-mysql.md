---
title: 'Datenverschlüsselung mit einem vom Kunden verwalteten Schlüssel: Azure Database for MySQL'
description: Azure Database for MySQL-Datenverschlüsselung mit einem vom Kunden verwalteten Schlüssel ermöglicht Ihnen BYOK (Bring Your Own Key) für den Schutz von Daten im Ruhezustand. Sie bietet Organisationen auch eine Möglichkeit der Trennung von Aufgaben bei der Verwaltung von Schlüsseln und Daten.
author: kummanish
ms.author: manishku
ms.service: mysql
ms.topic: conceptual
ms.date: 01/13/2020
ms.openlocfilehash: a97fee619858aa024ff208b72d3b2594c30d2fd5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79299123"
---
# <a name="azure-database-for-mysql-data-encryption-with-a-customer-managed-key"></a>Azure Database for MySQL-Datenverschlüsselung mit einem vom Kunden verwalteten Schlüssel

> [!NOTE]
> Derzeit müssen Sie den Zugriff anfordern, um diese Funktion verwenden zu können. Wenden Sie sich zu diesem Zweck an AskAzureDBforMySQL@service.microsoft.com.

Datenverschlüsselung mit vom Kunden verwalteten Schlüsseln für Azure Database for MySQL ermöglicht Ihnen BYOK (Bring Your Own Key) für den Schutz von Daten im Ruhezustand. Sie bietet Organisationen auch eine Möglichkeit der Trennung von Aufgaben bei der Verwaltung von Schlüsseln und Daten. Bei der vom Kunden verwalteten Verschlüsselung ist der Kunde vollständig für die Verwaltung des Lebenszyklus von Schlüsseln und der Schlüsselnutzungsberechtigungen sowie für die Überwachung von Vorgängen für Schlüssel verantwortlich, hat damit aber auch vollständige Kontrolle.

Datenverschlüsselung mit vom Kunden verwalteten Schlüsseln für Azure Database for MySQL wird auf Serverebene festgelegt. Bei einem bestimmten Server wird ein vom Kunden verwalteter Schlüssel verwendet, der als Schlüsselverschlüsselungsschlüssel (Key Encryption Key, KEK) bezeichnet wird, um den vom Dienst verwendeten Datenverschlüsselungsschlüssel (Data Encryption Key, DEK) zu verschlüsseln. Der KEK ist ein asymmetrischer Schlüssel, der in einer vom Kunden verwalteten [Azure Key Vault](../key-vault/key-Vault-secure-your-key-Vault.md)-Instanz im Besitz des Kunden gespeichert wird. Der Schlüsselverschlüsselungsschlüssel (Key Encryption Key, KEK) und der Datenverschlüsselungsschlüssel (Data Encryption Key, DEK) werden weiter unten in diesem Artikel ausführlicher beschrieben.

Key Vault ist ein cloudbasiertes, externes Schlüsselverwaltungssystem. Es bietet hochverfügbaren und skalierbaren sicheren Speicher für RSA-Kryptografieschlüssel, der optional von FIPS 140-2 Level 2-geprüften Hardwaresicherheitsmodulen (HSM) geschützt wird. Dabei wird kein direkter Zugriff auf einen gespeicherten Schlüssel zugelassen, sondern Verschlüsselung und Entschlüsselung werden für die autorisierten Entitäten bereitgestellt. Key Vault kann den Schlüssel generieren, importieren oder [von einem lokalen HSM-Gerät](../key-vault/key-Vault-hsm-protected-keys.md) übertragen lassen.

> [!NOTE]
> Dieses Feature steht in allen Azure-Regionen zur Verfügung, in denen Azure Database for MySQL die Tarife „Universell“ und „Arbeitsspeicheroptimiert“ unterstützt.

## <a name="benefits"></a>Vorteile

Die Datenverschlüsselung für Azure Database for MySQL bietet die folgenden Vorteile:

* Datenzugriff wird von Ihnen vollständig durch die Möglichkeit gesteuert, den Schlüssel zu entfernen und so die Datenbank nicht zugänglich zu machen. 
* Vollständige Kontrolle über den Schlüssellebenszyklus, einschließlich der Rotation des Schlüssels zum Einhalten von Unternehmensrichtlinien
* Zentrale Verwaltung und Organisation von Schlüsseln in Azure Key Vault
* Möglichkeit zur Implementierung der Trennung von Aufgaben zwischen Sicherheitsbeauftragten, DBAs und Systemadministratoren


## <a name="terminology-and-description"></a>Terminologie und Beschreibung

**Datenverschlüsselungsschlüssel (Data Encryption Key, DEK)** : Ein symmetrischer AES256-Schlüssel zum Verschlüsseln einer Partition oder eines Datenblocks. Das Verschlüsseln jedes Datenblocks mit einem anderen Schlüssel erschwert Kryptoanalyseangriffe. Der Ressourcenanbieter oder die Anwendungsinstanz, die einen spezifischen Block ver- oder entschlüsselt, muss Zugriff auf die DEKs gewähren. Wenn Sie einen DEK durch einen neuen Schlüssel ersetzen, müssen nur die Daten im verknüpften Block erneut mit dem neuen Schlüssel verschlüsselt werden.

**Schlüsselverschlüsselungsschlüssel (Key Encryption Key, KEK)** : Ein Verschlüsselungsschlüssel, der zum Verschlüsseln der DEKs verwendet wird. Mit einem KEK, der Key Vault nie verlässt, können die DEKs selbst verschlüsselt und gesteuert werden. Die Entität, die auf den KEK zugreifen kann, kann sich von der Entität unterscheiden, die den DEK erfordert. Da der KEK erforderlich ist, um DEKs zu entschlüsseln, ist der KEK ein Punkt, an dem DEKs gelöscht werden können, indem der KEK gelöscht wird.

Die mit den KEKs verschlüsselten DEKs werden separat gespeichert. Nur eine Entität mit Zugriff auf den KEK kann diese DEKs entschlüsseln. Weitere Informationen finden Sie unter [Sicherheit bei der Verschlüsselung ruhender Daten](../security/fundamentals/encryption-atrest.md).

## <a name="how-data-encryption-with-a-customer-managed-key-works"></a>Funktionsweise der Datenverschlüsselung mit einem kundenseitig verwalteten Schlüssel

![Abbildung, die eine Übersicht über BYOK (Bring Your Own Key) zeigt](media/concepts-data-access-and-security-data-encryption/mysqloverview.png)

Damit ein MySQL-Server vom Kunden verwaltete Schlüssel, die in Key Vault gespeichert sind, für die Verschlüsselung des DEK verwenden kann, erteilt ein Key Vault-Administrator dem Server die folgenden Zugriffsrechte:

* **get**: Zum Abrufen des öffentlichen Teils und der Eigenschaften des Schlüssels in Key Vault.
* **wrapKey**: Zum Verschlüsseln des DEK erforderlich.
* **unwrapKey**: Zum Entschlüsseln des DEK erforderlich.

Der Key Vault-Administrator kann auch die [Protokollierung von Key Vault-Überwachungsereignissen aktivieren](../azure-monitor/insights/azure-key-vault.md), damit sie später überprüft werden können.

Wenn der Server für die Verwendung des in Key Vault gespeicherten vom Kunden verwalteten Schlüssels konfiguriert ist, sendet der Server den DEK für Verschlüsselungen an Key Vault. Key Vault gibt den verschlüsselten DEK zurück, der in der Benutzerdatenbank gespeichert wird. Entsprechend sendet der Server bei Bedarf den geschützten DEK zur Entschlüsselung an Key Vault. Prüfer können Azure Monitor verwenden, um die Überwachungsereignisprotokoll von Key Vault zu überprüfen, sofern die Protokollierung aktiviert wurde.

## <a name="requirements-for-configuring-data-encryption-for-azure-database-for-mysql"></a>Anforderungen für die Konfiguration der Datenverschlüsselung für Azure Database for MySQL

Nachfolgend werden die Anforderungen für die Konfiguration von Key Vault aufgeführt:

* Key Vault und Azure Database for MySQL müssen demselben Azure Active Directory-Mandanten (Azure AD) angehören. Mandantenübergreifende Interaktionen zwischen Key Vault und dem Server werden nicht unterstützt. Wenn Sie später Ressourcen verschieben möchten, müssen Sie die Datenverschlüsselung neu konfigurieren.
* Sie müssen die Funktion zum vorläufigen Löschen in Key Vault aktivieren, um bei einer versehentlichen Löschung des Schlüssels (oder Schlüsseltresors) Datenverluste zu vermeiden. Vorläufig gelöschte Ressourcen werden 90 Tage lang aufbewahrt, sofern sie der Benutzer nicht in der Zwischenzeit wiederherstellt oder bereinigt. Den Aktionen zum Wiederherstellen und endgültigen Löschen sind über Key Vault-Zugriffsrichtlinien eigene Berechtigungen zugewiesen. Die Funktion für vorläufiges Löschen ist standardmäßig deaktiviert. Sie können sie jedoch über PowerShell oder die Azure CLI aktivieren (beachten Sie, dass Sie sie nicht über das Azure-Portal aktivieren können).
* Gewähren Sie Azure Database for MySQL mit den Berechtigungen get, wrapKey, unwrapKey unter Verwendung der eindeutigen verwalteten Identität Zugriff auf Key Vault. Im Azure-Portal wird die eindeutige Identität automatisch erstellt, wenn Datenverschlüsselung für MySQL aktiviert wird. Ausführliche schrittweise Anleitungen für die Verwendung des Azure-Portals finden Sie unter [Konfigurieren der Datenverschlüsselung für MySQL](howto-data-encryption-portal.md).

* Wenn Sie eine Firewall mit Key Vault verwenden, müssen Sie die Option **Vertrauenswürdigen Microsoft-Diensten Umgehung der Firewall erlauben** aktivieren.

Nachfolgend werden die Anforderungen für die Konfiguration des vom Kunden verwalteten Schlüssels aufgeführt:

* Der vom Kunden verwaltete Schlüssel, der zum Verschlüsseln des DEK verwendet werden soll, muss ein asymmetrischer RSA 2028-Schlüssel sein.
* Das Schlüsselaktivierungsdatum (sofern festgelegt) muss ein Datum und eine Uhrzeit in der Vergangenheit sein. Das Ablaufdatum (sofern festgelegt) muss ein Datum und eine Uhrzeit in der Zukunft sein.
* Der Schlüssel muss sich im Zustand *Aktiviert* befinden.
* Wenn Sie einen vorhandenen Schlüssel in Key Vault importieren, müssen Sie ihn in einem der unterstützten Dateiformate (`.pfx`, `.byok`, `.backup`) bereitstellen.

## <a name="recommendations"></a>Empfehlungen

Wenn Sie Datenverschlüsselung durch einen vom Kunden verwalteten Schlüssel verwenden, finden Sie hier Empfehlungen zum Konfigurieren von Key Vault:

* Legen Sie eine Ressourcensperre für Key Vault fest, um zu steuern, wer diese wichtige Ressource löschen kann, und um ein versehentliches oder nicht autorisiertes Löschen zu verhindern.
* Aktivieren Sie die Überwachung und Berichterstellung für alle Verschlüsselungsschlüssel. Key Vault stellt Protokolle bereit, die sich problemlos in andere SIEM-Tools (Security Information & Event Management) einfügen lassen. Log Analytics in Azure Monitor ist ein Beispiel für einen Dienst, der bereits integriert ist.

* Stellen Sie sicher, dass sich Key Vault und Azure Database for MySQL in derselben Region befinden, um einen schnelleren Zugriff für Wrap- und Unwrap-Vorgänge bei DEKs sicherzustellen.

Empfehlungen für die Konfiguration kundenseitig verwalteter Schlüssel:

* Bewahren Sie eine Kopie des kundenseitig verwalteten Schlüssels an einem sicheren Ort auf, oder hinterlegen Sie ihn bei einem entsprechenden Dienst.

* Wenn Key Vault den Schlüssel generiert, erstellen Sie eine Schlüsselsicherung, bevor Sie den Schlüssel erstmals verwenden. Sie können nur die Sicherung in Key Vault wiederherstellen. Weitere Informationen zum Sicherungsbefehl finden Sie unter [Backup-AzKeyVaultKey](https://docs.microsoft.com/powershell/module/az.keyVault/backup-azkeyVaultkey).

## <a name="inaccessible-customer-managed-key-condition"></a>Kein Zugriff auf den kundenseitig verwalteten Schlüssel

Wenn Sie Datenverschlüsselung mit einem vom Kunden verwalteten Schlüssel in Azure Key Vault konfigurieren, wird fortlaufender Zugriff auf diesen Schlüssel benötigt, damit der Server online bleiben kann. Wenn der Server in Key Vault den Zugriff auf den vom Kunden verwalteten Schlüssel verliert, beginnt der Server innerhalb von 10 Minuten damit, alle Verbindungen zu verweigern. Der Server gibt eine entsprechende Fehlermeldung aus und ändert den Serverstatus in *Zugriff nicht möglich*. Die einzige zulässige Aktion für eine Datenbank in diesem Zustand ist das Löschen der Datenbank.

### <a name="accidental-key-access-revocation-from-key-vault"></a>Versehentliche Sperrung des Zugriffs auf den Schlüssel durch Key Vault

Es kann vorkommen, dass ein Benutzer mit ausreichenden Zugriffsrechten für Key Vault den Serverzugriff auf den Schlüssel versehentlich durch eine der folgende Aktionen deaktiviert:

* Widerrufen der Berechtigungen „get“, „wrapKey“ und „unwrapKey“ vom Server.
* Löschen des Schlüssels.
* Löschen des Schlüsseltresors.
* Ändern der Firewallregeln des Schlüsseltresors.

* Löschen der verwalteten Identität des Servers in Azure AD.

## <a name="monitor-the-customer-managed-key-in-key-vault"></a>Überwachen des kundenseitig verwalteten Schlüssels in Key Vault

Konfigurieren Sie die folgenden Azure-Features, um den Datenbankzustand zu überwachen und Warnungen bei Verlust des Zugriffs auf die TDE-Schutzvorrichtung (Transparent Data Encryption) zu aktivieren:

* [Azure Resource Health](../service-health/resource-health-overview.md): Eine Datenbank, auf die nicht zugegriffen werden kann und die den Zugriff auf den Kundenschlüssel verloren hat, wird als „Zugriff nicht möglich“ angezeigt, nachdem die erste Verbindung mit der Datenbank verweigert wurde.
* [Aktivitätsprotokoll:](../service-health/alerts-activity-log-service-notifications.md) Ist der Zugriff auf den Kundenschlüssel im vom Kunden verwalteten Key Vault nicht möglich, werden dem Aktivitätsprotokoll entsprechende Einträge hinzugefügt. Sie können den Zugriff so bald wie möglich wiederherstellen, wenn Sie Warnungen für diese Ereignisse erstellen.

* [Aktionsgruppen](../azure-monitor/platform/action-groups.md): Definieren Sie diese, um Benachrichtigungen und Warnungen basierend auf Ihren Einstellungen zu senden.

## <a name="restore-and-replicate-with-a-customers-managed-key-in-key-vault"></a>Wiederherstellen und Replizieren mit einem vom Kunden verwalteten Schlüssel in Key Vault

Nachdem Azure Database for MySQL mit dem vom Kunden verwalteten Schlüssel, der in Key Vault gespeichert ist, verschlüsselt wurde, wird jede neu erstellte Kopie des Servers ebenfalls verschlüsselt. Sie können diese neue Kopie durch einen lokalen oder Geowiederherstellungsvorgang oder durch Lesereplikate erstellen. Die Kopie kann jedoch so geändert werden, dass für die Verschlüsselung ein neuer kundenseitig verwalteter Schlüssel angegeben wird. Wenn der vom Kunden verwaltete Schlüssel geändert wird, beginnen alte Sicherungen des Servers damit, den neuesten Schlüssel zu verwenden.

Um Probleme bei der Einrichtung von kundenseitig verwalteter Datenverschlüsselung während der Wiederherstellung oder der Lesereplikaterstellung zu vermeiden, sollten Sie unbedingt die folgenden Schritte auf dem Master und den wiederhergestellten bzw. Replikatservern ausführen:

* Initiieren Sie die Wiederherstellung oder die Erstellung eines Lesereplikats auf dem Master für Azure Database for MySQL.
* Der neu erstellte Server (wiederhergestellt oder Replikat) verbleibt im Zustand „Zugriff nicht möglich“, da seiner eindeutigen Identität noch keine Berechtigungen für Azure Key Vault erteilt wurden.
* Überprüfen Sie den vom Kunden verwalteten Schlüssel auf dem wiederhergestellten/Replikatserver in den Datenverschlüsselungseinstellungen erneut. Dadurch wird sichergestellt, dass dem neu erstellten Server die Berechtigungen „Wrap“ und „Unwrap“ für den in Key Vault gespeicherten Schlüssel erteilt werden.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie [Datenverschlüsselung mit einem vom Kunden verwalteten Schlüssel für Azure Database for MySQL über das Azure-Portal einrichten](howto-data-encryption-portal.md).

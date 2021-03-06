---
title: 'Tutorial: Importieren eines Zertifikats in Key Vault mithilfe des Azure-Portals | Microsoft-Dokumentation'
description: Tutorial zum Importieren eines Zertifikats in Azure Key Vault
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: certificates
ms.topic: tutorial
ms.custom: mvc
ms.date: 04/03/2020
ms.author: sebansal
ms.openlocfilehash: 754f30f7931f9fad6a95328cbf8ab34f70cb75a0
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81426109"
---
# <a name="tutorial-import-a-certificate-in-azure-key-vault"></a>Tutorial: Importieren eines Zertifikats in Azure Key Vault

Azure Key Vault ist ein Clouddienst, der als sicherer Geheimnisspeicher fungiert. Dadurch können Schlüssel, Kennwörter, Zertifikate und andere Geheimnisse sicher gespeichert werden. Azure Key Vault-Instanzen können über das Azure-Portal erstellt und verwaltet werden. In diesem Tutorial erstellen Sie einen Schlüsseltresor und verwenden ihn dann zum Importieren eines Zertifikats. Weitere Informationen zu Key Vault finden Sie in der [Übersicht](../general/overview.md).

Das Tutorial veranschaulicht folgende Vorgehensweisen:

> [!div class="checklist"]
> * Erstellen eines Schlüsseltresors
> * Importieren eines Zertifikats in Key Vault über das Portal
> * Importieren eines Zertifikats in Key Vault mithilfe der CLI


Machen Sie sich zunächst mit den [grundlegenden Konzepten von Key Vault](../general/basic-concepts.md) vertraut. 

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter https://portal.azure.com beim Azure-Portal an.

## <a name="create-a-vault"></a>Erstellen eines Tresors

1. Wählen Sie im Menü des Azure-Portals oder auf der **Startseite** die Option **Ressource erstellen** aus.
2. Geben Sie **Key Vault** in das Suchfeld ein.
3. Wählen Sie in der Ergebnisliste **Key Vault** aus.
4. Klicken Sie im Abschnitt „Key Vault“ auf **Erstellen**.
5. Geben Sie im Abschnitt **Schlüsseltresor erstellen** folgende Informationen ein:
    - **Name**: Es ist ein eindeutiger Name erforderlich. Für diesen Schnellstart verwenden wir **Example-Vault**. 
    - **Abonnement**: Wählen Sie ein Abonnement aus.
    - Klicken Sie unter **Ressourcengruppe** auf **Neu erstellen**, und geben Sie einen Ressourcengruppennamen ein.
    - Wählen Sie im Pulldownmenü **Speicherort** einen Speicherort aus.
    - Behalten Sie bei den anderen Optionen die Standardeinstellungen bei.
6. Klicken Sie nach der Angabe der obigen Informationen auf **Erstellen**.

Beachten Sie die beiden folgenden Eigenschaften:

* **Tresorname**: In diesem Beispiel ist das **Example-Vault**. Dieser Name wird noch für andere Schritte benötigt.
* **Tresor-URI**: In diesem Beispiel https://example-vault.vault.azure.net/. Anwendungen, die Ihren Tresor über die zugehörige REST-API nutzen, müssen diesen URI verwenden.

An diesem Punkt ist nur Ihr Azure-Konto zum Ausführen von Vorgängen für den neuen Tresor autorisiert.

![Ausgabe nach Erstellung der Key Vault-Instanz](../media/certificates/tutorial-import-cert/vault-properties.png)

## <a name="import-a-certificate-to-key-vault"></a>Importieren eines Zertifikats in Key Vault

Zum Importieren eines Zertifikats in den Tresor benötigen Sie eine PEM- oder PFX-Zertifikatsdatei auf einem Datenträger. In diesem Fall wird ein Zertifikat mit dem Dateinamen **ExampleCertificate** importiert.

> [!IMPORTANT]
> In Azure Key Vault werden die Zertifikatsformate PFX und PEM unterstützt. 
> - Das PEM-Dateiformat enthält mindestens eine X509-Zertifikatsdatei.
> - Das PFX-Dateiformat ist ein Archivdateiformat zum Speichern mehrerer kryptografischer Objekte in einer einzelnen Datei. Bei diesen Objekten handelt es sich um das für Ihre Domäne ausgestellte Serverzertifikat, einen entsprechenden privaten Schlüssel und optional um eine Zwischenzertifizierungsstelle.  

1. Wählen Sie auf den Key Vault-Eigenschaftenseiten die Option **Zertifikate** aus.
2. Klicken Sie auf **Generieren/Importieren**.
3. Wählen Sie auf dem Bildschirm **Zertifikat erstellen** folgende Werte aus:
    - **Methode der Zertifikaterstellung**: Importieren.
    - **Zertifikatsname**: ExampleCertificate
    - **Zertifikatdatei hochladen**: Wählen Sie die Zertifikatsdatei vom Datenträger aus.
    - Behalten Sie bei den anderen Optionen die Standardwerte bei. Klicken Sie auf **Erstellen**.

![Zertifikateigenschaften](../media/certificates/tutorial-import-cert/cert-import.png)

Nachdem Sie die Meldung erhalten haben, dass das Zertifikat erfolgreich importiert wurde, können Sie in der Liste auf das Zertifikat klicken. Daraufhin werden einige der Eigenschaften angezeigt. 

![Zertifikateigenschaften](../media/certificates/tutorial-import-cert/current-version-hidden.png)

## <a name="import-a-certificate-using-azure-cli"></a>Importieren eines Zertifikats mithilfe der Azure CLI

Importieren Sie ein Zertifikat in einen angegebenen Schlüsseltresor. Wenn Sie ein vorhandenes gültiges Zertifikat mit einem privaten Schlüssel in Azure Key Vault importieren möchten, muss die zu importierende Datei entweder im PFX- oder im PEM-Format vorliegen. Wenn das Zertifikat im PEM-Format vorliegt, muss die PEM-Datei sowohl den Schlüssel als auch x509-Zertifikate enthalten. Für diesen Vorgang ist die Berechtigung für Zertifikate bzw. den Import erforderlich.

```azurecli
az keyvault certificate import --file
                               --name
                               --vault-name
                               [--disabled {false, true}]
                               [--only-show-errors]
                               [--password]
                               [--policy]
                               [--subscription]
                               [--tags]
```
Weitere Informationen zu Parametern finden Sie [hier](https://docs.microsoft.com/cli/azure/keyvault/certificate?view=azure-cli-latest#az-keyvault-certificate-import).

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Andere Schnellstartanleitungen und Tutorials für Key Vault bauen auf dieser Schnellstartanleitung auf. Falls Sie mit weiteren Schnellstartanleitungen und Tutorials fortfahren möchten, sollten Sie die Ressourcen nicht bereinigen.
Wenn Sie die Ressourcen nicht mehr benötigen, löschen Sie die Ressourcengruppe. Dadurch werden die Key Vault-Instanz und die dazugehörigen Ressourcen gelöscht. So löschen Sie die Ressourcengruppe über das Portal:

1. Geben Sie den Namen Ihrer Ressourcengruppe in das Suchfeld am oberen Rand des Portals ein. Klicken Sie in den Suchergebnissen auf die Ressourcengruppe aus dieser Schnellstartanleitung.
2. Wählen Sie die Option **Ressourcengruppe löschen**.
3. Geben Sie im Feld **GEBEN SIE DEN RESSOURCENGRUPPENNAMEN EIN:** den Namen der Ressourcengruppe ein, und klicken Sie anschließend auf **Löschen**.


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie eine Key Vault-Instanz erstellt und ein Zertifikat importiert. Weitere Informationen zu Key Vault und zur Integration in Ihre Anwendungen finden Sie in den folgenden Artikeln:

- Informieren Sie sich ausführlicher über die [Verwaltung von Zertifikaten in Azure Key Vault](/archive/blogs/kv/manage-certificates-via-azure-key-vault).
- Sehen Sie sich Beispiele für das [Importieren von Zertifikaten mithilfe von Rest-APIs](/rest/api/keyvault/importcertificate/importcertificate) an.
- [Bewährte Methoden zum Verwenden von Key Vault](../general/best-practices.md)
---
title: Speichern des AppSource-Pakets in Azure Storage und Generieren einer URL mit SAS-Schlüssel
description: Beschreibt die Schritte, die erforderlich sind, um ein AppSource-Paket hochzuladen und zu sichern.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 09/18/2018
ms.author: dsindona
ms.openlocfilehash: 57bc370fd160b8b3d6d7941ea28cd460c99f3d96
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80285366"
---
<a name="store-your-appsource-package-to-azure-storage-and-generate-a-url-with-sas-key"></a>Speichern des AppSource-Pakets in Azure Storage und Generieren einer URL mit SAS-Schlüssel
=============================================================================

Um die Sicherheit Ihrer Dateien sicherzustellen, müssen alle Partner ihre AppSource-Paketdatei in einem Azure Blob Storage-Konto speichern und mit einem SAS-Schlüssel freigeben. Wir werden die Paketdatei von Ihrem Azure Storage-Speicherort zur Zertifizierung abrufen und für AppSource-Tests verwenden.

Führen Sie die folgenden Schritte aus, um Ihr Paket in den Blobspeicher hochzuladen:

1. Wechseln Sie zu <https://azure.microsoft.com> und erstellen Sie eine kostenlose Testversion oder ein kostenpflichtiges Konto.

2. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.

3. Erstellen Sie ein neues Speicherkonto, indem Sie auf **+ Neu** klicken und zum Konto **Daten und Speicher** wechseln.

   ![Blatt „Daten und Speicher“ im Microsoft Azure-Portal](media/CRMScreenShot7.png)

4. Geben Sie einen **Namen** und **Ressourcengruppennamen** ein und klicken Sie auf die Schaltfläche **Erstellen**.

   ![Erstellen eines Speicherkontos im Microsoft Azure-Portal](media/CRMScreenShot8.png)

5. Navigieren Sie zu Ihrer neu erstellten Ressourcengruppe und erstellen Sie einen neuen Blobcontainer.

   ![Hochladen des Pakets als Blob über das Microsoft Azure-Portal](media/CRMScreenShot9.png)

6. Laden Sie den [Microsoft Azure Storage-Explorer](https://storageexplorer.com/) herunter, und installieren Sie ihn, falls dies noch nicht geschehen ist.

7. Öffnen Sie den Storage Explorer und verwenden Sie das Symbol, um eine Verbindung zu Ihrem Azure Storage-Konto herzustellen.

8. Navigieren Sie zu dem von Ihnen erstellten Blobcontainer und klicken Sie auf **Hochladen**, um Ihre ZIP-Paketdatei hinzuzufügen.

   ![Hochladen von Paketen mit dem Microsoft Storage-Explorer](media/CRMScreenShot10.png)

9. Klicken Sie mit der rechten Maustaste auf Ihre Datei, und wählen Sie **Shared Access Signature abrufen** aus.

   ![Abrufen der Shared Access Signature einer Azure-Datei](media/CRMScreenShot11.png)

10. Ändern Sie die **Ablaufzeit**, um die SAS für einen Monat zu aktivieren, und klicken Sie dann auf **Erstellen**.

    ![Ändern des SAS-Ablaufdatums einer Azure-Datei](media/CRMScreenShot12.png)

11. Kopieren Sie das URL-Feld und speichern Sie es zur späteren Verwendung. Diese URL müssen Sie beim Erstellen des zugehörigen Angebots eingeben. 

    ![Kopieren der SAS-URL einer Azure-Datei](media/CRMScreenShot13.png)


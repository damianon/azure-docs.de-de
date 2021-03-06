---
title: Hochladen eines Zertifikats für die Azure-Dienstverwaltung | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie das Dienstverwaltungszertifikat für das Azure-Portal hochladen.
services: cloud-services
documentationcenter: .net
author: georgewallace
manager: carmonm
ms.assetid: 1b813833-39c8-46be-8666-fd0960cfbf04
ms.service: api-management
ms.topic: article
ms.date: 08/01/2017
ms.author: gwallace
ms.openlocfilehash: 4b49a9b391eeca2d2e249b171d99f231bda6fdff
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "78329118"
---
# <a name="upload-an-azure-service-management-certificate"></a>Hochladen eines Zertifikats für die Azure-Dienstverwaltung
Verwaltungszertifikate ermöglichen Ihnen die Authentifizierung mit dem von Azure bereitgestellten klassischen Bereitstellungsmodell. Diese Zertifikate werden in vielen Programmen und Tools (z.B. Visual Studio oder Azure SDK) zum Automatisieren der Konfiguration und Bereitstellung verschiedener Azure-Dienste verwendet. 

> [!WARNING]
> Vorsicht ist geboten! Alle Personen, die die Authentifizierung mit diesen Zertifikaten durchführen, können das zugeordnete Abonnement verwalten.
>
>

Weitere Informationen zu Azure-Zertifikaten (und zum Erstellen eines selbstsignierten Zertifikats) finden Sie unter [Übersicht über Zertifikate für Azure Cloud Services](cloud-services/cloud-services-certs-create.md#what-are-management-certificates).

Sie können auch [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) für die Authentifizierung von Clientcode zur Automatisierung verwenden.

**Hinweis:** Sie müssen ein Co-Administrator des Abonnements sein, um Vorgänge unter Verwaltungszertifikaten auszuführen. [Erfahren Sie mehr](https://go.microsoft.com/fwlink/?linkid=849300) darüber, wie Sie Co-Administratoren dem neuen Azure-Portal hinzufügen oder daraus entfernen können. 

## <a name="upload-a-management-certificate"></a>Hochladen eines Verwaltungszertifikats
Sobald Sie ein Verwaltungszertifikat erstellt haben (CER-Datei nur mit dem öffentlichen Schlüssel), können Sie es in das Portal hochladen. Wenn das Zertifikat im Portal verfügbar ist, kann jeder Benutzer mit einem entsprechenden Zertifikat (privater Schlüssel) über die Verwaltungs-API eine Verbindung herstellen und auf die Ressourcen für das zugehörige Abonnement zugreifen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Klicken Sie unten in der Azure-Dienstliste auf **Alle Dienste**, und wählen Sie dann in der Dienstgruppe **Allgemein** die Option _Abonnements_ aus.

    ![Abonnementmenü](./media/azure-api-management-certs/subscriptions_menu.png)

3. Stellen Sie sicher, dass Sie das Abonnement auswählen, das Sie dem Zertifikat zuordnen möchten.     
4. Wenn Sie das richtige Abonnement ausgewählt haben, klicken Sie in der Gruppe **Einstellungen** auf _Verwaltungszertifikate_.

    ![Einstellungen](./media/azure-api-management-certs/mgmtcerts_menu.png)

5. Wählen Sie die Schaltfläche **Hochladen** aus.

    ![Seite zum Hochladen für Zertifikate](./media/azure-api-management-certs/certificates_page.png)
6. Füllen Sie das Dialogfeld aus, und wählen Sie **Hochladen** aus.

    ![Einstellungen](./media/azure-api-management-certs/certificate_details.png)

## <a name="next-steps"></a>Nächste Schritte
Da Sie jetzt einem Abonnement ein Verwaltungszertifikat zugeordnet haben, können Sie (nachdem Sie das entsprechende Zertifikat lokal installiert haben) programmgesteuert eine Verbindung mit der [REST-API für das klassische Bereitstellungsmodell](/azure/?pivot=sdkstools) herstellen und die verschiedenen Azure-Ressourcen automatisieren, die auch diesem Abonnement zugeordnet sind.

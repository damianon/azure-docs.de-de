---
title: Problembehandlung – 500xxx-interne Fehler bei Azure IoT Hub
description: Grundlegendes zum Beheben von 500xxx-internen Fehlern
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.author: jlian
ms.openlocfilehash: 7f3f5177e084693c45bed1088a4e1d091be100ed
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79233182"
---
# <a name="500xxx-internal-errors"></a>500xxx Interne Fehler

In diesem Artikel werden die Ursachen von **500xxx-internen Fehlern** und die Lösungen dafür beschrieben.

## <a name="symptoms"></a>Symptome

Ihre Anforderung an IoT Hub schlägt mit einer Meldung fehl, die mit „500“ und/oder einer Art von „ServerError“ beginnt. Einige Möglichkeiten sind die folgenden:

* **500001 ServerError**: Bei IoT Hub ist ein serverseitiges Problem aufgetreten.

* **500008 GenericTimeout**: IoT Hub konnte die Verbindungsanforderung nicht vor dem Timeout abschließen.

* **ServiceUnavailable (kein Fehlercode)** : Bei IoT Hub ist ein interner Fehler aufgetreten.

* **InternalServerError (kein Fehlercode)** : Bei IoT Hub ist ein interner Fehler aufgetreten.

## <a name="cause"></a>Ursache

Für eine Fehlerantwort des Typs „500xxx“ gibt es eine Reihe von Gründen. In allen Fällen ist das Problem höchstwahrscheinlich vorübergehend. Obwohl das IoT Hub-Team daran arbeitet, [die SLA](https://azure.microsoft.com/support/legal/sla/iot-hub/) zu verwalten, können kleine Teilmengen von IoT Hub-Knoten gelegentlich vorübergehende Fehler aufweisen. Wenn Ihr Gerät versucht, eine Verbindung zu einem Knoten herzustellen, der Probleme aufweist, erhalten Sie diese Fehlermeldung.

## <a name="solution"></a>Lösung

Geben Sie zum Beheben von 500xxx-Fehlern einen Wiederholungsversuch vom Gerät aus. Um [Wiederholungsversuche automatisch zu verwalten](./iot-hub-reliability-features-in-sdks.md#connection-and-retry), stellen Sie sicher, dass Sie die neueste Version des [Azure IoT SDKs](./iot-hub-devguide-sdks.md) verwenden. Die Best Practices für die Behandlung vorübergehender Fehler und Wiederholungsversuche finden Sie unter [Behandeln vorübergehender Fehler](https://docs.microsoft.com/azure/architecture/best-practices/transient-faults).  Wenn das Problem weiterhin besteht, überprüfen Sie [Ressourcenintegrität](./iot-hub-monitor-resource-health.md#use-azure-resource-health) und [Azure-Status](https://status.azure.com/), um festzustellen, ob es bei IoT Hub ein bekanntes Problem gibt. Sie können auch das [Feature für manuelles Failover](./tutorial-manual-failover.md) verwenden. Wenn keine bekannten Probleme vorliegen und das Problem fortbesteht, [wenden Sie sich an den Support](https://azure.microsoft.com/support/options/), um weitere Unterstützung zu erhalten.

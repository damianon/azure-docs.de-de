---
title: Probleme mit JDBC/ODBC und Apache Thrift-Framework – Azure HDInsight
description: Große Datasets können nicht unter Verwendung von JDBC/ODBC und Apache Thrift-Softwareframework in Azure HDInsight heruntergeladen werden
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/29/2019
ms.openlocfilehash: 23693dcae2f361b88440ec88ca39fd8ed229d85a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "75894257"
---
# <a name="unable-to-download-large-data-sets-using-jdbcodbc-and-apache-thrift-software-framework-in-hdinsight"></a>Große Datasets können nicht unter Verwendung von JDBC/ODBC und Apache Thrift-Softwareframework in HDInsight heruntergeladen werden

In diesem Artikel werden Schritte zur Problembehandlung und mögliche Lösungen für Probleme bei der Verwendung von Apache Spark-Komponenten in Azure HDInsight-Clustern beschrieben.

## <a name="issue"></a>Problem

Wenn Sie versuchen, große Datasets unter Verwendung von JDBC/ODBC und dem Apache Thrift-Softwareframework in Azure HDInsight herunterzuladen, wird eine Fehlermeldung wie die folgende angezeigt:

```
org.apache.spark.SparkException: Kryo serialization failed:
Buffer overflow. Available: 0, required: 36518. To avoid this, increase spark.kryoserializer.buffer.max value.
```

## <a name="cause"></a>Ursache

Diese Ausnahme tritt auf, wenn der Serialisierungsprozess versucht, mehr Pufferspeicher zu verwenden als erlaubt. In Spark 2.0.0 wird für die Objektserialisierung die Klasse `org.apache.spark.serializer.KryoSerializer` verwendet, wenn über das Apache Thrift-Softwareframework auf Daten zugegriffen wird. Für Daten, die über das Netzwerk gesendet oder in serialisierter Form zwischengespeichert werden, wird eine andere Klasse verwendet.

## <a name="resolution"></a>Lösung

Erhöhen Sie den `Kryoserializer`-Pufferwert. Fügen Sie in „spark2 config“ unter `spark.kryoserializer.buffer.max` einen Schlüssel namens `2048` hinzu, und legen Sie ihn auf `Custom spark2-thrift-sparkconf` fest.

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Nutzen Sie den [Azure-Communitysupport](https://azure.microsoft.com/support/community/), um Antworten von Azure-Experten zu erhalten.

* Nutzen Sie [@AzureSupport](https://twitter.com/azuresupport) – das offizielle Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit. Hierüber hat die Azure-Community Zugriff auf die richtigen Ressourcen: Antworten, Support und Experten.

* Sollten Sie weitere Unterstützung benötigen, senden Sie eine Supportanfrage über das [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Wählen Sie dazu auf der Menüleiste die Option **Support** aus, oder öffnen Sie den Hub **Hilfe und Support**. Ausführlichere Informationen hierzu finden Sie unter [Erstellen einer Azure-Supportanfrage](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request). Zugang zu Abonnementverwaltung und Abrechnungssupport ist in Ihrem Microsoft Azure-Abonnement enthalten. Technischer Support wird über einen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) bereitgestellt.

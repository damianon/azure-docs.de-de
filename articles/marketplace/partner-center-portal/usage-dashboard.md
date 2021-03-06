---
title: Dashboard „Nutzung“ in Analysen für den kommerziellen Marketplace in Partner Center
description: Erfahren Sie, wie Sie auf Metriken zur Nutzung und getakteten Abrechnung für alle VM-Angebote zugreifen.
author: dsindona
ms.author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 12/11/2019
ms.openlocfilehash: 33762540d14ea51e8325abe9a466007cd7cca748
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "81262178"
---
# <a name="usage-dashboard-in-commercial-marketplace-analytics"></a>Dashboard „Nutzung“ in Analysen für den kommerziellen Marketplace

Dieser Artikel enthält Informationen zum Dashboard „Nutzung“ im Partner Center. In diesem Dashboard werden Metriken zur Nutzung und getakteten Abrechnung für alle VM-Angebote in zwei getrennten Registerkarten angezeigt: „VM-Nutzung“ und „Nutzung nach getakteter Abrechnung“

Um auf das Dashboard „Nutzung“ zuzugreifen, öffnen Sie das Dashboard **[Analyse](https://partner.microsoft.com/dashboard/commercial-marketplace/analytics/summary)** unter „Kommerzieller Marketplace“.

>[!NOTE]
> Ausführliche Definitionen der Analyseterminologie finden Sie unter [Häufig gestellte Fragen und Terminologie zu Analysen für den kommerziellen Marketplace](./faq-terminology.md).

## <a name="usage-dashboard"></a>Dashboard „Nutzung“

Im Dashboard „Nutzung“ sind die Metriken zur Nutzung und zur Nutzung nach getakteter Abrechnung für alle VM-Angebote dargestellt. Diese befinden sich auf zwei getrennten Registerkarten: „VM-Nutzung“ und „Nutzung nach getakteter Abrechnung“

Die Registerkarte „VM-Nutzung“ enthält grafische Darstellungen der folgenden Nutzungsdaten:

- [Nutzungszusammenfassung](#usage-summary)
- [Nutzung nach Geografie](#usage-by-geography)
- [Nutzung nach Angeboten](#usage-by-offers)
- [Nutzungstrend nach Angeboten und SKUs](#usage-trend-by-offers-and-skus)
- [Nutzung nach Angebotstyp](#usage-by-offer-type)
- [Nutzung nach VM-Größe](#usage-by-vm-size)
- [Nutzung nach Vertriebskanal](#usage-by-sales-channel)
- [Detaillierte Nutzungsdaten](#detailed-usage-data)

> [!NOTE]
> Analyseberichte werden im Cloud-Partnerportal (CPP) und im Partner Center unterschiedlich angezeigt. **Verkäufer-Insights** in CPP verfügt über eine Registerkarte „Aufträge und Nutzung“, auf der Daten für nutzungsbasierte Angebote und nicht nutzungsbasierte Angebote angezeigt werden. Im Partner Center werden die Nutzungsmetriken auf einer getrennten Seite angezeigt.

### <a name="usage-summary"></a>Nutzungszusammenfassung

In der Tabelle „Nutzungszusammenfassung“ werden die Nutzungsstunden der Kunden für alle erworbenen Angebote angezeigt.

- Normalisierte Nutzungsstunden werden als Nutzungsstunden definiert, die normalisiert wurden, um die Anzahl der VM-Kerne ([Anzahl der VM-Kerne] ·x [Stunden der Rohdatennutzung]) zu berücksichtigen. Für VMs, die als „SHAREDCORE“ festgelegt wurden, wird der Multiplikator 1/6 (oder 0,1666) für [Anzahl der VM-Kerne] verwendet.
- Die tatsächlichen Nutzungsstunden werden als die Zeitspanne (in Stunden) definiert, in der VMs ausgeführt wurden.
- Der Prozentwert stellt eine Zunahme/Abnahme der Nutzung im ausgewählten Datumsbereich dar ([Nutzung im letzten Monat – Nutzung im ersten Monat])/Nutzung im ersten Monat).
- Nach oben weisende grüne Dreiecke geben eine Zunahme an.
- Ein rotes, nach unten weisendes Dreieck gibt eine Abnahme gegenüber dem Vormonat an.
- In Mikrobalkendiagrammen werden monatliche Werte dargestellt. Sie können den Wert für jeden Monat anzeigen, indem Sie auf die Spalten innerhalb des Diagramms zeigen.

### <a name="usage-by-geography"></a>Nutzung nach Geografie

Im Wärmebild **Normalisierte Nutzung nach Geografie** werden die Nutzungsstunden jeweils nach dem Land des Kunden angezeigt. Variationen in den Länderfarben spiegeln wider, wie sich die normalisierte Nutzung verteilt. Um wieder zur ursprünglichen Ansicht zurückzukehren, wählen Sie in der Karte die Schaltfläche **Home** aus.

### <a name="usage-by-offers"></a>Nutzung nach Angeboten

- Im Kreisdiagramm **Normalisierte Nutzung nach Angeboten** sind die normalisierten Nutzungsstunden entsprechend dem ausgewählten Datumsbereich nach Angeboten aufgeschlüsselt. Die 5 Topangebote werden im Diagramm angezeigt, während die übrigen unter der Kategorie für sonstige Angebote gruppiert sind.
- Im Balkendiagramm wird der Wachstumstrend für den ausgewählten Datumsbereich nach Monaten angezeigt. In den Monatsspalten sind die Nutzungsstunden aus den Angeboten mit den meisten Nutzungsstunden für den jeweiligen Monat dargestellt. Im Liniendiagramm wird der prozentuale Wachstumstrend auf der sekundären Y-Achse dargestellt.
- Mit dem Schieberegler oben im Diagramm können Sie entlang der X-Achse nach rechts und links scrollen und/oder sich auf bestimmte Datenpunkte konzentrieren.

### <a name="usage-trend-by-offers-and-skus"></a>Nutzungstrend nach Angeboten und SKUs

In diesem Diagramm wird der Trend der normalisierten Nutzung für die ausgewählten SKUs eines Angebots angezeigt. In der Angebotsbestenliste werden die 50 Top-Angebote mit der höchsten Nutzung nach Nutzungsstunden sortiert aufgelistet. In der SKU-Bestenliste werden die 50 Top-SKUs mit der höchsten Nutzung für das ausgewählte Angebot aufgelistet.

### <a name="usage-by-offer-type"></a>Nutzung nach Angebotstyp

- Im Kreisdiagramm **Nutzung nach Angebotstyp** ist die Nutzung nach dem Angebotstyp organisiert.
- Die Topangebote werden im Diagramm angezeigt, während die übrigen unter der Kategorie für sonstige Angebote gruppiert sind.
- Im Diagramm **Trend** werden Wachstumstrends nach Monaten angezeigt. In der Monatsspalte wird die Nutzung nach den Top-Angebotstypen im jeweiligen Monat dargestellt.

### <a name="usage-by-vm-size"></a>Nutzung nach VM-Größe

In diesem Diagramm wird der Nutzungstrend für ausgewählte VM-Größen (max. 5) all Ihrer Angebote/SKUs dargestellt. Im Säulendiagramm sind Nutzungsstunden der ausgewählten VM-Größen gestapelt dargestellt.

In der Bestenliste werden die 50 Top-VM-Größen mit der höchsten Nutzung nach Nutzungsstunden sortiert aufgelistet.

### <a name="usage-by-sales-channel"></a>Nutzung nach Vertriebskanal

- Im Kreisdiagramm „Nutzung nach Vertriebskanal“ ist die Nutzung nach dem Vertriebskanal organisiert.
- Die Topvertriebskanäle mit der höchsten Nutzung werden im Diagramm angezeigt, während die übrigen unter der Kategorie für sonstige Vertriebskanäle gruppiert sind.
- In der Monatsspalte wird die Nutzung nach den Top-Vertriebskanälen im jeweiligen Monat dargestellt.
- Die Features dieses Diagramms sind identisch mit denen im Diagramm „Nutzung nach Angeboten“.

### <a name="detailed-usage-data"></a>Detaillierte Nutzungsdaten

In der **Tabelle „Nutzungsdetails“** wird eine nummerierte Liste der 1000 Top-Verwendungseinträge nach Nutzung sortiert angezeigt.

- Jede Spalte im Raster ist sortierbar.
- Die Daten können in eine CSV-Datei extrahiert werden, wenn die Anzahl weniger als 1000 Einträge beträgt.
- Liegt die Anzahl der Einträge über 1000, werden die exportierten Daten asynchron auf einer Downloadseite abgelegt und sind für die nächsten 30 Tage verfügbar.
- Sie können auf die **detaillierten Nutzungsdaten** Filter anwenden, um nur die Daten anzuzeigen, die für Sie von Interesse sind. Daten können nach Land/Region, Vertriebskanal, Marketplace-Lizenztyp, Nutzungstyp, Angebotsname, Angebotstyp, kostenlosen Testversionen, Marketplace-Abonnement-ID, Kunden-ID und Firmenname gefiltert werden.

> [!NOTE]
> Wählen Sie im Seitenfilter den **Nutzungstyp** aus, um die Diagramme auf der Seite entweder in der normalisierten Ansicht oder in der Rohdatenansicht anzuzeigen. Die normalisierte Ansicht ist die Standardansicht für diese Diagramme.

**Filter für die Seite „Nutzung“** werden auf Seitenebene angewendet. Sie können mehrere Filter auswählen, um das Diagramm nach den Kriterien zu rendern, die Sie für die Anzeige ausgewählt haben, und um die gewünschten Daten im Raster/Export für detaillierte Nutzungsdaten anzuzeigen. Filter werden auf die Daten angewendet, die für den Datenbereich extrahiert wurden, den Sie in der oberen rechten Ecke der Seite „Bestellungen“ ausgewählt haben.

- **Angebotstypen** und **Angebotsnamen** werden nur für die Angebote aufgelistet, die Sie im ausgewählten Datumsbereich erworben haben. In der Liste enthaltene Angebotsnamen werden für Angebotstypen angezeigt, die aus der Liste ausgewählt werden.
- Die Standardauswahl für jede der Filteroptionen lautet „Alle“, mit Ausnahme von **Verwendungstyp**. Die Standardauswahl für **Verwendungstyp** ist „Normalisierte Nutzung“. Um die Rohdatennutzung in den Diagrammen anzuzeigen, wählen Sie „Rohdatennutzung“ aus.
- Mithilfe der angewendeten Filter werden die ausgewählten Werte für die vorgenommenen Filterauswahlen anzeigt. Für die Standardauswahlen werden keine angewendeten Filter angezeigt.

> [!NOTE]
> Eine ausführliche Definition der einzelnen Felder im Raster für ausführliche Auftragsdaten, der Seitenfilter und aller Auswahlmöglichkeiten finden Sie im Abschnitt „Datenwörterbuch“ des Artikels [FAQs und Terminologie](link needed).

Auf der Registerkarte **Nutzung nach getakteter Abrechnung** werden Nutzungsinformationen für Angebotstypen dargestellt, wobei die Nutzung per Verbrauchseinheitsdimension gemessen wird. Derzeit wird die Überschreitung des SaaS-Angebotstyps dargestellt. Die Registerkarte enthält grafische Darstellungen von Überschreitungstrends für die SaaS-Nutzung nach getakteter Abrechnung:

- **Überschreitungstrend nach Verbrauchseinheitsdimension**: Zeigt den monatlichen Überschreitungstrend für die ausgewählte Verbrauchseinheitsdimension eines Angebots an. Die X-Achse stellt den Monat und die Y-Achse die Nutzungsmenge dar. Die Maßeinheit der benutzerdefinierten Verbrauchseinheit wird ebenfalls auf der Y-Achse angezeigt.
- **Überschreitungstrend nach SKU**: Stellt den Trend der Nutzungsmenge der ausgewählten Verbrauchseinheitsdimension nach SKUs dar. Die angezeigten SKUs stellen die 5 Top-SKUs mit der höchsten Nutzungsmenge für das ausgewählte Angebot dar.
- **Überschreitungstrend nach wichtigsten 50 Kunden**: Die 50 Top-Angebote mit den meisten Nutzungsstunden werden in einer ***Bestenliste*** angezeigt und nach der höchsten Nutzung der benutzerdefinierten Verbrauchseinheit bewertet. Wählen Sie einen Kunden in der Bestenliste aus, um den Nutzungstrend einer ausgewählten Verbrauchseinheitsdimension anzuzeigen.
- **Überschreitungstrend nach wichtigsten Kunden**: Stellt das bzw. die Perzentile der wichtigsten Kunden dar, die einen prozentualen Anteil an der Gesamtnutzung haben. Das Perzentil der wichtigsten Kunden wird entlang der X-Achse angezeigt und durch die Nutzungsmenge des Kunden bestimmt. Auf der Y-Achse wird die Nutzungsmenge angezeigt. Sie können Details anzeigen, indem Sie den Cursor über Punkte entlang des Liniendiagramms bewegen.

> [!NOTE]
> Die Nutzungsdetails und alle Diagramme auf dieser Seite werden für beliebige für den Seitenfilter ausgewählte Verbrauchseinheitsdimensionen angezeigt.

## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht über die im kommerziellen Marketplace in Partner Center verfügbaren Analyseberichte finden Sie unter [Analysen für den kommerziellen Marketplace in Partner Center](./analytics.md).
- Informationen zu Diagrammen, Trends und Werten von aggregierten Daten, mit deren Hilfe Marketplace-Aktivitäten für Ihr Angebot zusammengefasst werden, finden Sie unter [Dashboard „Zusammenfassung“ in Analysen für den kommerziellen Marketplace](./summary-dashboard.md).
- Informationen zu Ihren Aufträgen in einem grafischen und herunterladbaren Format finden Sie unter [Dashboard „Aufträge“ in Analysen für den kommerziellen Marketplace](./orders-dashboard.md).
- Ausführliche Informationen zu Ihren Kunden, einschließlich Wachstumstrends, finden Sie unter [Dashboard „Kunde“ in Analysen für den kommerziellen Marketplace](./customer-dashboard.md).
- Eine Liste Ihrer Downloadanforderungen der letzten 30 Tagen finden Sie unter [Dashboard „Downloads“ in Analysen für den kommerziellen Marketplace](./downloads-dashboard.md).
- Eine konsolidierte Ansicht des Kundenfeedbacks für Angebote im Azure Marketplace und in AppSource finden Sie unter [Dashboard „Bewertungen und Prüfungen“ in Analysen für den kommerziellen Marketplace](./ratings-reviews.md).
- Häufig gestellte Fragen zu Analysen für den kommerziellen Marketplace und ein umfassendes Wörterbuch mit Datenbegriffen finden Sie unter [Häufig gestellte Fragen und Terminologie zu Analysen für den kommerziellen Marketplace](./faq-terminology.md).

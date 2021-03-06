---
title: Was versteht man unter Azure Load Balancer?
titleSuffix: Azure Load Balancer
description: Übersicht über Features, Architektur und Implementierung des Azure Load Balancers. Hier erfahren Sie, wie Load Balancer funktioniert und wie Sie den Dienst in der Cloud verwenden.
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
Customer intent: As an IT administrator, I want to learn more about the Azure Load Balancer service and what I can use it for.
ms.devlang: na
ms.topic: overview
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 1/14/2020
ms.author: allensu
ms.openlocfilehash: 2853b1567618127866a4e9c61d81e599d3100823
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/26/2020
ms.locfileid: "80064269"
---
# <a name="what-is-azure-load-balancer"></a>Was versteht man unter Azure Load Balancer?

*Lastenausgleich* bezieht sich auf die gleichmäßige Verteilung von Last (oder eingehendem Netzwerkdatenverkehr) auf eine Gruppe von Back-End-Ressourcen oder Server. 

Azure Load Balancer setzt in der vierten Schicht des OSI-Modells (Open Systems Interconnection) an. Der Dienst ist der zentrale Kontaktpunkt für Clients. Load Balancer verteilt Datenflüsse, die beim Front-End des Lastenausgleichs eingehen, auf die Instanzen des Back-End-Pools. Diese Datenflüsse werden gemäß den konfigurierten Lastenausgleichsregeln und Integritätstests behandelt. Bei den Back-End-Poolinstanzen kann es sich um virtuelle Azure-Computer oder um Instanzen in einer VM-Skalierungsgruppe handeln.

Ein **[öffentlicher Lastenausgleich](./concepts-limitations.md#publicloadbalancer)** kann ausgehende Verbindungen für virtuelle Computer in Ihrem virtuellen Netzwerk bereitstellen. Diese Verbindungen werden durch die Übersetzung der privaten IP-Adressen in öffentliche IP-Adressen hergestellt. Öffentliche Load Balancer-Instanzen werden verwendet, um einen Lastausgleich für den eingehenden Internetdatenverkehr Ihrer virtuellen Computer vorzunehmen.

Ein **[interner (oder privater) Lastenausgleich](./concepts-limitations.md#internalloadbalancer)** wird verwendet, wenn private IP-Adressen nur am Front-End benötigt werden. Interne Lastenausgleichsmodule werden verwendet, um einen Lastausgleich für Datenverkehr innerhalb eines virtuellen Netzwerks vorzunehmen. Auf ein Lastenausgleichs-Front-End kann auch über ein lokales Netzwerk in einem Hybridszenario zugegriffen werden.

<p align="center">
  <img src="./media/load-balancer-overview/load-balancer.svg" width="512" title="Azure Load Balancer">
</p>

*Abbildung: Durchführen eines Lastenausgleichs für Anwendungen mit mehreren Ebenen mithilfe eines öffentlichen und eines internen Load Balancers*

Weitere Informationen zu den einzelnen Lastenausgleichskomponenten finden Sie unter [Azure Load Balancer-Komponenten und -Einschränkungen](./concepts-limitations.md).

>[!NOTE]
> Azure stellt eine Sammlung aus vollständig verwalteten Lastenausgleichslösungen für Ihre Szenarios bereit. Falls Sie einen Layer 7-Lastenausgleich mit hoher Leistung und geringer Latenz benötigen, helfen Ihnen die Informationen unter [Was ist Azure Application Gateway?](../application-gateway/overview.md) weiter. Wenn Sie nach Informationen zum globalen DNS-Lastenausgleich suchen, hilft Ihnen der Artikel [Was ist Traffic Manager?](../traffic-manager/traffic-manager-overview.md) weiter. Für Ihre End-to-End-Szenarien kann es vorteilhaft sein, diese Lösungen zu kombinieren.
>
> Einen Vergleich der Azure-Lastenausgleichsoptionen finden Sie unter [Übersicht über Lastenausgleichsoptionen in Azure](https://docs.microsoft.com/azure/architecture/guide/technology-choices/load-balancing-overview).

## <a name="why-use-azure-load-balancer"></a>Gründe für die Verwendung von Azure Load Balancer
Mit Load Balancer Standard können Sie Ihre Anwendungen skalieren und hochverfügbare Dienste erstellen. Der Lastenausgleich unterstützt Szenarien mit eingehenden und ausgehenden Verbindungen. Der Lastenausgleich sorgt für eine niedrige Wartezeit und einen hohen Durchsatz und kann eine zentrale Skalierung auf Millionen von Datenflüssen für alle TCP- und UDP-Anwendungen durchführen.

Im Anschluss finden Sie einige Schlüsselszenarien für Load Balancer Standard:

- Vornehmen eines Lastausgleichs für **[internen](https://docs.microsoft.com/azure/load-balancer/tutorial-load-balancer-standard-manage-portal)** und **[externen](https://docs.microsoft.com/azure/load-balancer/tutorial-load-balancer-standard-internal-portal)** Datenverkehr auf virtuellen Azure-Computern

- Erhöhen der Verfügbarkeit durch Verteilung von Ressourcen **[innerhalb](https://docs.microsoft.com/azure/load-balancer/tutorial-load-balancer-standard-public-zonal-portal)** von und **[zwischen](https://docs.microsoft.com/azure/load-balancer/tutorial-load-balancer-standard-public-zone-redundant-portal)** Zonen

- Konfigurieren der **[ausgehenden Konnektivität](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections)** für virtuelle Azure-Computer

- Verwenden von **[Integritätstests](https://docs.microsoft.com/azure/load-balancer/load-balancer-custom-probe-overview)** zur Überwachung von Ressourcen mit Lastenausgleich

- Verwenden der **[Portweiterleitung](https://docs.microsoft.com/azure/load-balancer/tutorial-load-balancer-port-forwarding-portal)** zum Zugreifen auf virtuelle Computer in einem virtuellen Netzwerk basierend auf der öffentlichen IP-Adresse und dem Port

- Aktivieren der Unterstützung für den **[Lastenausgleich](https://docs.microsoft.com/azure/virtual-network/virtual-network-ipv4-ipv6-dual-stack-standard-load-balancer-powershell)** für **[IPv6](https://docs.microsoft.com/azure/virtual-network/ipv6-overview)**

- Load Balancer Standard stellt mehrdimensionale Metriken über [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/overview) bereit.  Diese Metriken können für eine bestimmte Dimension gefiltert, gruppiert und unterteilt werden.  Sie liefern aktuelle und vergangenheitsbezogene Einblicke in die Leistung und Integrität Ihres Diensts.  Resource Health (Ressourcenintegrität) wird ebenfalls unterstützt. Ausführlichere Informationen finden Sie unter **[Load Balancer Standard-Diagnose mit Metriken, Warnungen und Ressourcenintegrität](load-balancer-standard-diagnostics.md)** .

- Vornehmen eines Lastausgleichs für Dienste an **[mehreren Ports, mehreren IP-Adressen oder beidem](https://docs.microsoft.com/azure/load-balancer/load-balancer-multivip-overview)**

- Verschieben der Ressourcen von **[internen](https://docs.microsoft.com/azure/load-balancer/move-across-regions-internal-load-balancer-portal)** und **[externen](https://docs.microsoft.com/azure/load-balancer/move-across-regions-external-load-balancer-portal)** Lastenausgleichsmodulen zwischen Azure-Regionen

- Vornehmen eines Lastausgleichs für TCP- und UDP-Datenflüsse an allen Ports gleichzeitig mithilfe von **[Hochverfügbarkeitsports](https://docs.microsoft.com/azure/load-balancer/load-balancer-ha-ports-overview)**

### <a name="secure-by-default"></a><a name="securebydefault"></a>Standardmäßig sicher

Load Balancer Standard basiert auf dem Zero Trust-Netzwerksicherheitsmodell. Load Balancer Standard ist standardmäßig sicher und Bestandteil Ihres virtuellen Netzwerks. Das virtuelle Netzwerk ist ein privates und isoliertes Netzwerk.  Das bedeutet, dass Load Balancer Standard-Instanzen sowie öffentliche Standard-IP-Adressen für eingehende Datenflüsse geschlossen sind, sofern sie nicht durch Netzwerksicherheitsgruppen geöffnet werden. NSGs werden verwendet, um zulässigen Datenverkehr explizit zuzulassen.  Wenn Sie über keine NSG für ein Subnetz oder für eine NIC Ihrer VM-Ressource verfügen, ist diese Ressource für Datenverkehr nicht erreichbar. Weitere Informationen zu NSGs und ihrer Verwendung in Ihrem Szenario finden Sie unter [Netzwerksicherheitsgruppen](../virtual-network/security-overview.md).
Load Balancer Basic ist standardmäßig für das Internet geöffnet.


## <a name="pricing-and-sla"></a>Preise und SLA

Informationen zu den Preisen für Load Balancer Standard finden Sie unter [Load Balancer – Preise](https://azure.microsoft.com/pricing/details/load-balancer/).
Basic Load Balancer wird kostenlos angeboten.
Weitere Informationen finden Sie unter [SLA für Load Balancer](https://aka.ms/lbsla). Für Load Balancer Basic steht keine SLA zur Verfügung.

## <a name="next-steps"></a>Nächste Schritte

Informationen zu den ersten Schritten mit einer Load Balancer-Instanz finden Sie unter [Schnellstart: Erstellen einer Load Balancer Standard-Instanz für den Lastenausgleich virtueller Computer im Azure-Portal](quickstart-load-balancer-standard-public-portal.md).

Weitere Informationen zu den Einschränkungen und Komponenten von Azure Load Balancer finden Sie unter [Azure Load Balancer-Komponenten und -Einschränkungen](./concepts-limitations.md).

---
title: Azure Linux-VM-Größen – vorherige Generationen | Microsoft-Dokumentation
description: Auflistung der vorherigen Generationen von verfügbaren Größen für virtuelle Linux-Computer in Azure. Dieser Artikel listet Informationen zur Anzahl von vCPUs, Datenträgern und Netzwerkschnittstellenkarten sowie zum Speicherdurchsatz und zur Netzwerkbandbreite für Größen dieser Serie auf.
services: virtual-machines-linux
documentationcenter: ''
author: mimckitt
manager: gwallace
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/20/2020
ms.author: jonbeck
ms.openlocfilehash: 6cf43df756e9bed0438169c9c01b868653d84b57
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2020
ms.locfileid: "80985727"
---
# <a name="previous-generations-of-virtual-machine-sizes"></a>Vorherige Generationen von Größen virtueller Computer

Dieser Abschnitt enthält Informationen zu den vorherigen Generationen von VM-Größen. Diese Größen können weiterhin verwendet werden, es sind jedoch neuere Generationen verfügbar.

## <a name="f-series"></a>F-Serie

Die F-Serie basiert auf dem Intel Xeon® E5-2673 v3-Prozessor (Haswell) mit 2,4 GHz, der mit der Intel Turbo Boost Technology 2.0 Taktfrequenzen von 3,1 GHz erreichen kann. Dies ist die gleiche CPU-Leistung wie bei virtuellen Computern (VMs) der Dv2-Serie.  

Virtuelle Computer der F-Serie sind eine hervorragende Wahl für Workloads, die schnellere CPUs erfordern, aber nicht so viel Arbeitsspeicher oder temporären Speicher pro vCPU benötigen.  Bei Arbeitslasten wie Analysen, Gamingservern, Webservern und Batchverarbeitung kommen die Vorteile und der Nutzen der F-Serie besonders gut zum Tragen.

ACU: 210–250

Storage Premium  Nicht unterstützt

Storage Premium-Zwischenspeicherung:  Nicht unterstützt

| Size | vCPU | Memory: GiB | Temporärer Speicher (SSD): GiB | Maximaler Durchsatz (temporärer Speicher): IOPS/MBit/s Lesen/MBit/s Schreiben | Max. Datenträger/Durchsatz: IOPS | Maximale Anzahl NICs/erwartete Netzwerkbandbreite (MBit/s) |
|---|---|---|---|---|---|---|
| Standard_F1  | 1  | 2  | 16  | 3000/46/23    | 4/4 x 500   | 2/750   |
| Standard_F2  | 2  | 4  | 32  | 6000/93/46    | 8/8 x 500   | 2/1500  |
| Standard_F4  | 4  | 8  | 64  | 12000/187/93  | 16/16 x 500 | 4/3000  |
| Standard_F8  | 8  | 16 | 128 | 24000/375/187 | 32/32 x 500 | 8/6000  |
| Standard_F16 | 16 | 32 | 256 | 48000/750/375 | 64/64 x 500 | 8/12000 |

## <a name="fs-series-sup1sup"></a>Fs-Serie <sup>1</sup>

Die Fs-Serie verfügt zusätzlich zum Premium-Speicher über alle Vorteile der F-Serie.

ACU: 210–250

Storage Premium  Unterstützt

Storage Premium-Zwischenspeicherung:  Unterstützt

| Size | vCPU | Memory: GiB | Temporärer Speicher (SSD): GiB | Max. Anzahl Datenträger | Maximaler Durchsatz (Cache und temporärer Speicher): IOPS/MBit/s (Cachegröße in GiB) | Maximaler Durchsatz des Datenträgers ohne Cache: IOPS/MBit/s | Maximale Anzahl NICs/erwartete Netzwerkbandbreite (MBit/s) |
|---|---|---|---|---|---|---|---|
| Standard_F1s  | 1  | 2  | 4  | 4  | 4000/32 (12)    | 3200/48   | 2/750   |
| Standard_F2s  | 2  | 4  | 8  | 8  | 8000/64 (24)    | 6400/96   | 2/1500  |
| Standard_F4s  | 4  | 8  | 16 | 16 | 16000/128 (48)  | 12800/192 | 4/3000  |
| Standard_F8s  | 8  | 16 | 32 | 32 | 32000/256 (96)  | 25600/384 | 8/6000  |
| Standard_F16s | 16 | 32 | 64 | 64 | 64000/512 (192) | 51200/768 | 8/12000 |

MB/s = 10^6 Bytes pro Sekunde und GB = 1.024^3 Bytes.

<sup>1</sup> Der mit einer VM der Fs-Serie maximal mögliche Datenträgerdurchsatz (IOPS oder MB/s) kann durch Anzahl, Größe und Striping der angefügten Datenträger beschränkt werden.  Details finden Sie unter Entwerfen für hohe Leistung für [Windows](windows/premium-storage-performance.md) oder [Linux](linux/premium-storage-performance.md).  


## <a name="nvv2-series"></a>NVv2-Serie

**Neuere Größenempfehlung**: [NVv3-Serie](nvv3-series.md)

Die virtuellen Computer der NVv2-Serie verfügen über GPUs vom Typ [NVIDIA Tesla M60](https://images.nvidia.com/content/tesla/pdf/188417-Tesla-M60-DS-A4-fnl-Web.pdf) sowie NVIDIA GRID-Technologie mit Intel Broadwell CPUs. Diese virtuellen Computer wurden für GPU-beschleunigte Grafikanwendungen und virtuelle Desktops entwickelt, um Kunden die Datenvisualisierung, Ergebnissimulation, CAD oder das Rendering und Streaming von Inhalten zu erleichtern. Außerdem können diese virtuellen Computer Workloads mit einfacher Genauigkeit wie Codierung und Rendering ausführen. Virtuelle Computer der NVv2-Serie unterstützen Storage Premium und verfügen im Vergleich zur NV-Vorgängerserie über doppelt so viel Systemspeicher (RAM).  

Alle GPUs in NVv2-Instanzen beinhalten eine GRID-Lizenz. Diese Lizenz bietet Ihnen die erforderliche Flexibilität für die Verwendung einer NV-Instanz als virtuelle Arbeitsstation für einen einzelnen Benutzer. Außerdem besteht für ein Szenario mit einer virtuellen Anwendung die Möglichkeit, dass 25 Benutzer gleichzeitig eine Verbindung mit dem virtuellen Computer herstellen.

| Size | vCPU | Memory: GiB | Temporärer Speicher (SSD): GiB | GPU | GPU-Arbeitsspeicher: GiB | Max. Anzahl Datenträger | Maximale Anzahl NICs | Virtuelle Arbeitsstationen | Virtuelle Anwendungen |
|---|---|---|---|---|---|---|---|---|---|
| Standard_NV6s_v2  | 6  | 112 | 320  | 1 | 8  | 12 | 4 | 1 | 25  |
| Standard_NV12s_v2 | 12 | 224 | 640  | 2 | 16 | 24 | 8 | 2 | 50  |
| Standard_NV24s_v2 | 24 | 448 | 1280 | 4 | 32 | 32 | 8 | 4 | 100 |

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="older-generations-of-virtual-machine-sizes"></a>Ältere Generationen von Größen virtueller Computer

Dieser Abschnitt enthält Informationen zu den älteren Generationen von Größen für virtuelle Computer. Diese Größen werden weiterhin unterstützt, erhalten jedoch keine zusätzliche Kapazität. Es gibt neuere oder alternative Größen, die allgemein verfügbar sind. Wählen Sie anhand des Artikels [Größen für virtuelle Linux-Computer in Azure](linux/sizes.md) die Größen für virtuelle Computer, die am besten zu Ihren Anforderungen passen.  

Weitere Informationen zum Ändern der Größe einer Linux-VM finden Sie unter [Ändern der Größe einer Linux-VM](linux/change-vm-size.md).  

<br>

### <a name="basic-a"></a>Basic A  

**Neuere Größenempfehlung**: [Av2-Serie](av2-series.md)

Storage Premium  Nicht unterstützt

Storage Premium-Zwischenspeicherung:  Nicht unterstützt

Die Größen des Basic-Tarifs sind hauptsächlich für Entwicklungsworkloads und andere Anwendungen bestimmt, für die kein Lastenausgleich, keine automatische Skalierung und keine speicherintensiven virtuellen Computer erforderlich sind.

| Größe – Größe\Name | vCPU | Arbeitsspeicher|Netzwerkkarten (max.)| Max. Größe der temporären Datenträger | Maximal Datenträger (jeweils 1023 GB)| Maximal IOPS (300 pro Datenträger) |
|---|---|---|---|---|---|---|
| A0\Basic_A0 | 1 | 768 MB  | 2 | 20 GB  | 1  | 1 x 300  |
| A1\Basic_A1 | 1 | 1,75 GB | 2 | 40 GB  | 2  | 2 x 300  |
| A2\Basic_A2 | 2 | 3,5 GB  | 2 | 60 GB  | 4  | 4 x 300  |
| A3\Basic_A3 | 4 | 7 GB    | 2 | 120 GB | 8  | 4 x 300  |
| A4\Basic_A4 | 8 | 14 GB   | 2 | 240 GB | 16 | 16 x 300 |

<br>

### <a name="standard-a0---a4-using-cli-and-powershell"></a>Standard_A0 – A4 mit Befehlszeilenschnittstelle (CLI) und PowerShell

Im klassischen Bereitstellungsmodell unterscheiden sich einige VM-Größennamen in der Befehlszeilenschnittstelle und in PowerShell:

* Standard_A0 ist „Sehr klein“
* Standard_A1 ist „Klein“
* Standard_A2 ist „Mittel“
* Standard_A3 ist „Groß“
* Standard_A4 ist „Extragroß“

### <a name="a-series"></a>A-Serie  

**Neuere Größenempfehlung**: [Av2-Serie](av2-series.md)

ACU: 50–100

Storage Premium  Nicht unterstützt

Storage Premium-Zwischenspeicherung:  Nicht unterstützt

| Size | vCPU | Memory: GiB | Temporärer Speicher (HDD): GiB | Max. Anzahl Datenträger | Max. Datenträgerdurchsatz: IOPS | Maximale Anzahl NICs/erwartete Netzwerkbandbreite (MBit/s) |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_A0&nbsp;<sup>1</sup> | 1 | 0,768 | 20 | 1 | 1 x 500 | 2/100 |
| Standard_A1 | 1 | 1,75 | 70  | 2  | 2 x 500  | 2/500  |
| Standard_A2 | 2 | 3,5  | 135 | 4  | 4 x 500  | 2/500  |
| Standard_A3 | 4 | 7    | 285 | 8  | 8 x 500  | 2/1000 |
| Standard_A4 | 8 | 14   | 605 | 16 | 16 x 500 | 4/2000 |
| Standard_A5 | 2 | 14   | 135 | 4  | 4 x 500  | 2/500  |
| Standard_A6 | 4 | 28   | 285 | 8  | 8 x 500  | 2/1000 |
| Standard_A7 | 8 | 56   | 605 | 16 | 16 x 500 | 4/2.000 |

<sup>1</sup> Die Größe „A0“ ist auf der physischen Hardware „überzeichnet“. Nur für diese spezielle Größe kann es dazu kommen, dass sich andere Kundenbereitstellungen negativ auf die Leistung Ihrer aktiven Workload auswirken. Die relative Leistung ist unten als erwartete Baseline beschrieben und unterliegt einer ungefähren Variabilität von 15 Prozent.

<br>

### <a name="a-series---compute-intensive-instances"></a>A-Serie: Rechenintensive Instanzen  

**Neuere Größenempfehlung**: [Av2-Serie](av2-series.md)

ACU: 225

Storage Premium  Nicht unterstützt

Storage Premium-Zwischenspeicherung:  Nicht unterstützt

Die Größen A8 bis A11 und die Größen der H-Reihe werden auch als *rechenintensive Instanzen* bezeichnet. Die Hardware, auf der diese Größen ausgeführt werden, wurde für rechenintensive Anwendungen mit hoher Netzwerkauslastung konzipiert und optimiert. Hierzu zählen beispielsweise HPC-Clusteranwendungen (High Performance Computing), Modellierung und Simulationen. Die Reihen A8 bis A11 nutzen Intel Xeon E5-2670 mit 2,6 GHZ und die H-Reihe Intel Xeon E5-2667 v3 mit 3,2 GHz.  

| Size | vCPU | Memory: GiB | Temporärer Speicher (HDD): GiB | Max. Anzahl Datenträger | Max. Datenträgerdurchsatz: IOPS | Maximale Anzahl NICs|
|---|---|---|---|---|---|---|
| Standard_A8&nbsp;<sup>1</sup> | 8  | 56  | 382 | 32 | 32 x 500 | 2 |
| Standard_A9&nbsp;<sup>1</sup> | 16 | 112 | 382 | 64 | 64 x 500 | 4 |
| Standard_A10 | 8  | 56  | 382 | 32 | 32 x 500 | 2 |
| Standard_A11 | 16 | 112 | 382 | 64 | 64 x 500 | 4 |

<sup>1</sup> Für MPI-Anwendungen ist ein dediziertes RDMA-Back-End-Netzwerk durch ein FDR InfiniBand-Netzwerk aktiviert, das eine äußerst geringe Latenz und eine hohe Bandbreite ermöglicht.  

> [!NOTE]
> Die Außerbetriebnahme von A8- bis A11-VMs ist für März 2021 geplant. Weitere Informationen finden Sie im [HPC-Migrationsleitfaden](https://azure.microsoft.com/resources/hpc-migration-guide/).

<br>

### <a name="d-series"></a>D-Serie  

**Neuere Größenempfehlung**: [Dv3-Serie](dv3-dsv3-series.md)

ACU: 160-250 <sup>1</sup>

Storage Premium  Nicht unterstützt

Storage Premium-Zwischenspeicherung:  Nicht unterstützt

| Size | vCPU | Memory: GiB | Temporärer Speicher (SSD): GiB | Maximaler Durchsatz (temporärer Speicher): IOPS/MBit/s Lesen/MBit/s Schreiben | Max. Datenträger/Durchsatz: IOPS | Maximale Anzahl NICs/erwartete Netzwerkbandbreite (MBit/s) |
|---|---|---|---|---|---|---|
| Standard_D1  | 1 | 3,5 | 50  | 3000/46/23    | 4/4 x 500   | 2/500  |
| Standard_D2  | 2 | 7   | 100 | 6000/93/46    | 8/8 x 500   | 2/1000 |
| Standard_D3  | 4 | 14  | 200 | 12000/187/93  | 16/16 x 500 | 4/2000 |
| Standard_D4  | 8 | 28  | 400 | 24000/375/187 | 32/32 x 500 | 8/4000 |

<sup>1</sup> Die VM-Familie kann auf einer der folgenden CPUs ausgeführt werden: Intel Xeon® E5-2660 v2 mit 2,2 GHz, Intel Xeon® E5-2673 v3 mit 2,4 GHz (Haswell) oder Intel XEON® E5-2673 v4 mit 2,3 GHz (Broadwell)  

<br>

### <a name="d-series---memory-optimized"></a>D-Serie – arbeitsspeicheroptimiert  

**Neuere Größenempfehlung**: [Dv3-Serie](dv3-dsv3-series.md)

ACU: 160-250 <sup>1</sup>

Storage Premium  Nicht unterstützt

Storage Premium-Zwischenspeicherung:  Nicht unterstützt

| Size | vCPU | Memory: GiB | Temporärer Speicher (SSD): GiB | Maximaler Durchsatz (temporärer Speicher): IOPS/MBit/s Lesen/MBit/s Schreiben | Max. Datenträger/Durchsatz: IOPS | Maximale Anzahl NICs/erwartete Netzwerkbandbreite (MBit/s) |
|---|---|---|---|---|---|---|
| Standard_D11 | 2  | 14  | 100 | 6000/93/46    | 8/8 x 500   | 2/1000 |
| Standard_D12 | 4  | 28  | 200 | 12000/187/93  | 16/16 x 500 | 4/2000 |
| Standard_D13 | 8  | 56  | 400 | 24000/375/187 | 32/32 x 500 | 8/4000 |
| Standard_D14 | 16 | 112 | 800 | 48000/750/375 | 64/64 x 500 | 8/8000 |

<sup>1</sup> Die VM-Familie kann auf einer der folgenden CPUs ausgeführt werden: Intel Xeon® E5-2660 v2 mit 2,2 GHz, Intel Xeon® E5-2673 v3 mit 2,4 GHz (Haswell) oder Intel XEON® E5-2673 v4 mit 2,3 GHz (Broadwell)  

<br>

## <a name="preview-dc-series"></a>Vorschau: DC-Serie

Storage Premium Unterstützt

Storage Premium-Zwischenspeicherung: Unterstützt

Die DC-Serie verwendet die neueste Generation von 3,7 GHz Intel XEON-E-2176g-Prozessoren mit SGX-Technologie, und mit der Intel Turbo Boost-Technologie können bis zu 4,7 GHz erreicht werden. 

| Size          | vCPU | Memory: GiB | Temporärer Speicher (SSD): GiB | Max. Anzahl Datenträger | Maximaler Durchsatz (Cache und temporärer Speicher): IOPS/MBps (Cachegröße in GiB) | Maximaler Durchsatz des Datenträgers ohne Cache: IOPS/MBps | Maximale Anzahl NICs/Erwartete Netzwerkbandbreite (MBps) |
|---------------|------|-------------|------------------------|----------------|-------------------------------------------------------------------------|-------------------------------------------|----------------------------------------------|
| Standard_DC2s | 2    | 8           | 100                    | 2              | 4000/32 (43)                                                          | 3200/48                                  | 2/1500                                     |
| Standard_DC4s | 4    | 16          | 200                    | 4              | 8000/64 (86)                                                          | 6400/96                                  | 2/3000                                     |

> [!IMPORTANT]
>
> Virtuelle Computer der DC-Serie sind [VMs der Generation 2](./linux/generation-2.md#creating-a-generation-2-vm) und unterstützen nur `Gen2`-Images.


### <a name="ds-series"></a>DS-Serie  

**Neuere Größenempfehlung**: [Dsv3-Serie](dv3-dsv3-series.md)

ACU: 160-250 <sup>1</sup>

Storage Premium  Unterstützt

Storage Premium-Zwischenspeicherung:  Unterstützt

| Size | vCPU | Memory: GiB | Temporärer Speicher (SSD): GiB | Max. Anzahl Datenträger | Maximaler Durchsatz (Cache und temporärer Speicher): IOPS/MBit/s (Cachegröße in GiB) | Maximaler Durchsatz des Datenträgers ohne Cache: IOPS/MBit/s | Maximale Anzahl NICs/erwartete Netzwerkbandbreite (MBit/s) |
|---|---|---|---|---|---|---|---|
| Standard_DS1 | 1 | 3,5 | 7  | 4  | 4000/32 (43)    | 3200/32   | 2/500  |
| Standard_DS2 | 2 | 7   | 14 | 8  | 8000/64 (86)    | 6400/64   | 2/1000 |
| Standard_DS3 | 4 | 14  | 28 | 16 | 16000/128 (172) | 12800/128 | 4/2000 |
| Standard_DS4 | 8 | 28  | 56 | 32 | 32000/256 (344) | 25600/256 | 8/4000 |

<sup>1</sup> Die VM-Familie kann auf einer der folgenden CPUs ausgeführt werden: Intel Xeon® E5-2660 v2 mit 2,2 GHz, Intel Xeon® E5-2673 v3 mit 2,4 GHz (Haswell) oder Intel XEON® E5-2673 v4 mit 2,3 GHz (Broadwell)  

<br>

### <a name="ds-series---memory-optimized"></a>DS-Serie – arbeitsspeicheroptimiert  

**Neuere Größenempfehlung**: [Dsv3-Serie](dv3-dsv3-series.md)

ACU: 160-250 <sup>1,2</sup>

Storage Premium  Unterstützt

Storage Premium-Zwischenspeicherung:  Unterstützt

| Size | vCPU | Memory: GiB | Temporärer Speicher (SSD): GiB | Max. Anzahl Datenträger | Maximaler Durchsatz (Cache und temporärer Speicher): IOPS/MBit/s (Cachegröße in GiB) | Maximaler Durchsatz des Datenträgers ohne Cache: IOPS/MBit/s | Maximale Anzahl NICs/erwartete Netzwerkbandbreite (MBit/s) |
|---|---|---|---|---|---|---|---|
| Standard_DS11 | 2  | 14  | 28  | 8  | 8000/64 (72)    | 6400/64   | 2/1000 |
| Standard_DS12 | 4  | 28  | 56  | 16 | 16000/128 (144) | 12800/128 | 4/2000 |
| Standard_DS13 | 8  | 56  | 112 | 32 | 32000/256 (288) | 25600/256 | 8/4000 |
| Standard_DS14 | 16 | 112 | 224 | 64 | 64000/512 (576) | 51200/512 | 8/8000 |

<sup>1</sup> Der mit einer VM der DS-Serie maximal mögliche Datenträgerdurchsatz (IOPS oder MB/s) kann durch Anzahl, Größe und Striping der angefügten Datenträger beschränkt werden.  Details finden Sie unter Entwerfen für hohe Leistung für [Windows](windows/premium-storage-performance.md) oder [Linux](linux/premium-storage-performance.md).
<sup>2</sup> Die VM-Familie kann auf einer der folgenden CPUs ausgeführt werden: Intel Xeon® E5-2660 v2 mit 2,2 GHz, Intel Xeon® E5-2673 v3 mit 2,4 GHz (Haswell) oder Intel XEON® E5-2673 v4 mit 2,3 GHz (Broadwell)  

<br>

### <a name="ls-series"></a>Ls-Serie

Die Ls-Reihe bietet bis zu 32 vCPUs aus der [Intel® Xeon® E5 v3-Prozessorfamilie](https://www.intel.com/content/www/us/en/processors/xeon/xeon-e5-solutions.html). Die Ls-Serie erreicht die gleiche CPU-Leistung wie die G/GS-Serie und bietet 8 GiB Arbeitsspeicher pro vCPU.

Die Ls-Serie unterstützt nicht die Erstellung eines lokalen Caches, um die von dauerhaften Datenträgern erreichbaren IOPS zu erhöhen. Dank des hohen Durchsatzes und der IOPS des lokalen Datenträgers eignen sich VMs der Ls-Reihe ideal für NoSQL-Speicher wie Apache Cassandra und MongoDB, die Daten über mehrere virtuelle Computer replizieren, um Persistenz im Falle eines Ausfalls eines einzelnen virtuellen Computers zu erzielen.

ACU: 180–240

Storage Premium  Unterstützt

Storage Premium-Zwischenspeicherung:  Nicht unterstützt

| Size | vCPU | Arbeitsspeicher (GiB) | Temporärer Speicher (GiB) | Max. Anzahl Datenträger | Maximaler Durchsatz temporärer Speicher (IOPS/MB/s) | Maximaler Datenträgerdurchsatz ohne Cache: (IOPS/MB/s) | Maximale Anzahl NICs/erwartete Netzwerkbandbreite (MBit/s) |
|---|---|---|---|---|---|---|---|
| Standard_L4s   | 4  | 32  | 678  | 16 | 20000/200 | 5000/125  | 2/4000  |
| Standard_L8s   | 8  | 64  | 1388 | 32 | 40000/400 | 10000/250 | 4/8000  |
| Standard_L16s  | 16 | 128 | 2807 | 64 | 80000/800 | 20000/500 | 8/16000 |
| Standard_L32s&nbsp;<sup>1</sup> | 32 | 256 | 5\.630 | 64 | 160000/1600 | 40000/1000 | 8/20000 |

Der mit einem virtuellen Computer der Ls-Serie maximal mögliche Datenträgerdurchsatz kann durch Anzahl, Größe und Striping angefügter Datenträger beschränkt sein. Details finden Sie unter Entwerfen für hohe Leistung für [Windows](windows/premium-storage-performance.md) oder [Linux](linux/premium-storage-performance.md).

<sup>1</sup> Instanz wird isoliert auf dedizierter Hardware ausgeführt, die für einen einzigen Kunden bereitgestellt wird.

### <a name="gs-series"></a>GS-Serie

ACU: 180–240 <sup>1</sup>

Storage Premium  Unterstützt

Storage Premium-Zwischenspeicherung:  Unterstützt

| Size | vCPU | Memory: GiB | Temporärer Speicher (SSD): GiB | Max. Anzahl Datenträger | Maximaler Durchsatz (Cache und temporärer Speicher): IOPS/MBps (Cachegröße in GiB) | Maximaler Durchsatz des Datenträgers ohne Cache: IOPS/MBit/s | Maximale Anzahl NICs/erwartete Netzwerkbandbreite (MBit/s) |
|---|---|---|---|---|---|---|---|
| Standard_GS1 | 2 | 28  | 56  | 8  | 10000/100 (264)  | 5000/125  | 2/2000 |
| Standard_GS2 | 4 | 56  | 112 | 16 | 20000/200 (528)  | 10000/250 | 2/4000 |
| Standard_GS3 | 8 | 112 | 224 | 32 | 40000/400 (1056) | 20000/500 | 4/8000 |
| Standard_GS4&nbsp;<sup>3</sup> | 16 | 224 | 448 | 64 | 80000/800 (2112) | 40000/1000 | 8/16000 |
| Standard_GS5&nbsp;<sup>2,&nbsp;3</sup> | 32 | 448 |896 | 64 |160000/1600 (4224) | 80000/2000 | 8/20000 |

<sup>1</sup> Der mit einer VM der GS-Serie maximal mögliche Datenträgerdurchsatz (IOPS oder MB/s) kann durch Anzahl, Größe und Striping der angefügten Datenträger beschränkt werden. Details finden Sie unter Entwerfen für hohe Leistung für [Windows](windows/premium-storage-performance.md) oder [Linux](linux/premium-storage-performance.md).

<sup>2</sup> Instanz wird isoliert auf dedizierter Hardware ausgeführt, die für einen einzigen Kunden bereitgestellt wird.

<sup>3</sup> Eingeschränkte Kerngrößen verfügbar.

<br>

### <a name="g-series"></a>G-Serie

ACU: 180–240

Storage Premium  Nicht unterstützt

Storage Premium-Zwischenspeicherung:  Nicht unterstützt

| Size | vCPU | Memory: GiB | Temporärer Speicher (SSD): GiB | Maximaler Durchsatz (temporärer Speicher): IOPS/MBit/s Lesen/MBit/s Schreiben | Max. Datenträger/Durchsatz: IOPS | Maximale Anzahl NICs/erwartete Netzwerkbandbreite (MBit/s) |
|---|---|---|---|---|---|---|
| Standard_G1  | 2  | 28  | 384  | 6000/93/46    | 8/8 x 500   | 2/2000  |
| Standard_G2  | 4  | 56  | 768  | 12000/187/93  | 16/16 x 500 | 2/4000  |
| Standard_G3  | 8  | 112 | 1536 | 24000/375/187 | 32/32 x 500 | 4/8000  |
| Standard_G4  | 16 | 224 | 3072 | 48000/750/375 | 64/64 x 500 | 8/16000 |
| Standard_G5&nbsp;<sup>1</sup> | 32 | 448 | 6\.144 | 96000/1500/750| 64/64 x 500 | 8/20000 |

<sup>1</sup> Instanz wird isoliert auf dedizierter Hardware ausgeführt, die für einen einzigen Kunden bereitgestellt wird.
<br>

## <a name="nv-series"></a>NV-Serie
**Neuere Größenempfehlung**: [NVv3-Serie](nvv3-series.md) und [NVv4-Serie](nvv4-series.md)

Die virtuellen Computer der NV-Serie nutzen NVIDIA-GPUs vom Typ [Tesla M60](https://images.nvidia.com/content/tesla/pdf/188417-Tesla-M60-DS-A4-fnl-Web.pdf) sowie NVIDIA GRID-Technologie und ermöglichen die Verwendung beschleunigter Desktopanwendungen und virtueller Desktops, mit denen Kunden ihre Daten oder Simulationen visualisieren können. Benutzer können ihre grafikintensiven Workflows mit den NV-Instanzen visualisieren, um überragende Grafikfunktionen zu erhalten, und darüber hinaus Workloads mit einfacher Genauigkeit ausführen (beispielsweise Codierung und Rendering). Virtuelle Computer der NV-Serie werden außerdem mit Intel Xeon E5-2690 v3 (Haswell)-CPUs betrieben.

Alle GPUs in NV-Instanzen beinhalten eine GRID-Lizenz. Diese Lizenz bietet Ihnen die erforderliche Flexibilität für die Verwendung einer NV-Instanz als virtuelle Arbeitsstation für einen einzelnen Benutzer. Außerdem besteht für ein Szenario mit einer virtuellen Anwendung die Möglichkeit, dass 25 Benutzer gleichzeitig eine Verbindung mit dem virtuellen Computer herstellen.

Storage Premium  Nicht unterstützt

Storage Premium-Zwischenspeicherung:  Nicht unterstützt

Livemigration: Nicht unterstützt

Updates mit Speicherbeibehaltung: Nicht unterstützt

| Size | vCPU | Memory: GiB | Temporärer Speicher (SSD): GiB | GPU | GPU-Arbeitsspeicher: GiB | Max. Anzahl Datenträger | Maximale Anzahl NICs | Virtuelle Arbeitsstationen | Virtuelle Anwendungen |
|---|---|---|---|---|---|---|---|---|---|
| Standard_NV6  | 6  | 56  | 340  | 1 | 8  | 24 | 1 | 1 | 25  |
| Standard_NV12 | 12 | 112 | 680  | 2 | 16 | 48 | 2 | 2 | 50  |
| Standard_NV24 | 24 | 224 | 1440 | 4 | 32 | 64 | 4 | 4 | 100 |

1 GPU = halbe M60-Karte
<br>

## <a name="other-sizes"></a>Andere Größen

* [Allgemeiner Zweck](sizes-general.md)
* [Computeoptimiert](sizes-compute.md)
* [Arbeitsspeicheroptimiert](sizes-memory.md)
* [Speicheroptimiert](sizes-storage.md)
* [GPU](sizes-gpu.md)
* [High Performance Computing](sizes-hpc.md)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen dazu, wie Sie mit [Azure-Computeeinheiten (ACU)](acu.md) die Computeleistung von Azure-SKUs vergleichen können.

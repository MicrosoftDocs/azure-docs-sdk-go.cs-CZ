---
title: Instalace Azure SDK for Go
description: Postup instalace, vendorizace a konfigurace sady Azure SDK for Go
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 03/14/2018
ms.topic: conceptual
ms.prod: azure
ms.technology: azure-sdk-go
ms.devlang: go
ms.openlocfilehash: 7990ec8bde5622078aa822fc7e66ba5c4384d682
ms.sourcegitcommit: 3d26b464f196f8675c636ae792637d4c882fb92c
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/27/2018
ms.locfileid: "52337139"
---
# <a name="install-the-azure-sdk-for-go"></a>Instalace Azure SDK for Go

Vítá vás Azure SDK for Go! Sada SDK umožňuje spravovat služby Azure a pracovat s nimi z aplikací Go.

## <a name="get-the-azure-sdk-for-go"></a>Získání sady Azure SDK for Go

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

Některé služby Azure mají vlastní sady Go SDK a nejsou zahrnuté v základním balíčku Azure SDK for Go. Následující tabulka uvádí služby s vlastními sadami SDK a názvy jejich balíčků. Všechny tyto balíčky se považují za verze Preview.

| Služba | Balíček |
|---------|---------|
| Blob Storage | [github.com/Azure/azure-storage-blob-go](https://github.com/Azure/azure-storage-blob-go) |
| File Storage | [github.com/Azure/azure-storage-file-go](https://github.com/Azure/azure-storage-file-go) |
| Fronta úložiště | [github.com/Azure/azure-storage-queue-go](https://github.com/Azure/azure-storage-queue-go) |
| Centrum událostí | [github.com/Azure/azure-event-hubs-go](https://github.com/Azure/azure-event-hubs-go) |
| Service Bus | [github.com/Azure/azure-service-bus-go](https://github.com/Azure/azure-service-bus-go) |
| Application Insights | [github.com/Microsoft/ApplicationInsights-go](https://github.com/Microsoft/ApplicationInsights-go) |

## <a name="vendor-the-azure-sdk-for-go"></a>Vendorizace sady Azure SDK for Go

K vendorizaci sady Azure SDK for Go je možné použít [dep](https://github.com/golang/dep). Z důvodů stability se doporučuje vendoring. Pokud chcete `dep` použít ve vašem vlastním projektu, přidejte `github.com/Azure/azure-sdk-for-go` do oddílu `[[constraint]]` ve vašem souboru `Gopkg.toml`. Pokud například chcete vendorizovat verzi `14.0.0`, přidejte následující položku:

```toml
[[constraint]]
name = "github.com/Azure/azure-sdk-for-go"
version = "14.0.0"
```

## <a name="include-the-azure-sdk-for-go-in-your-project"></a>Zahrnutí sady Azure SDK for Go do vašeho projektu

Pokud chcete používat služby Azure z kódu Go, naimportujte všechny služby, s nimiž interagujete, a požadované moduly `autorest`.
Získáte úplný seznam dostupných modulů z GoDoc pro [dostupné služby](https://godoc.org/github.com/Azure/azure-sdk-for-go) a [balíčky AutoRest](https://godoc.org/github.com/Azure/go-autorest). Nejběžnější balíčky, které potřebujete z `go-autorest`:

| Balíček | Popis |
|---------|-------------|
| [github.com/Azure/go-autorest/autorest][autorest] | Objekty pro zpracování ověřování klientů služby |
| [github.com/Azure/go-autorest/autorest/azure][autorest/azure] | Konstanty pro interakci se službami Azure |
| [github.com/Azure/go-autorest/autorest/adal][autorest/adal] | Mechanismy ověřování pro přístup ke službám Azure |
| [github.com/Azure/go-autorest/autorest/to][autorest/to] | Pomocné rutiny potvrzení typu pro práci s datovými strukturami Azure SDK |

[autorest]: https://godoc.org/github.com/Azure/go-autorest/autorest
[autorest/azure]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure
[autorest/adal]: https://godoc.org/github.com/Azure/go-autorest/autorest/adal
[autorest/to]: https://godoc.org/github.com/Azure/go-autorest/autorest/to

Balíčky Go a služby Azure mají verze nezávislé na sobě. Verze služeb jsou součástí cesty pro import modulu (pod modulem `services`). Úplnou cestu pro modul tvoří název příslušné služby, za kterým následuje verze ve formátu `YYYY-MM-DD` a potom znovu název služby. Pokud například chcete importovat verzi `2017-03-30` služby Compute:

```go
import "github.com/Azure/azure-sdk-for-go/services/compute/mgmt/2017-03-30/compute"
```

Při zahájení vývoje doporučujeme používat nejnovější verzi služby a zachovat její konzistenci.
Mezi jednotlivými verzemi může dojít ke změně požadavků na službu, které by mohly způsobit narušení vašeho kódu, a to i když během této doby nejsou žádné aktualizace sady Go SDK.

Pokud potřebujete souhrnný snímek služeb, můžete také vybrat jednu verzi profilu. Jediným uzamčeným profilem momentálně je verze `2017-03-09`, která nemusí mít nejnovější funkce služeb. Profily jsou umístěné v modulu `profiles` a jejich verze má formát `YYYY-MM-DD`. Služby jsou seskupené v příslušné verzi profilu. Pokud chcete například naimportovat modul správy prostředků Azure z profilu `2017-03-09`:

```go
import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/resources/mgmt/resources"
```

> [!WARNING]
> K dispozici jsou také profily `preview` a `latest`. Jejich použití se nedoporučuje. Tyto profily představují klouzavé verze a chování služeb se může kdykoli změnit.

## <a name="next-steps"></a>Další kroky

Pokud chcete začít používat sadu Azure SDK for Go, vyzkoušejte si rychlý start.

* [Nasazení virtuálního počítače ze šablony](azure-sdk-go-qs-vm.md)
* [Přenos objektů do Azure Blob Storage s využitím sady Azure Blob SDK for Go](/azure/storage/blobs/storage-quickstart-blobs-go?toc=%2fgo%2fazure%2ftoc.json)
* [Připojení k Azure Database for PostgreSQL](/azure/postgresql/connect-go?toc=%2fgo%2fazure%2ftoc.json)

Pokud chcete okamžitě začít pracovat s jinými službami v sadě Go SDK, prohlédněte si dostupný ukázkový kód.

* [Ověřování pomocí služeb Azure](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/internal/iam)
* [Nasazení nových virtuálních počítačů s ověřováním SSH](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute)
* [Nasazení image kontejneru do služby Azure Container Instances](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerinstance)
* [Vytvoření clusteru ve službě Azure Kubernetes](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerservice)
* [Práce se službami Azure Storage](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/storage)
* [Všechny ukázky pro Azure SDK for Go](https://github.com/azure-samples/azure-sdk-for-go-samples)

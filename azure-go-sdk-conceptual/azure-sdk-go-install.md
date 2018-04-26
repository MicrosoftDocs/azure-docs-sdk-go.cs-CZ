---
title: Instalace Azure SDK for Go
description: Postup instalace, vendorizace a konfigurace sady Azure SDK for Go
author: sptramer
ms.author: sttramer
ms.date: 03/14/2018
ms.topic: article
ms.devlang: go
manager: carmonm
ms.openlocfilehash: a6a92e080aea1a92f47a9d7083f133ca05a47541
ms.sourcegitcommit: 26520a8c6e812facb5b9432d68c370fa23c99888
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/16/2018
---
# <a name="install-the-azure-sdk-for-go"></a>Instalace Azure SDK for Go

Vítá vás Azure SDK for Go! Sada SDK umožňuje spravovat služby Azure a pracovat s nimi z aplikací Go.

## <a name="get-the-azure-sdk-for-go"></a>Získání sady Azure SDK for Go

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

Práce s rozšířeními Azure Storage Blob vyžaduje samostatnou sadu SDK.

```bash
go get -u -d github.com/Azure/azure-storage-blob-go/...
```

## <a name="vendor-the-azure-sdk-for-go"></a>Vendorizace sady Azure SDK for Go

K vendorizaci sady Azure SDK for Go je možné použít [dep](https://github.com/golang/dep). Z důvodů stability se doporučuje vendoring. Pokud chcete použít podporu `dep`, přidejte `github.com/Azure/azure-sdk-for-go` do části `[[constraint]]` v `Gopkg.toml`. Pokud například chcete vendorizovat verzi `14.0.0`, přidejte následující položku:

```
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

Moduly pro služby Azure mají verze nezávislé na příslušných rozhraních API SDK. Tyto verze jsou součástí cesty pro import modulu a jsou buď z _verze služby_, nebo z _profilu_. V současné době se doporučuje, abyste pro vývoj i vydání použili konkrétní verzi služby. Služby jsou umístěné v modulu `services`. Úplná cesta pro import je název služby, za kterým následuje verze ve formátu `YYYY-MM-DD` a potom znovu název služby. Pokud například chcete zahrnout verzi `2017-03-30` služby Compute:

```go
import "github.com/Azure/azure-sdk-for-go/services/compute/mgmt/2017-03-30/compute"
```

V tuto chvíli se doporučuje použít nejnovější verzi služby, pokud nemáte postupovat jinak.

Pokud potřebujete souhrnný snímek služeb, můžete také vybrat jednu verzi profilu. Jediným uzamčeným profilem momentálně je verze `2017-03-30`, která nemusí mít nejnovější funkce služeb. Profily jsou umístěné v modulu `profiles` a jejich verze má formát `YYYY-MM-DD`. Služby jsou seskupené v příslušné verzi profilu. Pokud chcete například naimportovat modul správy prostředků Azure z profilu `2017-03-09`:

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

* [Ověřování pomocí služeb Azure](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/iam)
* [Nasazení nových virtuálních počítačů s ověřováním SSH](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute)
* [Nasazení image kontejneru do služby Azure Container Instances](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerinstance)
* [Vytvoření clusteru ve službě Azure Kubernetes](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerservice)
* [Práce se službami Azure Storage](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/storage)
* [Všechny ukázky pro Azure SDK for Go](https://github.com/azure-samples/azure-sdk-for-go-samples)

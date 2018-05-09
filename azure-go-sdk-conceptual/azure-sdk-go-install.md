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
ms.openlocfilehash: ad77bdff881770512a828b19dc7af4821f4a55ad
ms.sourcegitcommit: f08abf902b48f8173aa6e261084ff2cfc9043305
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/03/2018
---
# <a name="install-the-azure-sdk-for-go"></a><span data-ttu-id="e3915-103">Instalace Azure SDK for Go</span><span class="sxs-lookup"><span data-stu-id="e3915-103">Install the Azure SDK for Go</span></span>

<span data-ttu-id="e3915-104">Vítá vás Azure SDK for Go!</span><span class="sxs-lookup"><span data-stu-id="e3915-104">Welcome to the Azure SDK for Go!</span></span> <span data-ttu-id="e3915-105">Sada SDK umožňuje spravovat služby Azure a pracovat s nimi z aplikací Go.</span><span class="sxs-lookup"><span data-stu-id="e3915-105">The SDK allows you to manage and interact with Azure services from your Go applications.</span></span>

## <a name="get-the-azure-sdk-for-go"></a><span data-ttu-id="e3915-106">Získání sady Azure SDK for Go</span><span class="sxs-lookup"><span data-stu-id="e3915-106">Get the Azure SDK for Go</span></span>

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

<span data-ttu-id="e3915-107">Práce s rozšířeními Azure Storage Blob vyžaduje samostatnou sadu SDK.</span><span class="sxs-lookup"><span data-stu-id="e3915-107">Working with Azure Storage Blobs requires a separate SDK.</span></span>

```bash
go get -u -d github.com/Azure/azure-storage-blob-go/...
```

## <a name="vendor-the-azure-sdk-for-go"></a><span data-ttu-id="e3915-108">Vendorizace sady Azure SDK for Go</span><span class="sxs-lookup"><span data-stu-id="e3915-108">Vendor the Azure SDK for Go</span></span>

<span data-ttu-id="e3915-109">K vendorizaci sady Azure SDK for Go je možné použít [dep](https://github.com/golang/dep).</span><span class="sxs-lookup"><span data-stu-id="e3915-109">The Azure SDK for Go may be vendored through [dep](https://github.com/golang/dep).</span></span> <span data-ttu-id="e3915-110">Z důvodů stability se doporučuje vendoring.</span><span class="sxs-lookup"><span data-stu-id="e3915-110">For stability reasons, vendoring is recommended.</span></span> <span data-ttu-id="e3915-111">Pokud chcete použít podporu `dep`, přidejte `github.com/Azure/azure-sdk-for-go` do části `[[constraint]]` v `Gopkg.toml`.</span><span class="sxs-lookup"><span data-stu-id="e3915-111">In order to use `dep` support, add `github.com/Azure/azure-sdk-for-go` to a `[[constraint]]` section of your `Gopkg.toml`.</span></span> <span data-ttu-id="e3915-112">Pokud například chcete vendorizovat verzi `14.0.0`, přidejte následující položku:</span><span class="sxs-lookup"><span data-stu-id="e3915-112">For example, to vendor on version `14.0.0`, add the following entry:</span></span>

```
[[constraint]]
name = "github.com/Azure/azure-sdk-for-go"
version = "14.0.0"
```

## <a name="include-the-azure-sdk-for-go-in-your-project"></a><span data-ttu-id="e3915-113">Zahrnutí sady Azure SDK for Go do vašeho projektu</span><span class="sxs-lookup"><span data-stu-id="e3915-113">Include the Azure SDK for Go in your project</span></span>

<span data-ttu-id="e3915-114">Pokud chcete používat služby Azure z kódu Go, naimportujte všechny služby, s nimiž interagujete, a požadované moduly `autorest`.</span><span class="sxs-lookup"><span data-stu-id="e3915-114">To use Azure services from your Go code, import any services you interact with and the required `autorest` modules.</span></span>
<span data-ttu-id="e3915-115">Získáte úplný seznam dostupných modulů z GoDoc pro [dostupné služby](https://godoc.org/github.com/Azure/azure-sdk-for-go) a [balíčky AutoRest](https://godoc.org/github.com/Azure/go-autorest).</span><span class="sxs-lookup"><span data-stu-id="e3915-115">You get a complete list of the available modules from GoDoc for [available services](https://godoc.org/github.com/Azure/azure-sdk-for-go) and [AutoRest packages](https://godoc.org/github.com/Azure/go-autorest).</span></span> <span data-ttu-id="e3915-116">Nejběžnější balíčky, které potřebujete z `go-autorest`:</span><span class="sxs-lookup"><span data-stu-id="e3915-116">The most common packages you need from `go-autorest` are:</span></span>

| <span data-ttu-id="e3915-117">Balíček</span><span class="sxs-lookup"><span data-stu-id="e3915-117">Package</span></span> | <span data-ttu-id="e3915-118">Popis</span><span class="sxs-lookup"><span data-stu-id="e3915-118">Description</span></span> |
|---------|-------------|
| <span data-ttu-id="e3915-119">[github.com/Azure/go-autorest/autorest][autorest]</span><span class="sxs-lookup"><span data-stu-id="e3915-119">[github.com/Azure/go-autorest/autorest][autorest]</span></span> | <span data-ttu-id="e3915-120">Objekty pro zpracování ověřování klientů služby</span><span class="sxs-lookup"><span data-stu-id="e3915-120">Objects for handling service client authentication</span></span> |
| <span data-ttu-id="e3915-121">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span><span class="sxs-lookup"><span data-stu-id="e3915-121">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span></span> | <span data-ttu-id="e3915-122">Konstanty pro interakci se službami Azure</span><span class="sxs-lookup"><span data-stu-id="e3915-122">Constants for interactions with Azure services</span></span> |
| <span data-ttu-id="e3915-123">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span><span class="sxs-lookup"><span data-stu-id="e3915-123">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span></span> | <span data-ttu-id="e3915-124">Mechanismy ověřování pro přístup ke službám Azure</span><span class="sxs-lookup"><span data-stu-id="e3915-124">Authentication mechanisms for accessing Azure services</span></span> |
| <span data-ttu-id="e3915-125">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span><span class="sxs-lookup"><span data-stu-id="e3915-125">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span></span> | <span data-ttu-id="e3915-126">Pomocné rutiny potvrzení typu pro práci s datovými strukturami Azure SDK</span><span class="sxs-lookup"><span data-stu-id="e3915-126">Type assertion helpers for working with Azure SDK data structures</span></span> |

[autorest]: https://godoc.org/github.com/Azure/go-autorest/autorest
[autorest/azure]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure
[autorest/adal]: https://godoc.org/github.com/Azure/go-autorest/autorest/adal
[autorest/to]: https://godoc.org/github.com/Azure/go-autorest/autorest/to

<span data-ttu-id="e3915-127">Moduly pro služby Azure mají verze nezávislé na příslušných rozhraních API SDK.</span><span class="sxs-lookup"><span data-stu-id="e3915-127">Modules for Azure services are versioned independently from the SDK APIs for them.</span></span> <span data-ttu-id="e3915-128">Tyto verze jsou součástí cesty pro import modulu a jsou buď z _verze služby_, nebo z _profilu_.</span><span class="sxs-lookup"><span data-stu-id="e3915-128">These versions are part of the module import path, and are from either a _service version_ or a _profile_.</span></span> <span data-ttu-id="e3915-129">V současné době se doporučuje, abyste pro vývoj i vydání použili konkrétní verzi služby.</span><span class="sxs-lookup"><span data-stu-id="e3915-129">Currently, it is recommended that you use a specific service version for both development and release.</span></span> <span data-ttu-id="e3915-130">Služby jsou umístěné v modulu `services`.</span><span class="sxs-lookup"><span data-stu-id="e3915-130">Services are located under the `services` module.</span></span> <span data-ttu-id="e3915-131">Úplná cesta pro import je název služby, za kterým následuje verze ve formátu `YYYY-MM-DD` a potom znovu název služby.</span><span class="sxs-lookup"><span data-stu-id="e3915-131">The full path for the import is the name of the service, followed by the version in `YYYY-MM-DD` format, followed by the service name again.</span></span> <span data-ttu-id="e3915-132">Pokud například chcete zahrnout verzi `2017-03-30` služby Compute:</span><span class="sxs-lookup"><span data-stu-id="e3915-132">For example, to include the `2017-03-30` version of the Compute service:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/services/compute/mgmt/2017-03-30/compute"
```

<span data-ttu-id="e3915-133">V tuto chvíli se doporučuje použít nejnovější verzi služby, pokud nemáte postupovat jinak.</span><span class="sxs-lookup"><span data-stu-id="e3915-133">Right now it is recommended that you use the latest version of a service, unless you have a reason to do otherwise.</span></span>

<span data-ttu-id="e3915-134">Pokud potřebujete souhrnný snímek služeb, můžete také vybrat jednu verzi profilu.</span><span class="sxs-lookup"><span data-stu-id="e3915-134">If you need a collective snapshot of services, you can also select a single profile version.</span></span> <span data-ttu-id="e3915-135">Jediným uzamčeným profilem momentálně je verze `2017-03-09`, která nemusí mít nejnovější funkce služeb.</span><span class="sxs-lookup"><span data-stu-id="e3915-135">Right now, the only locked profile is version `2017-03-09`, which may not have the latest features of services.</span></span> <span data-ttu-id="e3915-136">Profily jsou umístěné v modulu `profiles` a jejich verze má formát `YYYY-MM-DD`.</span><span class="sxs-lookup"><span data-stu-id="e3915-136">Profiles are located under the `profiles` module, with their version in the `YYYY-MM-DD` format.</span></span> <span data-ttu-id="e3915-137">Služby jsou seskupené v příslušné verzi profilu.</span><span class="sxs-lookup"><span data-stu-id="e3915-137">Services are grouped under their profile version.</span></span> <span data-ttu-id="e3915-138">Pokud chcete například naimportovat modul správy prostředků Azure z profilu `2017-03-09`:</span><span class="sxs-lookup"><span data-stu-id="e3915-138">For example, to import the Azure Resources management module from the `2017-03-09` profile:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/resources/mgmt/resources"
```

> [!WARNING]
> <span data-ttu-id="e3915-139">K dispozici jsou také profily `preview` a `latest`.</span><span class="sxs-lookup"><span data-stu-id="e3915-139">There are also `preview` and `latest` profiles available.</span></span> <span data-ttu-id="e3915-140">Jejich použití se nedoporučuje.</span><span class="sxs-lookup"><span data-stu-id="e3915-140">Using them is not recommended.</span></span> <span data-ttu-id="e3915-141">Tyto profily představují klouzavé verze a chování služeb se může kdykoli změnit.</span><span class="sxs-lookup"><span data-stu-id="e3915-141">These profiles are rolling versions and service behavior may change at any time.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e3915-142">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e3915-142">Next steps</span></span>

<span data-ttu-id="e3915-143">Pokud chcete začít používat sadu Azure SDK for Go, vyzkoušejte si rychlý start.</span><span class="sxs-lookup"><span data-stu-id="e3915-143">To begin using the Azure SDK for Go, try out a quickstart.</span></span>

* [<span data-ttu-id="e3915-144">Nasazení virtuálního počítače ze šablony</span><span class="sxs-lookup"><span data-stu-id="e3915-144">Deploy a virtual machine from a template</span></span>](azure-sdk-go-qs-vm.md)
* [<span data-ttu-id="e3915-145">Přenos objektů do Azure Blob Storage s využitím sady Azure Blob SDK for Go</span><span class="sxs-lookup"><span data-stu-id="e3915-145">Transfer objects to Azure Blob Storage with the Azure Blob SDK for Go</span></span>](/azure/storage/blobs/storage-quickstart-blobs-go?toc=%2fgo%2fazure%2ftoc.json)
* [<span data-ttu-id="e3915-146">Připojení k Azure Database for PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="e3915-146">Connect to Azure Database for PostgreSQL</span></span>](/azure/postgresql/connect-go?toc=%2fgo%2fazure%2ftoc.json)

<span data-ttu-id="e3915-147">Pokud chcete okamžitě začít pracovat s jinými službami v sadě Go SDK, prohlédněte si dostupný ukázkový kód.</span><span class="sxs-lookup"><span data-stu-id="e3915-147">If you want to get started with other services in the Go SDK immediately, take a look at some of the available sample code.</span></span>

* [<span data-ttu-id="e3915-148">Ověřování pomocí služeb Azure</span><span class="sxs-lookup"><span data-stu-id="e3915-148">Authenticate with Azure services</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/iam)
* [<span data-ttu-id="e3915-149">Nasazení nových virtuálních počítačů s ověřováním SSH</span><span class="sxs-lookup"><span data-stu-id="e3915-149">Deploy new virtual machines with SSH authentication</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute)
* [<span data-ttu-id="e3915-150">Nasazení image kontejneru do služby Azure Container Instances</span><span class="sxs-lookup"><span data-stu-id="e3915-150">Deploy a container image to Azure Container Instances</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerinstance)
* [<span data-ttu-id="e3915-151">Vytvoření clusteru ve službě Azure Kubernetes</span><span class="sxs-lookup"><span data-stu-id="e3915-151">Create a cluster in Azure Kubernetes Service</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerservice)
* [<span data-ttu-id="e3915-152">Práce se službami Azure Storage</span><span class="sxs-lookup"><span data-stu-id="e3915-152">Work with Azure Storage services</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/storage)
* [<span data-ttu-id="e3915-153">Všechny ukázky pro Azure SDK for Go</span><span class="sxs-lookup"><span data-stu-id="e3915-153">All samples for the Azure SDK for Go</span></span>](https://github.com/azure-samples/azure-sdk-for-go-samples)

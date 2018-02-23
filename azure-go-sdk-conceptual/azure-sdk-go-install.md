---
title: Instalace Azure SDK for Go
description: Postup instalace, vendorizace a konfigurace sady Azure SDK for Go
keywords: azure, sdk, go, golang, azure sdk
author: sptramer
ms.author: sttramer
ms.date: 01/30/2018
ms.topic: article
ms.devlang: go
manager: routlaw
ms.openlocfilehash: f822a9304a4744e0b0e93286303aa8bb80fec852
ms.sourcegitcommit: aaa8c37880332625f858a38f5918e6cf581bf48d
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/15/2018
---
# <a name="installing-the-azure-sdk-for-go"></a><span data-ttu-id="b0e40-104">Instalace Azure SDK for Go</span><span class="sxs-lookup"><span data-stu-id="b0e40-104">Installing the Azure SDK for Go</span></span>

<span data-ttu-id="b0e40-105">Vítá vás Azure SDK for Go!</span><span class="sxs-lookup"><span data-stu-id="b0e40-105">Welcome to the Azure SDK for Go!</span></span> <span data-ttu-id="b0e40-106">Tato sada SDK umožňuje spravovat a služby Azure a interagovat s nimi z aplikací Go.</span><span class="sxs-lookup"><span data-stu-id="b0e40-106">This SDK allows you to manage and interact with Azure services from your Go applications.</span></span>

## <a name="get-the-azure-sdk-for-go"></a><span data-ttu-id="b0e40-107">Získání sady Azure SDK for Go</span><span class="sxs-lookup"><span data-stu-id="b0e40-107">Get the Azure SDK for Go</span></span>

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

<span data-ttu-id="b0e40-108">Práce s rozšířeními Azure Storage Blob vyžaduje samostatnou sadu SDK.</span><span class="sxs-lookup"><span data-stu-id="b0e40-108">Working with Azure Storage Blobs requires a separate SDK.</span></span>

```bash
go get -u -d github.com/Azure/azure-storage-blob-go/...
```

## <a name="vendoring-the-azure-sdk-for-go"></a><span data-ttu-id="b0e40-109">Vendoring sady Azure SDK for Go</span><span class="sxs-lookup"><span data-stu-id="b0e40-109">Vendoring the Azure SDK for Go</span></span>

<span data-ttu-id="b0e40-110">K vendorizaci sady Azure SDK for Go je možné použít [dep](https://github.com/golang/dep).</span><span class="sxs-lookup"><span data-stu-id="b0e40-110">The Azure SDK for Go may be vendored through [dep](https://github.com/golang/dep).</span></span> <span data-ttu-id="b0e40-111">Z důvodů stability se doporučuje vendoring.</span><span class="sxs-lookup"><span data-stu-id="b0e40-111">For stability reasons, vendoring is recommended.</span></span> <span data-ttu-id="b0e40-112">Pokud chcete použít podporu `dep`, přidejte `gitub.com/Azure/azure-sdk-for-go` do části `[[constraint]]` v `Gopkg.toml`.</span><span class="sxs-lookup"><span data-stu-id="b0e40-112">In order to use `dep` support, add `gitub.com/Azure/azure-sdk-for-go` to a `[[constraint]]` section of your `Gopkg.toml`.</span></span> <span data-ttu-id="b0e40-113">Pokud například chcete vendorizovat verzi `14.0.0`, přidejte následující položku:</span><span class="sxs-lookup"><span data-stu-id="b0e40-113">For example, to vendor on version `14.0.0`, add the following entry:</span></span>

```
[[constraint]]
name = "github.com/Azure/azure-sdk-for-go"
version = "14.0.0"
```

## <a name="including-the-azure-sdk-for-go-in-your-project"></a><span data-ttu-id="b0e40-114">Zahrnutí sady Azure SDK for Go do vašeho projektu</span><span class="sxs-lookup"><span data-stu-id="b0e40-114">Including the Azure SDK for Go in your project</span></span>

<span data-ttu-id="b0e40-115">Pokud chcete používat služby Azure z kódu Go, naimportujte všechny služby, s nimiž interagujete, a požadované moduly `autorest`.</span><span class="sxs-lookup"><span data-stu-id="b0e40-115">To use Azure services from your Go code, import any services you interact with and the required `autorest` modules.</span></span>
<span data-ttu-id="b0e40-116">Získáte úplný seznam dostupných modulů z GoDoc pro [dostupné služby](https://godoc.org/github.com/Azure/azure-sdk-for-go) a [balíčky AutoRest](https://godoc.org/github.com/Azure/go-autorest).</span><span class="sxs-lookup"><span data-stu-id="b0e40-116">You get a complete list of the available modules from GoDoc for [available services](https://godoc.org/github.com/Azure/azure-sdk-for-go) and [AutoRest packages](https://godoc.org/github.com/Azure/go-autorest).</span></span> <span data-ttu-id="b0e40-117">Nejběžnější balíčky, které potřebujete z `go-autorest`:</span><span class="sxs-lookup"><span data-stu-id="b0e40-117">The most common packages you need from `go-autorest` are:</span></span>

| <span data-ttu-id="b0e40-118">Balíček</span><span class="sxs-lookup"><span data-stu-id="b0e40-118">Package</span></span> | <span data-ttu-id="b0e40-119">Popis</span><span class="sxs-lookup"><span data-stu-id="b0e40-119">Description</span></span> |
|---------|-------------|
| <span data-ttu-id="b0e40-120">[github.com/Azure/go-autorest/autorest][autorest]</span><span class="sxs-lookup"><span data-stu-id="b0e40-120">[github.com/Azure/go-autorest/autorest][autorest]</span></span> | <span data-ttu-id="b0e40-121">Objekty pro zpracování ověřování klientů služby</span><span class="sxs-lookup"><span data-stu-id="b0e40-121">Objects for handling service client authentication</span></span> |
| <span data-ttu-id="b0e40-122">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span><span class="sxs-lookup"><span data-stu-id="b0e40-122">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span></span> | <span data-ttu-id="b0e40-123">Konstanty pro interakci se službami Azure</span><span class="sxs-lookup"><span data-stu-id="b0e40-123">Constants for interactions with Azure services</span></span> |
| <span data-ttu-id="b0e40-124">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span><span class="sxs-lookup"><span data-stu-id="b0e40-124">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span></span> | <span data-ttu-id="b0e40-125">Mechanismy ověřování pro přístup ke službám Azure</span><span class="sxs-lookup"><span data-stu-id="b0e40-125">Authentication mechanisms for accessing Azure services</span></span> |
| <span data-ttu-id="b0e40-126">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span><span class="sxs-lookup"><span data-stu-id="b0e40-126">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span></span> | <span data-ttu-id="b0e40-127">Pomocné rutiny potvrzení typu pro práci s datovými strukturami Azure SDK</span><span class="sxs-lookup"><span data-stu-id="b0e40-127">Type assertion helpers for working with Azure SDK data structures</span></span> |

[autorest]: https://godoc.org/github.com/Azure/go-autorest/autorest
[autorest/azure]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure
[autorest/adal]: https://godoc.org/github.com/Azure/go-autorest/autorest/adal
[autorest/to]: https://godoc.org/github.com/Azure/go-autorest/autorest/to

<span data-ttu-id="b0e40-128">Moduly pro služby Azure mají verze nezávislé na příslušných rozhraních API SDK.</span><span class="sxs-lookup"><span data-stu-id="b0e40-128">Modules for Azure services are versioned independently from the SDK APIs for them.</span></span> <span data-ttu-id="b0e40-129">Tyto verze jsou součástí cesty pro import modulu a jsou buď z _verze služby_, nebo z _profilu_.</span><span class="sxs-lookup"><span data-stu-id="b0e40-129">These versions are part of the module import path, and are from either a _service version_ or a _profile_.</span></span> <span data-ttu-id="b0e40-130">V současné době se doporučuje, abyste pro vývoj i vydání použili konkrétní verzi služby.</span><span class="sxs-lookup"><span data-stu-id="b0e40-130">Currently, it is recommended that you use a specific service version for both development and release.</span></span> <span data-ttu-id="b0e40-131">Služby jsou umístěné v modulu `services`.</span><span class="sxs-lookup"><span data-stu-id="b0e40-131">Services are located under the `services` module.</span></span> <span data-ttu-id="b0e40-132">Úplná cesta pro import je název služby, za kterým následuje verze ve formátu `YYYY-MM-DD` a potom znovu název služby.</span><span class="sxs-lookup"><span data-stu-id="b0e40-132">The full path for the import is the name of the service, followed by the version in `YYYY-MM-DD` format, followed by the service name again.</span></span> <span data-ttu-id="b0e40-133">Pokud například chcete zahrnout verzi `2017-03-30` služby Compute:</span><span class="sxs-lookup"><span data-stu-id="b0e40-133">For example, to include the `2017-03-30` version of the Compute service:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/services/compute/mgmt/2017-03-30/compute"
```

<span data-ttu-id="b0e40-134">V tuto chvíli se doporučuje použít nejnovější verzi služby, pokud nemáte postupovat jinak.</span><span class="sxs-lookup"><span data-stu-id="b0e40-134">Right now it is recommended that you use the latest version of a service, unless you have a reason to do otherwise.</span></span>

<span data-ttu-id="b0e40-135">Pokud potřebujete souhrnný snímek služeb, můžete také vybrat jednu verzi profilu.</span><span class="sxs-lookup"><span data-stu-id="b0e40-135">If you need a collective snapshot of services, you can also select a single profile version.</span></span> <span data-ttu-id="b0e40-136">Jediným uzamčeným profilem momentálně je verze `2017-03-30`, která nemusí mít nejnovější funkce služeb.</span><span class="sxs-lookup"><span data-stu-id="b0e40-136">Right now, the only locked profile is version `2017-03-30`, which may not have the latest features of services.</span></span> <span data-ttu-id="b0e40-137">Profily jsou umístěné v modulu `profiles` a jejich verze má formát `YYYY-MM-DD`.</span><span class="sxs-lookup"><span data-stu-id="b0e40-137">Profiles are located under the `profiles` module, with their version in the `YYYY-MM-DD` format.</span></span> <span data-ttu-id="b0e40-138">Služby jsou seskupené v příslušné verzi profilu.</span><span class="sxs-lookup"><span data-stu-id="b0e40-138">Services are grouped under their profile version.</span></span> <span data-ttu-id="b0e40-139">Pokud chcete například naimportovat modul správy prostředků Azure z profilu `2017-03-09`:</span><span class="sxs-lookup"><span data-stu-id="b0e40-139">For example, to import the Azure Resources management module from the `2017-03-09` profile:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/resources/mgmt/resources"
```

> [!WARNING]
> <span data-ttu-id="b0e40-140">K dispozici jsou také profily `preview` a `latest`.</span><span class="sxs-lookup"><span data-stu-id="b0e40-140">There are also `preview` and `latest` profiles available.</span></span> <span data-ttu-id="b0e40-141">Jejich použití se nedoporučuje.</span><span class="sxs-lookup"><span data-stu-id="b0e40-141">Using them is not recommended.</span></span> <span data-ttu-id="b0e40-142">Tyto profily představují klouzavé verze a chování služeb se může kdykoli změnit.</span><span class="sxs-lookup"><span data-stu-id="b0e40-142">These profiles are rolling versions and service behavior may change at any time.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b0e40-143">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b0e40-143">Next steps</span></span>

<span data-ttu-id="b0e40-144">Pokud chcete začít používat sadu Azure SDK for Go, vyzkoušejte si rychlý start.</span><span class="sxs-lookup"><span data-stu-id="b0e40-144">To begin using the Azure SDK for Go, try out a quickstart.</span></span>

* [<span data-ttu-id="b0e40-145">Nasazení virtuálního počítače ze šablony</span><span class="sxs-lookup"><span data-stu-id="b0e40-145">Deploy a virtual machine from a template</span></span>](azure-sdk-go-qs-vm.md)
* [<span data-ttu-id="b0e40-146">Přenos objektů do Azure Blob Storage s využitím sady Azure Blob SDK for Go</span><span class="sxs-lookup"><span data-stu-id="b0e40-146">Transfer objects to Azure Blob Storage with the Azure Blob SDK for Go</span></span>](/azure/storage/blobs/storage-quickstart-blobs-go?toc=%2fgo%2fazure%2ftoc.json)
* [<span data-ttu-id="b0e40-147">Připojení k Azure Database for PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="b0e40-147">Connect to Azure Database for PostgreSQL</span></span>](/azure/postgresql/connect-go?toc=%2fgo%2fazure%2ftoc.json)

<span data-ttu-id="b0e40-148">Pokud chcete okamžitě začít pracovat s jinými službami v sadě Go SDK, prohlédněte si dostupný ukázkový kód.</span><span class="sxs-lookup"><span data-stu-id="b0e40-148">If you want to get started with other services in the Go SDK immediately, take a look at some of the available sample code.</span></span>

* [<span data-ttu-id="b0e40-149">Ověřování pomocí služeb Azure</span><span class="sxs-lookup"><span data-stu-id="b0e40-149">Authenticate with Azure services</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/iam)
* [<span data-ttu-id="b0e40-150">Nasazení nových virtuálních počítačů s ověřováním SSH</span><span class="sxs-lookup"><span data-stu-id="b0e40-150">Deploy new virtual machines with SSH authentication</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute)
* [<span data-ttu-id="b0e40-151">Nasazení image kontejneru do služby Azure Container Instances</span><span class="sxs-lookup"><span data-stu-id="b0e40-151">Deploy a container image to Azure Container Instances</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerinstance)
* [<span data-ttu-id="b0e40-152">Vytvoření clusteru ve službě Azure Kubernetes</span><span class="sxs-lookup"><span data-stu-id="b0e40-152">Create a cluster in Azure Kubernetes Service</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerservice)
* [<span data-ttu-id="b0e40-153">Práce se službami Azure Storage</span><span class="sxs-lookup"><span data-stu-id="b0e40-153">Work with Azure Storage services</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/storage)
* [<span data-ttu-id="b0e40-154">Všechny ukázky pro Azure SDK for Go</span><span class="sxs-lookup"><span data-stu-id="b0e40-154">All samples for the Azure SDK for Go</span></span>](https://github.com/azure-samples/azure-sdk-for-go-samples)

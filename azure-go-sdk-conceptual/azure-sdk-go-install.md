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
ms.openlocfilehash: 8423b3fedd89e57662bf96f777a5b30926914da9
ms.sourcegitcommit: b81b17cbb934399c195bfdcb87137aee935f5234
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/04/2018
ms.locfileid: "34755511"
---
# <a name="install-the-azure-sdk-for-go"></a><span data-ttu-id="24be1-103">Instalace Azure SDK for Go</span><span class="sxs-lookup"><span data-stu-id="24be1-103">Install the Azure SDK for Go</span></span>

<span data-ttu-id="24be1-104">Vítá vás Azure SDK for Go!</span><span class="sxs-lookup"><span data-stu-id="24be1-104">Welcome to the Azure SDK for Go!</span></span> <span data-ttu-id="24be1-105">Sada SDK umožňuje spravovat služby Azure a pracovat s nimi z aplikací Go.</span><span class="sxs-lookup"><span data-stu-id="24be1-105">The SDK allows you to manage and interact with Azure services from your Go applications.</span></span>

## <a name="get-the-azure-sdk-for-go"></a><span data-ttu-id="24be1-106">Získání sady Azure SDK for Go</span><span class="sxs-lookup"><span data-stu-id="24be1-106">Get the Azure SDK for Go</span></span>

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

<span data-ttu-id="24be1-107">Některé služby Azure mají vlastní sady Go SDK a nejsou zahrnuté v základním balíčku Azure SDK for Go.</span><span class="sxs-lookup"><span data-stu-id="24be1-107">Some Azure services have their own Go SDK and aren't included in the core Azure SDK for Go package.</span></span> <span data-ttu-id="24be1-108">Následující tabulka uvádí služby s vlastními sadami SDK a názvy jejich balíčků.</span><span class="sxs-lookup"><span data-stu-id="24be1-108">The following table lists the services with their own SDKs and their package names.</span></span> <span data-ttu-id="24be1-109">Všechny tyto balíčky se považují za verze Preview.</span><span class="sxs-lookup"><span data-stu-id="24be1-109">These packages are all considered to be in preview.</span></span>

| <span data-ttu-id="24be1-110">Služba</span><span class="sxs-lookup"><span data-stu-id="24be1-110">Service</span></span> | <span data-ttu-id="24be1-111">Balíček</span><span class="sxs-lookup"><span data-stu-id="24be1-111">Package</span></span> |
|---------|---------|
| <span data-ttu-id="24be1-112">Blob Storage</span><span class="sxs-lookup"><span data-stu-id="24be1-112">Blob Storage</span></span> | [<span data-ttu-id="24be1-113">github.com/Azure/azure-storage-blob-go</span><span class="sxs-lookup"><span data-stu-id="24be1-113">github.com/Azure/azure-storage-blob-go</span></span>](https://github.com/Azure/azure-storage-blob-go) |
| <span data-ttu-id="24be1-114">File Storage</span><span class="sxs-lookup"><span data-stu-id="24be1-114">File Storage</span></span> | [<span data-ttu-id="24be1-115">github.com/Azure/azure-storage-file-go</span><span class="sxs-lookup"><span data-stu-id="24be1-115">github.com/Azure/azure-storage-file-go</span></span>](https://github.com/Azure/azure-storage-file-go) |
| <span data-ttu-id="24be1-116">Centrum událostí</span><span class="sxs-lookup"><span data-stu-id="24be1-116">Event Hub</span></span> | [<span data-ttu-id="24be1-117">github.com/Azure/azure-event-hubs-go</span><span class="sxs-lookup"><span data-stu-id="24be1-117">github.com/Azure/azure-event-hubs-go</span></span>](https://github.com/Azure/azure-event-hubs-go) |
| <span data-ttu-id="24be1-118">Application Insights</span><span class="sxs-lookup"><span data-stu-id="24be1-118">Application Insights</span></span> | [<span data-ttu-id="24be1-119">github.com/Microsoft/ApplicationInsights-go</span><span class="sxs-lookup"><span data-stu-id="24be1-119">github.com/Microsoft/ApplicationInsights-go</span></span>](https://github.com/Microsoft/ApplicationInsights-go) |

## <a name="vendor-the-azure-sdk-for-go"></a><span data-ttu-id="24be1-120">Vendorizace sady Azure SDK for Go</span><span class="sxs-lookup"><span data-stu-id="24be1-120">Vendor the Azure SDK for Go</span></span>

<span data-ttu-id="24be1-121">K vendorizaci sady Azure SDK for Go je možné použít [dep](https://github.com/golang/dep).</span><span class="sxs-lookup"><span data-stu-id="24be1-121">The Azure SDK for Go may be vendored through [dep](https://github.com/golang/dep).</span></span> <span data-ttu-id="24be1-122">Z důvodů stability se doporučuje vendoring.</span><span class="sxs-lookup"><span data-stu-id="24be1-122">For stability reasons, vendoring is recommended.</span></span> <span data-ttu-id="24be1-123">Pokud chcete použít podporu `dep`, přidejte `github.com/Azure/azure-sdk-for-go` do části `[[constraint]]` v `Gopkg.toml`.</span><span class="sxs-lookup"><span data-stu-id="24be1-123">In order to use `dep` support, add `github.com/Azure/azure-sdk-for-go` to a `[[constraint]]` section of your `Gopkg.toml`.</span></span> <span data-ttu-id="24be1-124">Pokud například chcete vendorizovat verzi `14.0.0`, přidejte následující položku:</span><span class="sxs-lookup"><span data-stu-id="24be1-124">For example, to vendor on version `14.0.0`, add the following entry:</span></span>

```
[[constraint]]
name = "github.com/Azure/azure-sdk-for-go"
version = "14.0.0"
```

## <a name="include-the-azure-sdk-for-go-in-your-project"></a><span data-ttu-id="24be1-125">Zahrnutí sady Azure SDK for Go do vašeho projektu</span><span class="sxs-lookup"><span data-stu-id="24be1-125">Include the Azure SDK for Go in your project</span></span>

<span data-ttu-id="24be1-126">Pokud chcete používat služby Azure z kódu Go, naimportujte všechny služby, s nimiž interagujete, a požadované moduly `autorest`.</span><span class="sxs-lookup"><span data-stu-id="24be1-126">To use Azure services from your Go code, import any services you interact with and the required `autorest` modules.</span></span>
<span data-ttu-id="24be1-127">Získáte úplný seznam dostupných modulů z GoDoc pro [dostupné služby](https://godoc.org/github.com/Azure/azure-sdk-for-go) a [balíčky AutoRest](https://godoc.org/github.com/Azure/go-autorest).</span><span class="sxs-lookup"><span data-stu-id="24be1-127">You get a complete list of the available modules from GoDoc for [available services](https://godoc.org/github.com/Azure/azure-sdk-for-go) and [AutoRest packages](https://godoc.org/github.com/Azure/go-autorest).</span></span> <span data-ttu-id="24be1-128">Nejběžnější balíčky, které potřebujete z `go-autorest`:</span><span class="sxs-lookup"><span data-stu-id="24be1-128">The most common packages you need from `go-autorest` are:</span></span>

| <span data-ttu-id="24be1-129">Balíček</span><span class="sxs-lookup"><span data-stu-id="24be1-129">Package</span></span> | <span data-ttu-id="24be1-130">Popis</span><span class="sxs-lookup"><span data-stu-id="24be1-130">Description</span></span> |
|---------|-------------|
| <span data-ttu-id="24be1-131">[github.com/Azure/go-autorest/autorest][autorest]</span><span class="sxs-lookup"><span data-stu-id="24be1-131">[github.com/Azure/go-autorest/autorest][autorest]</span></span> | <span data-ttu-id="24be1-132">Objekty pro zpracování ověřování klientů služby</span><span class="sxs-lookup"><span data-stu-id="24be1-132">Objects for handling service client authentication</span></span> |
| <span data-ttu-id="24be1-133">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span><span class="sxs-lookup"><span data-stu-id="24be1-133">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span></span> | <span data-ttu-id="24be1-134">Konstanty pro interakci se službami Azure</span><span class="sxs-lookup"><span data-stu-id="24be1-134">Constants for interactions with Azure services</span></span> |
| <span data-ttu-id="24be1-135">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span><span class="sxs-lookup"><span data-stu-id="24be1-135">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span></span> | <span data-ttu-id="24be1-136">Mechanismy ověřování pro přístup ke službám Azure</span><span class="sxs-lookup"><span data-stu-id="24be1-136">Authentication mechanisms for accessing Azure services</span></span> |
| <span data-ttu-id="24be1-137">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span><span class="sxs-lookup"><span data-stu-id="24be1-137">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span></span> | <span data-ttu-id="24be1-138">Pomocné rutiny potvrzení typu pro práci s datovými strukturami Azure SDK</span><span class="sxs-lookup"><span data-stu-id="24be1-138">Type assertion helpers for working with Azure SDK data structures</span></span> |

[autorest]: https://godoc.org/github.com/Azure/go-autorest/autorest
[autorest/azure]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure
[autorest/adal]: https://godoc.org/github.com/Azure/go-autorest/autorest/adal
[autorest/to]: https://godoc.org/github.com/Azure/go-autorest/autorest/to

<span data-ttu-id="24be1-139">Moduly pro služby Azure mají verze nezávislé na příslušných rozhraních API SDK.</span><span class="sxs-lookup"><span data-stu-id="24be1-139">Modules for Azure services are versioned independently from the SDK APIs for them.</span></span> <span data-ttu-id="24be1-140">Tyto verze jsou součástí cesty pro import modulu a jsou buď z _verze služby_, nebo z _profilu_.</span><span class="sxs-lookup"><span data-stu-id="24be1-140">These versions are part of the module import path, and are from either a _service version_ or a _profile_.</span></span> <span data-ttu-id="24be1-141">V současné době se doporučuje, abyste pro vývoj i vydání použili konkrétní verzi služby.</span><span class="sxs-lookup"><span data-stu-id="24be1-141">Currently, it is recommended that you use a specific service version for both development and release.</span></span> <span data-ttu-id="24be1-142">Služby jsou umístěné v modulu `services`.</span><span class="sxs-lookup"><span data-stu-id="24be1-142">Services are located under the `services` module.</span></span> <span data-ttu-id="24be1-143">Úplná cesta pro import je název služby, za kterým následuje verze ve formátu `YYYY-MM-DD` a potom znovu název služby.</span><span class="sxs-lookup"><span data-stu-id="24be1-143">The full path for the import is the name of the service, followed by the version in `YYYY-MM-DD` format, followed by the service name again.</span></span> <span data-ttu-id="24be1-144">Pokud například chcete zahrnout verzi `2017-03-30` služby Compute:</span><span class="sxs-lookup"><span data-stu-id="24be1-144">For example, to include the `2017-03-30` version of the Compute service:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/services/compute/mgmt/2017-03-30/compute"
```

<span data-ttu-id="24be1-145">V tuto chvíli se doporučuje použít nejnovější verzi služby, pokud nemáte postupovat jinak.</span><span class="sxs-lookup"><span data-stu-id="24be1-145">Right now it is recommended that you use the latest version of a service, unless you have a reason to do otherwise.</span></span>

<span data-ttu-id="24be1-146">Pokud potřebujete souhrnný snímek služeb, můžete také vybrat jednu verzi profilu.</span><span class="sxs-lookup"><span data-stu-id="24be1-146">If you need a collective snapshot of services, you can also select a single profile version.</span></span> <span data-ttu-id="24be1-147">Jediným uzamčeným profilem momentálně je verze `2017-03-09`, která nemusí mít nejnovější funkce služeb.</span><span class="sxs-lookup"><span data-stu-id="24be1-147">Right now, the only locked profile is version `2017-03-09`, which may not have the latest features of services.</span></span> <span data-ttu-id="24be1-148">Profily jsou umístěné v modulu `profiles` a jejich verze má formát `YYYY-MM-DD`.</span><span class="sxs-lookup"><span data-stu-id="24be1-148">Profiles are located under the `profiles` module, with their version in the `YYYY-MM-DD` format.</span></span> <span data-ttu-id="24be1-149">Služby jsou seskupené v příslušné verzi profilu.</span><span class="sxs-lookup"><span data-stu-id="24be1-149">Services are grouped under their profile version.</span></span> <span data-ttu-id="24be1-150">Pokud chcete například naimportovat modul správy prostředků Azure z profilu `2017-03-09`:</span><span class="sxs-lookup"><span data-stu-id="24be1-150">For example, to import the Azure Resources management module from the `2017-03-09` profile:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/resources/mgmt/resources"
```

> [!WARNING]
> <span data-ttu-id="24be1-151">K dispozici jsou také profily `preview` a `latest`.</span><span class="sxs-lookup"><span data-stu-id="24be1-151">There are also `preview` and `latest` profiles available.</span></span> <span data-ttu-id="24be1-152">Jejich použití se nedoporučuje.</span><span class="sxs-lookup"><span data-stu-id="24be1-152">Using them is not recommended.</span></span> <span data-ttu-id="24be1-153">Tyto profily představují klouzavé verze a chování služeb se může kdykoli změnit.</span><span class="sxs-lookup"><span data-stu-id="24be1-153">These profiles are rolling versions and service behavior may change at any time.</span></span>

## <a name="next-steps"></a><span data-ttu-id="24be1-154">Další kroky</span><span class="sxs-lookup"><span data-stu-id="24be1-154">Next steps</span></span>

<span data-ttu-id="24be1-155">Pokud chcete začít používat sadu Azure SDK for Go, vyzkoušejte si rychlý start.</span><span class="sxs-lookup"><span data-stu-id="24be1-155">To begin using the Azure SDK for Go, try out a quickstart.</span></span>

* [<span data-ttu-id="24be1-156">Nasazení virtuálního počítače ze šablony</span><span class="sxs-lookup"><span data-stu-id="24be1-156">Deploy a virtual machine from a template</span></span>](azure-sdk-go-qs-vm.md)
* [<span data-ttu-id="24be1-157">Přenos objektů do Azure Blob Storage s využitím sady Azure Blob SDK for Go</span><span class="sxs-lookup"><span data-stu-id="24be1-157">Transfer objects to Azure Blob Storage with the Azure Blob SDK for Go</span></span>](/azure/storage/blobs/storage-quickstart-blobs-go?toc=%2fgo%2fazure%2ftoc.json)
* [<span data-ttu-id="24be1-158">Připojení k Azure Database for PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="24be1-158">Connect to Azure Database for PostgreSQL</span></span>](/azure/postgresql/connect-go?toc=%2fgo%2fazure%2ftoc.json)

<span data-ttu-id="24be1-159">Pokud chcete okamžitě začít pracovat s jinými službami v sadě Go SDK, prohlédněte si dostupný ukázkový kód.</span><span class="sxs-lookup"><span data-stu-id="24be1-159">If you want to get started with other services in the Go SDK immediately, take a look at some of the available sample code.</span></span>

* [<span data-ttu-id="24be1-160">Ověřování pomocí služeb Azure</span><span class="sxs-lookup"><span data-stu-id="24be1-160">Authenticate with Azure services</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/iam)
* [<span data-ttu-id="24be1-161">Nasazení nových virtuálních počítačů s ověřováním SSH</span><span class="sxs-lookup"><span data-stu-id="24be1-161">Deploy new virtual machines with SSH authentication</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute)
* [<span data-ttu-id="24be1-162">Nasazení image kontejneru do služby Azure Container Instances</span><span class="sxs-lookup"><span data-stu-id="24be1-162">Deploy a container image to Azure Container Instances</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerinstance)
* [<span data-ttu-id="24be1-163">Vytvoření clusteru ve službě Azure Kubernetes</span><span class="sxs-lookup"><span data-stu-id="24be1-163">Create a cluster in Azure Kubernetes Service</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerservice)
* [<span data-ttu-id="24be1-164">Práce se službami Azure Storage</span><span class="sxs-lookup"><span data-stu-id="24be1-164">Work with Azure Storage services</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/storage)
* [<span data-ttu-id="24be1-165">Všechny ukázky pro Azure SDK for Go</span><span class="sxs-lookup"><span data-stu-id="24be1-165">All samples for the Azure SDK for Go</span></span>](https://github.com/azure-samples/azure-sdk-for-go-samples)

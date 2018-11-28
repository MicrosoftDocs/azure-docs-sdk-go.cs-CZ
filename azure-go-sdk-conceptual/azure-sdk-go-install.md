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
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/27/2018
ms.locfileid: "52337139"
---
# <a name="install-the-azure-sdk-for-go"></a><span data-ttu-id="876fb-103">Instalace Azure SDK for Go</span><span class="sxs-lookup"><span data-stu-id="876fb-103">Install the Azure SDK for Go</span></span>

<span data-ttu-id="876fb-104">Vítá vás Azure SDK for Go!</span><span class="sxs-lookup"><span data-stu-id="876fb-104">Welcome to the Azure SDK for Go!</span></span> <span data-ttu-id="876fb-105">Sada SDK umožňuje spravovat služby Azure a pracovat s nimi z aplikací Go.</span><span class="sxs-lookup"><span data-stu-id="876fb-105">The SDK allows you to manage and interact with Azure services from your Go applications.</span></span>

## <a name="get-the-azure-sdk-for-go"></a><span data-ttu-id="876fb-106">Získání sady Azure SDK for Go</span><span class="sxs-lookup"><span data-stu-id="876fb-106">Get the Azure SDK for Go</span></span>

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

<span data-ttu-id="876fb-107">Některé služby Azure mají vlastní sady Go SDK a nejsou zahrnuté v základním balíčku Azure SDK for Go.</span><span class="sxs-lookup"><span data-stu-id="876fb-107">Some Azure services have their own Go SDK and aren't included in the core Azure SDK for Go package.</span></span> <span data-ttu-id="876fb-108">Následující tabulka uvádí služby s vlastními sadami SDK a názvy jejich balíčků.</span><span class="sxs-lookup"><span data-stu-id="876fb-108">The following table lists the services with their own SDKs and their package names.</span></span> <span data-ttu-id="876fb-109">Všechny tyto balíčky se považují za verze Preview.</span><span class="sxs-lookup"><span data-stu-id="876fb-109">These packages are all considered to be in preview.</span></span>

| <span data-ttu-id="876fb-110">Služba</span><span class="sxs-lookup"><span data-stu-id="876fb-110">Service</span></span> | <span data-ttu-id="876fb-111">Balíček</span><span class="sxs-lookup"><span data-stu-id="876fb-111">Package</span></span> |
|---------|---------|
| <span data-ttu-id="876fb-112">Blob Storage</span><span class="sxs-lookup"><span data-stu-id="876fb-112">Blob Storage</span></span> | [<span data-ttu-id="876fb-113">github.com/Azure/azure-storage-blob-go</span><span class="sxs-lookup"><span data-stu-id="876fb-113">github.com/Azure/azure-storage-blob-go</span></span>](https://github.com/Azure/azure-storage-blob-go) |
| <span data-ttu-id="876fb-114">File Storage</span><span class="sxs-lookup"><span data-stu-id="876fb-114">File Storage</span></span> | [<span data-ttu-id="876fb-115">github.com/Azure/azure-storage-file-go</span><span class="sxs-lookup"><span data-stu-id="876fb-115">github.com/Azure/azure-storage-file-go</span></span>](https://github.com/Azure/azure-storage-file-go) |
| <span data-ttu-id="876fb-116">Fronta úložiště</span><span class="sxs-lookup"><span data-stu-id="876fb-116">Storage Queue</span></span> | [<span data-ttu-id="876fb-117">github.com/Azure/azure-storage-queue-go</span><span class="sxs-lookup"><span data-stu-id="876fb-117">github.com/Azure/azure-storage-queue-go</span></span>](https://github.com/Azure/azure-storage-queue-go) |
| <span data-ttu-id="876fb-118">Centrum událostí</span><span class="sxs-lookup"><span data-stu-id="876fb-118">Event Hub</span></span> | [<span data-ttu-id="876fb-119">github.com/Azure/azure-event-hubs-go</span><span class="sxs-lookup"><span data-stu-id="876fb-119">github.com/Azure/azure-event-hubs-go</span></span>](https://github.com/Azure/azure-event-hubs-go) |
| <span data-ttu-id="876fb-120">Service Bus</span><span class="sxs-lookup"><span data-stu-id="876fb-120">Service Bus</span></span> | [<span data-ttu-id="876fb-121">github.com/Azure/azure-service-bus-go</span><span class="sxs-lookup"><span data-stu-id="876fb-121">github.com/Azure/azure-service-bus-go</span></span>](https://github.com/Azure/azure-service-bus-go) |
| <span data-ttu-id="876fb-122">Application Insights</span><span class="sxs-lookup"><span data-stu-id="876fb-122">Application Insights</span></span> | [<span data-ttu-id="876fb-123">github.com/Microsoft/ApplicationInsights-go</span><span class="sxs-lookup"><span data-stu-id="876fb-123">github.com/Microsoft/ApplicationInsights-go</span></span>](https://github.com/Microsoft/ApplicationInsights-go) |

## <a name="vendor-the-azure-sdk-for-go"></a><span data-ttu-id="876fb-124">Vendorizace sady Azure SDK for Go</span><span class="sxs-lookup"><span data-stu-id="876fb-124">Vendor the Azure SDK for Go</span></span>

<span data-ttu-id="876fb-125">K vendorizaci sady Azure SDK for Go je možné použít [dep](https://github.com/golang/dep).</span><span class="sxs-lookup"><span data-stu-id="876fb-125">The Azure SDK for Go may be vendored through [dep](https://github.com/golang/dep).</span></span> <span data-ttu-id="876fb-126">Z důvodů stability se doporučuje vendoring.</span><span class="sxs-lookup"><span data-stu-id="876fb-126">For stability reasons, vendoring is recommended.</span></span> <span data-ttu-id="876fb-127">Pokud chcete `dep` použít ve vašem vlastním projektu, přidejte `github.com/Azure/azure-sdk-for-go` do oddílu `[[constraint]]` ve vašem souboru `Gopkg.toml`.</span><span class="sxs-lookup"><span data-stu-id="876fb-127">To use `dep` in your own project, add `github.com/Azure/azure-sdk-for-go` to a `[[constraint]]` section of your `Gopkg.toml`.</span></span> <span data-ttu-id="876fb-128">Pokud například chcete vendorizovat verzi `14.0.0`, přidejte následující položku:</span><span class="sxs-lookup"><span data-stu-id="876fb-128">For example, to vendor on version `14.0.0`, add the following entry:</span></span>

```toml
[[constraint]]
name = "github.com/Azure/azure-sdk-for-go"
version = "14.0.0"
```

## <a name="include-the-azure-sdk-for-go-in-your-project"></a><span data-ttu-id="876fb-129">Zahrnutí sady Azure SDK for Go do vašeho projektu</span><span class="sxs-lookup"><span data-stu-id="876fb-129">Include the Azure SDK for Go in your project</span></span>

<span data-ttu-id="876fb-130">Pokud chcete používat služby Azure z kódu Go, naimportujte všechny služby, s nimiž interagujete, a požadované moduly `autorest`.</span><span class="sxs-lookup"><span data-stu-id="876fb-130">To use Azure services from your Go code, import any services you interact with and the required `autorest` modules.</span></span>
<span data-ttu-id="876fb-131">Získáte úplný seznam dostupných modulů z GoDoc pro [dostupné služby](https://godoc.org/github.com/Azure/azure-sdk-for-go) a [balíčky AutoRest](https://godoc.org/github.com/Azure/go-autorest).</span><span class="sxs-lookup"><span data-stu-id="876fb-131">You get a complete list of the available modules from GoDoc for [available services](https://godoc.org/github.com/Azure/azure-sdk-for-go) and [AutoRest packages](https://godoc.org/github.com/Azure/go-autorest).</span></span> <span data-ttu-id="876fb-132">Nejběžnější balíčky, které potřebujete z `go-autorest`:</span><span class="sxs-lookup"><span data-stu-id="876fb-132">The most common packages you need from `go-autorest` are:</span></span>

| <span data-ttu-id="876fb-133">Balíček</span><span class="sxs-lookup"><span data-stu-id="876fb-133">Package</span></span> | <span data-ttu-id="876fb-134">Popis</span><span class="sxs-lookup"><span data-stu-id="876fb-134">Description</span></span> |
|---------|-------------|
| <span data-ttu-id="876fb-135">[github.com/Azure/go-autorest/autorest][autorest]</span><span class="sxs-lookup"><span data-stu-id="876fb-135">[github.com/Azure/go-autorest/autorest][autorest]</span></span> | <span data-ttu-id="876fb-136">Objekty pro zpracování ověřování klientů služby</span><span class="sxs-lookup"><span data-stu-id="876fb-136">Objects for handling service client authentication</span></span> |
| <span data-ttu-id="876fb-137">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span><span class="sxs-lookup"><span data-stu-id="876fb-137">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span></span> | <span data-ttu-id="876fb-138">Konstanty pro interakci se službami Azure</span><span class="sxs-lookup"><span data-stu-id="876fb-138">Constants for interactions with Azure services</span></span> |
| <span data-ttu-id="876fb-139">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span><span class="sxs-lookup"><span data-stu-id="876fb-139">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span></span> | <span data-ttu-id="876fb-140">Mechanismy ověřování pro přístup ke službám Azure</span><span class="sxs-lookup"><span data-stu-id="876fb-140">Authentication mechanisms for accessing Azure services</span></span> |
| <span data-ttu-id="876fb-141">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span><span class="sxs-lookup"><span data-stu-id="876fb-141">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span></span> | <span data-ttu-id="876fb-142">Pomocné rutiny potvrzení typu pro práci s datovými strukturami Azure SDK</span><span class="sxs-lookup"><span data-stu-id="876fb-142">Type assertion helpers for working with Azure SDK data structures</span></span> |

[autorest]: https://godoc.org/github.com/Azure/go-autorest/autorest
[autorest/azure]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure
[autorest/adal]: https://godoc.org/github.com/Azure/go-autorest/autorest/adal
[autorest/to]: https://godoc.org/github.com/Azure/go-autorest/autorest/to

<span data-ttu-id="876fb-143">Balíčky Go a služby Azure mají verze nezávislé na sobě.</span><span class="sxs-lookup"><span data-stu-id="876fb-143">Go packages and Azure services are versioned independently.</span></span> <span data-ttu-id="876fb-144">Verze služeb jsou součástí cesty pro import modulu (pod modulem `services`).</span><span class="sxs-lookup"><span data-stu-id="876fb-144">The service versions are part of the module import path, underneath the `services` module.</span></span> <span data-ttu-id="876fb-145">Úplnou cestu pro modul tvoří název příslušné služby, za kterým následuje verze ve formátu `YYYY-MM-DD` a potom znovu název služby.</span><span class="sxs-lookup"><span data-stu-id="876fb-145">The full path for the module is the name of the service, followed by the version in `YYYY-MM-DD` format, followed by the service name again.</span></span> <span data-ttu-id="876fb-146">Pokud například chcete importovat verzi `2017-03-30` služby Compute:</span><span class="sxs-lookup"><span data-stu-id="876fb-146">For example, to import the `2017-03-30` version of the Compute service:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/services/compute/mgmt/2017-03-30/compute"
```

<span data-ttu-id="876fb-147">Při zahájení vývoje doporučujeme používat nejnovější verzi služby a zachovat její konzistenci.</span><span class="sxs-lookup"><span data-stu-id="876fb-147">It's recommended that you use the latest version of a service when starting development and keep it consistent.</span></span>
<span data-ttu-id="876fb-148">Mezi jednotlivými verzemi může dojít ke změně požadavků na službu, které by mohly způsobit narušení vašeho kódu, a to i když během této doby nejsou žádné aktualizace sady Go SDK.</span><span class="sxs-lookup"><span data-stu-id="876fb-148">Service requirements may change between versions that could break your code, even if there are no Go SDK updates during that time.</span></span>

<span data-ttu-id="876fb-149">Pokud potřebujete souhrnný snímek služeb, můžete také vybrat jednu verzi profilu.</span><span class="sxs-lookup"><span data-stu-id="876fb-149">If you need a collective snapshot of services, you can also select a single profile version.</span></span> <span data-ttu-id="876fb-150">Jediným uzamčeným profilem momentálně je verze `2017-03-09`, která nemusí mít nejnovější funkce služeb.</span><span class="sxs-lookup"><span data-stu-id="876fb-150">Right now, the only locked profile is version `2017-03-09`, which may not have the latest features of services.</span></span> <span data-ttu-id="876fb-151">Profily jsou umístěné v modulu `profiles` a jejich verze má formát `YYYY-MM-DD`.</span><span class="sxs-lookup"><span data-stu-id="876fb-151">Profiles are located under the `profiles` module, with their version in the `YYYY-MM-DD` format.</span></span> <span data-ttu-id="876fb-152">Služby jsou seskupené v příslušné verzi profilu.</span><span class="sxs-lookup"><span data-stu-id="876fb-152">Services are grouped under their profile version.</span></span> <span data-ttu-id="876fb-153">Pokud chcete například naimportovat modul správy prostředků Azure z profilu `2017-03-09`:</span><span class="sxs-lookup"><span data-stu-id="876fb-153">For example, to import the Azure Resources management module from the `2017-03-09` profile:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/resources/mgmt/resources"
```

> [!WARNING]
> <span data-ttu-id="876fb-154">K dispozici jsou také profily `preview` a `latest`.</span><span class="sxs-lookup"><span data-stu-id="876fb-154">There are also `preview` and `latest` profiles available.</span></span> <span data-ttu-id="876fb-155">Jejich použití se nedoporučuje.</span><span class="sxs-lookup"><span data-stu-id="876fb-155">Using them is not recommended.</span></span> <span data-ttu-id="876fb-156">Tyto profily představují klouzavé verze a chování služeb se může kdykoli změnit.</span><span class="sxs-lookup"><span data-stu-id="876fb-156">These profiles are rolling versions and service behavior may change at any time.</span></span>

## <a name="next-steps"></a><span data-ttu-id="876fb-157">Další kroky</span><span class="sxs-lookup"><span data-stu-id="876fb-157">Next steps</span></span>

<span data-ttu-id="876fb-158">Pokud chcete začít používat sadu Azure SDK for Go, vyzkoušejte si rychlý start.</span><span class="sxs-lookup"><span data-stu-id="876fb-158">To begin using the Azure SDK for Go, try out a quickstart.</span></span>

* [<span data-ttu-id="876fb-159">Nasazení virtuálního počítače ze šablony</span><span class="sxs-lookup"><span data-stu-id="876fb-159">Deploy a virtual machine from a template</span></span>](azure-sdk-go-qs-vm.md)
* [<span data-ttu-id="876fb-160">Přenos objektů do Azure Blob Storage s využitím sady Azure Blob SDK for Go</span><span class="sxs-lookup"><span data-stu-id="876fb-160">Transfer objects to Azure Blob Storage with the Azure Blob SDK for Go</span></span>](/azure/storage/blobs/storage-quickstart-blobs-go?toc=%2fgo%2fazure%2ftoc.json)
* [<span data-ttu-id="876fb-161">Připojení k Azure Database for PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="876fb-161">Connect to Azure Database for PostgreSQL</span></span>](/azure/postgresql/connect-go?toc=%2fgo%2fazure%2ftoc.json)

<span data-ttu-id="876fb-162">Pokud chcete okamžitě začít pracovat s jinými službami v sadě Go SDK, prohlédněte si dostupný ukázkový kód.</span><span class="sxs-lookup"><span data-stu-id="876fb-162">If you want to get started with other services in the Go SDK immediately, take a look at some of the available sample code.</span></span>

* [<span data-ttu-id="876fb-163">Ověřování pomocí služeb Azure</span><span class="sxs-lookup"><span data-stu-id="876fb-163">Authenticate with Azure services</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/internal/iam)
* [<span data-ttu-id="876fb-164">Nasazení nových virtuálních počítačů s ověřováním SSH</span><span class="sxs-lookup"><span data-stu-id="876fb-164">Deploy new virtual machines with SSH authentication</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute)
* [<span data-ttu-id="876fb-165">Nasazení image kontejneru do služby Azure Container Instances</span><span class="sxs-lookup"><span data-stu-id="876fb-165">Deploy a container image to Azure Container Instances</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerinstance)
* [<span data-ttu-id="876fb-166">Vytvoření clusteru ve službě Azure Kubernetes</span><span class="sxs-lookup"><span data-stu-id="876fb-166">Create a cluster in Azure Kubernetes Service</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerservice)
* [<span data-ttu-id="876fb-167">Práce se službami Azure Storage</span><span class="sxs-lookup"><span data-stu-id="876fb-167">Work with Azure Storage services</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/storage)
* [<span data-ttu-id="876fb-168">Všechny ukázky pro Azure SDK for Go</span><span class="sxs-lookup"><span data-stu-id="876fb-168">All samples for the Azure SDK for Go</span></span>](https://github.com/azure-samples/azure-sdk-for-go-samples)

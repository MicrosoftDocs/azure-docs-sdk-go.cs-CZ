---
title: "Nástroje pro vývojáře v Go"
description: "Nástroje pro práci se sadou Azure SDK for Go a službami Azure"
keywords: azure, go, golang, azure, visual studio, visual studio code
author: sptramer
ms.author: sttramer
ms.date: 01/30/2018
ms.topic: article
ms.devlang: go
manager: routlaw
ms.openlocfilehash: 4753775e608b39c6da43d64fd08c1532e03d5810
ms.sourcegitcommit: aaa8c37880332625f858a38f5918e6cf581bf48d
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/15/2018
---
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a><span data-ttu-id="f0699-104">Nástroje pro vývojáře, kteří používají Azure SDK for Go</span><span class="sxs-lookup"><span data-stu-id="f0699-104">Tools for developers using the Azure SDK for Go</span></span>

<span data-ttu-id="f0699-105">Pokud chcete zajistit efektivitu při psaní kódu Go a zajistit jeho bezproblémovou spolupráci se službami Azure, můžete využít některé z doporučených nástrojů.</span><span class="sxs-lookup"><span data-stu-id="f0699-105">To be effective at writing Go code and have it work seamlessly with Azure services, here are some recommended tools.</span></span>

## <a name="azure-cli-20"></a><span data-ttu-id="f0699-106">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="f0699-106">Azure CLI 2.0</span></span>

<span data-ttu-id="f0699-107">Azure CLI 2.0 poskytuje rozhraní příkazového řádku pro vytváření a konfiguraci prostředků Azure v rámci vašich předplatných.</span><span class="sxs-lookup"><span data-stu-id="f0699-107">The Azure 2.0 CLI provides a command-line interface to create and configure Azure resources in your subscriptions.</span></span> <span data-ttu-id="f0699-108">Toto rozhraní příkazového řádku vám umožní rychle začít s vytvářením běžných a sdílených prostředků Azure, abyste se mohli soustředit na složitější využití služeb.</span><span class="sxs-lookup"><span data-stu-id="f0699-108">The CLI can help you get started building common and shared Azure resources quickly, so that you can focus on more complex usage of services.</span></span> <span data-ttu-id="f0699-109">CLI má funkce dotazování a filtrování, takže můžete výstupy přesměrovat přímo do vašich oblíbených nástrojů příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="f0699-109">The CLI has query and filtering features so you can pipe output directly to your favorite command-line tools.</span></span> <span data-ttu-id="f0699-110">Rozhraní příkazového řádku je dostupné k instalaci v místním systému, jako image Dockeru nebo prostřednictvím [Azure Cloud Shellu](https://docs.microsoft.com/en-us/azure/cloud-shell/overview).</span><span class="sxs-lookup"><span data-stu-id="f0699-110">The CLI is available for installation on your local system, as a Docker image, or through [Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview).</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="f0699-111">Instalovat Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="f0699-111">Install the Azure CLI 2.0</span></span>](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a><span data-ttu-id="f0699-112">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f0699-112">Visual Studio Code</span></span>

<span data-ttu-id="f0699-113">Visual Studio Code je jednoduchý editor, který má prostřednictvím rozšíření komplexní podporu pro jazyk Go.</span><span class="sxs-lookup"><span data-stu-id="f0699-113">Visual Studio Code is a lightweight editor that has comprehensive support for the Go language through extensions.</span></span> <span data-ttu-id="f0699-114">Tato rozšíření zahrnují podporu pro funkce, jako je automatické dokončování, šablony `impl`, refaktoring a ladění.</span><span class="sxs-lookup"><span data-stu-id="f0699-114">These extensions include support for features like autocomplete, `impl` templates, refactoring, and debugging.</span></span> <span data-ttu-id="f0699-115">Visual Studio Code také nabízí řadu rozšíření pro běžné vývojářské nástroje, jako je třeba správa zdrojového kódu, a nabízí i rozšíření pro přímé interakce se službami Azure.</span><span class="sxs-lookup"><span data-stu-id="f0699-115">Visual Studio Code also offers many extensions for common developer tools such as source control, and even offers extensions for direct interactions with Azure services.</span></span> <span data-ttu-id="f0699-116">Společnost Microsoft udržuje oficiální metarozšíření, které zahrnuje tato rozšíření Azure a interaktivní rozhraní pro Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="f0699-116">Microsoft maintains an official meta-extension including these Azure extensions, including an interactive interface for the Azure CLI.</span></span>

* [<span data-ttu-id="f0699-117">Nainstalovat editor Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f0699-117">Install Visual Studio Code</span></span>](https://code.visualstudio.com/Download)
* [<span data-ttu-id="f0699-118">Získat rozšíření Visual Studio Code Go</span><span class="sxs-lookup"><span data-stu-id="f0699-118">Get the Visual Studio Code Go extension</span></span>](https://code.visualstudio.com/docs/languages/go)
* [<span data-ttu-id="f0699-119">Získat rozšíření Azure Tools</span><span class="sxs-lookup"><span data-stu-id="f0699-119">Get the Azure Tools extension</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="dependency-management-with-dep"></a><span data-ttu-id="f0699-120">Správa závislostí s využitím dep</span><span class="sxs-lookup"><span data-stu-id="f0699-120">Dependency management with dep</span></span>

<span data-ttu-id="f0699-121">Existuje řada způsobů, jak spravovat závislosti balíčků a provádět vendoring s využitím Go, protože ještě neexistuje žádné oficiální řešení.</span><span class="sxs-lookup"><span data-stu-id="f0699-121">There are many ways to manage your package dependencies and do vendoring with Go, since there is no official solution yet.</span></span> <span data-ttu-id="f0699-122">Doporučeným způsobem správy je využití správce závislostí `dep`.</span><span class="sxs-lookup"><span data-stu-id="f0699-122">The recommended way to perform this management is with the `dep` dependency manager.</span></span> <span data-ttu-id="f0699-123">Azure SDK for Go využívá dep pro svůj vendoring a je garancí správného získání závislostí pro libovolný jiný projekt s využitím správce dep.</span><span class="sxs-lookup"><span data-stu-id="f0699-123">The Azure SDK for Go uses dep for its vendoring, and is guaranteed to correctly get dependencies for any other project using dep.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="f0699-124">Získat správce závislostí dep</span><span class="sxs-lookup"><span data-stu-id="f0699-124">Get the dep dependency manager</span></span>](https://github.com/tools/godep)

## <a name="telemetry-with-application-insights"></a><span data-ttu-id="f0699-125">Telemetrie s Application Insights</span><span class="sxs-lookup"><span data-stu-id="f0699-125">Telemetry with Application Insights</span></span>

<span data-ttu-id="f0699-126">[Application Insights](https://azure.microsoft.com/en-us/services/application-insights/) je analytický produkt, který umožňuje snadno shromažďovat telemetrické informace z vašich aplikací a integruje se s ekosystémem Azure, řešením Visual Studio Team Services a GitHubem.</span><span class="sxs-lookup"><span data-stu-id="f0699-126">[Application Insights](https://azure.microsoft.com/en-us/services/application-insights/) is an analytics product that allows you to easily collect telemetry information from your applications and integrates with the Azure ecosystem, Visual Studio Team Services, and GitHub.</span></span> <span data-ttu-id="f0699-127">Je možné ho využít v řadě aplikací a Microsoft nabízí Go SDK pro práci s Application Insights.</span><span class="sxs-lookup"><span data-stu-id="f0699-127">It can be used in many applications, and Microsoft offers a Go SDK for working with Application Insights.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="f0699-128">Získat sadu Application Insights for Go SDK</span><span class="sxs-lookup"><span data-stu-id="f0699-128">Get the Application Insights for Go SDK</span></span>](https://github.com/Microsoft/ApplicationInsights-Go) 

---
title: Nástroje pro vývojáře, kteří používají Azure SDK for Go
description: Nástroje pro práci se sadou Azure SDK for Go a službami Azure
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 09/05/2018
ms.topic: conceptual
ms.prod: azure
ms.technology: azure-sdk-go
ms.devlang: go
ms.openlocfilehash: 70cf7d645f47df29e8e42599a0acd75858144783
ms.sourcegitcommit: 8b9e10b960150dc08f046ab840d6a5627410db29
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/07/2018
ms.locfileid: "44059199"
---
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a><span data-ttu-id="5d9c6-103">Nástroje pro vývojáře, kteří používají Azure SDK for Go</span><span class="sxs-lookup"><span data-stu-id="5d9c6-103">Tools for developers using the Azure SDK for Go</span></span>

<span data-ttu-id="5d9c6-104">Pokud chcete zajistit efektivitu při psaní kódu Go a zajistit jeho bezproblémovou spolupráci se službami Azure, můžete využít některé z doporučených nástrojů.</span><span class="sxs-lookup"><span data-stu-id="5d9c6-104">To be effective at writing Go code and have it work seamlessly with Azure services, here are some recommended tools.</span></span>

## <a name="azure-cli"></a><span data-ttu-id="5d9c6-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="5d9c6-105">Azure CLI</span></span>

<span data-ttu-id="5d9c6-106">Azure CLI poskytuje rozhraní příkazového řádku pro vytváření a konfiguraci prostředků Azure v rámci vašich předplatných.</span><span class="sxs-lookup"><span data-stu-id="5d9c6-106">The Azure CLI provides a command-line interface to create and configure Azure resources in your subscriptions.</span></span> <span data-ttu-id="5d9c6-107">Toto rozhraní příkazového řádku vám umožní rychle začít s vytvářením běžných a sdílených prostředků Azure, abyste se mohli soustředit na složitější využití služeb.</span><span class="sxs-lookup"><span data-stu-id="5d9c6-107">The CLI can help you get started building common and shared Azure resources quickly, so that you can focus on more complex usage of services.</span></span> <span data-ttu-id="5d9c6-108">CLI má funkce dotazování a filtrování, takže můžete výstupy přesměrovat přímo do vašich oblíbených nástrojů příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="5d9c6-108">The CLI has query and filtering features so you can pipe output directly to your favorite command-line tools.</span></span> <span data-ttu-id="5d9c6-109">Rozhraní příkazového řádku je dostupné k instalaci v místním systému, jako image Dockeru nebo prostřednictvím [Azure Cloud Shellu](https://docs.microsoft.com/azure/cloud-shell/overview).</span><span class="sxs-lookup"><span data-stu-id="5d9c6-109">The CLI is available for installation on your local system, as a Docker image, or through [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="5d9c6-110">Instalace rozhraní příkazového řádku Azure CLI</span><span class="sxs-lookup"><span data-stu-id="5d9c6-110">Install the Azure CLI</span></span>](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a><span data-ttu-id="5d9c6-111">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5d9c6-111">Visual Studio Code</span></span>

<span data-ttu-id="5d9c6-112">Visual Studio Code je jednoduchý editor, který nabízí podporu Go.</span><span class="sxs-lookup"><span data-stu-id="5d9c6-112">Visual Studio Code is a lightweight editor that offers Go support.</span></span> <span data-ttu-id="5d9c6-113">Toto rozšíření nabízí podporu pro funkce, jako je automatické dokončování, šablony `impl`, refaktoring a ladění.</span><span class="sxs-lookup"><span data-stu-id="5d9c6-113">This extension offers features like autocomplete, `impl` templates, refactoring, and debugging.</span></span> <span data-ttu-id="5d9c6-114">Visual Studio Code také nabízí podporu pro přístup ke správě zdrojového kódu přímo v editoru a rozšíření pro práci se službami Azure.</span><span class="sxs-lookup"><span data-stu-id="5d9c6-114">Visual Studio Code also offers support for in-editor access to source control, and extensions for working with Azure services.</span></span>

* [<span data-ttu-id="5d9c6-115">Nainstalovat editor Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5d9c6-115">Install Visual Studio Code</span></span>](https://code.visualstudio.com/Download)
* [<span data-ttu-id="5d9c6-116">Získat rozšíření Visual Studio Code Go</span><span class="sxs-lookup"><span data-stu-id="5d9c6-116">Get the Visual Studio Code Go extension</span></span>](https://code.visualstudio.com/docs/languages/go)
* [<span data-ttu-id="5d9c6-117">Získat rozšíření Visual Studio Code Azure Tools</span><span class="sxs-lookup"><span data-stu-id="5d9c6-117">Get the Visual Studio Code Azure Tools extension</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="cicd-with-azure-devops-project"></a><span data-ttu-id="5d9c6-118">CI/CD s využitím služby Azure DevOps Project</span><span class="sxs-lookup"><span data-stu-id="5d9c6-118">CI/CD with Azure DevOps Project</span></span>

<span data-ttu-id="5d9c6-119">Kanály Azure DevOps Project umožňují vytvořit systém kontinuální integrace pro vaše aplikace Go.</span><span class="sxs-lookup"><span data-stu-id="5d9c6-119">Azure DevOps Project pipelines allow you to set up a continuous integration system for your Go applications.</span></span> <span data-ttu-id="5d9c6-120">Stačí mít úložiště Git a můžete nasazovat a testovat přímo v Azure.</span><span class="sxs-lookup"><span data-stu-id="5d9c6-120">All it takes is a git repo, and you can deploy and test directly on Azure.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="5d9c6-121">Další informace o vytváření kanálů CI/CD s využitím služby Azure DevOps Project</span><span class="sxs-lookup"><span data-stu-id="5d9c6-121">Learn how to create a CI/CD pipeline with Azure DevOps Project</span></span>](/azure/devops-project/azure-devops-project-go)

## <a name="dependency-management-with-dep"></a><span data-ttu-id="5d9c6-122">Správa závislostí s využitím dep</span><span class="sxs-lookup"><span data-stu-id="5d9c6-122">Dependency management with dep</span></span>

<span data-ttu-id="5d9c6-123">Azure SDK pro Go pro správu závislostí využívá dep.</span><span class="sxs-lookup"><span data-stu-id="5d9c6-123">The Azure SDK for Go uses dep for dependency management.</span></span> <span data-ttu-id="5d9c6-124">Příkaz dep umožňuje přijmout a vendorizovat požadavky pro vaši aplikaci Go, vyhnout se konfliktům verzí a zajistit, že projekt pracuje správně.</span><span class="sxs-lookup"><span data-stu-id="5d9c6-124">The dep command allows you to pull and vendor requirements for your Go application, avoiding version conflicts and ensuring that your project works correctly.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="5d9c6-125">Získat správce závislostí dep</span><span class="sxs-lookup"><span data-stu-id="5d9c6-125">Get the dep dependency manager</span></span>](https://github.com/golang/dep)

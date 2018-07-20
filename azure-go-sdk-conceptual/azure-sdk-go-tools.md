---
title: Nástroje pro vývojáře v Go
description: Nástroje pro práci se sadou Azure SDK for Go a službami Azure
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 07/13/2018
ms.topic: conceptual
ms.prod: azure
ms.technology: azure-sdk-go
ms.devlang: go
ms.openlocfilehash: dfa3912ac13e6f6d52d607f9dcc150f3a5b57602
ms.sourcegitcommit: d1790b317a8fcb4d672c654dac2a925a976589d4
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/14/2018
ms.locfileid: "39039501"
---
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a><span data-ttu-id="78b57-103">Nástroje pro vývojáře, kteří používají Azure SDK for Go</span><span class="sxs-lookup"><span data-stu-id="78b57-103">Tools for developers using the Azure SDK for Go</span></span>

<span data-ttu-id="78b57-104">Pokud chcete zajistit efektivitu při psaní kódu Go a zajistit jeho bezproblémovou spolupráci se službami Azure, můžete využít některé z doporučených nástrojů.</span><span class="sxs-lookup"><span data-stu-id="78b57-104">To be effective at writing Go code and have it work seamlessly with Azure services, here are some recommended tools.</span></span>

## <a name="azure-cli"></a><span data-ttu-id="78b57-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="78b57-105">Azure CLI</span></span>

<span data-ttu-id="78b57-106">Azure CLI poskytuje rozhraní příkazového řádku pro vytváření a konfiguraci prostředků Azure v rámci vašich předplatných.</span><span class="sxs-lookup"><span data-stu-id="78b57-106">The Azure CLI provides a command-line interface to create and configure Azure resources in your subscriptions.</span></span> <span data-ttu-id="78b57-107">Toto rozhraní příkazového řádku vám umožní rychle začít s vytvářením běžných a sdílených prostředků Azure, abyste se mohli soustředit na složitější využití služeb.</span><span class="sxs-lookup"><span data-stu-id="78b57-107">The CLI can help you get started building common and shared Azure resources quickly, so that you can focus on more complex usage of services.</span></span> <span data-ttu-id="78b57-108">CLI má funkce dotazování a filtrování, takže můžete výstupy přesměrovat přímo do vašich oblíbených nástrojů příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="78b57-108">The CLI has query and filtering features so you can pipe output directly to your favorite command-line tools.</span></span> <span data-ttu-id="78b57-109">Rozhraní příkazového řádku je dostupné k instalaci v místním systému, jako image Dockeru nebo prostřednictvím [Azure Cloud Shellu](https://docs.microsoft.com/azure/cloud-shell/overview).</span><span class="sxs-lookup"><span data-stu-id="78b57-109">The CLI is available for installation on your local system, as a Docker image, or through [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="78b57-110">Instalace rozhraní příkazového řádku Azure CLI</span><span class="sxs-lookup"><span data-stu-id="78b57-110">Install the Azure CLI</span></span>](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a><span data-ttu-id="78b57-111">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="78b57-111">Visual Studio Code</span></span>

<span data-ttu-id="78b57-112">Visual Studio Code je jednoduchý editor, který má prostřednictvím rozšíření komplexní podporu pro jazyk Go.</span><span class="sxs-lookup"><span data-stu-id="78b57-112">Visual Studio Code is a lightweight editor that has comprehensive support for the Go language through extensions.</span></span> <span data-ttu-id="78b57-113">Tato rozšíření zahrnují podporu pro funkce, jako je automatické dokončování, šablony `impl`, refaktoring a ladění.</span><span class="sxs-lookup"><span data-stu-id="78b57-113">These extensions include support for features like autocomplete, `impl` templates, refactoring, and debugging.</span></span> <span data-ttu-id="78b57-114">Visual Studio Code také nabízí řadu rozšíření pro běžné vývojářské nástroje, jako je třeba správa zdrojového kódu, a nabízí i rozšíření pro přímé interakce se službami Azure.</span><span class="sxs-lookup"><span data-stu-id="78b57-114">Visual Studio Code also offers many extensions for common developer tools such as source control, and even offers extensions for direct interactions with Azure services.</span></span> <span data-ttu-id="78b57-115">Společnost Microsoft udržuje oficiální metarozšíření, které zahrnuje tato rozšíření Azure a interaktivní rozhraní pro Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="78b57-115">Microsoft maintains an official meta-extension including these Azure extensions, including an interactive interface for the Azure CLI.</span></span>

* [<span data-ttu-id="78b57-116">Nainstalovat editor Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="78b57-116">Install Visual Studio Code</span></span>](https://code.visualstudio.com/Download)
* [<span data-ttu-id="78b57-117">Získat rozšíření Visual Studio Code Go</span><span class="sxs-lookup"><span data-stu-id="78b57-117">Get the Visual Studio Code Go extension</span></span>](https://code.visualstudio.com/docs/languages/go)
* [<span data-ttu-id="78b57-118">Získat rozšíření Azure Tools</span><span class="sxs-lookup"><span data-stu-id="78b57-118">Get the Azure Tools extension</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="cicd-with-azure-devops-project"></a><span data-ttu-id="78b57-119">CI/CD s využitím služby Azure DevOps Project</span><span class="sxs-lookup"><span data-stu-id="78b57-119">CI/CD with Azure DevOps Project</span></span>

<span data-ttu-id="78b57-120">Pomocí kanálu Azure DevOps Project můžete nastavit průběžné sestavování a nasazování pro vaše aplikace Go.</span><span class="sxs-lookup"><span data-stu-id="78b57-120">With the Azure DevOps Project pipeline, you can set up a continuous build and deploy for your Go applications.</span></span> <span data-ttu-id="78b57-121">Nepotřebujete nic jiného než dostupné úložiště Git a můžete nastavit nasazování i testování přímo ve vašich prostředcích Azure.</span><span class="sxs-lookup"><span data-stu-id="78b57-121">All you need is an available git repo, and you can set up to deploy and test directly on your Azure resources.</span></span> <span data-ttu-id="78b57-122">Vytvoření i správa konfiguračního kanálu jsou snadné a vzhledem k tomu, že ho zřizujete přímo v Azure, můžete ho řídit stejným způsobem jako ostatní prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="78b57-122">The configuration pipeline is easy to create and manage, and since it's provisioned directly on Azure, you can control it in the same way that you handle your other Azure resources.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="78b57-123">Další informace o vytváření kanálů CI/CD s využitím služby Azure DevOps Project</span><span class="sxs-lookup"><span data-stu-id="78b57-123">Learn how to create a CI/CD pipeline with Azure DevOps Project</span></span>](/devops-project/azure-devops-project-go)

## <a name="dependency-management-with-dep"></a><span data-ttu-id="78b57-124">Správa závislostí s využitím dep</span><span class="sxs-lookup"><span data-stu-id="78b57-124">Dependency management with dep</span></span>

<span data-ttu-id="78b57-125">Existuje řada způsobů, jak spravovat závislosti balíčků a provádět vendoring s využitím Go, protože ještě neexistuje žádné oficiální řešení.</span><span class="sxs-lookup"><span data-stu-id="78b57-125">There are many ways to manage your package dependencies and do vendoring with Go, since there is no official solution yet.</span></span> <span data-ttu-id="78b57-126">Doporučeným způsobem správy je využití správce závislostí `dep`.</span><span class="sxs-lookup"><span data-stu-id="78b57-126">The recommended way to perform this management is with the `dep` dependency manager.</span></span> <span data-ttu-id="78b57-127">Azure SDK for Go využívá dep pro svůj vendoring a je garancí správného získání závislostí pro libovolný jiný projekt s využitím správce dep.</span><span class="sxs-lookup"><span data-stu-id="78b57-127">The Azure SDK for Go uses dep for its vendoring, and is guaranteed to correctly get dependencies for any other project using dep.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="78b57-128">Získat správce závislostí dep</span><span class="sxs-lookup"><span data-stu-id="78b57-128">Get the dep dependency manager</span></span>](https://github.com/golang/dep)

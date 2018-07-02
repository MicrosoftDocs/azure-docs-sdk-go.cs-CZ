---
title: Nástroje pro vývojáře v Go
description: Nástroje pro práci se sadou Azure SDK for Go a službami Azure
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 01/30/2018
ms.topic: conceptual
ms.prod: azure
ms.technology: azure-sdk-go
ms.devlang: go
ms.openlocfilehash: 25b46e3a1636c39e261ba11c6f8939d8721cc693
ms.sourcegitcommit: 79d216c6b0442d0f3b3660ff2a34dc8b2049390c
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/29/2018
ms.locfileid: "37093154"
---
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a><span data-ttu-id="427a9-103">Nástroje pro vývojáře, kteří používají Azure SDK for Go</span><span class="sxs-lookup"><span data-stu-id="427a9-103">Tools for developers using the Azure SDK for Go</span></span>

<span data-ttu-id="427a9-104">Pokud chcete zajistit efektivitu při psaní kódu Go a zajistit jeho bezproblémovou spolupráci se službami Azure, můžete využít některé z doporučených nástrojů.</span><span class="sxs-lookup"><span data-stu-id="427a9-104">To be effective at writing Go code and have it work seamlessly with Azure services, here are some recommended tools.</span></span>

## <a name="azure-cli-20"></a><span data-ttu-id="427a9-105">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="427a9-105">Azure CLI 2.0</span></span>

<span data-ttu-id="427a9-106">Azure CLI 2.0 poskytuje rozhraní příkazového řádku pro vytváření a konfiguraci prostředků Azure v rámci vašich předplatných.</span><span class="sxs-lookup"><span data-stu-id="427a9-106">The Azure 2.0 CLI provides a command-line interface to create and configure Azure resources in your subscriptions.</span></span> <span data-ttu-id="427a9-107">Toto rozhraní příkazového řádku vám umožní rychle začít s vytvářením běžných a sdílených prostředků Azure, abyste se mohli soustředit na složitější využití služeb.</span><span class="sxs-lookup"><span data-stu-id="427a9-107">The CLI can help you get started building common and shared Azure resources quickly, so that you can focus on more complex usage of services.</span></span> <span data-ttu-id="427a9-108">CLI má funkce dotazování a filtrování, takže můžete výstupy přesměrovat přímo do vašich oblíbených nástrojů příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="427a9-108">The CLI has query and filtering features so you can pipe output directly to your favorite command-line tools.</span></span> <span data-ttu-id="427a9-109">Rozhraní příkazového řádku je dostupné k instalaci v místním systému, jako image Dockeru nebo prostřednictvím [Azure Cloud Shellu](https://docs.microsoft.com/en-us/azure/cloud-shell/overview).</span><span class="sxs-lookup"><span data-stu-id="427a9-109">The CLI is available for installation on your local system, as a Docker image, or through [Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview).</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="427a9-110">Instalovat Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="427a9-110">Install the Azure CLI 2.0</span></span>](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a><span data-ttu-id="427a9-111">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="427a9-111">Visual Studio Code</span></span>

<span data-ttu-id="427a9-112">Visual Studio Code je jednoduchý editor, který má prostřednictvím rozšíření komplexní podporu pro jazyk Go.</span><span class="sxs-lookup"><span data-stu-id="427a9-112">Visual Studio Code is a lightweight editor that has comprehensive support for the Go language through extensions.</span></span> <span data-ttu-id="427a9-113">Tato rozšíření zahrnují podporu pro funkce, jako je automatické dokončování, šablony `impl`, refaktoring a ladění.</span><span class="sxs-lookup"><span data-stu-id="427a9-113">These extensions include support for features like autocomplete, `impl` templates, refactoring, and debugging.</span></span> <span data-ttu-id="427a9-114">Visual Studio Code také nabízí řadu rozšíření pro běžné vývojářské nástroje, jako je třeba správa zdrojového kódu, a nabízí i rozšíření pro přímé interakce se službami Azure.</span><span class="sxs-lookup"><span data-stu-id="427a9-114">Visual Studio Code also offers many extensions for common developer tools such as source control, and even offers extensions for direct interactions with Azure services.</span></span> <span data-ttu-id="427a9-115">Společnost Microsoft udržuje oficiální metarozšíření, které zahrnuje tato rozšíření Azure a interaktivní rozhraní pro Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="427a9-115">Microsoft maintains an official meta-extension including these Azure extensions, including an interactive interface for the Azure CLI.</span></span>

* [<span data-ttu-id="427a9-116">Nainstalovat editor Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="427a9-116">Install Visual Studio Code</span></span>](https://code.visualstudio.com/Download)
* [<span data-ttu-id="427a9-117">Získat rozšíření Visual Studio Code Go</span><span class="sxs-lookup"><span data-stu-id="427a9-117">Get the Visual Studio Code Go extension</span></span>](https://code.visualstudio.com/docs/languages/go)
* [<span data-ttu-id="427a9-118">Získat rozšíření Azure Tools</span><span class="sxs-lookup"><span data-stu-id="427a9-118">Get the Azure Tools extension</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="dependency-management-with-dep"></a><span data-ttu-id="427a9-119">Správa závislostí s využitím dep</span><span class="sxs-lookup"><span data-stu-id="427a9-119">Dependency management with dep</span></span>

<span data-ttu-id="427a9-120">Existuje řada způsobů, jak spravovat závislosti balíčků a provádět vendoring s využitím Go, protože ještě neexistuje žádné oficiální řešení.</span><span class="sxs-lookup"><span data-stu-id="427a9-120">There are many ways to manage your package dependencies and do vendoring with Go, since there is no official solution yet.</span></span> <span data-ttu-id="427a9-121">Doporučeným způsobem správy je využití správce závislostí `dep`.</span><span class="sxs-lookup"><span data-stu-id="427a9-121">The recommended way to perform this management is with the `dep` dependency manager.</span></span> <span data-ttu-id="427a9-122">Azure SDK for Go využívá dep pro svůj vendoring a je garancí správného získání závislostí pro libovolný jiný projekt s využitím správce dep.</span><span class="sxs-lookup"><span data-stu-id="427a9-122">The Azure SDK for Go uses dep for its vendoring, and is guaranteed to correctly get dependencies for any other project using dep.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="427a9-123">Získat správce závislostí dep</span><span class="sxs-lookup"><span data-stu-id="427a9-123">Get the dep dependency manager</span></span>](https://github.com/golang/dep)

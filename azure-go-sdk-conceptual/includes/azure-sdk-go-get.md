---
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 09/05/2018
ms.topic: include
ms.prod: azure
ms.technology: azure-cli
ms.openlocfilehash: 5df14f939efdd0550b49261c88c8dc6518ada459
ms.sourcegitcommit: 8b9e10b960150dc08f046ab840d6a5627410db29
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/07/2018
ms.locfileid: "44059243"
---
<span data-ttu-id="7b8a1-101">Sada [Azure SDK pro Go](https://github.com/Azure/azure-sdk-for-go) je kompatibilní s Go verze 1.8 a novější.</span><span class="sxs-lookup"><span data-stu-id="7b8a1-101">The [Azure SDK for Go](https://github.com/Azure/azure-sdk-for-go) is compatible with Go versions 1.8 and higher.</span></span> <span data-ttu-id="7b8a1-102">Pro prostředí využívající [profily Azure Stack Profiles](/azure/azure-stack/user/azure-stack-version-profiles-go) se vyžaduje minimálně Go verze 1.9.</span><span class="sxs-lookup"><span data-stu-id="7b8a1-102">For environments using [Azure Stack Profiles](/azure/azure-stack/user/azure-stack-version-profiles-go), Go version 1.9 is the minimum requirement.</span></span>
<span data-ttu-id="7b8a1-103">Pokud potřebujete Go nainstalovat, postupujte podle [pokynů pro instalaci Go](https://golang.org/doc/install).</span><span class="sxs-lookup"><span data-stu-id="7b8a1-103">If you need to install Go, follow [the Go installation instructions](https://golang.org/doc/install).</span></span>

<span data-ttu-id="7b8a1-104">Sadu Azure SDK pro Go a její závislosti můžete stáhnout prostřednictvím `go get`.</span><span class="sxs-lookup"><span data-stu-id="7b8a1-104">You can download the Azure SDK for Go and its dependencies via `go get`.</span></span>

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> <span data-ttu-id="7b8a1-105">Nezapomeňte pro `Azure` v adrese URL použít velká písmena.</span><span class="sxs-lookup"><span data-stu-id="7b8a1-105">Make sure that you capitalize `Azure` in the URL.</span></span> <span data-ttu-id="7b8a1-106">Pokud to neuděláte, při práci s touto sadou SDK mohou nastat problémy při importu, které souvisejí s použitím velkých a malých písmen.</span><span class="sxs-lookup"><span data-stu-id="7b8a1-106">Doing otherwise can cause case-related import problems when working with the SDK.</span></span> <span data-ttu-id="7b8a1-107">Velká písmena musíte použít také pro `Azure` v příkazech pro import.</span><span class="sxs-lookup"><span data-stu-id="7b8a1-107">You also need to capitalize `Azure` in your import statements.</span></span>

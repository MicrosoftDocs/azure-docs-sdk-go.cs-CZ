---
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 02/14/2018
ms.topic: include
ms.prod: azure
ms.technology: azure-cli
ms.openlocfilehash: ddd58efdbc0c2d3ded068a9bebf2466db702566f
ms.sourcegitcommit: f08abf902b48f8173aa6e261084ff2cfc9043305
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/03/2018
---
<span data-ttu-id="2fb89-101">Sada [Azure SDK for Go](https://github.com/Azure/azure-sdk-for-go) je kompatibilní s Go verze 1.8 a novější.</span><span class="sxs-lookup"><span data-stu-id="2fb89-101">The [Azure SDK for Go](https://github.com/Azure/azure-sdk-for-go) is compatible with Go versions 1.8 and later.</span></span> <span data-ttu-id="2fb89-102">Pro prostředí využívající [profily Azure Stack Profiles](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-version-profiles) se vyžaduje minimálně Go verze 1.9.</span><span class="sxs-lookup"><span data-stu-id="2fb89-102">For environments using [Azure Stack Profiles](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-version-profiles), Go version 1.9 is the minimum requirement.</span></span>
<span data-ttu-id="2fb89-103">Pokud nemáte Go v systému k dispozici, postupujte podle [pokynů pro instalaci Go](https://golang.org/doc/install).</span><span class="sxs-lookup"><span data-stu-id="2fb89-103">If you do not have Go available on your system, follow [the Go installation instructions](https://golang.org/doc/install).</span></span>

<span data-ttu-id="2fb89-104">Sadu Azure SDK for Go a její závislosti můžete získat prostřednictvím `go get`.</span><span class="sxs-lookup"><span data-stu-id="2fb89-104">You can obtain the Azure SDK for Go and its dependencies via `go get`.</span></span>

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> <span data-ttu-id="2fb89-105">Nezapomeňte pro `Azure` v adrese URL použít velká písmena.</span><span class="sxs-lookup"><span data-stu-id="2fb89-105">Make sure that you capitalize `Azure` in the URL.</span></span> <span data-ttu-id="2fb89-106">Pokud to neuděláte, při práci s touto sadou SDK mohou nastat problémy při importu, které souvisejí s použitím velkých a malých písmen.</span><span class="sxs-lookup"><span data-stu-id="2fb89-106">Doing otherwise can cause case-related import problems when working with the SDK.</span></span> <span data-ttu-id="2fb89-107">Velká písmena musíte použít také pro `Azure` v příkazech pro import.</span><span class="sxs-lookup"><span data-stu-id="2fb89-107">You also need to capitalize `Azure` in your import statements.</span></span>


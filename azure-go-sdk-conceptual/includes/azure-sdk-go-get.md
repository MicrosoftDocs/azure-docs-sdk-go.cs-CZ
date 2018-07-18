---
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 02/14/2018
ms.topic: include
ms.prod: azure
ms.technology: azure-cli
ms.openlocfilehash: d021dd8ef4744b7c50b296b231bf63481f92411a
ms.sourcegitcommit: 2a3bd491e087a1d0e7d269bed896c029357d62a6
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38988064"
---
Sada [Azure SDK for Go](https://github.com/Azure/azure-sdk-for-go) je kompatibilní s Go verze 1.8 a novější. Pro prostředí využívající [profily Azure Stack Profiles](https://docs.microsoft.com/azure/azure-stack/azure-stack-version-profiles) se vyžaduje minimálně Go verze 1.9.
Pokud nemáte Go v systému k dispozici, postupujte podle [pokynů pro instalaci Go](https://golang.org/doc/install).

Sadu Azure SDK for Go a její závislosti můžete získat prostřednictvím `go get`.

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> Nezapomeňte pro `Azure` v adrese URL použít velká písmena. Pokud to neuděláte, při práci s touto sadou SDK mohou nastat problémy při importu, které souvisejí s použitím velkých a malých písmen. Velká písmena musíte použít také pro `Azure` v příkazech pro import.

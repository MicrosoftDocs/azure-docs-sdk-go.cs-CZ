---
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 09/05/2018
ms.topic: include
ms.prod: azure
ms.technology: azure-cli
ms.openlocfilehash: 5df14f939efdd0550b49261c88c8dc6518ada459
ms.sourcegitcommit: 887b15afcdeaf926a5f3d21b64e4045167fd062c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/23/2018
ms.locfileid: "52293482"
---
Sada [Azure SDK pro Go](https://github.com/Azure/azure-sdk-for-go) je kompatibilní s Go verze 1.8 a novější. Pro prostředí využívající [profily Azure Stack Profiles](/azure/azure-stack/user/azure-stack-version-profiles-go) se vyžaduje minimálně Go verze 1.9.
Pokud potřebujete Go nainstalovat, postupujte podle [pokynů pro instalaci Go](https://golang.org/doc/install).

Sadu Azure SDK pro Go a její závislosti můžete stáhnout prostřednictvím `go get`.

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> Nezapomeňte pro `Azure` v adrese URL použít velká písmena. Pokud to neuděláte, při práci s touto sadou SDK mohou nastat problémy při importu, které souvisejí s použitím velkých a malých písmen. Velká písmena musíte použít také pro `Azure` v příkazech pro import.

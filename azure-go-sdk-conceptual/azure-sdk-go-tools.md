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
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a>Nástroje pro vývojáře, kteří používají Azure SDK for Go

Pokud chcete zajistit efektivitu při psaní kódu Go a zajistit jeho bezproblémovou spolupráci se službami Azure, můžete využít některé z doporučených nástrojů.

## <a name="azure-cli"></a>Azure CLI

Azure CLI poskytuje rozhraní příkazového řádku pro vytváření a konfiguraci prostředků Azure v rámci vašich předplatných. Toto rozhraní příkazového řádku vám umožní rychle začít s vytvářením běžných a sdílených prostředků Azure, abyste se mohli soustředit na složitější využití služeb. CLI má funkce dotazování a filtrování, takže můžete výstupy přesměrovat přímo do vašich oblíbených nástrojů příkazového řádku. Rozhraní příkazového řádku je dostupné k instalaci v místním systému, jako image Dockeru nebo prostřednictvím [Azure Cloud Shellu](https://docs.microsoft.com/azure/cloud-shell/overview).

> [!div class="nextstepaction"]
> [Instalace rozhraní příkazového řádku Azure CLI](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a>Visual Studio Code

Visual Studio Code je jednoduchý editor, který nabízí podporu Go. Toto rozšíření nabízí podporu pro funkce, jako je automatické dokončování, šablony `impl`, refaktoring a ladění. Visual Studio Code také nabízí podporu pro přístup ke správě zdrojového kódu přímo v editoru a rozšíření pro práci se službami Azure.

* [Nainstalovat editor Visual Studio Code](https://code.visualstudio.com/Download)
* [Získat rozšíření Visual Studio Code Go](https://code.visualstudio.com/docs/languages/go)
* [Získat rozšíření Visual Studio Code Azure Tools](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="cicd-with-azure-devops-project"></a>CI/CD s využitím služby Azure DevOps Project

Kanály Azure DevOps Project umožňují vytvořit systém kontinuální integrace pro vaše aplikace Go. Stačí mít úložiště Git a můžete nasazovat a testovat přímo v Azure.

> [!div class="nextstepaction"]
> [Další informace o vytváření kanálů CI/CD s využitím služby Azure DevOps Project](/azure/devops-project/azure-devops-project-go)

## <a name="dependency-management-with-dep"></a>Správa závislostí s využitím dep

Azure SDK pro Go pro správu závislostí využívá dep. Příkaz dep umožňuje přijmout a vendorizovat požadavky pro vaši aplikaci Go, vyhnout se konfliktům verzí a zajistit, že projekt pracuje správně.

> [!div class="nextstepaction"]
> [Získat správce závislostí dep](https://github.com/golang/dep)

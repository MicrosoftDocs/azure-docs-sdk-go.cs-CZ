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
ms.openlocfilehash: 006d140bffb66fdd769a14511232d4ea5081811d
ms.sourcegitcommit: 181d4e0b164cf39b3feac346f559596bd19c94db
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2018
ms.locfileid: "38066978"
---
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a>Nástroje pro vývojáře, kteří používají Azure SDK for Go

Pokud chcete zajistit efektivitu při psaní kódu Go a zajistit jeho bezproblémovou spolupráci se službami Azure, můžete využít některé z doporučených nástrojů.

## <a name="azure-cli-20"></a>Azure CLI 2.0

Azure CLI 2.0 poskytuje rozhraní příkazového řádku pro vytváření a konfiguraci prostředků Azure v rámci vašich předplatných. Toto rozhraní příkazového řádku vám umožní rychle začít s vytvářením běžných a sdílených prostředků Azure, abyste se mohli soustředit na složitější využití služeb. CLI má funkce dotazování a filtrování, takže můžete výstupy přesměrovat přímo do vašich oblíbených nástrojů příkazového řádku. Rozhraní příkazového řádku je dostupné k instalaci v místním systému, jako image Dockeru nebo prostřednictvím [Azure Cloud Shellu](https://docs.microsoft.com/azure/cloud-shell/overview).

> [!div class="nextstepaction"]
> [Instalovat Azure CLI 2.0](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a>Visual Studio Code

Visual Studio Code je jednoduchý editor, který má prostřednictvím rozšíření komplexní podporu pro jazyk Go. Tato rozšíření zahrnují podporu pro funkce, jako je automatické dokončování, šablony `impl`, refaktoring a ladění. Visual Studio Code také nabízí řadu rozšíření pro běžné vývojářské nástroje, jako je třeba správa zdrojového kódu, a nabízí i rozšíření pro přímé interakce se službami Azure. Společnost Microsoft udržuje oficiální metarozšíření, které zahrnuje tato rozšíření Azure a interaktivní rozhraní pro Azure CLI.

* [Nainstalovat editor Visual Studio Code](https://code.visualstudio.com/Download)
* [Získat rozšíření Visual Studio Code Go](https://code.visualstudio.com/docs/languages/go)
* [Získat rozšíření Azure Tools](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="dependency-management-with-dep"></a>Správa závislostí s využitím dep

Existuje řada způsobů, jak spravovat závislosti balíčků a provádět vendoring s využitím Go, protože ještě neexistuje žádné oficiální řešení. Doporučeným způsobem správy je využití správce závislostí `dep`. Azure SDK for Go využívá dep pro svůj vendoring a je garancí správného získání závislostí pro libovolný jiný projekt s využitím správce dep.

> [!div class="nextstepaction"]
> [Získat správce závislostí dep](https://github.com/golang/dep)

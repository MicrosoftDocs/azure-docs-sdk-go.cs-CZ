---
title: Ukázky z Azure SDK for Go pro výpočetní funkce a sítě
description: Vybrané ukázky ze sady Azure SDK for Go pro práci s výpočetními funkcemi, jako jsou virtuální počítače a virtuální sítě
author: sptramer
ms.author: sttramer
ms.date: 03/21/2018
ms.topic: article
ms.devlang: go
manager: carmonm
ms.openlocfilehash: 1e6f9d848213f0f70ab59d5e0bcc5c52127e97c5
ms.sourcegitcommit: fcc1786d59d2e32c97a9a8e0748e06f564a961bd
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/23/2018
---
# <a name="azure-sdk-for-go-samples-for-compute-and-networking"></a><span data-ttu-id="170bc-103">Ukázky z Azure SDK for Go pro výpočetní funkce a sítě</span><span class="sxs-lookup"><span data-stu-id="170bc-103">Azure SDK for Go samples for compute and networking</span></span>

<span data-ttu-id="170bc-104">Následující tabulka odkazuje na vybrané ukázky zdrojového kódu Go, které můžete použít pro správu virtuálních počítačů, sítí a podsítí v Azure.</span><span class="sxs-lookup"><span data-stu-id="170bc-104">The following table links to selected samples of Go source code that you can use to manage VMs, virtual networks, and subnets in Azure.</span></span> 

<span data-ttu-id="170bc-105">Všechny ukázky pro Azure SDK for Go jsou dostupné na [GitHubu](https://github.com/Azure-Samples/azure-sdk-for-go-samples).</span><span class="sxs-lookup"><span data-stu-id="170bc-105">All samples for the Azure SDK for Go are available on [GitHub](https://github.com/Azure-Samples/azure-sdk-for-go-samples).</span></span>

| <span data-ttu-id="170bc-106">Název</span><span class="sxs-lookup"><span data-stu-id="170bc-106">Name</span></span> | <span data-ttu-id="170bc-107">Popis</span><span class="sxs-lookup"><span data-stu-id="170bc-107">Description</span></span> |
|------|-------------|
| [<span data-ttu-id="170bc-108">network/network</span><span class="sxs-lookup"><span data-stu-id="170bc-108">network/network</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/network/network.go) | <span data-ttu-id="170bc-109">Vytváření, aktualizace a odstraňování síťových prostředků, včetně virtuálních sítí, podsítí a skupiny zabezpečení sítě, a dotazování na ně.</span><span class="sxs-lookup"><span data-stu-id="170bc-109">Create, update, delete, and query network resources including virtual networks, subnets, and network security groups.</span></span> |
| [<span data-ttu-id="170bc-110">compute/loadbalancer</span><span class="sxs-lookup"><span data-stu-id="170bc-110">compute/loadbalancer</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/loadbalancer.go) | <span data-ttu-id="170bc-111">Vytváření skupin dostupnosti a dotazování na ně, vytváření virtuálních počítačů s využitím nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="170bc-111">Create and query availability groups, and create VMs with a load balancer.</span></span> |
| [<span data-ttu-id="170bc-112">compute/compute</span><span class="sxs-lookup"><span data-stu-id="170bc-112">compute/compute</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/compute.go) | <span data-ttu-id="170bc-113">Vytváření, odstraňování, aktualizace a výkonná správa virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="170bc-113">Create, delete, update, and power-manage VMs.</span></span> <span data-ttu-id="170bc-114">Práce s datovými disky a správa disku operačního systému virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="170bc-114">Work with data disks and managing the OS disk of the VM.</span></span> |

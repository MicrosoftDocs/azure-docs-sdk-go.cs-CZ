---
title: Ukázky z Azure SDK for Go pro výpočetní funkce a sítě
description: Vybrané ukázky ze sady Azure SDK for Go pro práci s výpočetními funkcemi, jako jsou virtuální počítače a virtuální sítě
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 03/21/2018
ms.topic: sample
ms.prod: azure
ms.technology: azure-sdk-go
ms.service: virtual-machines
ms.devlang: go
ms.openlocfilehash: 4837572a50ae934e71bfe49d916c01e131bb6d83
ms.sourcegitcommit: f08abf902b48f8173aa6e261084ff2cfc9043305
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/03/2018
---
# <a name="azure-sdk-for-go-samples-for-compute-and-networking"></a><span data-ttu-id="3e8b9-103">Ukázky z Azure SDK for Go pro výpočetní funkce a sítě</span><span class="sxs-lookup"><span data-stu-id="3e8b9-103">Azure SDK for Go samples for compute and networking</span></span>

<span data-ttu-id="3e8b9-104">Následující tabulka odkazuje na vybrané ukázky zdrojového kódu Go, které můžete použít pro správu virtuálních počítačů, sítí a podsítí v Azure.</span><span class="sxs-lookup"><span data-stu-id="3e8b9-104">The following table links to selected samples of Go source code that you can use to manage VMs, virtual networks, and subnets in Azure.</span></span> 

<span data-ttu-id="3e8b9-105">Všechny ukázky pro Azure SDK for Go jsou dostupné na [GitHubu](https://github.com/Azure-Samples/azure-sdk-for-go-samples).</span><span class="sxs-lookup"><span data-stu-id="3e8b9-105">All samples for the Azure SDK for Go are available on [GitHub](https://github.com/Azure-Samples/azure-sdk-for-go-samples).</span></span>

| <span data-ttu-id="3e8b9-106">Název</span><span class="sxs-lookup"><span data-stu-id="3e8b9-106">Name</span></span> | <span data-ttu-id="3e8b9-107">Popis</span><span class="sxs-lookup"><span data-stu-id="3e8b9-107">Description</span></span> |
|------|-------------|
| [<span data-ttu-id="3e8b9-108">network/network</span><span class="sxs-lookup"><span data-stu-id="3e8b9-108">network/network</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/network/network.go) | <span data-ttu-id="3e8b9-109">Vytváření, aktualizace a odstraňování síťových prostředků, včetně virtuálních sítí, podsítí a skupiny zabezpečení sítě, a dotazování na ně.</span><span class="sxs-lookup"><span data-stu-id="3e8b9-109">Create, update, delete, and query network resources including virtual networks, subnets, and network security groups.</span></span> |
| [<span data-ttu-id="3e8b9-110">compute/loadbalancer</span><span class="sxs-lookup"><span data-stu-id="3e8b9-110">compute/loadbalancer</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/loadbalancer.go) | <span data-ttu-id="3e8b9-111">Vytváření skupin dostupnosti a dotazování na ně, vytváření virtuálních počítačů s využitím nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="3e8b9-111">Create and query availability groups, and create VMs with a load balancer.</span></span> |
| [<span data-ttu-id="3e8b9-112">compute/compute</span><span class="sxs-lookup"><span data-stu-id="3e8b9-112">compute/compute</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/compute.go) | <span data-ttu-id="3e8b9-113">Vytváření, odstraňování, aktualizace a výkonná správa virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="3e8b9-113">Create, delete, update, and power-manage VMs.</span></span> <span data-ttu-id="3e8b9-114">Práce s datovými disky a správa disku operačního systému virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3e8b9-114">Work with data disks and managing the OS disk of the VM.</span></span> |

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
ms.openlocfilehash: 3b31716ee42c638bab4a6dd99b9eb0d7c07e51a4
ms.sourcegitcommit: 0f581979216f7c9d4913681a6d9f6fe09af26e43
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/02/2018
ms.locfileid: "39475785"
---
# <a name="azure-sdk-for-go-samples-for-compute-and-networking"></a><span data-ttu-id="e6a7e-103">Ukázky z Azure SDK for Go pro výpočetní funkce a sítě</span><span class="sxs-lookup"><span data-stu-id="e6a7e-103">Azure SDK for Go samples for compute and networking</span></span>

<span data-ttu-id="e6a7e-104">Následující tabulka odkazuje na vybrané ukázky zdrojového kódu Go, které můžete použít pro správu virtuálních počítačů, sítí a podsítí v Azure.</span><span class="sxs-lookup"><span data-stu-id="e6a7e-104">The following table links to selected samples of Go source code that you can use to manage VMs, virtual networks, and subnets in Azure.</span></span> 

<span data-ttu-id="e6a7e-105">Všechny ukázky pro Azure SDK for Go jsou dostupné na [GitHubu](https://github.com/Azure-Samples/azure-sdk-for-go-samples).</span><span class="sxs-lookup"><span data-stu-id="e6a7e-105">All samples for the Azure SDK for Go are available on [GitHub](https://github.com/Azure-Samples/azure-sdk-for-go-samples).</span></span>

| <span data-ttu-id="e6a7e-106">Název</span><span class="sxs-lookup"><span data-stu-id="e6a7e-106">Name</span></span> | <span data-ttu-id="e6a7e-107">Popis</span><span class="sxs-lookup"><span data-stu-id="e6a7e-107">Description</span></span> |
|------|-------------|
| [<span data-ttu-id="e6a7e-108">network/network</span><span class="sxs-lookup"><span data-stu-id="e6a7e-108">network/network</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/network/network.go) | <span data-ttu-id="e6a7e-109">Vytváření, aktualizace a odstraňování síťových prostředků, včetně virtuálních sítí, podsítí a skupiny zabezpečení sítě, a dotazování na ně.</span><span class="sxs-lookup"><span data-stu-id="e6a7e-109">Create, update, delete, and query network resources including virtual networks, subnets, and network security groups.</span></span> |
| [<span data-ttu-id="e6a7e-110">compute/vm_disk</span><span class="sxs-lookup"><span data-stu-id="e6a7e-110">compute/vm_disk</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm_disk.go) | <span data-ttu-id="e6a7e-111">Vytváření, připojení, odpojení, aktualizace a šifrování datových disků pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="e6a7e-111">Create, attach, detatch, update, and encrypt data disks for a VM.</span></span> |
| [<span data-ttu-id="e6a7e-112">compute/vm</span><span class="sxs-lookup"><span data-stu-id="e6a7e-112">compute/vm</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm.go) | <span data-ttu-id="e6a7e-113">Vytváření, aktualizace, deaktivace a správa virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="e6a7e-113">Create, update, deactivate, and manage VMs.</span></span> |
| [<span data-ttu-id="e6a7e-114">compute/vm_with_availabilityset</span><span class="sxs-lookup"><span data-stu-id="e6a7e-114">compute/vm_with_availabilityset</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm_with_availabilityset.go) | <span data-ttu-id="e6a7e-115">Vytváření skupin dostupnosti a nástrojů pro vyrovnávání zatížení pro virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="e6a7e-115">Create availability sets and load balancers for VMs.</span></span> |
| [<span data-ttu-id="e6a7e-116">compute/vm_with_identity</span><span class="sxs-lookup"><span data-stu-id="e6a7e-116">compute/vm_with_identity</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm_with_identity.go) | <span data-ttu-id="e6a7e-117">Vytváření a správa identit spravované služby (souborů MSI) pro virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="e6a7e-117">Create and manage Managed Service Identities (MSIs) for VMs.</span></span> |

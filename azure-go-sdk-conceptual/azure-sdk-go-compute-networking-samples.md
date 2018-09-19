---
title: Ukázky z Azure SDK for Go pro výpočetní funkce a sítě
description: Vybrané ukázky ze sady Azure SDK for Go pro práci s výpočetními funkcemi, jako jsou virtuální počítače a virtuální sítě
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 09/05/2018
ms.topic: sample
ms.technology: azure-sdk-go
ms.service: virtual-machines
ms.devlang: go
ms.openlocfilehash: d570ad8598ae06633d0010245c207641161ee446
ms.sourcegitcommit: 8b9e10b960150dc08f046ab840d6a5627410db29
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/07/2018
ms.locfileid: "44059080"
---
# <a name="azure-sdk-for-go-samples-for-compute-and-networking"></a><span data-ttu-id="8c55d-103">Ukázky z Azure SDK for Go pro výpočetní funkce a sítě</span><span class="sxs-lookup"><span data-stu-id="8c55d-103">Azure SDK for Go samples for compute and networking</span></span>

<span data-ttu-id="8c55d-104">Následující tabulka odkazuje na vybrané ukázky, které ukazují správu výpočetních prostředků a prostředků virtuální sítě v sadě Azure SDK pro Go.</span><span class="sxs-lookup"><span data-stu-id="8c55d-104">The following table links to selected samples that demonstrate the management of compute and virtual network resources in the Azure SDK for Go.</span></span>

<span data-ttu-id="8c55d-105">Všechny ukázky pro Azure SDK for Go jsou dostupné na [GitHubu](https://github.com/Azure-Samples/azure-sdk-for-go-samples).</span><span class="sxs-lookup"><span data-stu-id="8c55d-105">All samples for the Azure SDK for Go are available on [GitHub](https://github.com/Azure-Samples/azure-sdk-for-go-samples).</span></span>

| <span data-ttu-id="8c55d-106">Název</span><span class="sxs-lookup"><span data-stu-id="8c55d-106">Name</span></span> | <span data-ttu-id="8c55d-107">Popis</span><span class="sxs-lookup"><span data-stu-id="8c55d-107">Description</span></span> |
|------|-------------|
| [<span data-ttu-id="8c55d-108">network/network</span><span class="sxs-lookup"><span data-stu-id="8c55d-108">network/network</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/network/network.go) | <span data-ttu-id="8c55d-109">Vytváření, aktualizace a odstraňování síťových prostředků, včetně virtuálních sítí, podsítí a skupiny zabezpečení sítě, a dotazování na ně.</span><span class="sxs-lookup"><span data-stu-id="8c55d-109">Create, update, delete, and query network resources including virtual networks, subnets, and network security groups.</span></span> |
| [<span data-ttu-id="8c55d-110">compute/vm_disk</span><span class="sxs-lookup"><span data-stu-id="8c55d-110">compute/vm_disk</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm_disk.go) | <span data-ttu-id="8c55d-111">Vytváření, připojení, odpojení, aktualizace a šifrování datových disků pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="8c55d-111">Create, attach, detach, update, and encrypt data disks for a VM.</span></span> |
| [<span data-ttu-id="8c55d-112">compute/vm</span><span class="sxs-lookup"><span data-stu-id="8c55d-112">compute/vm</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm.go) | <span data-ttu-id="8c55d-113">Vytváření, aktualizace, deaktivace a správa virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="8c55d-113">Create, update, deactivate, and manage VMs.</span></span> |
| [<span data-ttu-id="8c55d-114">compute/vm_with_availabilityset</span><span class="sxs-lookup"><span data-stu-id="8c55d-114">compute/vm_with_availabilityset</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm_with_availabilityset.go) | <span data-ttu-id="8c55d-115">Vytváření skupin dostupnosti a nástrojů pro vyrovnávání zatížení pro virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="8c55d-115">Create availability sets and load balancers for VMs.</span></span> |
| [<span data-ttu-id="8c55d-116">compute/vm_with_identity</span><span class="sxs-lookup"><span data-stu-id="8c55d-116">compute/vm_with_identity</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm_with_identity.go) | <span data-ttu-id="8c55d-117">Vytváření a správa identit spravované služby (souborů MSI) pro virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="8c55d-117">Create and manage Managed Service Identities (MSIs) for VMs.</span></span> |

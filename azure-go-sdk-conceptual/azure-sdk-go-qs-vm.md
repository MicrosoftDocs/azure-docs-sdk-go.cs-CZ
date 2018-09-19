---
title: Nasazení virtuálního počítače Azure z Go
description: Nasazení virtuálního počítače Azure s využitím sady Azure SDK pro Go
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 09/05/2018
ms.topic: quickstart
ms.technology: azure-sdk-go
ms.service: virtual-machines
ms.devlang: go
ms.openlocfilehash: a7970be0857fd414d776241b033af0c23457790c
ms.sourcegitcommit: 8b9e10b960150dc08f046ab840d6a5627410db29
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/07/2018
ms.locfileid: "44059131"
---
# <a name="quickstart-deploy-an-azure-virtual-machine-from-a-template-with-the-azure-sdk-for-go"></a><span data-ttu-id="f4373-103">Rychlý start: Nasazení virtuálního počítače Azure ze šablony s využitím sady Azure SDK for Go</span><span class="sxs-lookup"><span data-stu-id="f4373-103">Quickstart: Deploy an Azure virtual machine from a template with the Azure SDK for Go</span></span>

<span data-ttu-id="f4373-104">V tomto rychlém startu se dozvíte, jak nasadit prostředky z šablony Azure Resource Manageru pomocí sady Azure SDK pro Go.</span><span class="sxs-lookup"><span data-stu-id="f4373-104">This quickstart shows you how to deploy resources from an Azure Resource Manager template, using the Azure SDK for Go.</span></span> <span data-ttu-id="f4373-105">Šablony jsou snímky všech prostředků obsažených ve [skupině prostředků Azure](/azure/azure-resource-manager/resource-group-overview).</span><span class="sxs-lookup"><span data-stu-id="f4373-105">Templates are snapshots of all of the resources within an [Azure resource group](/azure/azure-resource-manager/resource-group-overview).</span></span> <span data-ttu-id="f4373-106">Průběžně se seznámíte s funkcemi a konvencemi této sady SDK.</span><span class="sxs-lookup"><span data-stu-id="f4373-106">Along the way, you'll become familiar with the functionality and conventions of the SDK.</span></span>

<span data-ttu-id="f4373-107">Na konci tohoto rychlého startu budete mít spuštěný virtuální počítač, ke kterému se přihlásíte pomocí uživatelského jména a hesla.</span><span class="sxs-lookup"><span data-stu-id="f4373-107">At the end of this quickstart, you have a running VM that you log into with a username and password.</span></span>

> [!NOTE]
> <span data-ttu-id="f4373-108">Pokud se chcete podívat na vytvoření virtuálního počítače v Go bez použití šablony Resource Manageru, tady je [imperativní ukázka](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm.go), která demonstruje sestavení a konfiguraci všech prostředků virtuálního počítače s využitím sady SDK.</span><span class="sxs-lookup"><span data-stu-id="f4373-108">To see the creation of a VM in Go without the use of a Resource Manager template, there is an [imperative sample](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm.go) demonstrating how to build and configure all VM resources with the SDK.</span></span> <span data-ttu-id="f4373-109">Použití šablony v této ukázce umožňuje zaměřit se na konvence SDK a nezacházet příliš do podrobností o architektuře služeb Azure.</span><span class="sxs-lookup"><span data-stu-id="f4373-109">Using a template in this sample allows a focus on SDK conventions without getting into too many details about Azure service architecture.</span></span>

[!INCLUDE [quickstarts-free-trial-note](includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](includes/cloud-shell-try-it.md)]

<span data-ttu-id="f4373-110">Pokud používáte místní instalaci Azure CLI, bude tento rychlý start vyžadovat CLI verze __2.0.28__ nebo novější.</span><span class="sxs-lookup"><span data-stu-id="f4373-110">If you use a local install of the Azure CLI, this quickstart requires CLI version __2.0.28__ or later.</span></span> <span data-ttu-id="f4373-111">Spusťte `az --version` a zkontrolujte, jestli vaše instalace CLI splňuje tento požadavek.</span><span class="sxs-lookup"><span data-stu-id="f4373-111">Run `az --version` to make sure your CLI install meets this requirement.</span></span> <span data-ttu-id="f4373-112">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="f4373-112">If you need to install or upgrade, see [Install the Azure CLI](/cli/azure/install-azure-cli).</span></span>

## <a name="install-the-azure-sdk-for-go"></a><span data-ttu-id="f4373-113">Instalace Azure SDK for Go</span><span class="sxs-lookup"><span data-stu-id="f4373-113">Install the Azure SDK for Go</span></span>

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

## <a name="create-a-service-principal"></a><span data-ttu-id="f4373-114">Vytvoření instančního objektu</span><span class="sxs-lookup"><span data-stu-id="f4373-114">Create a service principal</span></span>

<span data-ttu-id="f4373-115">Pokud se chcete přihlásit k Azure neinteraktivně pomocí aplikace, potřebujete instanční objekt.</span><span class="sxs-lookup"><span data-stu-id="f4373-115">To sign in non-interactively to Azure with an application, you need a service principal.</span></span> <span data-ttu-id="f4373-116">Instanční objekty jsou součástí řízení přístupu na základě role (RBAC), které vytvoří jedinečnou identitu uživatele.</span><span class="sxs-lookup"><span data-stu-id="f4373-116">Service principals are part of role-based access control (RBAC), which creates a unique user identity.</span></span> <span data-ttu-id="f4373-117">Pokud chcete vytvořit nový instanční objekt s využitím CLI, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f4373-117">To create a new service principal with the CLI, run the following command:</span></span>

```azurecli-interactive
az ad sp create-for-rbac --sdk-auth > quickstart.auth
```

<span data-ttu-id="f4373-118">Nastavte proměnnou prostředí `AZURE_AUTH_LOCATION` na úplnou cestu k tomuto souboru.</span><span class="sxs-lookup"><span data-stu-id="f4373-118">Set the environment variable `AZURE_AUTH_LOCATION` to be the full path to this file.</span></span> <span data-ttu-id="f4373-119">Sada SDK pak vyhledá a načte přihlašovací údaje přímo z tohoto souboru bez nutnosti provádět změny nebo zaznamenávat informace z instančního objektu.</span><span class="sxs-lookup"><span data-stu-id="f4373-119">Then the SDK locates and reads the credentials directly from this file, without you having to make any changes or record information from the service principal.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="f4373-120">Získání kódu</span><span class="sxs-lookup"><span data-stu-id="f4373-120">Get the code</span></span>

<span data-ttu-id="f4373-121">K získání kódu tohoto rychlého startu a všech jeho závislostí použijte `go get`.</span><span class="sxs-lookup"><span data-stu-id="f4373-121">Get the quickstart code and all of its dependencies with `go get`.</span></span>

```bash
go get -u -d github.com/azure-samples/azure-sdk-for-go-samples/quickstarts/deploy-vm/...
```

<span data-ttu-id="f4373-122">Pokud je proměnná `AZURE_AUTH_LOCATION` správně nastavená, nemusíte provádět žádné úpravy zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="f4373-122">You don't need to make any source code modifications if the `AZURE_AUTH_LOCATION` variable is properly set.</span></span> <span data-ttu-id="f4373-123">Když se program spustí, načte všechny potřebné ověřovací údaje z této proměnné.</span><span class="sxs-lookup"><span data-stu-id="f4373-123">When the program runs, it loads all the necessary authentication information from there.</span></span>

## <a name="running-the-code"></a><span data-ttu-id="f4373-124">Spuštění kódu</span><span class="sxs-lookup"><span data-stu-id="f4373-124">Running the code</span></span>

<span data-ttu-id="f4373-125">ke spuštění tohoto rychlého startu použijte příkaz `go run`.</span><span class="sxs-lookup"><span data-stu-id="f4373-125">Run the quickstart with the `go run` command.</span></span>

```bash
cd $GOPATH/src/github.com/azure-samples/azure-sdk-for-go-samples/quickstarts/deploy-vm
go run main.go
```

<span data-ttu-id="f4373-126">Pokud je nasazení úspěšné, zobrazí se zpráva s uvedením uživatelského jména, IP adresy a hesla pro přihlášení na nově vytvořený virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="f4373-126">If the deployment is successful, you see a message giving the username, IP address, and password for logging into the newly created virtual machine.</span></span> <span data-ttu-id="f4373-127">Připojte se k tomuto počítači přes SSH a podívejte se, jestli je v provozu.</span><span class="sxs-lookup"><span data-stu-id="f4373-127">SSH into this machine to see if it's up and running.</span></span> 

## <a name="cleaning-up"></a><span data-ttu-id="f4373-128">Čištění</span><span class="sxs-lookup"><span data-stu-id="f4373-128">Cleaning up</span></span>

<span data-ttu-id="f4373-129">Prostředky vytvořené v rámci tohoto rychlého startu můžete vyčistit odstraněním skupiny prostředků pomocí CLI.</span><span class="sxs-lookup"><span data-stu-id="f4373-129">Clean up the resources created during this quickstart by deleting the resource group with the CLI.</span></span>

```azurecli-interactive
az group delete -n GoVMQuickstart
```

<span data-ttu-id="f4373-130">Odstraňte také instanční objekt, který se vytvořil.</span><span class="sxs-lookup"><span data-stu-id="f4373-130">Also delete the service principal that was created.</span></span> <span data-ttu-id="f4373-131">Soubor `quickstart.auth` obsahuje klíč JSON pro `clientId`.</span><span class="sxs-lookup"><span data-stu-id="f4373-131">In the `quickstart.auth` file, there's a JSON key for `clientId`.</span></span> <span data-ttu-id="f4373-132">Zkopírujte tuto hodnotu do proměnné prostředí `CLIENT_ID_VALUE` a spusťte následující příkaz rozhraní příkazového řádku Azure:</span><span class="sxs-lookup"><span data-stu-id="f4373-132">Copy this value to the `CLIENT_ID_VALUE` environment variable and run the following Azure CLI command:</span></span>

```azurecli-interactive
az ad sp delete --id ${CLIENT_ID_VALUE}
```

<span data-ttu-id="f4373-133">kde zadáte hodnotu pro `CLIENT_ID_VALUE` z `quickstart.auth`.</span><span class="sxs-lookup"><span data-stu-id="f4373-133">Where you supply the value for `CLIENT_ID_VALUE` from `quickstart.auth`.</span></span>

> [!WARNING]
> <span data-ttu-id="f4373-134">Pokud se instanční objekt pro tuto službu neodstraní, zůstane ve vašem tenantovi Azure Active Directory aktivní.</span><span class="sxs-lookup"><span data-stu-id="f4373-134">Failing to delete the service principal for this application leaves it active in your Azure Active Directory tenant.</span></span>
> <span data-ttu-id="f4373-135">Přestože se název i heslo pro tento instanční objekt vygenerují jako UUID, nezapomeňte dodržovat osvědčené postupy zabezpečení a odstranit všechny nepoužité instanční objekty a aplikace Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="f4373-135">While both the name and password for the service principal are generated as UUIDs, make sure that you follow good security practices by deleting any unused service principals and Azure Active Directory Applications.</span></span>

## <a name="code-in-depth"></a><span data-ttu-id="f4373-136">Kód podrobněji</span><span class="sxs-lookup"><span data-stu-id="f4373-136">Code in depth</span></span>

<span data-ttu-id="f4373-137">To, co kód tohoto rychlého startu dělá, je rozdělené do bloku proměnných a několika malých funkcí, které tady probereme.</span><span class="sxs-lookup"><span data-stu-id="f4373-137">What the quickstart code does is broken down into a block of variables and several small functions, each of which are discussed here.</span></span>

### <a name="variables-constants-and-types"></a><span data-ttu-id="f4373-138">Proměnné, konstanty a typy</span><span class="sxs-lookup"><span data-stu-id="f4373-138">Variables, constants, and types</span></span>

<span data-ttu-id="f4373-139">Vzhledem k tomu, že je tento rychlý start samostatný, využívá globální konstanty a proměnné.</span><span class="sxs-lookup"><span data-stu-id="f4373-139">Since quickstart is self-contained, it uses global constants and variables.</span></span>

```go
const (
    resourceGroupName     = "GoVMQuickstart"
    resourceGroupLocation = "eastus"

    deploymentName = "VMDeployQuickstart"
    templateFile   = "vm-quickstart-template.json"
    parametersFile = "vm-quickstart-params.json"
)

// Information loaded from the authorization file to identify the client
type clientInfo struct {
    SubscriptionID string
    VMPassword     string
}

var (
    ctx        = context.Background()
    clientData clientInfo
    authorizer autorest.Authorizer
)
```

<span data-ttu-id="f4373-140">Jsou deklarované hodnoty, které poskytují názvy vytvořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="f4373-140">Values are declared which give the names of created resources.</span></span> <span data-ttu-id="f4373-141">Je tady také zadané umístění, které můžete změnit, abyste viděli, jak se nasazení chovají v jiných datových centrech.</span><span class="sxs-lookup"><span data-stu-id="f4373-141">The location is also specified here, which you can change to see how deployments behave in other datacenters.</span></span> <span data-ttu-id="f4373-142">Ne každé datové centrum má k dispozici všechny požadované prostředky.</span><span class="sxs-lookup"><span data-stu-id="f4373-142">Not every datacenter has all of the required resources available.</span></span>

<span data-ttu-id="f4373-143">Typ `clientInfo` obsahuje informace načtené z ověřovacího souboru kvůli nastavení klientů v sadě SDK a nastavení hesla virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f4373-143">The `clientInfo` type holds the information loaded from the authentication file to set up clients in the SDK and set the VM password.</span></span>

<span data-ttu-id="f4373-144">Konstanty `templateFile` a `parametersFile` odkazují na soubory potřebné pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="f4373-144">The `templateFile` and `parametersFile` constants point to the files needed for deployment.</span></span> <span data-ttu-id="f4373-145">Objekt `authorizer` nakonfiguruje sada Go SDK pro ověřování a proměnná `ctx` představuje [kontext Go](https://blog.golang.org/context) pro síťové operace.</span><span class="sxs-lookup"><span data-stu-id="f4373-145">The `authorizer` will be configured by the Go SDK for authentication, and the `ctx` variable is a [Go context](https://blog.golang.org/context) for the network operations.</span></span>

### <a name="authentication-and-initialization"></a><span data-ttu-id="f4373-146">Ověřování a inicializace</span><span class="sxs-lookup"><span data-stu-id="f4373-146">Authentication and initialization</span></span>

<span data-ttu-id="f4373-147">Funkce `init` nastaví ověřování.</span><span class="sxs-lookup"><span data-stu-id="f4373-147">The `init` function sets up authentication.</span></span> <span data-ttu-id="f4373-148">Vzhledem k tomu, že ověřování je předpokladem pro všechno, co je v rychlém startu, má smysl, aby bylo součástí inicializace.</span><span class="sxs-lookup"><span data-stu-id="f4373-148">Since authentication is a precondition for everything in the quickstart, it makes sense to have it as part of initialization.</span></span> <span data-ttu-id="f4373-149">Z ověřovacího souboru se také načtou některé informace potřebné ke konfiguraci klientů a virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f4373-149">It also loads some information needed from the authentication file to configure clients and the VM.</span></span>

```go
func init() {
    var err error
    authorizer, err = auth.NewAuthorizerFromFile(azure.PublicCloud.ResourceManagerEndpoint)
    if err != nil {
        log.Fatalf("Failed to get OAuth config: %v", err)
    }

    authInfo, err := readJSON(os.Getenv("AZURE_AUTH_LOCATION"))
    clientData.SubscriptionID = (*authInfo)["subscriptionId"].(string)
    clientData.VMPassword = (*authInfo)["clientSecret"].(string)
}
```

<span data-ttu-id="f4373-150">Nejprve se zavolá metoda [auth.NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile), která načte ověřovací údaje ze souboru v umístění `AZURE_AUTH_LOCATION`.</span><span class="sxs-lookup"><span data-stu-id="f4373-150">First, [auth.NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile) is called to load the authentication information from the file located at `AZURE_AUTH_LOCATION`.</span></span> <span data-ttu-id="f4373-151">Tento soubor se pak ručně načte funkcí `readJSON` (tady jsme ji vynechali) a přetáhnou se dvě hodnoty potřebné ke spuštění zbytku programu: ID předplatného klienta a tajný klíč instančního objektu, který se použije také jako heslo virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f4373-151">Next, this file is loaded manually by the `readJSON` function (omitted here) to pull the two values needed to run the rest of the program: The subscription ID of the client, and the service principal's secret, which is also used for the VM's password.</span></span>

> [!WARNING]
> <span data-ttu-id="f4373-152">Pro zjednodušení tohoto rychlého startu se znovu použije heslo instančního objektu.</span><span class="sxs-lookup"><span data-stu-id="f4373-152">To keep the quickstart simple, the service principal password is reused.</span></span> <span data-ttu-id="f4373-153">V produkčním prostředí dbejte na to, abyste __nikdy__ znovu nepoužívali heslo poskytující přístup k vašim prostředkům Azure.</span><span class="sxs-lookup"><span data-stu-id="f4373-153">In production, take care to __never__ reuse a password which gives access to your Azure resources.</span></span>

### <a name="flow-of-operations-in-main"></a><span data-ttu-id="f4373-154">Tok operací v main()</span><span class="sxs-lookup"><span data-stu-id="f4373-154">Flow of operations in main()</span></span>

<span data-ttu-id="f4373-155">Funkce `main` je jednoduchá – jenom ukazuje tok operací a provádí kontrolu chyb.</span><span class="sxs-lookup"><span data-stu-id="f4373-155">The `main` function is simple, only indicating the flow of operations and performing error-checking.</span></span>

```go
func main() {
    group, err := createGroup()
    if err != nil {
        log.Fatalf("failed to create group: %v", err)
    }
    log.Printf("Created group: %v", *group.Name)

    log.Printf("Starting deployment: %s", deploymentName)
    result, err := createDeployment()
    if err != nil {
        log.Fatalf("Failed to deploy: %v", err)
    }
    if result.Name != nil {
        log.Printf("Completed deployment %v: %v", deploymentName, *result.Properties.ProvisioningState)
    } else {
        log.Printf("Completed deployment %v (no data returned to SDK)", deploymentName)
    }
    getLogin()
}
```

<span data-ttu-id="f4373-156">Kód prochází těmito kroky (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="f4373-156">The steps that the code runs through are, in order:</span></span>

* <span data-ttu-id="f4373-157">Vytvoření skupiny prostředků pro nasazení do (`createGroup`)</span><span class="sxs-lookup"><span data-stu-id="f4373-157">Create the resource group to deploy to (`createGroup`)</span></span>
* <span data-ttu-id="f4373-158">Vytvoření nasazení v této skupině (`createDeployment`)</span><span class="sxs-lookup"><span data-stu-id="f4373-158">Create the deployment within this group (`createDeployment`)</span></span>
* <span data-ttu-id="f4373-159">Získání a zobrazení přihlašovacích informací pro nasazený virtuální počítač (`getLogin`)</span><span class="sxs-lookup"><span data-stu-id="f4373-159">Get and display login information for the deployed VM (`getLogin`)</span></span>

### <a name="create-the-resource-group"></a><span data-ttu-id="f4373-160">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="f4373-160">Create the resource group</span></span>

<span data-ttu-id="f4373-161">Funkce `createGroup` vytvoří skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="f4373-161">The `createGroup` function creates the resource group.</span></span> <span data-ttu-id="f4373-162">Pohled na tok volání a argumenty ukazuje způsob, jakým jsou interakce služeb v sadě SDK strukturované.</span><span class="sxs-lookup"><span data-stu-id="f4373-162">Looking at the call flow and arguments demonstrates the way that service interactions are structured in the SDK.</span></span>

```go
func createGroup() (group resources.Group, err error) {
    groupsClient := resources.NewGroupsClient(clientData.SubscriptionID)
    groupsClient.Authorizer = authorizer

        return groupsClient.CreateOrUpdate(
                ctx,
                resourceGroupName,
                resources.Group{
                        Location: to.StringPtr(resourceGroupLocation)})
}
```

<span data-ttu-id="f4373-163">Obecný tok interakcí se službou Azure vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="f4373-163">The general flow of interacting with an Azure service is:</span></span>

* <span data-ttu-id="f4373-164">Vytvořte klienta pomocí metody `service.New*Client()`, kde `*` je typ prostředku pro `service`, se kterým chcete interagovat.</span><span class="sxs-lookup"><span data-stu-id="f4373-164">Create the client using the `service.New*Client()` method, where `*` is the resource type of the `service` that you want to interact with.</span></span> <span data-ttu-id="f4373-165">Tato funkce vždycky použije ID předplatného.</span><span class="sxs-lookup"><span data-stu-id="f4373-165">This function always takes a subscription ID.</span></span>
* <span data-ttu-id="f4373-166">Pro tohoto klienta nastavte metodu autorizace a umožněte mu interagovat se vzdáleným rozhraním API.</span><span class="sxs-lookup"><span data-stu-id="f4373-166">Set the authorization method for the client, allowing it to interact with the remote API.</span></span>
* <span data-ttu-id="f4373-167">Použijte volání metody v klientovi odpovídající vzdálenému rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="f4373-167">Make the method call on the client corresponding to the remote API.</span></span> <span data-ttu-id="f4373-168">Metody klientů služby obvykle používají název prostředku a objekt metadat.</span><span class="sxs-lookup"><span data-stu-id="f4373-168">Service client methods usually take the name of the resource and a metadata object.</span></span>

<span data-ttu-id="f4373-169">Funkce [`to.StringPtr`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) se tady používá k převodu typů.</span><span class="sxs-lookup"><span data-stu-id="f4373-169">The [`to.StringPtr`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) function is used to perform a type conversion here.</span></span> <span data-ttu-id="f4373-170">Parametry pro metody sady SDK téměř výhradně přebírají ukazatele, takže jsou pro usnadnění převodu typů k dispozici pomocné metody.</span><span class="sxs-lookup"><span data-stu-id="f4373-170">The parameters for SDK methods almost exclusively take pointers, so convenience methods are provided to make the type conversions easy.</span></span> <span data-ttu-id="f4373-171">Úplný seznam a chování pomocných převaděčů najdete v dokumentaci k modulu [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to).</span><span class="sxs-lookup"><span data-stu-id="f4373-171">See the documentation for the [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) module for the complete list of convenience converters and their behavior.</span></span>

<span data-ttu-id="f4373-172">Metoda `groupsClient.CreateOrUpdate` vrací ukazatel na datový typ představující skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="f4373-172">The `groupsClient.CreateOrUpdate` method returns a pointer to a data type representing the resource group.</span></span> <span data-ttu-id="f4373-173">Přímá návratová hodnota tohoto druhu indikuje krátce běžící operaci, která je zamýšlená jako synchronní.</span><span class="sxs-lookup"><span data-stu-id="f4373-173">A direct return value of this kind indicates a short-running operation that is meant to be synchronous.</span></span> <span data-ttu-id="f4373-174">V další části uvidíte příklad dlouhotrvající operace a způsob práce s ní.</span><span class="sxs-lookup"><span data-stu-id="f4373-174">In the next section, you'll see an example of a long-running operation and how to interact with it.</span></span>

### <a name="perform-the-deployment"></a><span data-ttu-id="f4373-175">Realizace nasazení</span><span class="sxs-lookup"><span data-stu-id="f4373-175">Perform the deployment</span></span>

<span data-ttu-id="f4373-176">Jakmile se vytvoří skupina prostředků, je čas spustit nasazení.</span><span class="sxs-lookup"><span data-stu-id="f4373-176">Once the resource group is created, it's time to run the deployment.</span></span> <span data-ttu-id="f4373-177">Tento kód je rozdělený do menších oddílů, aby se zdůraznily různé části jeho logiky.</span><span class="sxs-lookup"><span data-stu-id="f4373-177">This code is broken up into smaller sections to emphasize different parts of its logic.</span></span>

```go
func createDeployment() (deployment resources.DeploymentExtended, err error) {
    template, err := readJSON(templateFile)
    if err != nil {
        return
    }
    params, err := readJSON(parametersFile)
    if err != nil {
        return
    }
    (*params)["vm_password"] = map[string]string{
        "value": clientData.VMPassword,
    }
        // ...
```

<span data-ttu-id="f4373-178">Soubory nasazení načte `readJSON`, podrobnosti tady neuvádíme.</span><span class="sxs-lookup"><span data-stu-id="f4373-178">The deployment files are loaded by `readJSON`, the details of which are skipped here.</span></span> <span data-ttu-id="f4373-179">Tato funkce vrátí `*map[string]interface{}`. Tento typ se používá při konstrukci metadat pro volání nasazení prostředků.</span><span class="sxs-lookup"><span data-stu-id="f4373-179">This function returns a `*map[string]interface{}`, the type used in constructing the metadata for the resource deployment call.</span></span> <span data-ttu-id="f4373-180">Také se ručně nastaví heslo virtuálního počítače pro parametry nasazení.</span><span class="sxs-lookup"><span data-stu-id="f4373-180">The VM's password is also set manually on the deployment parameters.</span></span>

```go
        // ...

    deploymentsClient := resources.NewDeploymentsClient(clientData.SubscriptionID)
    deploymentsClient.Authorizer = authorizer

    deploymentFuture, err := deploymentsClient.CreateOrUpdate(
        ctx,
        resourceGroupName,
        deploymentName,
        resources.Deployment{
            Properties: &resources.DeploymentProperties{
                Template:   template,
                Parameters: params,
                Mode:       resources.Incremental,
            },
        },
    )
    if err != nil {
        return
    }
```

<span data-ttu-id="f4373-181">Tento kód používá stejný vzor jako vytváření skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="f4373-181">This code follows the same pattern as creating the resource group.</span></span> <span data-ttu-id="f4373-182">Vytvoří se nový klient, získá možnost ověřovat pomocí Azure a potom se volá metoda.</span><span class="sxs-lookup"><span data-stu-id="f4373-182">A new client is created, given the ability to authenticate with Azure, and then a method is called.</span></span>
<span data-ttu-id="f4373-183">Tato metoda má dokonce stejný název (`CreateOrUpdate`) jako odpovídající metoda pro skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="f4373-183">The method even has the same name (`CreateOrUpdate`) as the corresponding method for resource groups.</span></span> <span data-ttu-id="f4373-184">Tento vzor se používá v celé sadě SDK.</span><span class="sxs-lookup"><span data-stu-id="f4373-184">This pattern is seen throughout the SDK.</span></span>
<span data-ttu-id="f4373-185">Metody, které provádějí podobné úkoly, mají obvykle stejný název.</span><span class="sxs-lookup"><span data-stu-id="f4373-185">Methods that perform similar work normally have the same name.</span></span>

<span data-ttu-id="f4373-186">Největší rozdíl spočívá v návratové hodnotě metody `deploymentsClient.CreateOrUpdate`.</span><span class="sxs-lookup"><span data-stu-id="f4373-186">The biggest difference comes in the return value of the `deploymentsClient.CreateOrUpdate` method.</span></span> <span data-ttu-id="f4373-187">Tato hodnota je typu [Future](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#Future), který využívá [vzor návrhu konstruktů future](https://en.wikipedia.org/wiki/Futures_and_promises).</span><span class="sxs-lookup"><span data-stu-id="f4373-187">This value is of the [Future](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#Future) type, which follows the [future design pattern](https://en.wikipedia.org/wiki/Futures_and_promises).</span></span> <span data-ttu-id="f4373-188">Konstrukty future představují dlouhotrvající operace v Azure, které můžete po dokončení dotazovat, zrušit nebo zablokovat.</span><span class="sxs-lookup"><span data-stu-id="f4373-188">Futures represent a long-running operation in Azure that you can poll, cancel, or block on their completion.</span></span>

```go
        //...
    err = deploymentFuture.Future.WaitForCompletion(ctx, deploymentsClient.BaseClient.Client)
    if err != nil {
        return
    }
    return deploymentFuture.Result(deploymentsClient)
}
```

<span data-ttu-id="f4373-189">V tomto příkladu je nejlepší počkat na dokončení operace.</span><span class="sxs-lookup"><span data-stu-id="f4373-189">For this example, the best thing to do is to wait for the operation to complete.</span></span> <span data-ttu-id="f4373-190">Čekání v konstruktu future vyžaduje [kontextový objekt](https://blog.golang.org/context) a klienta, který vytvořil objekt `Future`.</span><span class="sxs-lookup"><span data-stu-id="f4373-190">Waiting on a future requires both a [context object](https://blog.golang.org/context) and the client that created the `Future`.</span></span> <span data-ttu-id="f4373-191">Z toho vyplývají dva možné zdroje chyb: chyba způsobená na straně klienta při pokusu o volání metody a reakce na chybu ze serveru.</span><span class="sxs-lookup"><span data-stu-id="f4373-191">There are two possible error sources here: An error caused on the client side when trying to invoke the method, and an error response from the server.</span></span> <span data-ttu-id="f4373-192">Ta druhá se vrací jako součást volání `deploymentFuture.Result`.</span><span class="sxs-lookup"><span data-stu-id="f4373-192">The latter is returned as part of the `deploymentFuture.Result` call.</span></span>

### <a name="get-the-assigned-ip-address"></a><span data-ttu-id="f4373-193">Získání přiřazené IP adresy</span><span class="sxs-lookup"><span data-stu-id="f4373-193">Get the assigned IP address</span></span>

<span data-ttu-id="f4373-194">Abyste mohli s nově vytvořeným virtuálním počítačem něco udělat, potřebujete přiřazenou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="f4373-194">To do anything with the newly created VM, you need the assigned IP address.</span></span> <span data-ttu-id="f4373-195">IP adresy jsou vlastní samostatné prostředky Azure, které jsou vázané na prostředky NIC (Network Interface Controller).</span><span class="sxs-lookup"><span data-stu-id="f4373-195">IP addresses are their own separate Azure resource, bound to Network Interface Controller (NIC) resources.</span></span>

```go
func getLogin() {
    params, err := readJSON(parametersFile)
    if err != nil {
        log.Fatalf("Unable to read parameters. Get login information with `az network public-ip list -g %s", resourceGroupName)
    }

    addressClient := network.NewPublicIPAddressesClient(clientData.SubscriptionID)
    addressClient.Authorizer = authorizer
    ipName := (*params)["publicIPAddresses_QuickstartVM_ip_name"].(map[string]interface{})
    ipAddress, err := addressClient.Get(ctx, resourceGroupName, ipName["value"].(string), "")
    if err != nil {
        log.Fatalf("Unable to get IP information. Try using `az network public-ip list -g %s", resourceGroupName)
    }

    vmUser := (*params)["vm_user"].(map[string]interface{})

    log.Printf("Log in with ssh: %s@%s, password: %s",
        vmUser["value"].(string),
        *ipAddress.PublicIPAddressPropertiesFormat.IPAddress,
        clientData.VMPassword)
}
```

<span data-ttu-id="f4373-196">Tato metoda využívá informace uložené v souboru parametrů.</span><span class="sxs-lookup"><span data-stu-id="f4373-196">This method relies on information that is stored in the parameters file.</span></span> <span data-ttu-id="f4373-197">Kód by mohl pro zjištění NIC zadat dotaz přímo virtuálnímu počítači, potom zadat dotaz NIC k získání prostředku IP adresy a nakonec zadat dotaz přímo prostředku IP adresy.</span><span class="sxs-lookup"><span data-stu-id="f4373-197">The code could query the VM directly to get its NIC, query the NIC to get its IP resource, and then query the IP resource directly.</span></span> <span data-ttu-id="f4373-198">To je ale dlouhý řetězec závislostí a operací, které je potřeba vyřešit, a proto je nákladný.</span><span class="sxs-lookup"><span data-stu-id="f4373-198">That's a long chain of dependencies and operations to resolve, making it expensive.</span></span> <span data-ttu-id="f4373-199">Vzhledem k tomu, že informace JSON jsou místní, dají se načíst místo toho.</span><span class="sxs-lookup"><span data-stu-id="f4373-199">Since the JSON information is local, it can be loaded instead.</span></span>

<span data-ttu-id="f4373-200">Z JSON se také načte hodnota pro uživatele virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f4373-200">The value for the VM user is also loaded from the JSON.</span></span> <span data-ttu-id="f4373-201">Heslo virtuálního počítače se načetlo dříve z ověřovacího souboru.</span><span class="sxs-lookup"><span data-stu-id="f4373-201">The VM password was loaded earlier from the authentication file.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f4373-202">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f4373-202">Next steps</span></span>

<span data-ttu-id="f4373-203">V tomto rychlém startu jste vzali existující šablonu a nasadili ji prostřednictvím Go.</span><span class="sxs-lookup"><span data-stu-id="f4373-203">In this quickstart, you took an existing template and deployed it through Go.</span></span> <span data-ttu-id="f4373-204">Potom jste se přes SSH připojili k nově vytvořenému virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="f4373-204">Then you connected to the newly created VM via SSH.</span></span>

<span data-ttu-id="f4373-205">Pokud se chcete dál seznamovat s použitím virtuálních počítačů v prostředí Azure s Go, prohlédněte si témata s [ukázkami výpočetních funkcí Azure pro Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) a [ukázkami správy prostředků Azure pro Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).</span><span class="sxs-lookup"><span data-stu-id="f4373-205">To continue learning about working with virtual machines in the Azure environment with Go, take a look at the [Azure compute samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) or [Azure resource management samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).</span></span>

<span data-ttu-id="f4373-206">Další informace o dostupných metodách ověřování v sadě SDK a typech ověřování, které podporují, najdete v tématu [Ověřování pomocí sady Azure SDK for Go](azure-sdk-go-authorization.md).</span><span class="sxs-lookup"><span data-stu-id="f4373-206">To learn more about the available authentication methods in the SDK, and which authentication types they support, see [Authentication with the Azure SDK for Go](azure-sdk-go-authorization.md).</span></span>

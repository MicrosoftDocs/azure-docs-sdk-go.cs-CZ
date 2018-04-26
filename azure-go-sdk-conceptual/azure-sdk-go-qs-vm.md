---
title: Nasazení virtuálního počítače Azure z Go
description: Nasazení virtuálního počítače Azure s využitím sady Azure SDK for Go
author: sptramer
ms.author: sttramer
ms.date: 04/03/2018
ms.topic: quickstart
ms.devlang: go
manager: carmonm
ms.openlocfilehash: 565580e9e6c6ced543bd00bbaa01383834d9a41c
ms.sourcegitcommit: 2b2884ea7673c95ba45b3d6eec647200e75bfc5b
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/19/2018
---
# <a name="quickstart-deploy-an-azure-virtual-machine-from-a-template-with-the-azure-sdk-for-go"></a><span data-ttu-id="6b468-103">Rychlý start: Nasazení virtuálního počítače Azure ze šablony s využitím sady Azure SDK for Go</span><span class="sxs-lookup"><span data-stu-id="6b468-103">Quickstart: Deploy an Azure virtual machine from a template with the Azure SDK for Go</span></span>

<span data-ttu-id="6b468-104">Tento rychlý start se zaměřuje na nasazení prostředků ze šablony s využitím sady Azure SDK for Go.</span><span class="sxs-lookup"><span data-stu-id="6b468-104">This quickstart focuses on deploying resources from a template with the Azure SDK for Go.</span></span> <span data-ttu-id="6b468-105">Šablony jsou snímky všech prostředků obsažených ve [skupině prostředků Azure](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview).</span><span class="sxs-lookup"><span data-stu-id="6b468-105">Templates are snapshots of all of the resources contained within an [Azure resource group](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview).</span></span> <span data-ttu-id="6b468-106">Při provádění užitečných úloh se průběžně seznámíte s funkcemi a konvencemi této sady SDK.</span><span class="sxs-lookup"><span data-stu-id="6b468-106">Along the way, you'll become familiar with the functionality and conventions of the SDK while performing a useful task.</span></span>

<span data-ttu-id="6b468-107">Na konci tohoto rychlého startu budete mít spuštěný virtuální počítač, ke kterému se přihlásíte pomocí uživatelského jména a hesla.</span><span class="sxs-lookup"><span data-stu-id="6b468-107">At the end of this quickstart, you have a running VM that you log into with a username and password.</span></span>

[!INCLUDE [quickstarts-free-trial-note](includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](includes/cloud-shell-try-it.md)]

<span data-ttu-id="6b468-108">Pokud používáte místní instalaci Azure CLI, bude tento rychlý start vyžadovat CLI verze __2.0.28__ nebo novější.</span><span class="sxs-lookup"><span data-stu-id="6b468-108">If you use a local install of the Azure CLI, this quickstart requires CLI version __2.0.28__ or later.</span></span> <span data-ttu-id="6b468-109">Spusťte `az --version` a zkontrolujte, jestli vaše instalace CLI splňuje tento požadavek.</span><span class="sxs-lookup"><span data-stu-id="6b468-109">Run `az --version` to make sure your CLI install meets this requirement.</span></span> <span data-ttu-id="6b468-110">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="6b468-110">If you need to install or upgrade, see [Install the Azure CLI 2.0](/cli/azure/install-azure-cli).</span></span>

## <a name="install-the-azure-sdk-for-go"></a><span data-ttu-id="6b468-111">Instalace Azure SDK for Go</span><span class="sxs-lookup"><span data-stu-id="6b468-111">Install the Azure SDK for Go</span></span> 

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

## <a name="create-a-service-principal"></a><span data-ttu-id="6b468-112">Vytvoření instančního objektu</span><span class="sxs-lookup"><span data-stu-id="6b468-112">Create a service principal</span></span>


<span data-ttu-id="6b468-113">Pokud se chcete přihlásit neinteraktivně pomocí aplikace, potřebujete instanční objekt.</span><span class="sxs-lookup"><span data-stu-id="6b468-113">To log in non-interactively with an application, you need a service principal.</span></span> <span data-ttu-id="6b468-114">Instanční objekty jsou součástí řízení přístupu na základě role (RBAC), které vytvoří jedinečnou identitu uživatele.</span><span class="sxs-lookup"><span data-stu-id="6b468-114">Service principals are part of role-based access control (RBAC), which creates a unique user identity.</span></span> <span data-ttu-id="6b468-115">Pokud chcete vytvořit nový instanční objekt s využitím CLI, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="6b468-115">To create a new service principal with the CLI, run the following command:</span></span>

```azurecli-interactive
az ad sp create-for-rbac --name az-go-vm-quickstart --sdk-auth > quickstart.auth
```

<span data-ttu-id="6b468-116">Nastavte proměnnou prostředí `AZURE_AUTH_LOCATION` na úplnou cestu k tomuto souboru.</span><span class="sxs-lookup"><span data-stu-id="6b468-116">Set the environment variable `AZURE_AUTH_LOCATION` to be the full path to this file.</span></span> <span data-ttu-id="6b468-117">Sada SDK pak vyhledá a načte přihlašovací údaje přímo z tohoto souboru bez nutnosti provádět změny nebo zaznamenávat informace z instančního objektu.</span><span class="sxs-lookup"><span data-stu-id="6b468-117">Then the SDK locates and reads the credentials directly from this file, without you having to make any changes or record information from the service principal.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="6b468-118">Získání kódu</span><span class="sxs-lookup"><span data-stu-id="6b468-118">Get the code</span></span>

<span data-ttu-id="6b468-119">K získání kódu tohoto rychlého startu a všech jeho závislostí použijte `go get`.</span><span class="sxs-lookup"><span data-stu-id="6b468-119">Get the quickstart code and all of its dependencies with `go get`.</span></span>

```bash
go get -u -d github.com/azure-samples/azure-sdk-for-go-samples/quickstarts/deploy-vm/...
```

<span data-ttu-id="6b468-120">Pokud je proměnná `AZURE_AUTH_LOCATION` správně nastavená, nemusíte provádět žádné úpravy zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="6b468-120">You don't need to make any source code modifications if the `AZURE_AUTH_LOCATION` variable is properly set.</span></span> <span data-ttu-id="6b468-121">Když se program spustí, načte všechny potřebné ověřovací údaje z této proměnné.</span><span class="sxs-lookup"><span data-stu-id="6b468-121">When the program runs, it loads all the necessary authentication information from there.</span></span>

## <a name="running-the-code"></a><span data-ttu-id="6b468-122">Spuštění kódu</span><span class="sxs-lookup"><span data-stu-id="6b468-122">Running the code</span></span>

<span data-ttu-id="6b468-123">ke spuštění tohoto rychlého startu použijte příkaz `go run`.</span><span class="sxs-lookup"><span data-stu-id="6b468-123">Run the quickstart with the `go run` command.</span></span>

```bash
cd $GOPATH/src/github.com/azure-samples/azure-sdk-for-go-samples/quickstarts/deploy-vm
go run main.go
```

<span data-ttu-id="6b468-124">Pokud při nasazení dojde k selhání, zobrazí se zpráva informující, že došlo k problému. Zpráva však nemusí obsahovat dostatek podrobností.</span><span class="sxs-lookup"><span data-stu-id="6b468-124">If there is a failure in the deployment, you get a message indicating that there was an issue, but it may not include enough detail.</span></span> <span data-ttu-id="6b468-125">S využitím Azure CLI můžete úplné podrobnosti o selhání nasazení získat pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="6b468-125">Using the Azure CLI, get the full details of the deployment failure with the following command:</span></span>

```azurecli-interactive
az group deployment show -g GoVMQuickstart -n VMDeployQuickstart
```

<span data-ttu-id="6b468-126">Pokud je nasazení úspěšné, zobrazí se zpráva s uvedením uživatelského jména, IP adresy a hesla pro přihlášení na nově vytvořený virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="6b468-126">If the deployment is successful, you see a message giving the username, IP address, and password for logging into the newly created virtual machine.</span></span> <span data-ttu-id="6b468-127">Připojte se k tomuto počítači přes SSH a potvrďte, že je zprovozněný.</span><span class="sxs-lookup"><span data-stu-id="6b468-127">SSH into this machine to confirm that it is up and running.</span></span>

## <a name="cleaning-up"></a><span data-ttu-id="6b468-128">Čištění</span><span class="sxs-lookup"><span data-stu-id="6b468-128">Cleaning up</span></span>

<span data-ttu-id="6b468-129">Prostředky vytvořené v rámci tohoto rychlého startu můžete vyčistit odstraněním skupiny prostředků pomocí CLI.</span><span class="sxs-lookup"><span data-stu-id="6b468-129">Clean up the resources created during this quickstart by deleting the resource group with the CLI.</span></span>

```azurecli-interactive
az group delete -n GoVMQuickstart
```

## <a name="code-in-depth"></a><span data-ttu-id="6b468-130">Kód podrobněji</span><span class="sxs-lookup"><span data-stu-id="6b468-130">Code in depth</span></span>

<span data-ttu-id="6b468-131">To, co kód tohoto rychlého startu dělá, je rozdělené do bloku proměnných a několika malých funkcí, které tady probereme.</span><span class="sxs-lookup"><span data-stu-id="6b468-131">What the quickstart code does is broken down into a block of variables and several small functions, each of which are discussed here.</span></span>

### <a name="variables-constants-and-types"></a><span data-ttu-id="6b468-132">Proměnné, konstanty a typy</span><span class="sxs-lookup"><span data-stu-id="6b468-132">Variables, constants, and types</span></span>

<span data-ttu-id="6b468-133">Vzhledem k tomu, že je tento rychlý start samostatný, využívá globální konstanty a proměnné.</span><span class="sxs-lookup"><span data-stu-id="6b468-133">Since quickstart is self-contained, it uses global constants and variables.</span></span>

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

<span data-ttu-id="6b468-134">Jsou deklarované hodnoty, které poskytují názvy vytvořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="6b468-134">Values are declared which give the names of created resources.</span></span> <span data-ttu-id="6b468-135">Je tady také zadané umístění, které můžete změnit, abyste viděli, jak se nasazení chovají v jiných datových centrech.</span><span class="sxs-lookup"><span data-stu-id="6b468-135">The location is also specified here, which you can change to see how deployments behave in other datacenters.</span></span> <span data-ttu-id="6b468-136">Ne každé datové centrum má k dispozici všechny požadované prostředky.</span><span class="sxs-lookup"><span data-stu-id="6b468-136">Not every datacenter has all of the required resources available.</span></span>

<span data-ttu-id="6b468-137">Typ `clientInfo` se deklaruje za účelem zapouzdření veškerých informací, které je potřeba nezávisle načíst z ověřovacího souboru kvůli nastavení klientů v sadě SDK a nastavení hesla virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6b468-137">The `clientInfo` type is declared to encapsulate all of the information that must be independently loaded from the authentication file to set up clients in the SDK and set the VM password.</span></span>

<span data-ttu-id="6b468-138">Konstanty `templateFile` a `parametersFile` odkazují na soubory potřebné pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="6b468-138">The `templateFile` and `parametersFile` constants point to the files needed for deployment.</span></span> <span data-ttu-id="6b468-139">Objekt `authorizer` nakonfiguruje sada Go SDK pro ověřování a proměnná `ctx` představuje [kontext Go](https://blog.golang.org/context) pro síťové operace.</span><span class="sxs-lookup"><span data-stu-id="6b468-139">The `authorizer` will be configured by the Go SDK for authentication, and the `ctx` variable is a [Go context](https://blog.golang.org/context) for the network operations.</span></span>

### <a name="authentication-and-initialization"></a><span data-ttu-id="6b468-140">Ověřování a inicializace</span><span class="sxs-lookup"><span data-stu-id="6b468-140">Authentication and initialization</span></span>

<span data-ttu-id="6b468-141">Funkce `init` nastaví ověřování.</span><span class="sxs-lookup"><span data-stu-id="6b468-141">The `init` function sets up authentication.</span></span> <span data-ttu-id="6b468-142">Vzhledem k tomu, že ověřování je předpokladem pro všechno, co je v rychlém startu, má smysl, aby bylo součástí inicializace.</span><span class="sxs-lookup"><span data-stu-id="6b468-142">Since authentication is a precondition for everything in the quickstart, it makes sense to have it as part of initialization.</span></span> <span data-ttu-id="6b468-143">Z ověřovacího souboru se také načtou některé informace potřebné ke konfiguraci klientů a virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6b468-143">It also loads some information needed from the authentication file to configure clients and the VM.</span></span>

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

<span data-ttu-id="6b468-144">Nejprve se zavolá metoda [auth.NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile), která načte ověřovací údaje ze souboru v umístění `AZURE_AUTH_LOCATION`.</span><span class="sxs-lookup"><span data-stu-id="6b468-144">First, [auth.NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile) is called to load the authentication information from the file located at `AZURE_AUTH_LOCATION`.</span></span> <span data-ttu-id="6b468-145">Tento soubor se pak ručně načte funkcí `readJSON` (tady jsme ji vynechali) a přetáhnou se dvě hodnoty potřebné ke spuštění zbytku programu: ID předplatného klienta a tajný klíč instančního objektu, který se použije také jako heslo virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6b468-145">Next, this file is loaded manually by the `readJSON` function (omitted here) to pull the two values needed to run the rest of the program: The subscription ID of the client, and the service principal's secret, which is also used for the VM's password.</span></span>

> [!WARNING]
> <span data-ttu-id="6b468-146">Pro zjednodušení tohoto rychlého startu se znovu použije heslo instančního objektu.</span><span class="sxs-lookup"><span data-stu-id="6b468-146">To keep the quickstart simple, the service principal password is reused.</span></span> <span data-ttu-id="6b468-147">V produkčním prostředí dbejte na to, abyste __nikdy__ znovu nepoužívali heslo poskytující přístup k vašim prostředkům Azure.</span><span class="sxs-lookup"><span data-stu-id="6b468-147">In production, take care to __never__ reuse a password which gives access to your Azure resources.</span></span>

### <a name="flow-of-operations-in-main"></a><span data-ttu-id="6b468-148">Tok operací v main()</span><span class="sxs-lookup"><span data-stu-id="6b468-148">Flow of operations in main()</span></span>

<span data-ttu-id="6b468-149">Funkce `main` je jednoduchá – jenom ukazuje tok operací a provádí kontrolu chyb.</span><span class="sxs-lookup"><span data-stu-id="6b468-149">The `main` function is simple, only indicating the flow of operations and performing error-checking.</span></span>

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

<span data-ttu-id="6b468-150">Kód prochází těmito kroky (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="6b468-150">The steps that the code runs through are, in order:</span></span>

* <span data-ttu-id="6b468-151">Vytvoření skupiny prostředků pro nasazení do (`createGroup`)</span><span class="sxs-lookup"><span data-stu-id="6b468-151">Create the resource group to deploy to (`createGroup`)</span></span>
* <span data-ttu-id="6b468-152">Vytvoření nasazení v této skupině (`createDeployment`)</span><span class="sxs-lookup"><span data-stu-id="6b468-152">Create the deployment within this group (`createDeployment`)</span></span>
* <span data-ttu-id="6b468-153">Získání a zobrazení přihlašovacích informací pro nasazený virtuální počítač (`getLogin`)</span><span class="sxs-lookup"><span data-stu-id="6b468-153">Obtain and display login information for the deployed VM (`getLogin`)</span></span>

### <a name="creating-the-resource-group"></a><span data-ttu-id="6b468-154">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="6b468-154">Creating the resource group</span></span>

<span data-ttu-id="6b468-155">Funkce `createGroup` vytvoří skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="6b468-155">The `createGroup` function creates the resource group.</span></span> <span data-ttu-id="6b468-156">Pohled na tok volání a argumenty ukazuje způsob, jakým jsou interakce služeb v sadě SDK strukturované.</span><span class="sxs-lookup"><span data-stu-id="6b468-156">Looking at the call flow and arguments demonstrates the way that service interactions are structured in the SDK.</span></span>

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

<span data-ttu-id="6b468-157">Obecný tok interakcí se službou Azure vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="6b468-157">The general flow of interacting with an Azure service is:</span></span>

* <span data-ttu-id="6b468-158">Vytvořte klienta pomocí metody `service.New*Client()`, kde `*` je typ prostředku pro `service`, se kterým chcete interagovat.</span><span class="sxs-lookup"><span data-stu-id="6b468-158">Create the client using the `service.New*Client()` method, where `*` is the resource type of the `service` that you want to interact with.</span></span> <span data-ttu-id="6b468-159">Tato funkce vždycky použije ID předplatného.</span><span class="sxs-lookup"><span data-stu-id="6b468-159">This function always takes a subscription ID.</span></span>
* <span data-ttu-id="6b468-160">Pro tohoto klienta nastavte metodu autorizace a umožněte mu interagovat se vzdáleným rozhraním API.</span><span class="sxs-lookup"><span data-stu-id="6b468-160">Set the authorization method for the client, allowing it to interact with the remote API.</span></span>
* <span data-ttu-id="6b468-161">Použijte volání metody v klientovi odpovídající vzdálenému rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="6b468-161">Make the method call on the client corresponding to the remote API.</span></span> <span data-ttu-id="6b468-162">Metody klientů služby obvykle používají název prostředku a objekt metadat.</span><span class="sxs-lookup"><span data-stu-id="6b468-162">Service client methods usually take the name of the resource and a metadata object.</span></span>

<span data-ttu-id="6b468-163">Funkce [`to.StringPtr`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) se tady používá k převodu typů.</span><span class="sxs-lookup"><span data-stu-id="6b468-163">The [`to.StringPtr`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) function is used to perform a type conversion here.</span></span> <span data-ttu-id="6b468-164">Parametry pro metody sady SDK téměř výhradně přebírají ukazatele, takže jsou pro usnadnění převodu typů k dispozici pomocné metody.</span><span class="sxs-lookup"><span data-stu-id="6b468-164">The parameters for SDK methods almost exclusively take pointers, so convenience methods are provided to make the type conversions easy.</span></span> <span data-ttu-id="6b468-165">Úplný seznam a chování pomocných převaděčů najdete v dokumentaci k modulu [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to).</span><span class="sxs-lookup"><span data-stu-id="6b468-165">See the documentation for the [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) module for the complete list of convenience converters and their behavior.</span></span>

<span data-ttu-id="6b468-166">Metoda `groupsClient.CreateOrUpdate` vrací ukazatel na datový typ představující skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="6b468-166">The `groupsClient.CreateOrUpdate` method returns a pointer to a data type representing the resource group.</span></span> <span data-ttu-id="6b468-167">Přímá návratová hodnota tohoto druhu indikuje krátce běžící operaci, která je zamýšlená jako synchronní.</span><span class="sxs-lookup"><span data-stu-id="6b468-167">A direct return value of this kind indicates a short-running operation that is meant to be synchronous.</span></span> <span data-ttu-id="6b468-168">V další části uvidíte příklad dlouhotrvající operace a způsob práce s ní.</span><span class="sxs-lookup"><span data-stu-id="6b468-168">In the next section, you'll see an example of a long-running operation and how to interact with it.</span></span>

### <a name="performing-the-deployment"></a><span data-ttu-id="6b468-169">Realizace nasazení</span><span class="sxs-lookup"><span data-stu-id="6b468-169">Performing the deployment</span></span>

<span data-ttu-id="6b468-170">Jakmile se vytvoří skupina prostředků, je čas spustit nasazení.</span><span class="sxs-lookup"><span data-stu-id="6b468-170">Once the resource group is created, it's time to run the deployment.</span></span> <span data-ttu-id="6b468-171">Tento kód je rozdělený do menších oddílů, aby se zdůraznily různé části jeho logiky.</span><span class="sxs-lookup"><span data-stu-id="6b468-171">This code is broken up into smaller sections to emphasize different parts of its logic.</span></span>

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

<span data-ttu-id="6b468-172">Soubory nasazení načte `readJSON`, podrobnosti tady neuvádíme.</span><span class="sxs-lookup"><span data-stu-id="6b468-172">The deployment files are loaded by `readJSON`, the details of which are skipped here.</span></span> <span data-ttu-id="6b468-173">Tato funkce vrátí `*map[string]interface{}`. Tento typ se používá při konstrukci metadat pro volání nasazení prostředků.</span><span class="sxs-lookup"><span data-stu-id="6b468-173">This function returns a `*map[string]interface{}`, the type used in constructing the metadata for the resource deployment call.</span></span> <span data-ttu-id="6b468-174">Také se ručně nastaví heslo virtuálního počítače pro parametry nasazení.</span><span class="sxs-lookup"><span data-stu-id="6b468-174">The VM's password is also set manually on the deployment parameters.</span></span>

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

<span data-ttu-id="6b468-175">Tento kód používá stejný vzor jako vytváření skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="6b468-175">This code follows the same pattern as creating the resource group.</span></span> <span data-ttu-id="6b468-176">Vytvoří se nový klient, získá možnost ověřovat pomocí Azure a potom se volá metoda.</span><span class="sxs-lookup"><span data-stu-id="6b468-176">A new client is created, given the ability to authenticate with Azure, and then a method is called.</span></span> <span data-ttu-id="6b468-177">Tato metoda má dokonce stejný název (`CreateOrUpdate`) jako odpovídající metoda pro skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="6b468-177">The method even has the same name (`CreateOrUpdate`) as the corresponding method for resource groups.</span></span> <span data-ttu-id="6b468-178">Tento vzor se používá v celé sadě SDK.</span><span class="sxs-lookup"><span data-stu-id="6b468-178">This pattern is seen throughout the SDK.</span></span> <span data-ttu-id="6b468-179">Metody, které provádějí podobné úkoly, mají obvykle stejný název.</span><span class="sxs-lookup"><span data-stu-id="6b468-179">Methods that perform similar work normally have the same name.</span></span>

<span data-ttu-id="6b468-180">Největší rozdíl spočívá v návratové hodnotě metody `deploymentsClient.CreateOrUpdate`.</span><span class="sxs-lookup"><span data-stu-id="6b468-180">The biggest difference comes in the return value of the `deploymentsClient.CreateOrUpdate` method.</span></span> <span data-ttu-id="6b468-181">Tato hodnota je typu [Future](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#Future), který využívá [vzor návrhu konstruktů future](https://en.wikipedia.org/wiki/Futures_and_promises).</span><span class="sxs-lookup"><span data-stu-id="6b468-181">This value is of the [Future](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#Future) type, which follows the [future design pattern](https://en.wikipedia.org/wiki/Futures_and_promises).</span></span> <span data-ttu-id="6b468-182">Konstrukty future představují dlouhotrvající operace v Azure, které můžete po dokončení dotazovat, zrušit nebo zablokovat.</span><span class="sxs-lookup"><span data-stu-id="6b468-182">Futures represent a long-running operation in Azure that you can poll, cancel, or block on their completion.</span></span>

```go
        //...
    err = deploymentFuture.Future.WaitForCompletion(ctx, deploymentsClient.BaseClient.Client)
    if err != nil {
        return
    }
    deployment, err = deploymentFuture.Result(deploymentsClient)

    // Work around possible bugs or late-stage failures
    if deployment.Name == nil || err != nil {
        deployment, _ = deploymentsClient.Get(ctx, resourceGroupName, deploymentName)
    }
    return
```

<span data-ttu-id="6b468-183">V tomto příkladu je nejlepší počkat na dokončení operace.</span><span class="sxs-lookup"><span data-stu-id="6b468-183">For this example, the best thing to do is to wait for the operation to complete.</span></span> <span data-ttu-id="6b468-184">Čekání v konstruktu future vyžaduje [kontextový objekt](https://blog.golang.org/context) a klienta, který vytvořil objekt `Future`.</span><span class="sxs-lookup"><span data-stu-id="6b468-184">Waiting on a future requires both a [context object](https://blog.golang.org/context) and the client that created the `Future`.</span></span> <span data-ttu-id="6b468-185">Z toho vyplývají dva možné zdroje chyb: chyba způsobená na straně klienta při pokusu o volání metody a reakce na chybu ze serveru.</span><span class="sxs-lookup"><span data-stu-id="6b468-185">There are two possible error sources here: An error caused on the client side when trying to invoke the method, and an error response from the server.</span></span> <span data-ttu-id="6b468-186">Ta druhá se vrací jako součást volání `deploymentFuture.Result`.</span><span class="sxs-lookup"><span data-stu-id="6b468-186">The latter is returned as part of the `deploymentFuture.Result` call.</span></span>

<span data-ttu-id="6b468-187">Pro případné chyby, kdy informace o nasazení můžou být po načtení prázdné, existuje alternativní řešení, a to ruční zavolání metody `deploymentsClient.Get`, která zajistí doplnění dat.</span><span class="sxs-lookup"><span data-stu-id="6b468-187">Once the deployment information is retrieved, there is a workaround for possible bugs where the deployment information may be empty with a manual call to `deploymentsClient.Get` to ensure that the data is populated.</span></span>

### <a name="obtaining-the-assigned-ip-address"></a><span data-ttu-id="6b468-188">Získání přiřazené IP adresy</span><span class="sxs-lookup"><span data-stu-id="6b468-188">Obtaining the assigned IP address</span></span>

<span data-ttu-id="6b468-189">Abyste mohli s nově vytvořeným virtuálním počítačem něco udělat, potřebujete přiřazenou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="6b468-189">To do anything with the newly created VM, you need the assigned IP address.</span></span> <span data-ttu-id="6b468-190">IP adresy jsou vlastní samostatné prostředky Azure, které jsou vázané na prostředky NIC (Network Interface Controller).</span><span class="sxs-lookup"><span data-stu-id="6b468-190">IP addresses are their own separate Azure resource, bound to Network Interface Controller (NIC) resources.</span></span>

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

<span data-ttu-id="6b468-191">Tato metoda využívá informace uložené v souboru parametrů.</span><span class="sxs-lookup"><span data-stu-id="6b468-191">This method relies on information that is stored in the parameters file.</span></span> <span data-ttu-id="6b468-192">Kód by mohl pro zjištění NIC zadat dotaz přímo virtuálnímu počítači, potom zadat dotaz NIC k získání prostředku IP adresy a nakonec zadat dotaz přímo prostředku IP adresy.</span><span class="sxs-lookup"><span data-stu-id="6b468-192">The code could query the VM directly to get its NIC, query the NIC to get its IP resource, and then query the IP resource directly.</span></span> <span data-ttu-id="6b468-193">To je ale dlouhý řetězec závislostí a operací, které je potřeba vyřešit, a proto je nákladný.</span><span class="sxs-lookup"><span data-stu-id="6b468-193">That's a long chain of dependencies and operations to resolve, making it expensive.</span></span> <span data-ttu-id="6b468-194">Vzhledem k tomu, že informace JSON jsou místní, dají se načíst místo toho.</span><span class="sxs-lookup"><span data-stu-id="6b468-194">Since the JSON information is local, it can be loaded instead.</span></span>

<span data-ttu-id="6b468-195">Z JSON se také načte hodnota pro uživatele virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6b468-195">The value for the VM user is also loaded from the JSON.</span></span> <span data-ttu-id="6b468-196">Heslo virtuálního počítače se načetlo dříve z ověřovacího souboru.</span><span class="sxs-lookup"><span data-stu-id="6b468-196">The VM password was loaded earlier from the authentication file.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6b468-197">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6b468-197">Next steps</span></span>

<span data-ttu-id="6b468-198">V tomto rychlém startu jste vzali existující šablonu a nasadili ji prostřednictvím Go.</span><span class="sxs-lookup"><span data-stu-id="6b468-198">In this quickstart, you took an existing template and deployed it through Go.</span></span> <span data-ttu-id="6b468-199">Potom jste se přes SSH připojili k nově vytvořenému virtuálnímu počítači a ověřili, že je spuštěný.</span><span class="sxs-lookup"><span data-stu-id="6b468-199">Then you connected to the newly created VM via SSH to ensure that it's running.</span></span>

<span data-ttu-id="6b468-200">Pokud se chcete dál seznamovat s použitím virtuálních počítačů v prostředí Azure s Go, prohlédněte si témata s [ukázkami výpočetních funkcí Azure pro Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) a [ukázkami správy prostředků Azure pro Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).</span><span class="sxs-lookup"><span data-stu-id="6b468-200">To continue learning about working with virtual machines in the Azure environment with Go, take a look at the [Azure compute samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) or [Azure resource management samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).</span></span>

<span data-ttu-id="6b468-201">Další informace o dostupných metodách ověřování v sadě SDK a typech ověřování, které podporují, najdete v tématu [Ověřování pomocí sady Azure SDK for Go](azure-sdk-go-authorization.md).</span><span class="sxs-lookup"><span data-stu-id="6b468-201">To learn more about the available authentication methods in the SDK, and which authentication types they support, see [Authentication with the Azure SDK for Go](azure-sdk-go-authorization.md).</span></span>

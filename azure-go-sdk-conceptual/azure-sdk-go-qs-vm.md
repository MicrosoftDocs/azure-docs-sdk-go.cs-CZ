---
title: "Nasazení virtuálního počítače Azure z Go"
description: "Nasazení virtuálního počítače Azure s využitím sady Azure SDK for Go"
keywords: azure, virtual machine, vm, go, golang, azure sdk
author: sptramer
ms.author: sttramer
ms.date: 02/08/2018
ms.topic: quickstart
ms.devlang: go
manager: routlaw
ms.openlocfilehash: e530d944deca40e9e6c29b6c2768e2367822714e
ms.sourcegitcommit: aaa8c37880332625f858a38f5918e6cf581bf48d
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/15/2018
---
# <a name="quickstart-deploy-an-azure-virtual-machine-from-a-template-with-the-azure-sdk-for-go"></a><span data-ttu-id="d3658-104">Rychlý start: Nasazení virtuálního počítače Azure ze šablony s využitím sady Azure SDK for Go</span><span class="sxs-lookup"><span data-stu-id="d3658-104">Quickstart: Deploy an Azure virtual machine from a template with the Azure SDK for Go</span></span>

<span data-ttu-id="d3658-105">Tento rychlý start se zaměřuje na nasazení prostředků ze šablony s využitím sady Azure SDK for Go.</span><span class="sxs-lookup"><span data-stu-id="d3658-105">This quickstart focuses on deploying resources from a template with the Azure SDK for Go.</span></span> <span data-ttu-id="d3658-106">Šablony jsou snímky všech prostředků obsažených ve [skupině prostředků Azure](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview).</span><span class="sxs-lookup"><span data-stu-id="d3658-106">Templates are snapshots of all of the resources contained within an [Azure resource group](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview).</span></span> <span data-ttu-id="d3658-107">Při provádění užitečných úloh se průběžně seznámíte s funkcemi a konvencemi této sady SDK.</span><span class="sxs-lookup"><span data-stu-id="d3658-107">Along the way, you'll become familiar with the functionality and conventions of the SDK while performing a useful task.</span></span>

<span data-ttu-id="d3658-108">Na konci tohoto rychlého startu budete mít spuštěný virtuální počítač, ke kterému se přihlásíte pomocí uživatelského jména a hesla.</span><span class="sxs-lookup"><span data-stu-id="d3658-108">At the end of this quickstart, you have a running VM that you log into with a username and password.</span></span>

[!INCLUDE [quickstarts-free-trial-note](includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](includes/cloud-shell-try-it.md)]

<span data-ttu-id="d3658-109">Pokud používáte místní instalaci Azure CLI, bude tento rychlý start vyžadovat CLI verze 2.0.24 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="d3658-109">If you use a local install of the Azure CLI, this quickstart requires CLI version 2.0.24 or later.</span></span> <span data-ttu-id="d3658-110">Spusťte `az --version` a zkontrolujte, jestli vaše instalace CLI splňuje tento požadavek.</span><span class="sxs-lookup"><span data-stu-id="d3658-110">Run `az --version` to make sure your CLI install meets this requirement.</span></span> <span data-ttu-id="d3658-111">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="d3658-111">If you need to install or upgrade, see [Install the Azure CLI 2.0](/cli/azure/install-azure-cli).</span></span>

## <a name="install-the-azure-sdk-for-go"></a><span data-ttu-id="d3658-112">Instalace Azure SDK for Go</span><span class="sxs-lookup"><span data-stu-id="d3658-112">Install the Azure SDK for Go</span></span> 

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

## <a name="create-a-service-principal"></a><span data-ttu-id="d3658-113">Vytvoření instančního objektu</span><span class="sxs-lookup"><span data-stu-id="d3658-113">Create a service principal</span></span>

<span data-ttu-id="d3658-114">Pokud se chcete přihlásit neinteraktivně pomocí aplikace, potřebujete instanční objekt.</span><span class="sxs-lookup"><span data-stu-id="d3658-114">To log in non-interactively with an application, you need a service principal.</span></span> <span data-ttu-id="d3658-115">Instanční objekty jsou součástí ověřování na základě rolí (RBAC), které vytvoří jedinečnou identitu uživatele.</span><span class="sxs-lookup"><span data-stu-id="d3658-115">Service principals are part of Role-Based Authentication (RBAC), which creates a unique user identity.</span></span> <span data-ttu-id="d3658-116">Pokud chcete vytvořit nový instanční objekt s využitím CLI, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="d3658-116">To create a new service principal with the CLI, run the following command:</span></span>

```azurecli-interactive
az ad sp create-for-rbac --name az-go-vm-quickstart
```

<span data-ttu-id="d3658-117">__Nezapomeňte__ ve výstupu zaznamenat hodnoty `appId`, `password` a `tenant`.</span><span class="sxs-lookup"><span data-stu-id="d3658-117">__Make sure__ to record the `appId`, `password`, and `tenant` values in the output.</span></span> <span data-ttu-id="d3658-118">Aplikace tyto hodnoty používá k ověření pomocí Azure.</span><span class="sxs-lookup"><span data-stu-id="d3658-118">These values are used by the application to authenticate with Azure.</span></span>

<span data-ttu-id="d3658-119">Další informace o vytváření a správě instančních objektů s využitím Azure CLI 2.0 najdete v tématu [Vytvoření instančního objektu Azure pomocí Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="d3658-119">For more information on creating and managing Service Principals with the Azure CLI 2.0, see [Create an Azure service principal with Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli).</span></span>

## <a name="get-the-code"></a><span data-ttu-id="d3658-120">Získání kódu</span><span class="sxs-lookup"><span data-stu-id="d3658-120">Get the code</span></span>

<span data-ttu-id="d3658-121">K získání kódu tohoto rychlého startu a všech jeho závislostí použijte `go get`.</span><span class="sxs-lookup"><span data-stu-id="d3658-121">Get the quickstart code and all of its dependencies with `go get`.</span></span>

```bash
go get -u -d github.com/azure-samples/azure-sdk-for-go-samples/quickstart/deploy-vm/...
```

<span data-ttu-id="d3658-122">Tento kód se zkompiluje, ale nefunguje správně, dokud nezadáte informace o vašem účtu Azure a vytvořeném instančním objektu.</span><span class="sxs-lookup"><span data-stu-id="d3658-122">This code compiles, but doesn't run correctly until you provide it information about your Azure account and the created service principal.</span></span> <span data-ttu-id="d3658-123">V `main.go` je proměnná `config`, která obsahuje strukturu `authInfo`.</span><span class="sxs-lookup"><span data-stu-id="d3658-123">In `main.go` there is a variable, `config`, which contains an `authInfo` struct.</span></span> <span data-ttu-id="d3658-124">V této struktuře je potřeba nahradit hodnoty příslušných polí, aby ověřování fungovalo správně.</span><span class="sxs-lookup"><span data-stu-id="d3658-124">This struct needs to have its field values replaced in order to authenticate correctly.</span></span>

```go
    config = authInfo{ // Your application credentials
        TenantID:               "", // Azure account tenantID
        SubscriptionID:         "", // Azure subscription subscriptionID
        ServicePrincipalID:     "", // Service principal appId
        ServicePrincipalSecret: "", // Service principal password/secret
    }
```

* <span data-ttu-id="d3658-125">`SubscriptionID`: Vaše ID předplatného, které můžete zjistit pomocí příkazu CLI</span><span class="sxs-lookup"><span data-stu-id="d3658-125">`SubscriptionID`: Your subscription ID, which can be obtained from the CLI command</span></span>

  ```azurecli-interactive
  az account show --query id -o tsv
  ```

* <span data-ttu-id="d3658-126">`TenantID`: Vaše ID tenanta, hodnota `tenant` zaznamenaná při vytváření instančního objektu</span><span class="sxs-lookup"><span data-stu-id="d3658-126">`TenantID`: Your tenant ID, the `tenant` value recorded when creating the service principal</span></span>
* <span data-ttu-id="d3658-127">`ServicePrincipalID`: Hodnota `appId` zaznamenaná při vytváření instančního objektu</span><span class="sxs-lookup"><span data-stu-id="d3658-127">`ServicePrincipalID`: The `appId` value recorded when creating the service principal</span></span>
* <span data-ttu-id="d3658-128">`ServicePrincipalSecret`: Hodnota `password` zaznamenaná při vytváření instančního objektu</span><span class="sxs-lookup"><span data-stu-id="d3658-128">`ServicePrincipalSecret`: The `password` value recorded when creating the service principal</span></span>

<span data-ttu-id="d3658-129">Musíte také upravit hodnotu v poli `vm-quickstart-params.json`.</span><span class="sxs-lookup"><span data-stu-id="d3658-129">You also need to edit a value in the `vm-quickstart-params.json` file.</span></span>

```json
    "vm_password": {
        "value": "_"
    }
```

* <span data-ttu-id="d3658-130">`vm_password`: Heslo pro uživatelský účet virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="d3658-130">`vm_password`: The password for the VM user account.</span></span> <span data-ttu-id="d3658-131">Musí mít délku 6 až 72 znaků a obsahovat 3 z následujících znaků:</span><span class="sxs-lookup"><span data-stu-id="d3658-131">It must be 6-72 characters in length and contain 3 of the following characters:</span></span>
  * <span data-ttu-id="d3658-132">Malé písmeno</span><span class="sxs-lookup"><span data-stu-id="d3658-132">A lowercase letter</span></span>
  * <span data-ttu-id="d3658-133">Velké písmeno</span><span class="sxs-lookup"><span data-stu-id="d3658-133">An uppercase letter</span></span>
  * <span data-ttu-id="d3658-134">Číslo</span><span class="sxs-lookup"><span data-stu-id="d3658-134">A number</span></span>
  * <span data-ttu-id="d3658-135">Symbol</span><span class="sxs-lookup"><span data-stu-id="d3658-135">A symbol</span></span>

## <a name="running-the-code"></a><span data-ttu-id="d3658-136">Spuštění kódu</span><span class="sxs-lookup"><span data-stu-id="d3658-136">Running the code</span></span>

<span data-ttu-id="d3658-137">ke spuštění tohoto rychlého startu použijte příkaz `go run`.</span><span class="sxs-lookup"><span data-stu-id="d3658-137">Run the quickstart with the `go run` command.</span></span>

```bash
cd $GOPATH/src/github.com/azure-samples/azure-sdk-for-go-samples/quickstart/deploy-vm
go run main.go
```

<span data-ttu-id="d3658-138">Jestliže při nasazení dojde k selhání, dostanete zprávu informující, že nastal problém, ale bez jakýchkoli konkrétních podrobností.</span><span class="sxs-lookup"><span data-stu-id="d3658-138">If there is a failure in the deployment, you get a message indicating that there was an issue, but without any specific details.</span></span> <span data-ttu-id="d3658-139">S využitím Azure CLI můžete podrobné informace o selhání nasazení získat pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="d3658-139">Using the Azure CLI, get the details of the deployment failure with the following command:</span></span>

```azurecli-interactive
az group deployment show -g GoVMQuickstart -n VMDeployQuickstart
```

<span data-ttu-id="d3658-140">Pokud je nasazení úspěšné, zobrazí se zpráva s uvedením uživatelského jména, IP adresy a hesla pro přihlášení na nově vytvořený virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="d3658-140">If the deployment is successful, you see a message giving the username, IP address, and password for logging into the newly created virtual machine.</span></span> <span data-ttu-id="d3658-141">Připojte se k tomuto počítači přes SSH a potvrďte, že je zprovozněný.</span><span class="sxs-lookup"><span data-stu-id="d3658-141">SSH into this machine to confirm that it is up and running.</span></span>

## <a name="cleaning-up"></a><span data-ttu-id="d3658-142">Čištění</span><span class="sxs-lookup"><span data-stu-id="d3658-142">Cleaning up</span></span>

<span data-ttu-id="d3658-143">Prostředky vytvořené v rámci tohoto rychlého startu můžete vyčistit odstraněním skupiny prostředků pomocí CLI.</span><span class="sxs-lookup"><span data-stu-id="d3658-143">Clean up the resources created during this quickstart by deleting the resource group with the CLI.</span></span>

```azurecli-interactive
az group delete -n GoVMQuickstart
```

## <a name="code-in-depth"></a><span data-ttu-id="d3658-144">Kód podrobněji</span><span class="sxs-lookup"><span data-stu-id="d3658-144">Code in depth</span></span>

<span data-ttu-id="d3658-145">To, co kód tohoto rychlého startu dělá, je rozdělené do bloku proměnných a několika malých funkcí, které tady probereme.</span><span class="sxs-lookup"><span data-stu-id="d3658-145">What the quickstart code does is broken down into a block of variables and several small functions, each of which are discussed here.</span></span>

### <a name="variable-assignments-and-structs"></a><span data-ttu-id="d3658-146">Struktury a přiřazení proměnných</span><span class="sxs-lookup"><span data-stu-id="d3658-146">Variable assignments and structs</span></span>

<span data-ttu-id="d3658-147">Vzhledem k tomu, že rychlý start je samostatný, využívá globální proměnné (a ne možnosti příkazového řádku nebo proměnné prostředí).</span><span class="sxs-lookup"><span data-stu-id="d3658-147">Since quickstart is self-contained, it uses global variables rather than command-line options or environment variables.</span></span>

```go
type authInfo struct {
        TenantID               string
        SubscriptionID         string
        ServicePrincipalID     string
        ServicePrincipalSecret string
}
```

<span data-ttu-id="d3658-148">Struktura `authInfo` je deklarovaná k zapouzdření všech informací potřebných pro ověřování s využitím služeb Azure.</span><span class="sxs-lookup"><span data-stu-id="d3658-148">The `authInfo` struct is declared to encapsulate all of the information needed for authorization with Azure services.</span></span>

```go
const (
    resourceGroupName     = "GoVMQuickstart"
    resourceGroupLocation = "eastus"

    deploymentName = "VMDeployQuickstart"
    templateFile   = "vm-quickstart-template.json"
    parametersFile = "vm-quickstart-params.json"
)

var (
    config = authInfo{ // Your application credentials
        TenantID:               "", // Azure account tenantID
        SubscriptionID:         "", // Azure subscription subscriptionID
        ServicePrincipalID:     "", // Service principal appId
        ServicePrincipalSecret: "", // Service principal password/secret
    }

    ctx = context.Background()

    token *adal.ServicePrincipalToken
)
```

<span data-ttu-id="d3658-149">Jsou deklarované hodnoty, které poskytují názvy vytvořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="d3658-149">Values are declared which give the names of created resources.</span></span> <span data-ttu-id="d3658-150">Je tady také zadané umístění, které můžete změnit, abyste viděli, jak se nasazení chovají v jiných datových centrech.</span><span class="sxs-lookup"><span data-stu-id="d3658-150">The location is also specified here, which you can change to see how deployments behave in other datacenters.</span></span> <span data-ttu-id="d3658-151">Ne každé datové centrum má k dispozici všechny požadované prostředky.</span><span class="sxs-lookup"><span data-stu-id="d3658-151">Not every datacenter has all of the required resources available.</span></span>

<span data-ttu-id="d3658-152">Konstanty `templateFile` a `parametersFile` odkazují na soubory potřebné pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="d3658-152">The `templateFile` and `parametersFile` constants point to the files needed for deployment.</span></span> <span data-ttu-id="d3658-153">Tokenu instančního objektu se věnujeme později a proměnná `ctx` je [kontext Go](https://blog.golang.org/context) pro síťové operace.</span><span class="sxs-lookup"><span data-stu-id="d3658-153">The service principal token is covered later, and the `ctx` variable is a [Go context](https://blog.golang.org/context) for the network operations.</span></span>

### <a name="init-and-authorization"></a><span data-ttu-id="d3658-154">init() a autorizace</span><span class="sxs-lookup"><span data-stu-id="d3658-154">init() and authorization</span></span>

<span data-ttu-id="d3658-155">Metoda `init()` pro kód nastavuje autorizaci.</span><span class="sxs-lookup"><span data-stu-id="d3658-155">The `init()` method for the code sets up authorization.</span></span> <span data-ttu-id="d3658-156">Vzhledem k tomu, že autorizace je předpokladem pro všechno, co je v rychlém startu, má smysl, aby byla součástí inicializace.</span><span class="sxs-lookup"><span data-stu-id="d3658-156">Since authorization is a precondition for everything in the quickstart, it makes sense to have it as part of initialization.</span></span> 

```go
// Authenticate with the Azure services over OAuth, using a service principal.
func init() {
    oauthConfig, err := adal.NewOAuthConfig(azure.PublicCloud.ActiveDirectoryEndpoint, config.TenantID)
    if err != nil {
        log.Fatalf("Failed to get OAuth config: %v\n", err)
    }
    token, err = adal.NewServicePrincipalToken(
        *oauthConfig,
        config.ServicePrincipalID,
        config.ServicePrincipalSecret,
        azure.PublicCloud.ResourceManagerEndpoint)
    if err != nil {
        log.Fatalf("faled to get token: %v\n", err)
    }
}
```

<span data-ttu-id="d3658-157">Tento kód dokončuje dva kroky pro autorizaci:</span><span class="sxs-lookup"><span data-stu-id="d3658-157">This code completes two steps for authorization:</span></span>

* <span data-ttu-id="d3658-158">Informace o konfiguraci OAuth pro `TenantID` se načtou z Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="d3658-158">OAuth configuration information for the `TenantID` is retrieved by interfacing with Azure Active Directory.</span></span> <span data-ttu-id="d3658-159">Objekt [`azure.PublicCloud`](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#PublicCloud) obsahuje koncové body použité ve standardní konfiguraci Azure.</span><span class="sxs-lookup"><span data-stu-id="d3658-159">The [`azure.PublicCloud`](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#PublicCloud) object contains endpoints used in the standard Azure configuration.</span></span>
* <span data-ttu-id="d3658-160">Volá se funkce [`adal.NewServicePrincipalToken()`](https://godoc.org/github.com/Azure/go-autorest/autorest/adal#NewServicePrincipalToken).</span><span class="sxs-lookup"><span data-stu-id="d3658-160">The [`adal.NewServicePrincipalToken()`](https://godoc.org/github.com/Azure/go-autorest/autorest/adal#NewServicePrincipalToken) function is called.</span></span> <span data-ttu-id="d3658-161">Tato funkce přebírá informace OAuth spolu s přihlášením instančního objektu a také to, jaký styl správy Azure se používá.</span><span class="sxs-lookup"><span data-stu-id="d3658-161">This function takes the OAuth information along with the service principal login, as well as which style of Azure management is being used.</span></span> <span data-ttu-id="d3658-162">Pokud nemáte konkrétní požadavky a víte, co děláte, tato hodnota by vždycky měla být `.ResourceManagerEndpoint`.</span><span class="sxs-lookup"><span data-stu-id="d3658-162">Unless you have specific requirements and know what you're doing, this value should always be `.ResourceManagerEndpoint`.</span></span>

### <a name="flow-of-operations-in-main"></a><span data-ttu-id="d3658-163">Tok operací v main()</span><span class="sxs-lookup"><span data-stu-id="d3658-163">Flow of operations in main()</span></span>

<span data-ttu-id="d3658-164">Funkce `main()` je jednoduchá – jenom ukazuje tok operací a provádí kontrolu chyb.</span><span class="sxs-lookup"><span data-stu-id="d3658-164">The `main()` function is simple, only indicating the flow of operations and performing error-checking.</span></span>

```go
func main() {
    group, err := createGroup()
    if err != nil {
        log.Fatalf("failed to create group: %v", err)
    }
    log.Printf("created group: %v\n", *group.Name)

    log.Println("starting deployment")
    result, err := createDeployment()
    if err != nil {
        log.Fatalf("Failed to deploy correctly: %v", err)
    }
    log.Printf("Completed deployment: %v", *result.Name)
    getLogin()
}
```

<span data-ttu-id="d3658-165">Kód prochází těmito kroky (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="d3658-165">The steps that the code runs through are, in order:</span></span>

* <span data-ttu-id="d3658-166">Vytvoření skupiny prostředků pro nasazení do (`createGroup()`)</span><span class="sxs-lookup"><span data-stu-id="d3658-166">Create the resource group to deploy to (`createGroup()`)</span></span>
* <span data-ttu-id="d3658-167">Vytvoření nasazení v této skupině (`createDeployment()`)</span><span class="sxs-lookup"><span data-stu-id="d3658-167">Create the deployment within this group (`createDeployment()`)</span></span>
* <span data-ttu-id="d3658-168">Získání a zobrazení přihlašovacích informací pro nasazený virtuální počítač (`getLogin()`)</span><span class="sxs-lookup"><span data-stu-id="d3658-168">Obtain and display login information for the deployed VM (`getLogin()`)</span></span>

### <a name="creating-the-resource-group"></a><span data-ttu-id="d3658-169">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="d3658-169">Creating the resource group</span></span>

<span data-ttu-id="d3658-170">Funkce `createGroup()` vytvoří skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="d3658-170">The `createGroup()` function creates the resource group.</span></span> <span data-ttu-id="d3658-171">Pohled na tok volání a argumenty ukazuje způsob, jakým jsou interakce služeb v sadě SDK strukturované.</span><span class="sxs-lookup"><span data-stu-id="d3658-171">Looking at the call flow and arguments demonstrates the way that service interactions are structured in the SDK.</span></span>

```go
func createGroup() (group resources.Group, err error) {
        groupsClient := resources.NewGroupsClient(config.SubscriptionID)
        groupsClient.Authorizer = autorest.NewBearerAuthorizer(token)

        return groupsClient.CreateOrUpdate(
                ctx,
                resourceGroupName,
                resources.Group{
                        Location: to.StringPtr(resourceGroupLocation)})
}
```

<span data-ttu-id="d3658-172">Obecný tok interakcí se službou Azure vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="d3658-172">The general flow of interacting with an Azure service is:</span></span>

* <span data-ttu-id="d3658-173">Vytvořte klienta pomocí metody `service.NewXClient()`, kde `X` je typ prostředku pro `service`, se kterým chcete interagovat.</span><span class="sxs-lookup"><span data-stu-id="d3658-173">Create the client using the `service.NewXClient()` method, where `X` is the resource type of the `service` that you want to interact with.</span></span> <span data-ttu-id="d3658-174">Tato funkce vždycky použije ID předplatného.</span><span class="sxs-lookup"><span data-stu-id="d3658-174">This function always takes a subscription ID.</span></span>
* <span data-ttu-id="d3658-175">Pro tohoto klienta nastavte metodu autorizace a umožněte mu interagovat se vzdáleným rozhraním API.</span><span class="sxs-lookup"><span data-stu-id="d3658-175">Set the authorization method for the client, allowing it to interact with the remote API.</span></span>
* <span data-ttu-id="d3658-176">Použijte volání metody v klientovi odpovídající vzdálenému rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="d3658-176">Make the method call on the client corresponding to the remote API.</span></span> <span data-ttu-id="d3658-177">Metody klientů služby obvykle používají název prostředku a objekt metadat.</span><span class="sxs-lookup"><span data-stu-id="d3658-177">Service client methods usually take the name of the resource and a metadata object.</span></span>

<span data-ttu-id="d3658-178">Funkce [`to.StringPtr()`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) se tady používá k převodu typů.</span><span class="sxs-lookup"><span data-stu-id="d3658-178">The [`to.StringPtr()`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) function is used to perform a type conversion here.</span></span> <span data-ttu-id="d3658-179">Struktury parametrů pro metody sady SDK téměř výhradně přebírají ukazatele, takže tyto metody se poskytují pro usnadnění převodu typů.</span><span class="sxs-lookup"><span data-stu-id="d3658-179">The parameters structs for methods of the SDK almost exclusively take pointers, so these methods are provided to make the type conversions easy.</span></span> <span data-ttu-id="d3658-180">Úplný seznam a chování převaděčů najdete v dokumentaci k modulu [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to).</span><span class="sxs-lookup"><span data-stu-id="d3658-180">See the documentation for the [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) module for the complete list and behavior of convenience converters.</span></span>

<span data-ttu-id="d3658-181">Operace `groupsClient.CreateOrUpdate()` vrací ukazatel na datovou strukturu představující skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="d3658-181">The `groupsClient.CreateOrUpdate()` operation returns a pointer to a data struct representing the resource group.</span></span> <span data-ttu-id="d3658-182">Přímá návratová hodnota tohoto druhu indikuje krátce běžící operaci, která je zamýšlená jako synchronní.</span><span class="sxs-lookup"><span data-stu-id="d3658-182">A direct return value of this kind indicates a short-running operation that is meant to be synchronous.</span></span> <span data-ttu-id="d3658-183">V další části uvidíte příklad dlouhotrvající operace a způsob interakce s operacemi tohoto typu.</span><span class="sxs-lookup"><span data-stu-id="d3658-183">In the next section, you'll see an example of a long-running operation and how to interact with them.</span></span>

### <a name="performing-the-deployment"></a><span data-ttu-id="d3658-184">Realizace nasazení</span><span class="sxs-lookup"><span data-stu-id="d3658-184">Performing the deployment</span></span>

<span data-ttu-id="d3658-185">Jakmile se vytvoří skupina pro prostředky, je čas spustit nasazení.</span><span class="sxs-lookup"><span data-stu-id="d3658-185">Once the group to contain its resources is created, it's time to run the deployment.</span></span> <span data-ttu-id="d3658-186">Tento kód je rozdělený do menších oddílů, aby se zdůraznily různé části jeho logiky.</span><span class="sxs-lookup"><span data-stu-id="d3658-186">This code is broken up into smaller sections to emphasize different parts of its logic.</span></span>


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

        // ...
```

<span data-ttu-id="d3658-187">Soubory nasazení načte `readJSON`, podrobnosti tady neuvádíme.</span><span class="sxs-lookup"><span data-stu-id="d3658-187">The deployment files are loaded by `readJSON`, the details of which are skipped here.</span></span> <span data-ttu-id="d3658-188">Tato funkce vrátí `*map[string]interface{}`. Tento typ se používá při konstrukci metadat pro volání nasazení prostředků.</span><span class="sxs-lookup"><span data-stu-id="d3658-188">This function returns a `*map[string]interface{}`, the type used in constructing the metadata for the resource deployment call.</span></span>

```go
        // ...
        
        deploymentsClient := resources.NewDeploymentsClient(config.SubscriptionID)
        deploymentsClient.Authorizer = autorest.NewBearerAuthorizer(token)

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
                log.Fatalf("Failed to create deployment: %v", err)
        }
        //...
```

<span data-ttu-id="d3658-189">Tento kód používá stejný vzor jako při vytváření skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="d3658-189">This code follows the same pattern as with creating the resource group.</span></span> <span data-ttu-id="d3658-190">Vytvoří se nový klient, získá možnost ověřovat pomocí Azure a potom se volá metoda.</span><span class="sxs-lookup"><span data-stu-id="d3658-190">A new client is created, given the ability to authenticate with Azure, and then a method is called.</span></span> <span data-ttu-id="d3658-191">Tato metoda má dokonce stejný název (`CreateOrUpdate`) jako odpovídající metoda pro skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="d3658-191">The method even has the same name (`CreateOrUpdate`) as the corresponding method for resource groups.</span></span> <span data-ttu-id="d3658-192">Tento vzor se v sadě SDK používá opakovaně.</span><span class="sxs-lookup"><span data-stu-id="d3658-192">This pattern is seen again and again in the SDK.</span></span> <span data-ttu-id="d3658-193">Metody, které provádějí podobné úkoly, mají obvykle stejný název.</span><span class="sxs-lookup"><span data-stu-id="d3658-193">Methods that perform similar work normally have the same name.</span></span>

<span data-ttu-id="d3658-194">Největší rozdíl spočívá v návratové hodnotě metody `deploymentsClient.CreateOrUpdate()`.</span><span class="sxs-lookup"><span data-stu-id="d3658-194">The biggest difference comes in the return value of the `deploymentsClient.CreateOrUpdate()` method.</span></span> <span data-ttu-id="d3658-195">Tato hodnota je objekt `Future`, který využívá [vzor návrhu konstruktů future](https://en.wikipedia.org/wiki/Futures_and_promises).</span><span class="sxs-lookup"><span data-stu-id="d3658-195">This value is a `Future` object, which follows the [future design pattern](https://en.wikipedia.org/wiki/Futures_and_promises).</span></span> <span data-ttu-id="d3658-196">Konstrukty future představují dlouho běžící operace v Azure, na které je možné se příležitostně dotazovat při provádění jiné práce.</span><span class="sxs-lookup"><span data-stu-id="d3658-196">Futures represent a long-running operation in Azure that you may want to occasionally poll while performing other work.</span></span>

```go
        //...
        err = deploymentFuture.Future.WaitForCompletion(ctx, deploymentsClient.BaseClient.Client)
        if err != nil {
                log.Fatalf("Error while waiting for deployment creation: %v", err)
        }
        return deploymentFuture.Result(deploymentsClient)
}
```

<span data-ttu-id="d3658-197">V tomto příkladu je nejlepší počkat na dokončení operace.</span><span class="sxs-lookup"><span data-stu-id="d3658-197">For this example, the best thing to do is to wait for the operation to complete.</span></span> <span data-ttu-id="d3658-198">Čekání v konstruktu future vyžaduje [kontextový objekt](https://blog.golang.org/context) a klienta, který vytvořil objekt Future.</span><span class="sxs-lookup"><span data-stu-id="d3658-198">Waiting on a future requires both a [context object](https://blog.golang.org/context) and the client that created the Future object.</span></span> <span data-ttu-id="d3658-199">Z toho vyplývají dva možné zdroje chyb: chyba způsobená na straně klienta při pokusu o volání metody a reakce na chybu ze serveru.</span><span class="sxs-lookup"><span data-stu-id="d3658-199">There are two possible error sources here: An error caused on the client side when trying to invoke the method, and an error response from the server.</span></span> <span data-ttu-id="d3658-200">Ta druhá se vrací jako součást volání `deploymentFuture.Result()`.</span><span class="sxs-lookup"><span data-stu-id="d3658-200">The latter is returned as part of the `deploymentFuture.Result()` call.</span></span>

### <a name="obtaining-the-assigned-ip-address"></a><span data-ttu-id="d3658-201">Získání přiřazené IP adresy</span><span class="sxs-lookup"><span data-stu-id="d3658-201">Obtaining the assigned IP address</span></span>

<span data-ttu-id="d3658-202">Abyste mohli s nově vytvořeným virtuálním počítačem něco udělat, potřebujete přiřazenou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="d3658-202">To do anything with the newly created VM, you need the assigned IP address.</span></span> <span data-ttu-id="d3658-203">IP adresy jsou vlastní samostatné prostředky Azure, které jsou vázané na prostředky NIC (Network Interface Controller).</span><span class="sxs-lookup"><span data-stu-id="d3658-203">IP addresses are their own separate Azure resource, bound to Network Interface Controller (NIC) resources.</span></span>

```go
func getLogin() {
        params, err := readJSON(parametersFile)
        if err != nil {
                log.Fatalf("Unable to read parameters. Get login information with `az network public-ip list -g %s", resourceGroupName)
        }

        addressClient := network.NewPublicIPAddressesClient(config.SubscriptionID)
        addressClient.Authorizer = autorest.NewBearerAuthorizer(token)
        ipName := (*params)["publicIPAddresses_QuickstartVM_ip_name"].(map[string]interface{})
        ipAddress, err := addressClient.Get(ctx, resourceGroupName, ipName["value"].(string), "")
        if err != nil {
                log.Fatalf("Unable to get IP information. Try using `az network public-ip list -g %s", resourceGroupName)
        }

        vmUser := (*params)["vm_user"].(map[string]interface{})
        vmPass := (*params)["vm_password"].(map[string]interface{})

        log.Printf("Log in with ssh: %s@%s, password: %s",
                vmUser["value"].(string),
                *ipAddress.PublicIPAddressPropertiesFormat.IPAddress,
                vmPass["value"].(string))
}
```

<span data-ttu-id="d3658-204">Tato metoda využívá informace uložené v souboru parametrů.</span><span class="sxs-lookup"><span data-stu-id="d3658-204">This method relies on information that is stored in the parameters file.</span></span> <span data-ttu-id="d3658-205">Kód by mohl pro zjištění NIC zadat dotaz přímo virtuálnímu počítači, potom zadat dotaz NIC k získání prostředku IP adresy a nakonec zadat dotaz přímo prostředku IP adresy.</span><span class="sxs-lookup"><span data-stu-id="d3658-205">The code could query the VM directly to get its NIC, query the NIC to get its IP resource, and then query the IP resource directly.</span></span> <span data-ttu-id="d3658-206">To je ale dlouhý řetězec závislostí a operací, které je potřeba vyřešit, a proto je nákladný.</span><span class="sxs-lookup"><span data-stu-id="d3658-206">That's a long chain of dependencies and operations to resolve, making it expensive.</span></span> <span data-ttu-id="d3658-207">Vzhledem k tomu, že informace JSON jsou místní, dají se načíst místo toho.</span><span class="sxs-lookup"><span data-stu-id="d3658-207">Since the JSON information is local, it can be loaded instead.</span></span>

<span data-ttu-id="d3658-208">Hodnoty pro heslo a uživatele virtuálního počítače se také načítají z JSON.</span><span class="sxs-lookup"><span data-stu-id="d3658-208">The values for the VM user and password are likewise loaded from the JSON.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d3658-209">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d3658-209">Next steps</span></span>

<span data-ttu-id="d3658-210">V tomto rychlém startu jste vzali existující šablonu a nasadili ji prostřednictvím Go.</span><span class="sxs-lookup"><span data-stu-id="d3658-210">In this quickstart, you took an existing template and deployed it through Go.</span></span> <span data-ttu-id="d3658-211">Potom jste se přes SSH připojili k nově vytvořenému virtuálnímu počítači a ověřili, že je spuštěný.</span><span class="sxs-lookup"><span data-stu-id="d3658-211">Then you connected to the newly created VM via SSH to ensure that it's running.</span></span>

<span data-ttu-id="d3658-212">Pokud se chcete dál seznamovat s použitím virtuálních počítačů v prostředí Azure s Go, prohlédněte si témata s [ukázkami výpočetních funkcí Azure pro Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) a [ukázkami správy prostředků Azure pro Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).</span><span class="sxs-lookup"><span data-stu-id="d3658-212">To continue learning about working with virtual machines in the Azure environment with Go, take a look at the [Azure compute samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) or [Azure resource management samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).</span></span>

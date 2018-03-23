---
title: Nasazení virtuálního počítače Azure z Go
description: Nasazení virtuálního počítače Azure s využitím sady Azure SDK for Go
author: sptramer
ms.author: sttramer
ms.date: 02/08/2018
ms.topic: quickstart
ms.devlang: go
manager: carmonm
ms.openlocfilehash: 46a1243ff2ff6bfcf3831e2cea3137c1f6051c78
ms.sourcegitcommit: fcc1786d59d2e32c97a9a8e0748e06f564a961bd
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/23/2018
---
# <a name="quickstart-deploy-an-azure-virtual-machine-from-a-template-with-the-azure-sdk-for-go"></a><span data-ttu-id="b8873-103">Rychlý start: Nasazení virtuálního počítače Azure ze šablony s využitím sady Azure SDK for Go</span><span class="sxs-lookup"><span data-stu-id="b8873-103">Quickstart: Deploy an Azure virtual machine from a template with the Azure SDK for Go</span></span>

<span data-ttu-id="b8873-104">Tento rychlý start se zaměřuje na nasazení prostředků ze šablony s využitím sady Azure SDK for Go.</span><span class="sxs-lookup"><span data-stu-id="b8873-104">This quickstart focuses on deploying resources from a template with the Azure SDK for Go.</span></span> <span data-ttu-id="b8873-105">Šablony jsou snímky všech prostředků obsažených ve [skupině prostředků Azure](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview).</span><span class="sxs-lookup"><span data-stu-id="b8873-105">Templates are snapshots of all of the resources contained within an [Azure resource group](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview).</span></span> <span data-ttu-id="b8873-106">Při provádění užitečných úloh se průběžně seznámíte s funkcemi a konvencemi této sady SDK.</span><span class="sxs-lookup"><span data-stu-id="b8873-106">Along the way, you'll become familiar with the functionality and conventions of the SDK while performing a useful task.</span></span>

<span data-ttu-id="b8873-107">Na konci tohoto rychlého startu budete mít spuštěný virtuální počítač, ke kterému se přihlásíte pomocí uživatelského jména a hesla.</span><span class="sxs-lookup"><span data-stu-id="b8873-107">At the end of this quickstart, you have a running VM that you log into with a username and password.</span></span>

[!INCLUDE [quickstarts-free-trial-note](includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](includes/cloud-shell-try-it.md)]

<span data-ttu-id="b8873-108">Pokud používáte místní instalaci Azure CLI, bude tento rychlý start vyžadovat CLI verze 2.0.24 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="b8873-108">If you use a local install of the Azure CLI, this quickstart requires CLI version 2.0.24 or later.</span></span> <span data-ttu-id="b8873-109">Spusťte `az --version` a zkontrolujte, jestli vaše instalace CLI splňuje tento požadavek.</span><span class="sxs-lookup"><span data-stu-id="b8873-109">Run `az --version` to make sure your CLI install meets this requirement.</span></span> <span data-ttu-id="b8873-110">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="b8873-110">If you need to install or upgrade, see [Install the Azure CLI 2.0](/cli/azure/install-azure-cli).</span></span>

## <a name="install-the-azure-sdk-for-go"></a><span data-ttu-id="b8873-111">Instalace Azure SDK for Go</span><span class="sxs-lookup"><span data-stu-id="b8873-111">Install the Azure SDK for Go</span></span> 

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

## <a name="create-a-service-principal"></a><span data-ttu-id="b8873-112">Vytvoření instančního objektu</span><span class="sxs-lookup"><span data-stu-id="b8873-112">Create a service principal</span></span>

<span data-ttu-id="b8873-113">Pokud se chcete přihlásit neinteraktivně pomocí aplikace, potřebujete instanční objekt.</span><span class="sxs-lookup"><span data-stu-id="b8873-113">To log in non-interactively with an application, you need a service principal.</span></span> <span data-ttu-id="b8873-114">Instanční objekty jsou součástí řízení přístupu na základě role (RBAC), které vytvoří jedinečnou identitu uživatele.</span><span class="sxs-lookup"><span data-stu-id="b8873-114">Service principals are part of role-based access control (RBAC), which creates a unique user identity.</span></span> <span data-ttu-id="b8873-115">Pokud chcete vytvořit nový instanční objekt s využitím CLI, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="b8873-115">To create a new service principal with the CLI, run the following command:</span></span>

```azurecli-interactive
az ad sp create-for-rbac --name az-go-vm-quickstart
```

<span data-ttu-id="b8873-116">__Nezapomeňte__ ve výstupu zaznamenat hodnoty `appId`, `password` a `tenant`.</span><span class="sxs-lookup"><span data-stu-id="b8873-116">__Make sure__ to record the `appId`, `password`, and `tenant` values in the output.</span></span> <span data-ttu-id="b8873-117">Aplikace tyto hodnoty používá k ověření pomocí Azure.</span><span class="sxs-lookup"><span data-stu-id="b8873-117">These values are used by the application to authenticate with Azure.</span></span>

<span data-ttu-id="b8873-118">Další informace o vytváření a správě instančních objektů s využitím Azure CLI 2.0 najdete v tématu [Vytvoření instančního objektu Azure pomocí Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="b8873-118">For more information on creating and managing service principals with the Azure CLI 2.0, see [Create an Azure service principal with Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli).</span></span>

## <a name="get-the-code"></a><span data-ttu-id="b8873-119">Získání kódu</span><span class="sxs-lookup"><span data-stu-id="b8873-119">Get the code</span></span>

<span data-ttu-id="b8873-120">K získání kódu tohoto rychlého startu a všech jeho závislostí použijte `go get`.</span><span class="sxs-lookup"><span data-stu-id="b8873-120">Get the quickstart code and all of its dependencies with `go get`.</span></span>

```bash
go get -u -d github.com/azure-samples/azure-sdk-for-go-samples/quickstart/deploy-vm/...
```

<span data-ttu-id="b8873-121">Tento kód se zkompiluje, ale nefunguje správně, dokud nezadáte informace o vašem účtu Azure a vytvořeném instančním objektu.</span><span class="sxs-lookup"><span data-stu-id="b8873-121">This code compiles, but doesn't run correctly until you provide it information about your Azure account and the created service principal.</span></span> <span data-ttu-id="b8873-122">V `main.go` je proměnná `config`, která obsahuje strukturu `authInfo`.</span><span class="sxs-lookup"><span data-stu-id="b8873-122">In `main.go` there is a variable, `config`, which contains an `authInfo` struct.</span></span> <span data-ttu-id="b8873-123">V této struktuře je potřeba nahradit hodnoty příslušných polí, aby ověřování fungovalo správně.</span><span class="sxs-lookup"><span data-stu-id="b8873-123">This struct needs to have its field values replaced in order to authenticate correctly.</span></span>

```go
    config = authInfo{ // Your application credentials
        TenantID:               "", // Azure account tenantID
        SubscriptionID:         "", // Azure subscription subscriptionID
        ServicePrincipalID:     "", // Service principal appId
        ServicePrincipalSecret: "", // Service principal password/secret
    }
```

* <span data-ttu-id="b8873-124">`SubscriptionID`: Vaše ID předplatného, které můžete zjistit pomocí příkazu CLI</span><span class="sxs-lookup"><span data-stu-id="b8873-124">`SubscriptionID`: Your subscription ID, which can be obtained from the CLI command</span></span>

  ```azurecli-interactive
  az account show --query id -o tsv
  ```

* <span data-ttu-id="b8873-125">`TenantID`: Vaše ID tenanta, hodnota `tenant` zaznamenaná při vytváření instančního objektu</span><span class="sxs-lookup"><span data-stu-id="b8873-125">`TenantID`: Your tenant ID, the `tenant` value recorded when creating the service principal</span></span>
* <span data-ttu-id="b8873-126">`ServicePrincipalID`: Hodnota `appId` zaznamenaná při vytváření instančního objektu</span><span class="sxs-lookup"><span data-stu-id="b8873-126">`ServicePrincipalID`: The `appId` value recorded when creating the service principal</span></span>
* <span data-ttu-id="b8873-127">`ServicePrincipalSecret`: Hodnota `password` zaznamenaná při vytváření instančního objektu</span><span class="sxs-lookup"><span data-stu-id="b8873-127">`ServicePrincipalSecret`: The `password` value recorded when creating the service principal</span></span>

<span data-ttu-id="b8873-128">Musíte také upravit hodnotu v poli `vm-quickstart-params.json`.</span><span class="sxs-lookup"><span data-stu-id="b8873-128">You also need to edit a value in the `vm-quickstart-params.json` file.</span></span>

```json
    "vm_password": {
        "value": "_"
    }
```

* <span data-ttu-id="b8873-129">`vm_password`: Heslo pro uživatelský účet virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b8873-129">`vm_password`: The password for the VM user account.</span></span> <span data-ttu-id="b8873-130">Musí mít délku 12 až 72 znaků a obsahovat 3 z následujících znaků:</span><span class="sxs-lookup"><span data-stu-id="b8873-130">It must be 12-72 characters in length and contain 3 of the following characters:</span></span>
  * <span data-ttu-id="b8873-131">Malé písmeno</span><span class="sxs-lookup"><span data-stu-id="b8873-131">A lowercase letter</span></span>
  * <span data-ttu-id="b8873-132">Velké písmeno</span><span class="sxs-lookup"><span data-stu-id="b8873-132">An uppercase letter</span></span>
  * <span data-ttu-id="b8873-133">Číslo</span><span class="sxs-lookup"><span data-stu-id="b8873-133">A number</span></span>
  * <span data-ttu-id="b8873-134">Symbol</span><span class="sxs-lookup"><span data-stu-id="b8873-134">A symbol</span></span>

## <a name="running-the-code"></a><span data-ttu-id="b8873-135">Spuštění kódu</span><span class="sxs-lookup"><span data-stu-id="b8873-135">Running the code</span></span>

<span data-ttu-id="b8873-136">ke spuštění tohoto rychlého startu použijte příkaz `go run`.</span><span class="sxs-lookup"><span data-stu-id="b8873-136">Run the quickstart with the `go run` command.</span></span>

```bash
cd $GOPATH/src/github.com/azure-samples/azure-sdk-for-go-samples/quickstart/deploy-vm
go run main.go
```

<span data-ttu-id="b8873-137">Jestliže při nasazení dojde k selhání, dostanete zprávu informující, že nastal problém, ale bez jakýchkoli konkrétních podrobností.</span><span class="sxs-lookup"><span data-stu-id="b8873-137">If there is a failure in the deployment, you get a message indicating that there was an issue, but without any specific details.</span></span> <span data-ttu-id="b8873-138">S využitím Azure CLI můžete podrobné informace o selhání nasazení získat pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="b8873-138">Using the Azure CLI, get the details of the deployment failure with the following command:</span></span>

```azurecli-interactive
az group deployment show -g GoVMQuickstart -n VMDeployQuickstart
```

<span data-ttu-id="b8873-139">Pokud je nasazení úspěšné, zobrazí se zpráva s uvedením uživatelského jména, IP adresy a hesla pro přihlášení na nově vytvořený virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="b8873-139">If the deployment is successful, you see a message giving the username, IP address, and password for logging into the newly created virtual machine.</span></span> <span data-ttu-id="b8873-140">Připojte se k tomuto počítači přes SSH a potvrďte, že je zprovozněný.</span><span class="sxs-lookup"><span data-stu-id="b8873-140">SSH into this machine to confirm that it is up and running.</span></span>

## <a name="cleaning-up"></a><span data-ttu-id="b8873-141">Čištění</span><span class="sxs-lookup"><span data-stu-id="b8873-141">Cleaning up</span></span>

<span data-ttu-id="b8873-142">Prostředky vytvořené v rámci tohoto rychlého startu můžete vyčistit odstraněním skupiny prostředků pomocí CLI.</span><span class="sxs-lookup"><span data-stu-id="b8873-142">Clean up the resources created during this quickstart by deleting the resource group with the CLI.</span></span>

```azurecli-interactive
az group delete -n GoVMQuickstart
```

## <a name="code-in-depth"></a><span data-ttu-id="b8873-143">Kód podrobněji</span><span class="sxs-lookup"><span data-stu-id="b8873-143">Code in depth</span></span>

<span data-ttu-id="b8873-144">To, co kód tohoto rychlého startu dělá, je rozdělené do bloku proměnných a několika malých funkcí, které tady probereme.</span><span class="sxs-lookup"><span data-stu-id="b8873-144">What the quickstart code does is broken down into a block of variables and several small functions, each of which are discussed here.</span></span>

### <a name="variable-assignments-and-structs"></a><span data-ttu-id="b8873-145">Struktury a přiřazení proměnných</span><span class="sxs-lookup"><span data-stu-id="b8873-145">Variable assignments and structs</span></span>

<span data-ttu-id="b8873-146">Vzhledem k tomu, že rychlý start je samostatný, využívá globální proměnné (a ne možnosti příkazového řádku nebo proměnné prostředí).</span><span class="sxs-lookup"><span data-stu-id="b8873-146">Since quickstart is self-contained, it uses global variables rather than command-line options or environment variables.</span></span>

```go
type authInfo struct {
        TenantID               string
        SubscriptionID         string
        ServicePrincipalID     string
        ServicePrincipalSecret string
}
```

<span data-ttu-id="b8873-147">Struktura `authInfo` je deklarovaná k zapouzdření všech informací potřebných pro ověřování s využitím služeb Azure.</span><span class="sxs-lookup"><span data-stu-id="b8873-147">The `authInfo` struct is declared to encapsulate all of the information needed for authorization with Azure services.</span></span>

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

<span data-ttu-id="b8873-148">Jsou deklarované hodnoty, které poskytují názvy vytvořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="b8873-148">Values are declared which give the names of created resources.</span></span> <span data-ttu-id="b8873-149">Je tady také zadané umístění, které můžete změnit, abyste viděli, jak se nasazení chovají v jiných datových centrech.</span><span class="sxs-lookup"><span data-stu-id="b8873-149">The location is also specified here, which you can change to see how deployments behave in other datacenters.</span></span> <span data-ttu-id="b8873-150">Ne každé datové centrum má k dispozici všechny požadované prostředky.</span><span class="sxs-lookup"><span data-stu-id="b8873-150">Not every datacenter has all of the required resources available.</span></span>

<span data-ttu-id="b8873-151">Konstanty `templateFile` a `parametersFile` odkazují na soubory potřebné pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="b8873-151">The `templateFile` and `parametersFile` constants point to the files needed for deployment.</span></span> <span data-ttu-id="b8873-152">Tokenu instančního objektu se věnujeme později a proměnná `ctx` je [kontext Go](https://blog.golang.org/context) pro síťové operace.</span><span class="sxs-lookup"><span data-stu-id="b8873-152">The service principal token is covered later, and the `ctx` variable is a [Go context](https://blog.golang.org/context) for the network operations.</span></span>

### <a name="init-and-authorization"></a><span data-ttu-id="b8873-153">init() a autorizace</span><span class="sxs-lookup"><span data-stu-id="b8873-153">init() and authorization</span></span>

<span data-ttu-id="b8873-154">Metoda `init()` pro kód nastavuje autorizaci.</span><span class="sxs-lookup"><span data-stu-id="b8873-154">The `init()` method for the code sets up authorization.</span></span> <span data-ttu-id="b8873-155">Vzhledem k tomu, že autorizace je předpokladem pro všechno, co je v rychlém startu, má smysl, aby byla součástí inicializace.</span><span class="sxs-lookup"><span data-stu-id="b8873-155">Since authorization is a precondition for everything in the quickstart, it makes sense to have it as part of initialization.</span></span> 

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

<span data-ttu-id="b8873-156">Tento kód dokončuje dva kroky pro autorizaci:</span><span class="sxs-lookup"><span data-stu-id="b8873-156">This code completes two steps for authorization:</span></span>

* <span data-ttu-id="b8873-157">Informace o konfiguraci OAuth pro `TenantID` se načtou z Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="b8873-157">OAuth configuration information for the `TenantID` is retrieved by interfacing with Azure Active Directory.</span></span> <span data-ttu-id="b8873-158">Objekt [`azure.PublicCloud`](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#PublicCloud) obsahuje koncové body použité ve standardní konfiguraci Azure.</span><span class="sxs-lookup"><span data-stu-id="b8873-158">The [`azure.PublicCloud`](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#PublicCloud) object contains endpoints used in the standard Azure configuration.</span></span>
* <span data-ttu-id="b8873-159">Volá se funkce [`adal.NewServicePrincipalToken()`](https://godoc.org/github.com/Azure/go-autorest/autorest/adal#NewServicePrincipalToken).</span><span class="sxs-lookup"><span data-stu-id="b8873-159">The [`adal.NewServicePrincipalToken()`](https://godoc.org/github.com/Azure/go-autorest/autorest/adal#NewServicePrincipalToken) function is called.</span></span> <span data-ttu-id="b8873-160">Tato funkce přebírá informace OAuth spolu s přihlášením instančního objektu a také to, jaký styl správy Azure se používá.</span><span class="sxs-lookup"><span data-stu-id="b8873-160">This function takes the OAuth information along with the service principal login, as well as which style of Azure management is being used.</span></span> <span data-ttu-id="b8873-161">Pokud nemáte konkrétní požadavky a víte, co děláte, tato hodnota by vždycky měla být `.ResourceManagerEndpoint`.</span><span class="sxs-lookup"><span data-stu-id="b8873-161">Unless you have specific requirements and know what you're doing, this value should always be `.ResourceManagerEndpoint`.</span></span>

### <a name="flow-of-operations-in-main"></a><span data-ttu-id="b8873-162">Tok operací v main()</span><span class="sxs-lookup"><span data-stu-id="b8873-162">Flow of operations in main()</span></span>

<span data-ttu-id="b8873-163">Funkce `main()` je jednoduchá – jenom ukazuje tok operací a provádí kontrolu chyb.</span><span class="sxs-lookup"><span data-stu-id="b8873-163">The `main()` function is simple, only indicating the flow of operations and performing error-checking.</span></span>

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

<span data-ttu-id="b8873-164">Kód prochází těmito kroky (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="b8873-164">The steps that the code runs through are, in order:</span></span>

* <span data-ttu-id="b8873-165">Vytvoření skupiny prostředků pro nasazení do (`createGroup()`)</span><span class="sxs-lookup"><span data-stu-id="b8873-165">Create the resource group to deploy to (`createGroup()`)</span></span>
* <span data-ttu-id="b8873-166">Vytvoření nasazení v této skupině (`createDeployment()`)</span><span class="sxs-lookup"><span data-stu-id="b8873-166">Create the deployment within this group (`createDeployment()`)</span></span>
* <span data-ttu-id="b8873-167">Získání a zobrazení přihlašovacích informací pro nasazený virtuální počítač (`getLogin()`)</span><span class="sxs-lookup"><span data-stu-id="b8873-167">Obtain and display login information for the deployed VM (`getLogin()`)</span></span>

### <a name="creating-the-resource-group"></a><span data-ttu-id="b8873-168">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="b8873-168">Creating the resource group</span></span>

<span data-ttu-id="b8873-169">Funkce `createGroup()` vytvoří skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="b8873-169">The `createGroup()` function creates the resource group.</span></span> <span data-ttu-id="b8873-170">Pohled na tok volání a argumenty ukazuje způsob, jakým jsou interakce služeb v sadě SDK strukturované.</span><span class="sxs-lookup"><span data-stu-id="b8873-170">Looking at the call flow and arguments demonstrates the way that service interactions are structured in the SDK.</span></span>

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

<span data-ttu-id="b8873-171">Obecný tok interakcí se službou Azure vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="b8873-171">The general flow of interacting with an Azure service is:</span></span>

* <span data-ttu-id="b8873-172">Vytvořte klienta pomocí metody `service.NewXClient()`, kde `X` je typ prostředku pro `service`, se kterým chcete interagovat.</span><span class="sxs-lookup"><span data-stu-id="b8873-172">Create the client using the `service.NewXClient()` method, where `X` is the resource type of the `service` that you want to interact with.</span></span> <span data-ttu-id="b8873-173">Tato funkce vždycky použije ID předplatného.</span><span class="sxs-lookup"><span data-stu-id="b8873-173">This function always takes a subscription ID.</span></span>
* <span data-ttu-id="b8873-174">Pro tohoto klienta nastavte metodu autorizace a umožněte mu interagovat se vzdáleným rozhraním API.</span><span class="sxs-lookup"><span data-stu-id="b8873-174">Set the authorization method for the client, allowing it to interact with the remote API.</span></span>
* <span data-ttu-id="b8873-175">Použijte volání metody v klientovi odpovídající vzdálenému rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="b8873-175">Make the method call on the client corresponding to the remote API.</span></span> <span data-ttu-id="b8873-176">Metody klientů služby obvykle používají název prostředku a objekt metadat.</span><span class="sxs-lookup"><span data-stu-id="b8873-176">Service client methods usually take the name of the resource and a metadata object.</span></span>

<span data-ttu-id="b8873-177">Funkce [`to.StringPtr()`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) se tady používá k převodu typů.</span><span class="sxs-lookup"><span data-stu-id="b8873-177">The [`to.StringPtr()`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) function is used to perform a type conversion here.</span></span> <span data-ttu-id="b8873-178">Struktury parametrů pro metody sady SDK téměř výhradně přebírají ukazatele, takže tyto metody se poskytují pro usnadnění převodu typů.</span><span class="sxs-lookup"><span data-stu-id="b8873-178">The parameters structs for methods of the SDK almost exclusively take pointers, so these methods are provided to make the type conversions easy.</span></span> <span data-ttu-id="b8873-179">Úplný seznam a chování převaděčů najdete v dokumentaci k modulu [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to).</span><span class="sxs-lookup"><span data-stu-id="b8873-179">See the documentation for the [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) module for the complete list and behavior of convenience converters.</span></span>

<span data-ttu-id="b8873-180">Operace `groupsClient.CreateOrUpdate()` vrací ukazatel na datovou strukturu představující skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="b8873-180">The `groupsClient.CreateOrUpdate()` operation returns a pointer to a data struct representing the resource group.</span></span> <span data-ttu-id="b8873-181">Přímá návratová hodnota tohoto druhu indikuje krátce běžící operaci, která je zamýšlená jako synchronní.</span><span class="sxs-lookup"><span data-stu-id="b8873-181">A direct return value of this kind indicates a short-running operation that is meant to be synchronous.</span></span> <span data-ttu-id="b8873-182">V další části uvidíte příklad dlouhotrvající operace a způsob interakce s operacemi tohoto typu.</span><span class="sxs-lookup"><span data-stu-id="b8873-182">In the next section, you'll see an example of a long-running operation and how to interact with them.</span></span>

### <a name="performing-the-deployment"></a><span data-ttu-id="b8873-183">Realizace nasazení</span><span class="sxs-lookup"><span data-stu-id="b8873-183">Performing the deployment</span></span>

<span data-ttu-id="b8873-184">Jakmile se vytvoří skupina pro prostředky, je čas spustit nasazení.</span><span class="sxs-lookup"><span data-stu-id="b8873-184">Once the group to contain its resources is created, it's time to run the deployment.</span></span> <span data-ttu-id="b8873-185">Tento kód je rozdělený do menších oddílů, aby se zdůraznily různé části jeho logiky.</span><span class="sxs-lookup"><span data-stu-id="b8873-185">This code is broken up into smaller sections to emphasize different parts of its logic.</span></span>


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

<span data-ttu-id="b8873-186">Soubory nasazení načte `readJSON`, podrobnosti tady neuvádíme.</span><span class="sxs-lookup"><span data-stu-id="b8873-186">The deployment files are loaded by `readJSON`, the details of which are skipped here.</span></span> <span data-ttu-id="b8873-187">Tato funkce vrátí `*map[string]interface{}`. Tento typ se používá při konstrukci metadat pro volání nasazení prostředků.</span><span class="sxs-lookup"><span data-stu-id="b8873-187">This function returns a `*map[string]interface{}`, the type used in constructing the metadata for the resource deployment call.</span></span>

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

<span data-ttu-id="b8873-188">Tento kód používá stejný vzor jako při vytváření skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="b8873-188">This code follows the same pattern as with creating the resource group.</span></span> <span data-ttu-id="b8873-189">Vytvoří se nový klient, získá možnost ověřovat pomocí Azure a potom se volá metoda.</span><span class="sxs-lookup"><span data-stu-id="b8873-189">A new client is created, given the ability to authenticate with Azure, and then a method is called.</span></span> <span data-ttu-id="b8873-190">Tato metoda má dokonce stejný název (`CreateOrUpdate`) jako odpovídající metoda pro skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="b8873-190">The method even has the same name (`CreateOrUpdate`) as the corresponding method for resource groups.</span></span> <span data-ttu-id="b8873-191">Tento vzor se v sadě SDK používá opakovaně.</span><span class="sxs-lookup"><span data-stu-id="b8873-191">This pattern is seen again and again in the SDK.</span></span> <span data-ttu-id="b8873-192">Metody, které provádějí podobné úkoly, mají obvykle stejný název.</span><span class="sxs-lookup"><span data-stu-id="b8873-192">Methods that perform similar work normally have the same name.</span></span>

<span data-ttu-id="b8873-193">Největší rozdíl spočívá v návratové hodnotě metody `deploymentsClient.CreateOrUpdate()`.</span><span class="sxs-lookup"><span data-stu-id="b8873-193">The biggest difference comes in the return value of the `deploymentsClient.CreateOrUpdate()` method.</span></span> <span data-ttu-id="b8873-194">Tato hodnota je objekt `Future`, který využívá [vzor návrhu konstruktů future](https://en.wikipedia.org/wiki/Futures_and_promises).</span><span class="sxs-lookup"><span data-stu-id="b8873-194">This value is a `Future` object, which follows the [future design pattern](https://en.wikipedia.org/wiki/Futures_and_promises).</span></span> <span data-ttu-id="b8873-195">Konstrukty future představují dlouho běžící operace v Azure, na které je možné se příležitostně dotazovat při provádění jiné práce.</span><span class="sxs-lookup"><span data-stu-id="b8873-195">Futures represent a long-running operation in Azure that you may want to occasionally poll while performing other work.</span></span>

```go
        //...
        err = deploymentFuture.Future.WaitForCompletion(ctx, deploymentsClient.BaseClient.Client)
        if err != nil {
                log.Fatalf("Error while waiting for deployment creation: %v", err)
        }
        return deploymentFuture.Result(deploymentsClient)
}
```

<span data-ttu-id="b8873-196">V tomto příkladu je nejlepší počkat na dokončení operace.</span><span class="sxs-lookup"><span data-stu-id="b8873-196">For this example, the best thing to do is to wait for the operation to complete.</span></span> <span data-ttu-id="b8873-197">Čekání v konstruktu future vyžaduje [kontextový objekt](https://blog.golang.org/context) a klienta, který vytvořil objekt Future.</span><span class="sxs-lookup"><span data-stu-id="b8873-197">Waiting on a future requires both a [context object](https://blog.golang.org/context) and the client that created the Future object.</span></span> <span data-ttu-id="b8873-198">Z toho vyplývají dva možné zdroje chyb: chyba způsobená na straně klienta při pokusu o volání metody a reakce na chybu ze serveru.</span><span class="sxs-lookup"><span data-stu-id="b8873-198">There are two possible error sources here: An error caused on the client side when trying to invoke the method, and an error response from the server.</span></span> <span data-ttu-id="b8873-199">Ta druhá se vrací jako součást volání `deploymentFuture.Result()`.</span><span class="sxs-lookup"><span data-stu-id="b8873-199">The latter is returned as part of the `deploymentFuture.Result()` call.</span></span>

### <a name="obtaining-the-assigned-ip-address"></a><span data-ttu-id="b8873-200">Získání přiřazené IP adresy</span><span class="sxs-lookup"><span data-stu-id="b8873-200">Obtaining the assigned IP address</span></span>

<span data-ttu-id="b8873-201">Abyste mohli s nově vytvořeným virtuálním počítačem něco udělat, potřebujete přiřazenou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="b8873-201">To do anything with the newly created VM, you need the assigned IP address.</span></span> <span data-ttu-id="b8873-202">IP adresy jsou vlastní samostatné prostředky Azure, které jsou vázané na prostředky NIC (Network Interface Controller).</span><span class="sxs-lookup"><span data-stu-id="b8873-202">IP addresses are their own separate Azure resource, bound to Network Interface Controller (NIC) resources.</span></span>

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

<span data-ttu-id="b8873-203">Tato metoda využívá informace uložené v souboru parametrů.</span><span class="sxs-lookup"><span data-stu-id="b8873-203">This method relies on information that is stored in the parameters file.</span></span> <span data-ttu-id="b8873-204">Kód by mohl pro zjištění NIC zadat dotaz přímo virtuálnímu počítači, potom zadat dotaz NIC k získání prostředku IP adresy a nakonec zadat dotaz přímo prostředku IP adresy.</span><span class="sxs-lookup"><span data-stu-id="b8873-204">The code could query the VM directly to get its NIC, query the NIC to get its IP resource, and then query the IP resource directly.</span></span> <span data-ttu-id="b8873-205">To je ale dlouhý řetězec závislostí a operací, které je potřeba vyřešit, a proto je nákladný.</span><span class="sxs-lookup"><span data-stu-id="b8873-205">That's a long chain of dependencies and operations to resolve, making it expensive.</span></span> <span data-ttu-id="b8873-206">Vzhledem k tomu, že informace JSON jsou místní, dají se načíst místo toho.</span><span class="sxs-lookup"><span data-stu-id="b8873-206">Since the JSON information is local, it can be loaded instead.</span></span>

<span data-ttu-id="b8873-207">Hodnoty pro heslo a uživatele virtuálního počítače se také načítají z JSON.</span><span class="sxs-lookup"><span data-stu-id="b8873-207">The values for the VM user and password are likewise loaded from the JSON.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b8873-208">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b8873-208">Next steps</span></span>

<span data-ttu-id="b8873-209">V tomto rychlém startu jste vzali existující šablonu a nasadili ji prostřednictvím Go.</span><span class="sxs-lookup"><span data-stu-id="b8873-209">In this quickstart, you took an existing template and deployed it through Go.</span></span> <span data-ttu-id="b8873-210">Potom jste se přes SSH připojili k nově vytvořenému virtuálnímu počítači a ověřili, že je spuštěný.</span><span class="sxs-lookup"><span data-stu-id="b8873-210">Then you connected to the newly created VM via SSH to ensure that it's running.</span></span>

<span data-ttu-id="b8873-211">Pokud se chcete dál seznamovat s použitím virtuálních počítačů v prostředí Azure s Go, prohlédněte si témata s [ukázkami výpočetních funkcí Azure pro Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) a [ukázkami správy prostředků Azure pro Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).</span><span class="sxs-lookup"><span data-stu-id="b8873-211">To continue learning about working with virtual machines in the Azure environment with Go, take a look at the [Azure compute samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) or [Azure resource management samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).</span></span>

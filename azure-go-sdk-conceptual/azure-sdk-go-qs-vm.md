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
# <a name="quickstart-deploy-an-azure-virtual-machine-from-a-template-with-the-azure-sdk-for-go"></a>Rychlý start: Nasazení virtuálního počítače Azure ze šablony s využitím sady Azure SDK for Go

Tento rychlý start se zaměřuje na nasazení prostředků ze šablony s využitím sady Azure SDK for Go. Šablony jsou snímky všech prostředků obsažených ve [skupině prostředků Azure](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview). Při provádění užitečných úloh se průběžně seznámíte s funkcemi a konvencemi této sady SDK.

Na konci tohoto rychlého startu budete mít spuštěný virtuální počítač, ke kterému se přihlásíte pomocí uživatelského jména a hesla.

[!INCLUDE [quickstarts-free-trial-note](includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](includes/cloud-shell-try-it.md)]

Pokud používáte místní instalaci Azure CLI, bude tento rychlý start vyžadovat CLI verze 2.0.24 nebo novější. Spusťte `az --version` a zkontrolujte, jestli vaše instalace CLI splňuje tento požadavek. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0](/cli/azure/install-azure-cli).

## <a name="install-the-azure-sdk-for-go"></a>Instalace Azure SDK for Go 

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

## <a name="create-a-service-principal"></a>Vytvoření instančního objektu

Pokud se chcete přihlásit neinteraktivně pomocí aplikace, potřebujete instanční objekt. Instanční objekty jsou součástí ověřování na základě rolí (RBAC), které vytvoří jedinečnou identitu uživatele. Pokud chcete vytvořit nový instanční objekt s využitím CLI, spusťte následující příkaz:

```azurecli-interactive
az ad sp create-for-rbac --name az-go-vm-quickstart
```

__Nezapomeňte__ ve výstupu zaznamenat hodnoty `appId`, `password` a `tenant`. Aplikace tyto hodnoty používá k ověření pomocí Azure.

Další informace o vytváření a správě instančních objektů s využitím Azure CLI 2.0 najdete v tématu [Vytvoření instančního objektu Azure pomocí Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli).

## <a name="get-the-code"></a>Získání kódu

K získání kódu tohoto rychlého startu a všech jeho závislostí použijte `go get`.

```bash
go get -u -d github.com/azure-samples/azure-sdk-for-go-samples/quickstart/deploy-vm/...
```

Tento kód se zkompiluje, ale nefunguje správně, dokud nezadáte informace o vašem účtu Azure a vytvořeném instančním objektu. V `main.go` je proměnná `config`, která obsahuje strukturu `authInfo`. V této struktuře je potřeba nahradit hodnoty příslušných polí, aby ověřování fungovalo správně.

```go
    config = authInfo{ // Your application credentials
        TenantID:               "", // Azure account tenantID
        SubscriptionID:         "", // Azure subscription subscriptionID
        ServicePrincipalID:     "", // Service principal appId
        ServicePrincipalSecret: "", // Service principal password/secret
    }
```

* `SubscriptionID`: Vaše ID předplatného, které můžete zjistit pomocí příkazu CLI

  ```azurecli-interactive
  az account show --query id -o tsv
  ```

* `TenantID`: Vaše ID tenanta, hodnota `tenant` zaznamenaná při vytváření instančního objektu
* `ServicePrincipalID`: Hodnota `appId` zaznamenaná při vytváření instančního objektu
* `ServicePrincipalSecret`: Hodnota `password` zaznamenaná při vytváření instančního objektu

Musíte také upravit hodnotu v poli `vm-quickstart-params.json`.

```json
    "vm_password": {
        "value": "_"
    }
```

* `vm_password`: Heslo pro uživatelský účet virtuálního počítače. Musí mít délku 6 až 72 znaků a obsahovat 3 z následujících znaků:
  * Malé písmeno
  * Velké písmeno
  * Číslo
  * Symbol

## <a name="running-the-code"></a>Spuštění kódu

ke spuštění tohoto rychlého startu použijte příkaz `go run`.

```bash
cd $GOPATH/src/github.com/azure-samples/azure-sdk-for-go-samples/quickstart/deploy-vm
go run main.go
```

Jestliže při nasazení dojde k selhání, dostanete zprávu informující, že nastal problém, ale bez jakýchkoli konkrétních podrobností. S využitím Azure CLI můžete podrobné informace o selhání nasazení získat pomocí následujícího příkazu:

```azurecli-interactive
az group deployment show -g GoVMQuickstart -n VMDeployQuickstart
```

Pokud je nasazení úspěšné, zobrazí se zpráva s uvedením uživatelského jména, IP adresy a hesla pro přihlášení na nově vytvořený virtuální počítač. Připojte se k tomuto počítači přes SSH a potvrďte, že je zprovozněný.

## <a name="cleaning-up"></a>Čištění

Prostředky vytvořené v rámci tohoto rychlého startu můžete vyčistit odstraněním skupiny prostředků pomocí CLI.

```azurecli-interactive
az group delete -n GoVMQuickstart
```

## <a name="code-in-depth"></a>Kód podrobněji

To, co kód tohoto rychlého startu dělá, je rozdělené do bloku proměnných a několika malých funkcí, které tady probereme.

### <a name="variable-assignments-and-structs"></a>Struktury a přiřazení proměnných

Vzhledem k tomu, že rychlý start je samostatný, využívá globální proměnné (a ne možnosti příkazového řádku nebo proměnné prostředí).

```go
type authInfo struct {
        TenantID               string
        SubscriptionID         string
        ServicePrincipalID     string
        ServicePrincipalSecret string
}
```

Struktura `authInfo` je deklarovaná k zapouzdření všech informací potřebných pro ověřování s využitím služeb Azure.

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

Jsou deklarované hodnoty, které poskytují názvy vytvořených prostředků. Je tady také zadané umístění, které můžete změnit, abyste viděli, jak se nasazení chovají v jiných datových centrech. Ne každé datové centrum má k dispozici všechny požadované prostředky.

Konstanty `templateFile` a `parametersFile` odkazují na soubory potřebné pro nasazení. Tokenu instančního objektu se věnujeme později a proměnná `ctx` je [kontext Go](https://blog.golang.org/context) pro síťové operace.

### <a name="init-and-authorization"></a>init() a autorizace

Metoda `init()` pro kód nastavuje autorizaci. Vzhledem k tomu, že autorizace je předpokladem pro všechno, co je v rychlém startu, má smysl, aby byla součástí inicializace. 

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

Tento kód dokončuje dva kroky pro autorizaci:

* Informace o konfiguraci OAuth pro `TenantID` se načtou z Azure Active Directory. Objekt [`azure.PublicCloud`](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#PublicCloud) obsahuje koncové body použité ve standardní konfiguraci Azure.
* Volá se funkce [`adal.NewServicePrincipalToken()`](https://godoc.org/github.com/Azure/go-autorest/autorest/adal#NewServicePrincipalToken). Tato funkce přebírá informace OAuth spolu s přihlášením instančního objektu a také to, jaký styl správy Azure se používá. Pokud nemáte konkrétní požadavky a víte, co děláte, tato hodnota by vždycky měla být `.ResourceManagerEndpoint`.

### <a name="flow-of-operations-in-main"></a>Tok operací v main()

Funkce `main()` je jednoduchá – jenom ukazuje tok operací a provádí kontrolu chyb.

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

Kód prochází těmito kroky (v uvedeném pořadí):

* Vytvoření skupiny prostředků pro nasazení do (`createGroup()`)
* Vytvoření nasazení v této skupině (`createDeployment()`)
* Získání a zobrazení přihlašovacích informací pro nasazený virtuální počítač (`getLogin()`)

### <a name="creating-the-resource-group"></a>Vytvoření skupiny prostředků

Funkce `createGroup()` vytvoří skupinu prostředků. Pohled na tok volání a argumenty ukazuje způsob, jakým jsou interakce služeb v sadě SDK strukturované.

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

Obecný tok interakcí se službou Azure vypadá takto:

* Vytvořte klienta pomocí metody `service.NewXClient()`, kde `X` je typ prostředku pro `service`, se kterým chcete interagovat. Tato funkce vždycky použije ID předplatného.
* Pro tohoto klienta nastavte metodu autorizace a umožněte mu interagovat se vzdáleným rozhraním API.
* Použijte volání metody v klientovi odpovídající vzdálenému rozhraní API. Metody klientů služby obvykle používají název prostředku a objekt metadat.

Funkce [`to.StringPtr()`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) se tady používá k převodu typů. Struktury parametrů pro metody sady SDK téměř výhradně přebírají ukazatele, takže tyto metody se poskytují pro usnadnění převodu typů. Úplný seznam a chování převaděčů najdete v dokumentaci k modulu [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to).

Operace `groupsClient.CreateOrUpdate()` vrací ukazatel na datovou strukturu představující skupinu prostředků. Přímá návratová hodnota tohoto druhu indikuje krátce běžící operaci, která je zamýšlená jako synchronní. V další části uvidíte příklad dlouhotrvající operace a způsob interakce s operacemi tohoto typu.

### <a name="performing-the-deployment"></a>Realizace nasazení

Jakmile se vytvoří skupina pro prostředky, je čas spustit nasazení. Tento kód je rozdělený do menších oddílů, aby se zdůraznily různé části jeho logiky.


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

Soubory nasazení načte `readJSON`, podrobnosti tady neuvádíme. Tato funkce vrátí `*map[string]interface{}`. Tento typ se používá při konstrukci metadat pro volání nasazení prostředků.

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

Tento kód používá stejný vzor jako při vytváření skupiny prostředků. Vytvoří se nový klient, získá možnost ověřovat pomocí Azure a potom se volá metoda. Tato metoda má dokonce stejný název (`CreateOrUpdate`) jako odpovídající metoda pro skupiny prostředků. Tento vzor se v sadě SDK používá opakovaně. Metody, které provádějí podobné úkoly, mají obvykle stejný název.

Největší rozdíl spočívá v návratové hodnotě metody `deploymentsClient.CreateOrUpdate()`. Tato hodnota je objekt `Future`, který využívá [vzor návrhu konstruktů future](https://en.wikipedia.org/wiki/Futures_and_promises). Konstrukty future představují dlouho běžící operace v Azure, na které je možné se příležitostně dotazovat při provádění jiné práce.

```go
        //...
        err = deploymentFuture.Future.WaitForCompletion(ctx, deploymentsClient.BaseClient.Client)
        if err != nil {
                log.Fatalf("Error while waiting for deployment creation: %v", err)
        }
        return deploymentFuture.Result(deploymentsClient)
}
```

V tomto příkladu je nejlepší počkat na dokončení operace. Čekání v konstruktu future vyžaduje [kontextový objekt](https://blog.golang.org/context) a klienta, který vytvořil objekt Future. Z toho vyplývají dva možné zdroje chyb: chyba způsobená na straně klienta při pokusu o volání metody a reakce na chybu ze serveru. Ta druhá se vrací jako součást volání `deploymentFuture.Result()`.

### <a name="obtaining-the-assigned-ip-address"></a>Získání přiřazené IP adresy

Abyste mohli s nově vytvořeným virtuálním počítačem něco udělat, potřebujete přiřazenou IP adresu. IP adresy jsou vlastní samostatné prostředky Azure, které jsou vázané na prostředky NIC (Network Interface Controller).

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

Tato metoda využívá informace uložené v souboru parametrů. Kód by mohl pro zjištění NIC zadat dotaz přímo virtuálnímu počítači, potom zadat dotaz NIC k získání prostředku IP adresy a nakonec zadat dotaz přímo prostředku IP adresy. To je ale dlouhý řetězec závislostí a operací, které je potřeba vyřešit, a proto je nákladný. Vzhledem k tomu, že informace JSON jsou místní, dají se načíst místo toho.

Hodnoty pro heslo a uživatele virtuálního počítače se také načítají z JSON.

## <a name="next-steps"></a>Další kroky

V tomto rychlém startu jste vzali existující šablonu a nasadili ji prostřednictvím Go. Potom jste se přes SSH připojili k nově vytvořenému virtuálnímu počítači a ověřili, že je spuštěný.

Pokud se chcete dál seznamovat s použitím virtuálních počítačů v prostředí Azure s Go, prohlédněte si témata s [ukázkami výpočetních funkcí Azure pro Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) a [ukázkami správy prostředků Azure pro Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).

---
title: Nasazení virtuálního počítače Azure z Go
description: Nasazení virtuálního počítače Azure s využitím sady Azure SDK for Go
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 04/03/2018
ms.topic: quickstart
ms.prod: azure
ms.technology: azure-sdk-go
ms.service: virtual-machines
ms.devlang: go
ms.openlocfilehash: 1fbcc54df2a2aebce56c5a5800361f3d3aed1ccc
ms.sourcegitcommit: f08abf902b48f8173aa6e261084ff2cfc9043305
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/03/2018
ms.locfileid: "32319930"
---
# <a name="quickstart-deploy-an-azure-virtual-machine-from-a-template-with-the-azure-sdk-for-go"></a>Rychlý start: Nasazení virtuálního počítače Azure ze šablony s využitím sady Azure SDK for Go

Tento rychlý start se zaměřuje na nasazení prostředků ze šablony s využitím sady Azure SDK for Go. Šablony jsou snímky všech prostředků obsažených ve [skupině prostředků Azure](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview). Při provádění užitečných úloh se průběžně seznámíte s funkcemi a konvencemi této sady SDK.

Na konci tohoto rychlého startu budete mít spuštěný virtuální počítač, ke kterému se přihlásíte pomocí uživatelského jména a hesla.

[!INCLUDE [quickstarts-free-trial-note](includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](includes/cloud-shell-try-it.md)]

Pokud používáte místní instalaci Azure CLI, bude tento rychlý start vyžadovat CLI verze __2.0.28__ nebo novější. Spusťte `az --version` a zkontrolujte, jestli vaše instalace CLI splňuje tento požadavek. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0](/cli/azure/install-azure-cli).

## <a name="install-the-azure-sdk-for-go"></a>Instalace Azure SDK for Go 

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

## <a name="create-a-service-principal"></a>Vytvoření instančního objektu


Pokud se chcete přihlásit neinteraktivně pomocí aplikace, potřebujete instanční objekt. Instanční objekty jsou součástí řízení přístupu na základě role (RBAC), které vytvoří jedinečnou identitu uživatele. Pokud chcete vytvořit nový instanční objekt s využitím CLI, spusťte následující příkaz:

```azurecli-interactive
az ad sp create-for-rbac --name az-go-vm-quickstart --sdk-auth > quickstart.auth
```

Nastavte proměnnou prostředí `AZURE_AUTH_LOCATION` na úplnou cestu k tomuto souboru. Sada SDK pak vyhledá a načte přihlašovací údaje přímo z tohoto souboru bez nutnosti provádět změny nebo zaznamenávat informace z instančního objektu.

## <a name="get-the-code"></a>Získání kódu

K získání kódu tohoto rychlého startu a všech jeho závislostí použijte `go get`.

```bash
go get -u -d github.com/azure-samples/azure-sdk-for-go-samples/quickstarts/deploy-vm/...
```

Pokud je proměnná `AZURE_AUTH_LOCATION` správně nastavená, nemusíte provádět žádné úpravy zdrojového kódu. Když se program spustí, načte všechny potřebné ověřovací údaje z této proměnné.

## <a name="running-the-code"></a>Spuštění kódu

ke spuštění tohoto rychlého startu použijte příkaz `go run`.

```bash
cd $GOPATH/src/github.com/azure-samples/azure-sdk-for-go-samples/quickstarts/deploy-vm
go run main.go
```

Pokud při nasazení dojde k selhání, zobrazí se zpráva informující, že došlo k problému. Zpráva však nemusí obsahovat dostatek podrobností. S využitím Azure CLI můžete úplné podrobnosti o selhání nasazení získat pomocí následujícího příkazu:

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

### <a name="variables-constants-and-types"></a>Proměnné, konstanty a typy

Vzhledem k tomu, že je tento rychlý start samostatný, využívá globální konstanty a proměnné.

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

Jsou deklarované hodnoty, které poskytují názvy vytvořených prostředků. Je tady také zadané umístění, které můžete změnit, abyste viděli, jak se nasazení chovají v jiných datových centrech. Ne každé datové centrum má k dispozici všechny požadované prostředky.

Typ `clientInfo` se deklaruje za účelem zapouzdření veškerých informací, které je potřeba nezávisle načíst z ověřovacího souboru kvůli nastavení klientů v sadě SDK a nastavení hesla virtuálního počítače.

Konstanty `templateFile` a `parametersFile` odkazují na soubory potřebné pro nasazení. Objekt `authorizer` nakonfiguruje sada Go SDK pro ověřování a proměnná `ctx` představuje [kontext Go](https://blog.golang.org/context) pro síťové operace.

### <a name="authentication-and-initialization"></a>Ověřování a inicializace

Funkce `init` nastaví ověřování. Vzhledem k tomu, že ověřování je předpokladem pro všechno, co je v rychlém startu, má smysl, aby bylo součástí inicializace. Z ověřovacího souboru se také načtou některé informace potřebné ke konfiguraci klientů a virtuálního počítače.

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

Nejprve se zavolá metoda [auth.NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile), která načte ověřovací údaje ze souboru v umístění `AZURE_AUTH_LOCATION`. Tento soubor se pak ručně načte funkcí `readJSON` (tady jsme ji vynechali) a přetáhnou se dvě hodnoty potřebné ke spuštění zbytku programu: ID předplatného klienta a tajný klíč instančního objektu, který se použije také jako heslo virtuálního počítače.

> [!WARNING]
> Pro zjednodušení tohoto rychlého startu se znovu použije heslo instančního objektu. V produkčním prostředí dbejte na to, abyste __nikdy__ znovu nepoužívali heslo poskytující přístup k vašim prostředkům Azure.

### <a name="flow-of-operations-in-main"></a>Tok operací v main()

Funkce `main` je jednoduchá – jenom ukazuje tok operací a provádí kontrolu chyb.

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

Kód prochází těmito kroky (v uvedeném pořadí):

* Vytvoření skupiny prostředků pro nasazení do (`createGroup`)
* Vytvoření nasazení v této skupině (`createDeployment`)
* Získání a zobrazení přihlašovacích informací pro nasazený virtuální počítač (`getLogin`)

### <a name="creating-the-resource-group"></a>Vytvoření skupiny prostředků

Funkce `createGroup` vytvoří skupinu prostředků. Pohled na tok volání a argumenty ukazuje způsob, jakým jsou interakce služeb v sadě SDK strukturované.

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

Obecný tok interakcí se službou Azure vypadá takto:

* Vytvořte klienta pomocí metody `service.New*Client()`, kde `*` je typ prostředku pro `service`, se kterým chcete interagovat. Tato funkce vždycky použije ID předplatného.
* Pro tohoto klienta nastavte metodu autorizace a umožněte mu interagovat se vzdáleným rozhraním API.
* Použijte volání metody v klientovi odpovídající vzdálenému rozhraní API. Metody klientů služby obvykle používají název prostředku a objekt metadat.

Funkce [`to.StringPtr`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) se tady používá k převodu typů. Parametry pro metody sady SDK téměř výhradně přebírají ukazatele, takže jsou pro usnadnění převodu typů k dispozici pomocné metody. Úplný seznam a chování pomocných převaděčů najdete v dokumentaci k modulu [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to).

Metoda `groupsClient.CreateOrUpdate` vrací ukazatel na datový typ představující skupinu prostředků. Přímá návratová hodnota tohoto druhu indikuje krátce běžící operaci, která je zamýšlená jako synchronní. V další části uvidíte příklad dlouhotrvající operace a způsob práce s ní.

### <a name="performing-the-deployment"></a>Realizace nasazení

Jakmile se vytvoří skupina prostředků, je čas spustit nasazení. Tento kód je rozdělený do menších oddílů, aby se zdůraznily různé části jeho logiky.

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

Soubory nasazení načte `readJSON`, podrobnosti tady neuvádíme. Tato funkce vrátí `*map[string]interface{}`. Tento typ se používá při konstrukci metadat pro volání nasazení prostředků. Také se ručně nastaví heslo virtuálního počítače pro parametry nasazení.

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

Tento kód používá stejný vzor jako vytváření skupiny prostředků. Vytvoří se nový klient, získá možnost ověřovat pomocí Azure a potom se volá metoda. Tato metoda má dokonce stejný název (`CreateOrUpdate`) jako odpovídající metoda pro skupiny prostředků. Tento vzor se používá v celé sadě SDK. Metody, které provádějí podobné úkoly, mají obvykle stejný název.

Největší rozdíl spočívá v návratové hodnotě metody `deploymentsClient.CreateOrUpdate`. Tato hodnota je typu [Future](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#Future), který využívá [vzor návrhu konstruktů future](https://en.wikipedia.org/wiki/Futures_and_promises). Konstrukty future představují dlouhotrvající operace v Azure, které můžete po dokončení dotazovat, zrušit nebo zablokovat.

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

V tomto příkladu je nejlepší počkat na dokončení operace. Čekání v konstruktu future vyžaduje [kontextový objekt](https://blog.golang.org/context) a klienta, který vytvořil objekt `Future`. Z toho vyplývají dva možné zdroje chyb: chyba způsobená na straně klienta při pokusu o volání metody a reakce na chybu ze serveru. Ta druhá se vrací jako součást volání `deploymentFuture.Result`.

Pro případné chyby, kdy informace o nasazení můžou být po načtení prázdné, existuje alternativní řešení, a to ruční zavolání metody `deploymentsClient.Get`, která zajistí doplnění dat.

### <a name="obtaining-the-assigned-ip-address"></a>Získání přiřazené IP adresy

Abyste mohli s nově vytvořeným virtuálním počítačem něco udělat, potřebujete přiřazenou IP adresu. IP adresy jsou vlastní samostatné prostředky Azure, které jsou vázané na prostředky NIC (Network Interface Controller).

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

Tato metoda využívá informace uložené v souboru parametrů. Kód by mohl pro zjištění NIC zadat dotaz přímo virtuálnímu počítači, potom zadat dotaz NIC k získání prostředku IP adresy a nakonec zadat dotaz přímo prostředku IP adresy. To je ale dlouhý řetězec závislostí a operací, které je potřeba vyřešit, a proto je nákladný. Vzhledem k tomu, že informace JSON jsou místní, dají se načíst místo toho.

Z JSON se také načte hodnota pro uživatele virtuálního počítače. Heslo virtuálního počítače se načetlo dříve z ověřovacího souboru.

## <a name="next-steps"></a>Další kroky

V tomto rychlém startu jste vzali existující šablonu a nasadili ji prostřednictvím Go. Potom jste se přes SSH připojili k nově vytvořenému virtuálnímu počítači a ověřili, že je spuštěný.

Pokud se chcete dál seznamovat s použitím virtuálních počítačů v prostředí Azure s Go, prohlédněte si témata s [ukázkami výpočetních funkcí Azure pro Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) a [ukázkami správy prostředků Azure pro Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).

Další informace o dostupných metodách ověřování v sadě SDK a typech ověřování, které podporují, najdete v tématu [Ověřování pomocí sady Azure SDK for Go](azure-sdk-go-authorization.md).

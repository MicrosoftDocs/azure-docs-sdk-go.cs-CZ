---
title: Ověřování pomocí sady Azure SDK for Go
description: Seznamte se s metodami ověřování dostupnými v sadě Azure SDK for Go a jejich použitím.
services: azure
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 04/03/2018
ms.topic: conceptual
ms.prod: azure
ms.technology: azure-sdk-go
ms.devlang: go
ms.service: active-directory
ms.component: authentication
ms.openlocfilehash: c7970167070bdf1f3fc75692f3e34268801c65df
ms.sourcegitcommit: 181d4e0b164cf39b3feac346f559596bd19c94db
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2018
ms.locfileid: "38066995"
---
# <a name="authentication-methods-in-the-azure-sdk-for-go"></a>Metody ověřování v sadě Azure SDK for Go

Sada Azure SDK for Go nabízí různé typy a metody ověřování, které může vaše aplikace použít. Podporuje se řada metod ověřování, od načítání informací z proměnných prostředí až po interaktivní webové ověřování. Tento článek vás seznámí s dostupnými typy ověřování v sadě SDK a metodami jejich použití. Naučíte se také osvědčené postupy pro výběr správného typu ověřování pro vaši aplikaci.

## <a name="available-authentication-types-and-methods"></a>Dostupné typy a metody ověřování

Sada Azure SDK for Go nabízí několik různých typů ověřování s využitím různých sad přihlašovacích údajů. Jednotlivé typy ověřování jsou k dispozici prostřednictvím různých metod ověřování, které představují způsob, jakým sada SDK přijímá přihlašovací údaje jako vstup. Následující tabulka obsahuje popis dostupných typů ověřování a situací, ve kterých se doporučuje jejich použití pro vaši aplikaci.

| Typ ověřování | Doporučuje se pro situaci |
|---------------------|---------------------|
| Ověřování pomocí certifikátů | Máte certifikát X509 nakonfigurovaný pro uživatele nebo instanční objekt Azure Active Directory (AAD). Další informace najdete v tématu [Začínáme s ověřováním pomocí certifikátů v Azure Active Directory]. |
| Přihlašovací údaje klienta | Máte nakonfigurovaný instanční objekt, který je nastavený pro tuto aplikaci nebo třídu aplikací, do které patří. Další informace najdete v tématu [Vytvoření instančního objektu pomocí Azure CLI 2.0]. |
| Identita spravované služby | Vaše aplikace se spouští v prostředku Azure nakonfigurovaném s použitím identity spravované služby. Další informace najdete v tématu [Identita spravované služby pro prostředky Azure]. |
| Token zařízení | Vaše aplikace je určená __pouze__ k interaktivnímu používání a bude mít řadu různých uživatelů, kteří můžou pocházet z několika tenantů AAD. Uživatelé mají přístup k webovému prohlížeči, přes který se můžou přihlásit. Další informace najdete v tématu [Použití ověřování pomocí tokenu zařízení](#use-device-token-authentication).|
| Uživatelské jméno a heslo | Máte interaktivní aplikaci, která neumožňuje použití žádné jiné metody ověřování. Vaši uživatelé nemají povolené vícefaktorové ověřování pro přihlášení k AAD. |

> [!IMPORTANT]
> Pokud používáte jiný typ ověřování než přihlašovací údaje klienta, vaše aplikace musí být zaregistrovaná v Azure Active Directory. Postup najdete v tématu [Integrace aplikací s Azure Active Directory](/azure/active-directory/develop/active-directory-integrating-applications).

> [!NOTE]
> Pokud nemáte speciální požadavky, vyhněte se ověřování pomocí uživatelského jména a hesla. V situacích, kdy je vhodné použít přihlašování jednotlivých uživatelů, je obvykle možné místo toho použít ověřování pomocí tokenu zařízení.

[Začínáme s ověřováním pomocí certifikátů v Azure Active Directory]: /azure/active-directory/active-directory-certificate-based-authentication-get-started
[Vytvoření instančního objektu pomocí Azure CLI 2.0]: /cli/azure/create-an-azure-service-principal-azure-cli
[Identita spravované služby pro prostředky Azure]: /azure/active-directory/managed-service-identity/overview

Tyto typy ověřování jsou dostupné prostřednictvím různých metod. [_Ověřování na základě prostředí_](#use-environment-based-authentication) čte přihlašovací údaje přímo z prostředí programu. [_Ověřování na základě souboru_](#use-file-based-authentication) načítá soubor obsahující přihlašovací údaje instančního objektu. [_Ověřování na základě klienta_](#use-an-authentication-client) používá objekt v kódu Go a přenáší na vás zodpovědnost za zadání přihlašovacích údajů během provádění programu. A konečně, [_ověřování pomocí tokenu zařízení_](#use-device-token-authentication) vyžaduje interaktivní přihlašování uživatelů prostřednictvím webového prohlížeče pomocí tokenu a není možné ho používat s ověřováním na základě prostředí ani souboru.

Všechny funkce a typy ověřování jsou k dispozici v balíčku `github.com/Azure/go-autorest/autorest/azure/auth`.

> [!NOTE]
> Pokud nemáte speciální požadavky, vyhněte se ověřování na základě klienta. Tato metoda ověřování podporuje špatné postupy. Při použití ověřování na základě klienta je zejména lákavé přihlašovací údaje pevně zakódovat. Psaní vlastního kódu pro účely ověřování také může vést k selhání u budoucích vydaných verzí sady SDK, pokud se změní požadavky na ověřování.

## <a name="use-environment-based-authentication"></a>Použití ověřování na základě prostředí

Pokud svou aplikaci spouštíte v přísně kontrolovaném prostředí, například v kontejneru, ověřování na základě prostředí je přirozenou volbou. Před spuštěním aplikace nakonfigurujete příkazové prostředí a sada Go SDK za běhu načte tyto proměnné prostředí a provede ověření v Azure. 

Ověřování na základě prostředí podporuje všechny metody ověřování s výjimkou tokenů zařízení a vyhodnocuje je v následujícím pořadí: přihlašovací údaje klienta, certifikáty, uživatelské jméno a heslo a identita spravované služby. Pokud požadovaná proměnná prostředí není nastavená nebo pokud sada SDK obdrží z ověřovací služby odmítnutí, vyzkouší se další typ ověřování. Pokud sada SDK nedokáže provést ověření z prostředí, vrátí chybu.

Následující tabulka obsahuje podrobnosti o proměnných prostředí, které je potřeba nastavit pro jednotlivé typy ověřování podporované ověřováním na základě prostředí.

| Typ ověřování | Proměnná prostředí | Popis |
| ------------------- | -------------------- | ----------- |
| __Přihlašovací údaje klienta__ | `AZURE_TENANT_ID` | ID tenanta Active Directory, do kterého instanční objekt patří. |
| | `AZURE_CLIENT_ID` | Název nebo ID instančního objektu. |
| | `AZURE_CLIENT_SECRET` | Tajný klíč přidružený k instančnímu objektu. |
| __Certifikát__ | `AZURE_TENANT_ID` | ID tenanta Active Directory, ve kterém je certifikát zaregistrovaný. |
| | `AZURE_CLIENT_ID` | ID klienta aplikace přidruženého k certifikátu. |
| | `AZURE_CERTIFICATE_PATH` | Cesta k souboru klientského certifikátu. |
| | `AZURE_CERTIFICATE_PASSWORD` | Heslo pro klientský certifikát. |
| __Uživatelské jméno a heslo__ | `AZURE_TENANT_ID` | ID tenanta Active Directory, do kterého uživatel patří. |
| | `AZURE_CLIENT_ID` | ID klienta aplikace. |
| | `AZURE_USERNAME` | Uživatelské jméno pro přihlášení. |
| | `AZURE_PASSWORD` | Heslo pro přihlášení. |
| __Identita spravované služby__ | | Identita spravované služby nevyžaduje nastavení žádných přihlašovacích údajů. Aplikace musí být spuštěná v prostředku Azure s nakonfigurovaným používáním identity spravované služby. Podrobnosti najdete v tématu [Identita spravované služby pro prostředky Azure]. |

Pokud se potřebujete připojit k jinému cloudu nebo koncovému bodu správy, než je výchozí veřejný cloud Azure, můžete nastavit také následující proměnné prostředí. Mezi nejběžnější důvody jejich nastavení patří používání služby Azure Stack, cloudu v jiné geografické oblasti nebo modelu nasazení Azure Classic.

| Proměnná prostředí | Popis  |
|----------------------|--------------|
| `AZURE_ENVIRONMENT` | Název cloudového prostředí, ke kterému se má připojit. |
| `AZURE_AD_RESOURCE` | ID prostředku Active Directory, který se má při připojování použít. Měl by to být identifikátor URI odkazující na váš koncový bod správy. |

Pokud používáte ověřování na základě prostředí, zavoláním funkce [NewAuthorizerFromEnvironment](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromEnvironment) získejte objekt Authorizer. Tento objekt se pak nastaví ve vlastnosti `Authorizer` klientů a umožní jim přístup k Azure.

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
authorizer, err := auth.NewAuthorizerFromEnvironment()
```

### <a name="authentication-on-azure-stack"></a>Ověřování v Azure Stacku

Pro ověřování v Azure Stacku musíte nastavit následující proměnné:

| Proměnná prostředí | Popis  |
|----------------------|--------------|
| `AZURE_AD_ENDPOINT` | Koncový bod služby Azure Active Directory. |
| `AZURE_AD_RESOURCE` | Identifikátor prostředku služby Azure Active Directory. |

Tyto proměnné se dají načíst z informací o metadatech Azure Stacku. Pokud chcete tato metadata načíst, otevřete webový prohlížeč v prostředí Azure Stacku a použijte adresu URL: `(ResourceManagerURL)/metadata/endpoints?api-version=1.0`.

`ResourceManagerURL` se liší v závislosti na názvu oblasti, názvu počítače a plně kvalifikovaném názvu domény (FQDN) vašeho nasazení Azure Stack:

| Prostředí | ResourceManagerURL |
|----------------------|--------------|
| Vývojová sada | `https://management.local.azurestack.external/` |
| Integrované systémy | `https://management.(region).ext-(machine-name).(FQDN)` |

Další informace o použití Azure SDK for Go v Azure Stacku najdete v tématu věnovaném [použití profilů verzí API s Go v Azure Stacku](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-version-profiles-go)


## <a name="use-file-based-authentication"></a>Použití ověřování na základě souboru

Ověřování na základě souboru pracuje s přihlašovacími údaji klientů pouze v případě, že jsou uložené ve formátu místního souboru vygenerovaném prostřednictvím [Azure CLI 2.0](/cli/azure). Tento soubor můžete jednoduše vytvořit při vytváření nového instančního objektu pomocí parametru `--sdk-auth`. Pokud plánujete používat ověřování na základě souboru, nezapomeňte tento argument zadat při vytváření instančního objektu. Vzhledem k tomu, že rozhraní příkazového řádku vypisuje výstup do `stdout`, přesměrujte výstup do souboru.

```azurecli
az ad sp create-for-rbac --sdk-auth > azure.auth
```

Nastavte proměnnou prostředí `AZURE_AUTH_LOCATION` na umístění autorizačního souboru. Aplikace tuto proměnnou prostředí přečte a provede parsování přihlašovacích údajů, které soubor obsahuje. Pokud potřebujete vybrat autorizační soubor za běhu, pracujte s prostředím programu pomocí funkce [os.Setenv](https://golang.org/pkg/os/#Setenv).

Pokud chcete načíst ověřovací údaje, zavolejte funkci [NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile). Na rozdíl od ověřování na základě prostředí vyžaduje ověřování na základě souboru koncový bod prostředku.

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
authorizer, err := NewAuthorizerFromFile(azure.PublicCloud.ResourceManagerEndpoint)
```

Další informace o používání instančních objektů a správě jejich přístupových oprávnění najdete v tématu [Vytvoření instančního objektu pomocí Azure CLI 2.0].

## <a name="use-device-token-authentication"></a>Použití ověřování pomocí tokenu zařízení

Pokud chcete, aby se uživatelé přihlašovali interaktivně, nejlepší způsob, jak to umožnit, je prostřednictvím ověřování pomocí tokenu zařízení. Tento tok ověřování předá uživateli token pro vložení na přihlašovací stránku Microsoftu, kde se pak ověří pomocí účtu Azure Active Directory (AAD). Na rozdíl od standardního ověřování pomocí uživatelského jména a hesla tato metoda ověřování podporuje účty s povoleným vícefaktorovým ověřováním.

Pokud chcete použít ověřování pomocí tokenu zařízení, pomocí funkce [NewDeviceFlowConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewDeviceFlowConfig) vytvořte objekt Authorizer [DeviceFlowConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig). Proces ověřování spustíte zavoláním metody [Authorizer](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig.Authorizer) pro výsledný objekt. Ověřování tokenu zařízení zablokuje provádění programu, dokud se celý tok ověřování nedokončí.

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
deviceConfig := auth.NewDeviceFlowConfig(applicationID, tenantID)
authorizer, err := deviceConfig.Authorizer()
```

## <a name="use-an-authentication-client"></a>Použití klienta ověřování

Pokud požadujete určitý typ ověřování a nevadí vám, že načtení ověřovacích údajů od uživatele bude obstarávat váš program, můžete použít jakéhokoli klienta, který je v souladu s rozhraním [auth.AuthorizerConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#AuthorizerConfig). Pokud chcete mít interaktivní program, použít specializované konfigurační soubory nebo máte požadavek, který vám brání použít jinou metodu ověřování, použijte typ, který implementuje toto rozhraní.

> [!WARNING]
> Nikdy nekódujte přihlašovací údaje Azure napevno do aplikace. Vložením tajných klíčů do binárních souborů aplikace usnadníte útočníkům jejich extrakci, a to bez ohledu na to, jestli je aplikace spuštěná. Ohrozíte tím všechny prostředky Azure, ke kterým mají přihlašovací údaje autorizaci.

Následující tabulka uvádí typy v sadě SDK, které jsou v souladu s rozhraním `AuthorizerConfig`.

| Typ ověřování | Typ objektu Authorizer |
|---------------------|-----------------------|
| Ověřování pomocí certifikátů | [ClientCertificateConfig] |
| Přihlašovací údaje klienta | [ClientCredentialsConfig] |
| Identita spravované služby | [MSIConfig] |
| Uživatelské jméno a heslo | [UsernamePasswordConfig] |

[ClientCertificateConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#ClientCertificateConfig
[ClientCredentialsConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#ClientCredentialsConfig
[MSIConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#MSIConfig
[DeviceFlowConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig
[UsernamePasswordConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#UsernamePasswordConfig

Pomocí příslušné funkce `New` vytvořte objekt Authenticator a pak proveďte ověření zavoláním metody `Authorize` pro výsledný objekt. Příklad při použití ověřování pomocí certifikátů:

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
certificateAuthorizer := auth.NewClientCertificateConfig(certificatePath, certificatePassword, clientID, tenantID)
authorizerToken, err := certificateAuthorizer.Authorize()
```

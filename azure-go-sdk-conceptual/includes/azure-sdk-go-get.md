Sada Azure SDK for Go je kompatibilní s Go verze 1.8 a novější. Pro prostředí využívající [profily Azure Stack Profiles](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-version-profiles) se vyžaduje minimálně Go verze 1.9. Pokud nemáte Go v systému k dispozici, postupujte podle [pokynů pro instalaci Go](https://golang.org/doc/install).

Sadu Azure SDK for Go a její závislosti můžete získat prostřednictvím `go get`.

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> Nezapomeňte pro `Azure` v adrese URL použít velká písmena. Pokud to neuděláte, při práci s touto sadou SDK mohou nastat problémy při importu, které souvisejí s použitím velkých a malých písmen. Velká písmena musíte použít také pro `Azure` v příkazech pro import.


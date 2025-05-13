# Del 2

## Forutsetninger

Du trenger å ha installert følgende programvare:

- [Helm](https://helm.sh/docs/intro/install)

Hvis du klarer å kjøre denne kommandoen i terminalen din, er alt klart:

```bash
helm --version
```

### Nytt `kind`-cluster

Det er anbefalt å starte med et helt fersk `kind`-cluster.
Du kan bruke `kind delete cluster` for å slette det gamle, og så kjøre `kind create cluster` på nytt.

```bash
kind delete cluster
kind create cluster
```

## Oppgaver

1. [Helm](helm.md)
2. [GitOps](gitops.md)

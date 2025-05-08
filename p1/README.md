# Del 1

## Forutsetninger

Du trenger å ha installert følgende programvare:

- en eller annen terminal-emulator du kan kjøre Bash-kommandoer i (helst WSL for Windows-brukere)
- [Docker Engine](https://docs.docker.com/get-docker) eller [podman](https://podman.io/docs/installation) eller [Colima](https://github.com/abiosoft/colima/blob/main/docs/INSTALL.md)
- [kubectl](https://kubernetes.io/docs/tasks/tools)
- [kind](https://kind.sigs.k8s.io/docs/user/quick-start)

Hvis du klarer å kjøre disse kommandene i terminalen din, er alt klart:

```bash
kind create cluster
kubectl cluster-info --context kind-kind
kind delete cluster
```

### Windows

Hvis du er på Windows, er det anbefalt å kjøre `kind` i WSL2.
Det finnes en guide [her](https://kind.sigs.k8s.io/docs/user/using-wsl2).

## Oppgaver

1. [Grunnlegende](basics.md)
2. [Konfigurasjon](config.md)
3. [Nettverk](networking.md)

## Nyttig informasjon

- [Kubernetes sin offisielle dokumentasjon](https://kubernetes.io/docs/home)
- `kubectl explain`: Forklarer hva en ressurs er og hvilke felt den har. Eksempel:
  ```bash
  kubectl explain pod
  kubectl explain pod.spec
  kubectl explain pod.spec.containers
  ```

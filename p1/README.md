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

Hvis du er på Windows, må du også ha installert WSL2.
kind har en guide [her](https://kind.sigs.k8s.io/docs/user/using-wsl2).

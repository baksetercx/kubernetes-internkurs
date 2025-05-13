## Helm

### Applikasjoner

### 🔨 Oppgave 1.1

Vi har lyst til å bruke Helm til å installere en applikasjon på clusteret vårt.
Lag et nytt Helm chart som heter `my-chart` ved å bruke `helm create`-kommandoen.

<details>
  <summary>✨ Se fasit</summary>

```bash
helm create my-chart
```

</details>

### 🔨 Oppgave 1.2

Nå har vi lyst til å installere `my-chart`-chartet vårt på clusteret.
Skriv en kommando som installerer chartet vårt med navnet `my-release` i namespace `my-namespace`.
Vi ønsker også at Helm skal lage namespace hvis det ikke allerede eksisterer.

💡 _HINT:_ Bruk `helm install`-kommandoen.

<details>
  <summary>✨ Se fasit</summary>

```bash
# vi antar mappen du er i inneholder mappen `my-chart`
helm -n my-namespace install my-release my-chart --create-namespace
```

</details>

Se om Helm har installert chartet vårt ved å liste opp alle releases i `my-namespace`:

```bash
helm -n my-namespace list
```

Dersom alt gikk bra, burde du ha laget noen ressurser i `my-namespace`:

```bash
kubectl -n my-namespace get all
```

### 🔨 Oppgave 1.3

Nå har vi lyst til å oppdatere verdiene i `values.yaml`-filen i `my-chart`-chartet vårt.
Endre `replicaCount` til 3 og kjør Helm på nytt.

💡 _HINT:_ Bruk `helm upgrade`-kommandoen.

<details>
  <summary>✨ Se fasit</summary>

```bash
# Endre replicaCount i values.yaml til 3
# Kjør helm upgrade
helm -n my-namespace upgrade my-release .
```

</details>

### Cluster Addons

### 🔨 Oppgave 2.1

Vi har lyst til å bruke Helm til å installere en **cluster addon**.
Installer Grafana ved å bruke Helm.
Det skal innstalleres i namespacet `grafana`. Opprett det hvis det ikke finnes fra før.

Du finner Helm-chartet til Grafana på [Grafana Helm Chart](https://artifacthub.io/packages/helm/grafana/grafana).

💡 _HINT:_ Kjør `helm repo add`-kommandoen for å legge til Helm-repoet til Grafana før du installerer chartet.

<details>
  <summary>✨ Se fasit</summary>

```bash
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update
helm -n grafana install grafana grafana/grafana --create-namespace
```

</details>

### 🔨 Oppgave 2.2

Følg guiden du får etter å ha installert Grafana for å komme deg inn i Grafana-grensesnittet.

<details>
  <summary>✨ Se fasit</summary>

1. Get your 'admin' user password by running:

```bash
kubectl get secret --namespace grafana grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
```

2. The Grafana server can be accessed via port 80 on the following DNS name from within your cluster:

```
grafana.grafana.svc.cluster.local
```

Get the Grafana URL to visit by running these commands in the same shell:

```bash
export POD_NAME=$(kubectl get pods --namespace grafana -l "app.kubernetes.io/name=grafana,app.kubernetes.io/instance=grafana" -o jsonpath="{.items[0].metadata.name}")
kubectl --namespace grafana port-forward $POD_NAME 3000
```

3. Login with the password from step 1 and the username: admin

</details>

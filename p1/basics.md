# Grunnleggende

## Pods

### 🔨 Oppgave 1.1

Vi har lyst til å lage et namespace for applikasjonene våre.

Skriv en `kubectl`-kommando for å lage et namespace med navnet `app`.

💡 _HINT:_ Bruk `kubectl create`-kommandoen.

<details>
  <summary>✨ Se fasit</summary>

```bash
kubectl create namespace app
```

</details>

### 🔨 Oppgave 1.2

Vi har lyst til å lage en `Pod` med navnet `my-pod` i som kjører en `nginx`-container i namespace `app`.

Skriv en `kubectl`-kommando for å lage denne pod'en.

💡 _HINT:_ Bruk `kubectl run`-kommandoen.

<details>
  <summary>✨ Se fasit</summary>

```bash
kubectl -n app run my-pod --image=nginx
```

</details>

### 🔨 Oppgave 1.3

Nå som du har laget en `Pod`, kan du sjekke at den kjører ved å liste opp alle pods i `app`-namespacet:

```bash
kubectl -n app get pods
```

For å rydde opp etter oss, har vi nå lyst til å slette pod'en.
Skriv en `kubectl`-kommando for å slette pod'en.

<details>
  <summary>✨ Se fasit</summary>

```bash
kubectl -n app delete pod my-pod
```

</details>

## Deployments

### 🔨 Oppgave 2.1

Vi har lyst til å lage en `Deployment` med navnet `my-deployment` som kjører en `nginx`-container i namespace `app`.
Den skal ha 2 replicas og kjøre kommandoen `/bin/sh -c 'while true; do echo "Hello, $MY_NAME!"; sleep 10; done'`.

Skriv en `kubectl`-kommando for å lage denne deployment'en.

💡 _HINT:_ Bruk `kubectl create deployment`-kommandoen. Skriv `kubectl create deployment -h` for å få hjelp.

<details>
  <summary>✨ Se fasit</summary>

```bash
kubectl -n app create deployment my-deployment --image=nginx --replicas=1 -- /bin/sh -c 'while true; do echo hello; sleep 10;done'
```

</details>

### 🔨 Oppgave 2.2

Nå som du har laget en `Deployment`, kan du sjekke at den kjører ved å liste alle deployments i `app`-namespacet:

```bash
kubectl -n app get deployments
```

Skriv en `kubectl`-kommando som viser loggene til pod'en i deployment'en.

💡 _HINT:_ Bruk `kubectl logs`-kommandoen.

<details>
  <summary>✨ Se fasit</summary>

```bash
kubectl -n app logs -f deployment/my-deployment
```

</details>

### 🔨 Oppgave 2.3

En viktig del av deployments er skalering, eller _replicas_.

Skalér opp `my-deployment` til 4 replicas ved hjelp av `kubectl`.

<details>
  <summary>✨ Se fasit</summary>

```bash
kubectl -n app scale deployment my-deployment --replicas 2
```

Nå kan du følge med på oppskaleringen ved å kjøre `kubectl -n app get pods --watch`.

</details>

### 🔨 Oppgave 2.4

Noen ganger trenger vi å redigere selve konfigurasjonsfilen (YAML) som
definerere en Kubernetes-ressurs, fordi `kubectl` ikke støtter alle mulige endringer.

#### a) Hent ut en YAML-spec for `my-deployment` ved hjelp av `kubectl`, og lagre det til en fil `deployment.yaml`.

💡 _HINT:_ Bruk `<kommando her> > deployment.yaml` for å lagre output fra en kommando til en fil.

<details>
  <summary>✨ Se fasit</summary>

```bash
kubectl -n app get deployment my-deployment -o yaml > deployment.yaml
```

</details>

#### b) Legg til en miljøvariabel `MY_NAME` med ditt navn som verdi ved å endre `deployment.yaml`.

💡 _HINT:_ Bruk `kubectl explain deployment.spec.template.spec.containers` for å se hvilke felter du må legge til under `containers` for å lage en miljøvariabel.
Hvis du legger på eller fjernel felter for `kubectl explain` kan du se alle mulige felter på forskjellige nivå i `Deployment`-spec'en.
F.eks. vil `kubectl explain deployment.spec` vise alle mulige felter under `spec:` i en `Deployment`.

<details>
  <summary>✨ Se fasit</summary>

```bash
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: my-deployment
  name: my-deployment
  namespace: app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: my-deployment
  strategy: {}
  template:
    metadata:
      labels:
        app: my-deployment
    spec:
      containers:
      - command:
        - /bin/sh
        - -c
        - while true; do echo "Hello, $MY_NAME!"; sleep 10; done
        env:
          MY_NAME: andreas
        image: nginx
        name: nginx
```

</details>

#### c) Bruk `kubectl` for å pushe endringen i konfiruasjonsfilen `deployment.yml` til Kubernetes.

💡 _HINT:_ Bruk `kubectl apply`.

<details>
  <summary>✨ Se fasit</summary>

```bash
kubectl apply -f deployment.yml
```

</details>

Gjerne sjekk loggen for å se om pod'en printer riktig.

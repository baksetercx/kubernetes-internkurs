# Konfigurasjon

## Secrets

### 🔨 Oppgave 3.1

Lag en `Secret` med navn `my-postgres-secret` i namespacet `postgres` som inneholder verdiene
`POSTGRES_USER=postgres` og `POSTGRES_PASSWORD=hemmelig-passord`.
Hvis namespace `postgres` ikke eksisterer fra før, må du lage det først.

<details>
  <summary>✨ Se fasit</summary>

```bash
kubectl -n postgres create secret generic my-postgres-secret --from-literal=POSTGRES_USER=postgres --from-literal=POSTGRES_PASSWORD=hemmelig-passord
```

</details>

### 🔨 Oppgave 3.2

Nå har vi lyst til å bruke `my-postgres-secret` i en `Deployment`.
Lag en `Deployment` med navn `my-postgres-deployment` i ditt namespace som bruker `my-postgres-secret` som en `envFrom`-variabel.
Den skal kjøre en `postgres`-container med `image=postgres:latest` og ha 1 replica.

<details>
  <summary>✨ Se fasit</summary>

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-postgres-deployment
  namespace: postgres
spec:
  replicas: 1
  selector:
    matchLabels:
      app: my-postgres-deployment
  template:
    metadata:
      labels:
        app: my-postgres-deployment
    spec:
      containers:
        - name: postgres
          image: postgres:latest
          envFrom:
            - secretRef:
                name: my-postgres-secret
```

</details>

## ConfigMaps

### 🔨 Oppgave 4.1

Lag en `ConfigMap` med navn `my-app-config` i namespacet `app` som inneholder verdiene
`APP_NAME=my-app` og `APP_VERSION=1.0.0`.

<details>
  <summary>✨ Se fasit</summary>

```bash
kubectl -n app create configmap my-app-config --from-literal=APP_NAME=my-app --from-literal=APP_VERSION=1.0.0
```

</details>

### 🔨 Oppgave 4.2

Nå har vi lyst til å bruke `my-app-config` i en `Deployment`.
Lag en `Deployment` med navn `my-app-deployment` i ditt namespacet `app` som bruker `my-app-config` som en `envFrom`-variabel.

Den skal kjøre kommandoen `/bin/sh -c 'echo "Running $APP_NAME, version $APP_VERSION"' && sleep 3600'` i en `nginx`-container med `image=nginx:latest` og ha 1 replica.

<details>
  <summary>✨ Se fasit</summary>

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app-deployment
  namespace: app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: my-app-deployment
  template:
    metadata:
      labels:
        app: my-app-deployment
    spec:
      containers:
        - name: nginx
          image: nginx:latest
          command:
            [
              '/bin/sh',
              '-c',
              'echo "Running $APP_NAME, version $APP_VERSION" && sleep 3600',
            ]
          envFrom:
            - configMapRef:
                name: my-app-config
```

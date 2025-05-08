# Nettverk

## Services

### 🔨 Oppgave 5.1

Vi har lyst til å lage en `Service` med navnet `my-service` i namespace `app` som eksponerer port 80 til port 80 for Poden `my-pod`.
Service'en skal være av typen `NodePort`.

`my-pod` er en pod som kjører en `nginx`-container i namespace `app` der port 80 er eksponert.

Skriv en `kubectl`-kommando for å lage både pod'en og servicen.

💡 _HINT:_ Bruk `kubectl expose`-kommandoen.

```bash
kubectl -n app run my-pod --image=nginx --port=80
kubectl -n app expose pod my-pod --port=80 --target-port=80 --name=my-service --type=NodePort
```

### 🔨 Oppgave 5.2

Nå skal vi i teorien kunne nå `my-pod` via `my-service`.
Vi har brukt `NodePort`-typen for å eksponere servicen vår, som betyr at
vi kan nå den via IP-adressen til en av nodene i clusteret vårt og porten som ble tildelt servicen.

Finn ut hvilken addresse du kan nå `my-service`, ved å kombinere IP-adressen til en av nodene i clusteret med porten som ble tildelt servicen.
Du skal kunne bruke netttleseren din til å nå `my-service` via `http://<node-ip>:<node-port>`.

💡 _HINT:_ Bruk `kubectl get nodes -o wide` for å finne IP-adressen til nodene i clusteret.

```bash
# Hent IP-adressen til noden i clusteret
kubectl get nodes -o wide
# Hent porten som ble tildelt servicen
kubectl -n app get service my-service
# Kombiner IP-adressen og porten


# Fancy one-liner
echo "$(kubectl get nodes -o jsonpath='{.items[0].status.addresses[?(@.type == "InternalIP")].address}'):$(kubectl get svc -n app my-service -o jsonpath='{.spec.ports[0].nodePort}')"
```

## NetworkPolicies

### 🔨 Oppgave 6.1

Vi har lyst til å lage en `NetworkPolicy` med navnet `my-network-policy` i namespace `app` som blokkerer all innkommende trafikk til pod'en `my-pod`.

Lag en `NetworkPolicy` som blokkerer all innkommende trafikk til pod'en `my-pod`.
Den skal matche på en label som treffer `my-pod`; dersom du lagde `my-pod` med `kubectl run`, så har den label'en `run=my-pod`.
Hvis ikke, kan du lage den på nytt elle redigere med `kubectl edit` eller `kubectl label`.

💡 _HINT:_ Bruk `kubectl apply`-kommandoen med en yaml-fil.

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: my-network-policy
  namespace: app
spec:
  podSelector:
    matchLabels:
      app: my-pod
  policyTypes:
    - Ingress
  ingress: []
```

Hvis alt er riktig, burde du ikke lenger kunne nå `my-pod`, pga. networkpolicy.

## GitOps

### 🔨 Oppgave 3.1

Nå har vi lyst til å bruke GitOps til å gjøre det samme vi har gjort med Helm.
Først må vi installere Argo CD i clusteret vårt.

Installer Argo CD i `argocd`-namespace.

Du finner ArgoCD Helm-chartet på [Argo CD Helm Chart](https://artifacthub.io/packages/helm/argo/argo-cd).

💡 _HINT:_ Kjør `helm repo add`-kommandoen for å legge til Helm-repoet til Argo CD før du installerer chartet.

<details>
  <summary>✨ Se fasit</summary>

```bash
helm repo add argo https://argoproj.github.io/argo-helm
helm repo update
helm -n argocd install argocd argo/argo-cd --create-namespace
```

</details>

### 🔨 Oppgave 3.2

Nå har vi lyst til å gå inn i Argo CD-grensesnittet.
Først må vi kjøre en `kubectl port-forward`-kommando for å eksponere Argo CD API-serveren.
Deretter må vi finner admin-passordet i secreten `argocd-initial-admin-secret`.

Eksponer Argo CD API-serveren, finn admin-passordet og logg inn i Argo CD-grensesnittet.

💡 _HINT:_ Bruk `kubectl port-forward`-kommandoen for å eksponere Argo CD API-serveren og `kubectl get secret`-kommandoen for å finne admin-passordet.

<details>
  <summary>✨ Se fasit</summary>

```bash
# Eksponer Argo CD API-serveren
kubectl -n argocd port-forward svc/argocd-server 8080:443
# Finn admin-passordet
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 --decode ; echo
```

Gå til [localhost:8080](http://localhost:8080) i nettleseren din og logg inn med brukernavnet `admin` og passordet du fikk fra forrige steg.

</details>

### 🔨 Oppgave 3.3

Nå kan vi endelig lage en Argo CD-applikasjon!

Lag en Argo CD-applikasjon som peker til `my-chart`-chartet vårt i Git-repoet vårt (kubernetes-internkurs) i mappen `p2/charts/my-chart`.
Du skal lage applikasjonen i `argocd`-namespace og bruke `my-namespace` som destinasjons-namespace.

**IKKE LAG DEN I GUI! Lag en YAML-spec.**

💡 _HINT:_ Les dokumentasjonen til Argo CD for å finne ut hvordan du lager en applikasjon: https://argo-cd.readthedocs.io/en/stable/user-guide/application-specification/

<details>
  <summary>✨ Se fasit</summary>

```yaml
# my-chart.yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: my-chart
  namespace: argocd
spec:
  project: default
  source:
    repoURL: https://github.com/baksetercx/kubernetes-internkurs
    targetRevision: HEAD
    path: p2/charts/my-chart
  destination:
    server: https://kubernetes.default.svc
    namespace: my-namespace
```

```bash
# Lag applikasjonen
kubectl apply -f my-chart.yaml
```

</details>

Når du er ferdig kan du gå inn i Argo CD-grensesnittet og se trykke på **Sync**.

### 🔨 Oppgave 3.4

Vi kan også peke direkte på et Helm-chart når vi lager en Argo CD-applikasjon, istedet for å peke på en Git-repo med YAML-filer.
Lag enda en Argo CD-applikasjon som installer Helm-chartet til Grafana i `grafana`-namespace.

Du skal lage applikasjonen i `argocd`-namespace og bruke `grafana` som destinasjons-namespace.

**IKKE LAG DEN I GUI! Lag en YAML-spec.**

💡 _HINT 1:_ Les dokumentasjonen til Argo CD for å finne ut hvordan du lager en applikasjon som bruker Helm direkte: https://argo-cd.readthedocs.io/en/stable/user-guide/helm

💡 _HINT 2:_ Du kan bruke `targetRevision: '*'` for å bruke den nyeste versjonen av chartet.

<details>
  <summary>✨ Se fasit</summary>

```yaml
# grafana.yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: grafana
  namespace: argocd
spec:
  project: default
  source:
    repoURL: https://grafana.github.io/helm-charts
    chart: grafana
    targetRevision: '*'
  destination:
    server: https://kubernetes.default.svc
    namespace: grafana
```

```bash
kubectl apply -f grafana.yaml
```

</details>

### 🔨 Oppgave 3.5

Nå vil vi at Argo CD applikasjonene våre skal synces automatisk.

Skru på automatisk sync og self-healing ved å endre spec'en til `my-chart`- og `grafana`-applikasjonen.

💡 _HINT:_ Les dokumentasjonen til Argo CD for å finne ut hvordan du aktiverer automatisk sync og self-healing: https://argo-cd.readthedocs.io/en/stable/user-guide/auto_sync

<details>
  <summary>✨ Se fasit</summary>

```yaml
# my-chart.yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: my-chart
  namespace: argocd
spec:
  project: default
  source:
    repoURL: https://github.com/baksetercx/kubernetes-internkurs
    targetRevision: HEAD
    path: p2/charts/my-chart
  destination:
    server: https://kubernetes.default.svc
    namespace: my-namespace
# grafana.yaml
```

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: grafana
  namespace: argocd
spec:
  project: default
  source:
    repoURL: https://grafana.github.io/helm-charts
    chart: grafana
    targetRevision: '*'
  destination:
    server: https://kubernetes.default.svc
    namespace: grafana
  syncPolicy:
    automated:
      selfHeal: true
```

```bash
kubectl apply -f my-chart.yaml
kubectl apply -f grafana.yaml
```

</details>

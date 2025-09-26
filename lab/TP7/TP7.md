# 🔐 TP7 : Contrôler le trafic réseau avec les règles Ingress et Egress dans Kubernetes

## 🎯 Objectif

Comprendre et appliquer des **NetworkPolicies** dans Kubernetes pour contrôler les flux **ingress** (entrant) et **egress** (sortant) entre les pods.

---

## 🧱 Contexte

Votre cluster Kubernetes est déjà prêt. Vous allez :

- Déployer deux namespaces : `frontend` et `backend`.
- Déployer une application simple dans chaque namespace (serveur + client).
- Appliquer progressivement des règles réseau pour contrôler les communications.

> ⚠️ **Important :** Assurez-vous que votre cluster a un CNI compatible avec les NetworkPolicies (comme Calico, Cilium, etc). Sinon, les règles ne seront pas appliquées.

---

## 1️⃣ Préparation de l’environnement

### Créez deux namespaces

```bash
kubectl create namespace frontend
kubectl create namespace backend
````

### Déployez une application serveur dans `backend`

```yaml
# backend-deploy.yaml
apiVersion: v1
kind: Pod
metadata:
  name: backend
  namespace: backend
  labels:
    app: backend
spec:
  containers:
  - name: web
    image: nginx
    ports:
    - containerPort: 80
```

```bash
kubectl apply -f backend-deploy.yaml
```

### Déployez un client dans `frontend`

```yaml
# frontend-client.yaml
apiVersion: v1
kind: Pod
metadata:
  name: frontend
  namespace: frontend
  labels:
    app: frontend
spec:
  containers:
  - name: busybox
    image: busybox
    command: ["sleep", "3600"]
```

```bash
kubectl apply -f frontend-client.yaml
```

---

## 2️⃣ Tester la communication entre pods

Depuis le pod `frontend`, tentez de contacter le serveur `backend` :

```bash
kubectl exec -n frontend frontend -- wget -qO- backend.backend.svc.cluster.local
```

Cela devrait fonctionner au début.

---

## 3️⃣ Créer une règle Ingress restrictive

Créez une **NetworkPolicy** qui bloque tout trafic entrant vers le pod `backend`.

```yaml
# deny-all-ingress.yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: deny-all-ingress
  namespace: backend
spec:
  podSelector:
    matchLabels:
      app: backend
  policyTypes:
  - Ingress
```

Appliquez-la :

```bash
kubectl apply -f deny-all-ingress.yaml
```

### 🔍 Question :

* Que se passe-t-il si vous relancez la commande `wget` depuis le pod `frontend` ?

---

## 4️⃣ Créer une règle Ingress autorisant uniquement `frontend`

Ajoutez maintenant une règle plus fine qui **autorise uniquement** le pod `frontend` à communiquer avec le pod `backend`.

```yaml
# allow-frontend.yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-frontend
  namespace: backend
spec:
  podSelector:
    matchLabels:
      app: backend
  policyTypes:
  - Ingress
  ingress:
  - from:
    - namespaceSelector:
        matchLabels:
          name: frontend
```

> 💡 Pour que cette règle fonctionne, ajoutez un label au namespace `frontend` :

```bash
kubectl label namespace frontend name=frontend
```

---

## 5️⃣ Créer une règle Egress

Ajoutez une règle Egress dans le namespace `frontend` qui **bloque toute sortie**, sauf vers le namespace `backend`.

```yaml
# egress-policy.yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-egress-backend
  namespace: frontend
spec:
  podSelector:
    matchLabels:
      app: frontend
  policyTypes:
  - Egress
  egress:
  - to:
    - namespaceSelector:
        matchLabels:
          name: backend
```

N'oubliez pas de labeler le namespace `backend` :

```bash
kubectl label namespace backend name=backend
```

---

## 📌 Questions de révision

1. Quelle est la différence entre `Ingress` et `Egress` ?
2. Une `NetworkPolicy` agit-elle comme un pare-feu ou comme une ACL ?
3. Que se passe-t-il si **aucune** `NetworkPolicy` n’existe dans un namespace ?
4. Une `NetworkPolicy` sans règle `egress` bloque-t-elle tout le trafic sortant ?
5. Peut-on filtrer par labels de **pods** et de **namespaces** ? Pourquoi est-ce utile ?

---

## ✅ Bonus (facultatif)

* Testez une règle qui autorise seulement les requêtes HTTP vers un port donné.
* Ajoutez un troisième namespace (`test`) et essayez d'en limiter l’accès vers `backend`.
* Essayez d’ajouter des `ipBlock` pour autoriser certaines IP seulement.

---

## 🧠 À retenir

* Les **NetworkPolicies** permettent de sécuriser les communications **entre pods**.
* Par défaut, **tout est permis** jusqu’à ce qu’une règle vienne le restreindre.
* Les règles sont **non globales** : elles doivent être créées dans le namespace des pods **à protéger**.
* Il est essentiel d’avoir un **CNI compatible** avec les politiques réseau.

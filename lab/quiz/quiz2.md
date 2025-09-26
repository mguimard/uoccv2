## 🧠 Quiz : Kubernetes, persitance, traffic, tolérance à la panne

---

### **1. À quoi sert un ReplicaSet dans Kubernetes ?**

A. À créer des volumes persistants
B. À maintenir un nombre défini de pods en fonctionnement
C. À garantir la haute disponibilité des pods
D. À déployer un pod sur chaque nœud

---

### **2. Quelle est la principale utilité d’un DaemonSet ?**

A. Déployer un pod unique sur tout le cluster
B. Assurer qu’un pod spécifique tourne sur chaque nœud
C. Gérer la montée en charge d'une application web
D. Déployer des agents système (monitoring, logs...)

---

### **3. Quelle(s) affirmation(s) concernant les règles Ingress est/sont vraie(s) ?**

A. Elles contrôlent le trafic entrant dans le cluster
B. Elles s'appliquent uniquement au trafic sortant
C. Elles permettent le routage HTTP/HTTPS
D. Elles remplacent totalement les services Kubernetes

---

### **4. À quoi servent les règles Egress dans Kubernetes ?**

A. À autoriser ou restreindre le trafic sortant des pods
B. À définir la bande passante maximale d’un pod
C. À sécuriser les connexions entre services internes
D. À filtrer les connexions vers Internet ou d'autres réseaux

---

### **5. Laquelle de ces options contribue à la tolérance à la panne dans Kubernetes ?**

A. Déploiement sur plusieurs nœuds
B. Utilisation de ReplicaSet avec plusieurs réplicas
C. Utilisation exclusive de StatefulSet
D. Configuration d’anti-affinity rules

---

### **6. Que se passe-t-il si un pod géré par un ReplicaSet est supprimé manuellement ?**

A. Il est recréé automatiquement
B. Il reste supprimé jusqu’à redéploiement manuel
C. Le ReplicaSet crée un nouveau pod pour respecter le nombre souhaité
D. Tous les pods du ReplicaSet sont redéployés

---

### **7. Quelle est la différence principale entre un volume ephemeral (comme `emptyDir`) et un volume persistant (`PersistentVolume`) ?**

A. Le volume `emptyDir` survit à la suppression du pod
B. Un `PersistentVolume` permet de conserver les données après la suppression d’un pod
C. `emptyDir` est partagé entre plusieurs pods
D. `PersistentVolume` peut être fourni par des systèmes de stockage externes (NFS, AWS EBS...)

---

### **8. Quelles pratiques permettent d’assurer la persistance des données dans un cluster Kubernetes ?**

A. Utiliser un volume de type `hostPath` pour tout
B. Définir un `PersistentVolumeClaim` et le lier à un `PersistentVolume`
C. Sauvegarder les fichiers dans le conteneur
D. Utiliser un système de stockage réseau (comme NFS ou CSI)

---

### **9. Que se passe-t-il si un nœud contenant un pod avec un volume persistant tombe en panne ?**

A. Le volume est toujours disponible si le backend le permet (ex : NFS, EBS)
B. Le pod est recréé sur un autre nœud, mais le volume peut ne pas être ré-attaché automatiquement
C. Le volume est perdu définitivement
D. Kubernetes gère automatiquement la migration du volume si la configuration le permet

---

### **10. Quelles sont les meilleures pratiques pour déployer une application critique dans Kubernetes ?**

A. Utiliser des ReplicaSets avec au moins 3 réplicas
B. Stocker les données importantes dans le conteneur
C. Configurer des probes (readiness/liveness)
D. Utiliser des volumes persistants avec redondance

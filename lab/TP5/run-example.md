# Exemple de création d'un pod (nginx) avec dossier monté en volume.

## Préparation data

```bash
sudo mkdir /mnt/data
sudo sh -c "echo 'Salut depuis /mnt/data :)' > /mnt/data/index.html"
cat /mnt/data/index.html
```

## Création volumes, claims, pod

```bash
kubectl apply -f storage-class.yaml
kubectl apply -f pv-volume.yaml
kubectl apply -f pv-claim.yaml
kubectl apply -f pv-pod.yaml
```

## Récupération IP et tests

```bash
kubectl get pods -o wide
curl <ip>
```





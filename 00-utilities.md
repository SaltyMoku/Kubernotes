Creare un pod/deployment/ecc (imperative):
```bash
kubectl create deployment --image=nginx nginx
```

Creare uno yaml template:
```bash
kubectl run nginx --image=nginx --dry-run=client -o yaml
kubectl create deployment --image=nginx nginx --replicas=4 --dry-run=client -o yaml > nginx-deployment.yaml 
```

Ottenere tutti gli oggetti nel namespace
```bash
kubectl get all
```

Contare numero di pod
```bash
kubectl get pods --no-headers | wc -l
```

Sostituire un pod con uno nuovo creato da file:
```bash
kubectl replace --force -f pod.yaml
```
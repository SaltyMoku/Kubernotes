Creare uno yaml template
```
kubectl run nginx --image=nginx --dry-run=client -o yaml
kubectl create deployment --image=nginx nginx --replicas=4 --dry-run=client -o yaml > nginx-deployment.yaml 
```

Ottenere tutti gli oggetti nel namespace
```
kubectl get all
```

Contare numero di pod
```
kubectl get pods --no-headers | wc -l
```
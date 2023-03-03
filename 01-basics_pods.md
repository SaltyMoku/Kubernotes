Basics Pods
===========

Vedere informazioni:
```
kubectl get pods
```

Vedere informazioni in yaml:
```
kubectl get pods NOMEPOD -o yaml
```

Creare rapidamente:
```
kubectl run nginx --image=nginx
```

Vedere informazioni dettagliate:
```
kubectl describe pod NOMEPOD
```

Cancellare:
```
kubectl delete pod webapp
```

Modificare un pod in esecusione:
```
kubectl edit pod NOMEPOD
```
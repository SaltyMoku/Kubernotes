Basics Pods
===========

Vedere informazioni:
```bash
kubectl get pods
```

Vedere informazioni in yaml:
```bash
kubectl get pods NOMEPOD -o yaml
```

Creare rapidamente:
```bash
kubectl run nginx --image=nginx
```

Vedere informazioni dettagliate:
```bash
kubectl describe pod NOMEPOD
```

Cancellare:
```bash
kubectl delete pod webapp
```

Modificare un pod in esecusione:
```bash
kubectl edit pod NOMEPOD
```
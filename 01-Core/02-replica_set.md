Basics Replicaset
=================

Un replica set tiene un certo numero di pod vivo.
Se ho 4 Pod, e ne cancello uno, il RS ne spawnera' uno nuovo.

```bash
kubectl get rs
```

```bash
kubectl describe rs
```

```bash
kubectl scale rs new-replica-set --replicas 5
```

```bash
kubectl edit rs
```

Esempio configurazione base:
```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: replicaset-1
spec:
  replicas: 2
  selector:
    matchLabels:
      tier: frontend
  template:
    metadata:
      labels:
        tier: frontend
    spec:
      containers:
      - name: nginx
        image: nginx
```
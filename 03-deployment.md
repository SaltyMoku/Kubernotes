Deployment
==========

Un deployment permette di gestire i rolling updates, rollback e pause negli update.
Un deployment contiene e gestisce almeno un replicaset.  
Se creo un deployment creo automaticamente un replicaset.

Esempio creazione deployment:
```
kubectl create deployment --image=httpd:2.4-alpine httpd-frontend --replicas 3
```

Esempio creazione file template:
```
kubectl create deployment --image=nginx nginx --replicas=4 --dry-run=client -o yaml > nginx-deployment.yaml
```

Esempio scaling numero di pod:
```
kubectl scale deployment nginx --replicas=4
```

Esempio configurazione base yaml:
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: deployment-1
spec:
  replicas: 2
  selector:
    matchLabels:
      name: busybox-pod
  template:
    metadata:
      labels:
        name: busybox-pod
    spec:
      containers:
      - name: busybox-container
        image: busybox
        command:
        - sh
        - "-c"
        - echo Hello Kubernetes! && sleep 3600
```
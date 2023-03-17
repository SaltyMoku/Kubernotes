Labels and Selectors
====================

Labels
------

Una Label aggiunge delle etichette a un oggetto.  
Si specificano nell yaml in `metadata`:
```
apiVersion: v1
kind: Pod
metadata:
  name: simple-webapp
  labels:
    app: App1
    function: Front-end
specs:
...
```

Selector
--------

Selector filtra in base alle etichette.
Sono utilizzati sia per fare ricerche manuali, che internamente a Kubernetes.

Es. ricerca manuale
```
kubectl get pods --selector app=App1
kubectl get all --selector env=prod --no-headers | wc -l
kubectl get all --selector env=prod,bu=finance,tier=frontend
```

es. utilizzo di Kube - i Replicas e i Pod sotto di essi hanno le stesse labels.
```
apiVersion: apps/v1
kind: ReplicaSet
metadata:
   name: replicaset-1
spec:
   replicas: 2
   selector:
      matchLabels:
        tier: front-end #Label usata dal ReplicaSet
   template:
     metadata:
       labels:
        tier: front-end #Label assegnata ai Pod
     spec:
       containers:
       - name: nginx
         image: nginx
```
Annotations
-----------

Le Annotations sono delle note opzionali inseribili in Metadata in cui prendere appunti:
```
...
metadata:
  annotations:
    buildversion: 1.34
...
```

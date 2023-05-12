Multicontainer Pods
===================

Non si puo' creare un Pod Multicontainer con Run.  
Bisogna creare il file .yaml e runnarlo (quindi dry-run, e modifichi il file aggiungento i container in piu').

`Multicontainer.yaml`:
```
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  name: yellow
spec:
  containers:
  - image: busybox
    name: lemon
    command:
    - sleep
    - "1000"
  - image: redis
    name: gold
```

Container Elastic con Sidecar per invio log a Kibana.  
Sidecar e' gia' configurato per inviare log in questo caso.  
Il Pod ha due container, ed entrambi sharano il volume `log-volume`.
- app: lo monta in `/log`
- sidecar: lo monta in `/var/log/event-simulator/`
```
apiVersion: v1
kind: Pod
metadata:
  name: app
  namespace: elastic-stack
  labels:
    name: app
spec:
  containers:
  - name: app
    image: kodekloud/event-simulator
    volumeMounts:
    - mountPath: /log
      name: log-volume
  - name: sidecar
    image: kodekloud/filebeat-configured
    volumeMounts:
    - mountPath: /var/log/event-simulator/
      name: log-volume
  volumes:
  - name: log-volume
    hostPath:
      # directory location on host
      path: /var/log/webapp
      # this field is optional
      type: DirectoryOrCreate
```

InitContainers
--------------

Se ho bisogno di un Container usa e getta di supporto, che runna una volta e poi non mi serve piu', uso InitContainer.
Gli InitContainer runnano una volta prima dell'esecuzione degli altri container.

`InitContainer.yaml`:
```
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  labels:
    app: myapp
spec:
  containers:
  - name: myapp-container
    image: busybox:1.28
    command: ['sh', '-c', 'echo The app is running! && sleep 3600']
  initContainers:
  - name: init-myservice
    image: busybox:1.28
    command: ['sh', '-c', 'until nslookup myservice; do echo waiting for myservice; sleep 2; done;']
  - name: init-mydb
    image: busybox:1.28
    command: ['sh', '-c', 'until nslookup mydb; do echo waiting for mydb; sleep 2; done;']
```

initContainer che fa sleep 20 secondi prima di partire:
```
...
spec:
  initContainers:
  - command:
    - sh
    - -c
    - sleep 20
    image: busybox
    name: sleep
  containers:
  - command:
    - sh
    - -c
    - echo The app is running! && sleep 3600
    image: busybox:1.28
...
```
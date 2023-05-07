Environment Variables
=====================

Le variabili d'ambiente possono essere configurate nel Pod:
```
  spec:
    containers:
    - env:
      - name: APP_COLOR
        value: pink
```
Per una migliore gestione e' possibile configurare delle ConfigMap da cui prendere le variabili:
```
kubectl get configmaps
kubectl create configmap webapp-config-map --from-literal=APP_COLOR=darkblue --from-literal=APP_OTHER=disregard
```

Per recuperare le variabili dalla ConfigMap, modificare il Pod:
```
spec:
  containers:
  - env:
    - name: APP_COLOR
      valueFrom:
       configMapKeyRef:
         name: webapp-config-map
         key: APP_COLOR
    image: kodekloud/webapp-color
```

E' possibile anche prendere la configMap da un Volume [ESEMPI NON PRESENTI - DA TESTARE]:
```
volumes:
- name: app-config-volume
  configMap:
    name: app-config
```


Note
----

Yaml delle ConfigMap:
```
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  APP_COLOR: blue
  APP_MODE: prod
```
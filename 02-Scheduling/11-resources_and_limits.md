Resource & Limits
=================

Lo Scheduler assegna i Pod anche a seconda delle risorse che richiede.  
Le risorse e i limiti di default sono definiti per ogni namespace in un oggetto `LimitRange`:
```yaml
apiVersion: v1
kind: LimitRange
metadata:
    name: mem-limit-range
spec:
    limits:
    - default:
        memory: 512Mi
    defaultRequest:
        memory: 256Mi
    type: Container
```  
```yaml
apiVersion: v1
kind: LimitRange
metadata:
    name: cpu-limit-range
spec:
    limits:
    - default:
        cpu: 1
    defaultRequest:
        cpu: 0.5
    type: Container
```

Resource Requests
-----------------

I requisiti di default di un container sono:
- CPU: 0.5
- MEM: 256 Mi
- Disk

Se il Pod ha bisogno di maggiori risorse, e' possibile indicarlo in `resources` nel pod-definition file (non e' possibile modificare un Pod running):
```yaml
spec:
  containers:
  - name: simple-webapp-color
    image: simple-webapp-color
    ports:
    - containerPort: 8080
    resources:
      requests:
        memory: "1Gi"
        cpu: 1
```
Limits
------

I limiti di default di un container (per ciascun container nel pod) sono:
- CPU: 1
- MEM: 512 Mi

Per modificarli si aggiunte `limits` sotto `resources`:

```yaml
spec:
  containers:
  - name: simple-webapp-color
    image: simple-webapp-color
    ports:
    - containerPort: 8080
    resources:
      requests:
        memory: "1Gi"
        cpu: 1
      limits:
        memory: "2Gi"
        cpu: 2
```

Se il Pod supera il limite di CPU, fa Throttle.  
Se il Pod supera la RAM, per un picco gli viene data senza problemi, ma se e' un picco costante viene abbattuto.

Note
----
1 G = 1,000,000,000 bytes  
1 M = 1,000,000 bytes  
1 K = 1,000 bytes  

1 Gi = 1,073,741,824 bytes  
1 Mi = 1,048,576 bytes  
1 Ki = 1,024 bytes  
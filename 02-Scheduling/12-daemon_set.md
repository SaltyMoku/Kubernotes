Daemon Sets
===========

Il compito del Daemon Set e' assicurarsi che ci sia una copia del Pod X su ogni nodo (usando NodeAffinity).  
Ottimo per Monitoring e Logging.
E' utilizzato per deployare `Kube-Proxy` o gestione Network in alcuni casi.  

Il definition file e' molto simile al `Replica Set`:
```
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: monitoring-daemon
spec:
  selector:
    matchLabels:
      app: monitoring-agent
  template:
    metadata:
      labels:
        app: monitoring-agent
    spec:
      containersL
      - name: monitoring-agent
        image: monitoring-agent
```

Notes
-----

Per sapere su quali nodi gira un Daemonset (es. `kube-proxy`):
```
kubectl describe daemonset kube-proxy --namespace=kube-system
```
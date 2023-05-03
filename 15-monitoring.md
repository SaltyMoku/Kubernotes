Monitoring
==========

Kubernetes non ha un monitoraggio nativo.
Alcuni tra i monitoraggi generalmente usati sono:
- Metrics Server
- Prometeus
- Elastic Stack

Installazione Metrics
---------------------

```
git clone https://github.com/kodekloudhub/kubernetes-metrics-server.git
cd kubernetes-metrics-server/
kubectl create -f .
```

Controllare utilizzo Pod:
```
kubectl top node
kubectl top pods
```

Tipi di logging
---------------

Controllo live log docker:
```
docker logs -f CONTAINER_ID
```

Controllo ultimi log:
Se ci sono diversi Container va specificato il nome (lanciando il comando senza nome container suggerisce i nomi):
```
kubectl logs PODNAME
kubectl logs PODNAME NAMECONTAINER
```

Controllo log live dei container dentro un Pod.
```
kubectl logs -f PODNAME
kubectl logs -f PODNAME NOMECONTAINER
```
Monitoring
==========

Kubernetes non ha un monitoraggio nativo.
Alcuni tra i monitoraggi generalmente usati sono:
- Metrics Server
- Prometeus
- Elastic Stack

Installazione Metrics
---------------------

```bash
git clone https://github.com/kodekloudhub/kubernetes-metrics-server.git
cd kubernetes-metrics-server/
kubectl create -f .
```

Controllare utilizzo Pod:
```bash
kubectl top node
kubectl top pods
```

Tipi di logging
---------------

Controllo live log docker:
```bash
docker logs -f CONTAINER_ID
```

Controllo ultimi log:
Se ci sono diversi Container va specificato il nome (lanciando il comando senza nome container suggerisce i nomi):
```bash
kubectl logs PODNAME
kubectl logs PODNAME NAMECONTAINER
```

Controllo log live dei container dentro un Pod.
```bash
kubectl logs -f PODNAME
kubectl logs -f PODNAME NOMECONTAINER
```
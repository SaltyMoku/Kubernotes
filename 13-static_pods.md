Static Pods
===========

Si tratta di Pod gestiti solo da Kubelet e Docker (senza Kube API).  
Il Worker si occupera' autonomamente di ritirarli su in caso di Crash ecc, senza Kube API.

Come utilizzarli
----------------

Senza Kube API non e' possibile normalmente schedulare Pod.  
Ci sono due modi per utilizzare gli static Pod:
1. Inserire il file .yaml nella folder `/etc/kubernetes/manifests` (path contenuto in `kubelet.service` [`--pod-manifest-path`]).
2. Inserire in `kubelet.service` (`systemctl status kubelet.service | grep config`) un path di config (es. `--config=kubeconfig.yaml`), e li' dentro inserire `staticPodPath: /etc/kubernetes/manifests` (kubeadmin fa cosi').

Note
----

- Non e' possibile gestire Replicaset o Daemonset, ma solamente Pod.
- Non e' possibile usare `kubectl` non avendo Kube API, quindi bisogna usare `docker`
- Static Pod e Pod classici possono coesistere, e il Kube API server vede gli static Pod (ma sono simili a dei link, e quindi NON modificabili da kubectl)
```
kubectl run static-busybox --dry-run=client --image busybox -o yaml --command -- sleep 1000 > /etc/kubernetes/manifests/temp.yaml

kubectl run --restart=Never --image=busybox static-busybox --dry-run=client -o yaml --command -- sleep 1000 > /etc/kubernetes/manifests/static-busybox.yaml
```

Da verificare
-------------

Gli static pod hanno `-HOSTNAME` alla fine del nome.
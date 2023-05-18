Cluster Upgrade
===============

Componenti di Kubernetes
------------------------

I componenti core che lo compongono sono:
- kube-apiserver
- Control-manager
- kube-scheduler
- kubelet
- kube-proxy
- kubectl

I componenti possono avere versioni diverse, a seconda della versione di `kube-apiserver` (componente principale con cui parlano tutti).  
Le versioni seguono queste regole:

- kube-apiserver: X
- Control-manager, kube-scheduler: X-1
- kubelet, kube-proxy: X-2
- kubectl: X+1 > X-1

Se kube-apiserver e' versione `1.10` le versioni possono essere:
- kube-apiserver: v1.10
- Control-manager, kube-scheduler: v1.9 o v1.10
- kubelet, kube-proxy: v1.8 o v1.9 o v1.10
- kubectl: X+1 > X-1

Aggiornamento
-------------

Kubernetes supporta solo le ultime 3 minor release.  
Kubernetes non puo' skippare subrelease.  
Deve necessariamente passarle tutte quante:  

`... 1.10 -> 1.11 -> 1.12 ...`

A seconda di come e' stato deployato va aggiornato in maniera differente.
- __Kubeadm__: lanciando il comando:
    `kubeadm upgrade plan`
- __Cloud Google__: pulsante di Upgrade da GUI
- __The hard way__: a mano, componente per componente

Strategie
---------

- __Disservizio__: aggiorno tutti i worker insieme, ottenendo un upgrade rapido ma con disservizio
- __Rollout Update__: aggiorno i worker 1 alla volta in modo da non ottenere disservizio, spostando il carico sugli altri nodi.
- __Nuovi Nodi__: buon metodo sul cloud. Creo semplicemente nei nuovi worker alla versione piu' aggiornata, sposto i pod sui nuovi, e butto i vecchi.


### Kubeadm

In ordine bisogna aggiornare:
1. __Master Node__
2. __Workers__

Step by step di upgrade dalla v1.11.0 -> v1.12.0

#### Master Node

Ottengo versioni di kubeadm e dei nodi (e se necessario faccio un drain):
```
kubeadm upgrade plan
```

Aggiorno la versione di kubeadm:
```
apt update
apt-get install kubeadm=1.12.0-00
kubeadm upgrade apply v1.12.0
```

Se presente __kubelet__ sul Master Node (`systemctl status kubelet`), aggiornarlo:
```
apt-get install kubelet=.1.12.0-00
systemctl daemon-reload
systemctl restart kubelet
```


#### Worker Nodes

Faccio drain del nodo:
```
kubectl drain node-1
```

Eseguo upgrade kubeadm:
```
apt-get update
apt-get install kubeadm=1.12.0-00
```

Lancio upgrade del nodo e kubelet, restart il servizio, e lo rendo nuovamente disponibile:
```
kubeadm upgrade node
apt-get install kubelet=1.12.0-00
systemctl daemon-reload
systemctl restart kubelet
kubectl uncordon node-1
```

Note
----

Nei Lab lanciare l'update di Kubelet o Kubeadm con `apt-get upgrade -y kubexxx=1.XX.0-00` non ha funzionato.  
Lanciando invece `apt-get install kubexxx=1.X.X.0-00` e' andato senza problemi.
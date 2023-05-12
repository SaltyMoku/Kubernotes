Encrypt Secrets
===============

I segreti sono encodati in base64, ma non criptati.  
Chiunque potrebbe andare in /etc/kubernetes/pki/etcd/ e vedere i secret lanciando (necessario `apt-get install etcd-client` se etcdctl non e' presente):  
```
ETCDCTL_API=3 etcdctl \
   --cacert=/etc/kubernetes/pki/etcd/ca.crt   \
   --cert=/etc/kubernetes/pki/etcd/server.crt \
   --key=/etc/kubernetes/pki/etcd/server.key  \
   get /registry/secrets/default/NOMESECRET | hexdump -C
```
Per evitarlo configuriamo Encryption at Rest.  
Per verificare se l'encryption e' gia' attiva:
```
ps -aux | grep kube-api | grep "encryption-provider-config"
```
OPPURE
```
cat /etc/kubernetes/manifests/kube-apiserver.yaml | grep -i "encrypt"
```

Passaggi (ad alto livello):
---------------------------

1. Generare una chiave random di 32-byte e encodarla in base64:
```
head -c 32 /dev/urandom | base64
```
2. Creare EncryptionConfiguration.yaml inserendo la Key:
```
apiVersion: apiserver.config.k8s.io/v1
kind: EncryptionConfiguration
resources:
  - resources:
      - secrets
      - configmaps
      - pandas.awesome.bears.example
    providers:
      - aescbc:
          keys:
            - name: key1
              secret: <BASE 64 ENCODED SECRET>
      - identity: {}
```
3. Spostare il file .yaml in `/etc/kubernetes/enc` (path cambiabile).
4. Aggiungere i riferimenti e i volumi nel Pod di kube-api:
```
apiVersion: v1
kind: Pod
metadata:
  annotations:
    kubeadm.kubernetes.io/kube-apiserver.advertise-address.endpoint: 10.10.30.4:6443
  creationTimestamp: null
  labels:
    component: kube-apiserver
    tier: control-plane
  name: kube-apiserver
  namespace: kube-system
spec:
  containers:
  - command:
    - kube-apiserver
    ...
    - --encryption-provider-config=/etc/kubernetes/enc/enc.yaml  # <-- add this line
    volumeMounts:
    ...
    - name: enc                           # <-- add this line
      mountPath: /etc/kubernetes/enc      # <-- add this line
      readonly: true                      # <-- add this line
    ...
  volumes:
  ...
  - name: enc                             # <-- add this line
    hostPath:                             # <-- add this line
      path: /etc/kubernetes/enc           # <-- add this line
      type: DirectoryOrCreate             # <-- add this line
  ...
```
5. Se erano gia' presenti secret, forzare l'encryption:
```
kubectl get secrets --all-namespaces -o json | kubectl replace -f -
```

Documentazione
--------------

https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data/
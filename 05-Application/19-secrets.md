Secrets
=======

Le ConfigMap sono ottime per passare variabili, ma NON per passare segreti, in quanto vengono salvati in chiaro.  
I Secret di default vengono salvati in etcd, e vengono solo encodati in base64.  
I Secret quindi NON vengono criptati.  
E' possibile criptarli usando `EncryptionConfiguration`, ma non viene visto in CKA.  
Meglio ancora sarebbe usare tool terzi, come AWS o HashCorp Vault.  

Usando Kubernetes ci sono 3 modi per passare un Secret a un Pod:

Secrets via Env
---------------

Crearli manualmente/da file:
```
kubectl create secret generic NOMESECRET --from-literal=CHIAVE=VALORE --from-literal=CHIAVE2=VALORE2
kubectl create secret generic NOMESECRET --from-file=FILEPATH
```

Oppure con `kubectl create -f`.
In questo caso pero' i secrets devono essere salvati gia' encodati (es. usando `echo -n "paswrd" | bae64`):
```
apiVersion: v1
kind: Secret
metadata:
  name: app-secret
data:
  DB_Host: bXlzcWw=
  DB_User: cm9vdA==
  DB_Password: cGFzd3Jk
```

Una volta creati, i Secret vanno inseriti nella config del Pod:
```
...
spec:
  containers:
- name: simple-webapp-color
  envFrom:
  - secretRef:
      name: app-secret
...
```

Secrets via Single Env
----------------------
Direttamente nel Pod:
```
env:
- name: DB_Password
  valueFrom:
    secretKeyRef:
      name: app-secret
      key: DB_Password
```

Secrets via Volume
------------------

Secret passato come File da un volume.  
In questo caso ogni Secret consiste in un file nel container per ciascun Secret.
Il file conterra' l'effettivo secret (visibile es. con `cat`)
```
volumes:
- name: app-secret-volume
  secret:
    secretName: app-secret
```
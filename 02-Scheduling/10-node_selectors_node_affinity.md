Node Selectors & Node Affinity
==============================

Sono due metodi per far si' che un Pod venga schedulato su un certo Nodo.

Node Selectors
--------------

Molto facile, ma poco versatile (non puoi usare OR o NOT).  
Assegni una Label a un nodo:
```bash
kubectl label nodes <node-name> <label-key>=<label-value>
kubectl label nodes node-1 size=Large
```
Dici al Pod di usare nodi con quella label:
```yaml
...
spec:
  nodeSelector:
    size: Large
...
```

Node Affinity
-------------

Piu' complesso, ma piu' versatile.
Per fare la stessa regola dell'esempio di prima, bisogna dire al Pod:
```yaml
...
spec:
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: size
            operator: In # Possono essere diversi es. Exists, NotIn, ecc...
            values:
            - Large
...
```
I tipi di NodeAffinity sono:

- `requiredDuringSchedulingIgnoredDuringExecution`
- `preferredDuringSchedulingIgnoredDuringExecution`
- `requiredDuringSchedulingRequiredDuringExecution`: al momento Planned e non utilizzabile

Combo Affinity & Taints
-----------------------

Node Affinity permette di dire a un Pod di utilizzare precisamente un certo Nodo.
Altri Pod pero' potrebbero utilizzare quel Nodo.
Se vogliamo dedicare un Nodo SOLO a un certo Pod, ci aggiungiamo i Taints.

Usandoli insieme e' quindi possibile dedicare un Nodo a un certo Pod, e bannare tutti gli altri.

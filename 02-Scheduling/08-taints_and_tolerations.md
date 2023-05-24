Taints & Tolerations
====================

Servono ad evitare che dei Pod finiscano su certi nodi.  
Es. il Master node di default non accetta alcun Pod perche' ha un Taint.

Taints
------

Assegnato al nodo, blocca tutti i Pod che non lo tollerano.
Come uno spray anti-zanzare.

Puo' avere diversi comportamenti:
- `NoSchedule`: non schedula Pod sul Nodo
- `PreferNoSchedule`: non schedula Pod sul Nodo, a meno che' non sia necessario
- `NoExecute`: non schedula Pod sul Nodo, e uccide tutti i Pod attualmente sul nodo che non tollerano il Taint

```bash
kubectl taint nodes node-name key=value:taint-effect
kubectl taint nodes node 1 app=blue:NoSchedule
```

Tolerations
-----------

Permette a un Pod di essere immune a un Taint.  
Di default i Pod non hanno alcuna Toleration.  
I valori nello yaml devono essere identici a quelli del taint.

Es. se sul nodo lanciamo `kubectl taint nodes node 1 app=blue:NoSchedule`, il Pod deve avere:
```yaml
...
specs:
  tolerations:
  - key: "app"
    operator: "Equal"
    value: "blue"
    effect: "NoSchedule"
...
```

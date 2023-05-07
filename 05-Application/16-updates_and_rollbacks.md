Deployment Update Strategies
----------------------------

Ci sono due principali strategie (sotto `strategy` nel Deployment yaml):
- `Recreate`: uccide i pod vecchi e successivamente tira su i nuovi. Ha dei Downtime.
- `RollingUpdate`: per ogni pod nel replicaset, ne crea uno nuovo aggiornato, e successivamente ne killa uno vecchio. Non ha dei Downtime, ed e' il metodo di default.

Rolling Update
--------------

Il Deployment lo gestisce via ReplicaSet.  
Quando viene fatto un update viene creato un nuovo RS.  
A questo punto scala +1 nel nuovo RS, creando un Pod aggiornato, e scala a -1 nel vecchio RS togliendo un Pod vecchio.  
Questa operazione viene effettuata fino a quando i Pod del vecchio sono a 0.

Commands
--------

```
kubectl create -f deployment-definition.yml
kubectl get deployments
kubectl apply -d deployment-definition.yml
kubectl set image deployment/myapp-deployment nginx=nginx:1.9.1
kubectl rollout status deployment/myapp-deployment
kubectl rollout history deployment/myapp-deployment
kubectl rollout undo deployment/myapp-deployment
```
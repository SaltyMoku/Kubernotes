Namespaces
==========

I namespace permettono di isolare pod, policy, risorse e DNS in diversi ambienti.

DNS name esteso, per pingare servizi in altri namespace:
```
db-service.dev.svc.cluster.local
```
- db-service: Service Name
- dev: Namespace
- svc: Service
- cluster.local: domain

```
kubectl get pods --namespace=dev
```
```
kubectl get pods -n dev
```
```
kubectl get pods --all-namespaces
```
```
kubectl run --image redis redis --namespace finance
```


Configurare un altro namesace come dafault, per non specificarlo piu' ogni volta:
```
kubectl config set-context $(kubectl config current-context) --namespace=dev
```
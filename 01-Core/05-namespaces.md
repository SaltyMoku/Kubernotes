Namespaces
==========

I namespace permettono di isolare pod, policy, risorse e DNS in diversi ambienti.

DNS name esteso, per pingare servizi in altri namespace:
```bash
db-service.dev.svc.cluster.local
```
- db-service: Service Name
- dev: Namespace
- svc: Service
- cluster.local: domain

```bash
kubectl get pods --namespace=dev
```
```bash
kubectl get pods -n dev
```
```bash
kubectl get pods --all-namespaces
```
```bash
kubectl run --image redis redis --namespace finance
```


Configurare un altro namesace come dafault, per non specificarlo piu' ogni volta:
```bash
kubectl config set-context $(kubectl config current-context) --namespace=dev
```
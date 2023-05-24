Scheduling
==========
Kubernetes schedula i Pod autonomamente.
E' possibile comunque schedularli a mano (es. se Kube si rompe).

Per schedulare un nuovo Pod:
```yaml
...
spec:
  containers:
  - name nginx
    image: nginx
    ports:
      - containerPort: 8080
  nodeName: node02
```

Per modificare un Pod gia' esistente invece devo creare un binding, e inviare una richiesta manualmente alle API:
```yaml
apiVersion v1
kind: Binding
metadata:
  name: nginx
target:
  apiVersion: v1
  kind: Node
  name: node02
```
```bash
curl -X POST \
  -H "Content-Type: application/json" \
  --data @binding.json \
  http://<your-kubernetes-api-server>/api/v1/namespaces/<your-namespace>/pods/<your-pod-name>/binding/
```
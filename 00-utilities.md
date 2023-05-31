## Utility varie

Creare un pod/deployment/ecc (imperative):
```bash
kubectl create deployment --image=nginx nginx
```

Creare uno yaml template:
```bash
kubectl run nginx --image=nginx --dry-run=client -o yaml
kubectl create deployment --image=nginx nginx --replicas=4 --dry-run=client -o yaml > nginx-deployment.yaml 
```

Spiega un oggetto di kube, e mostra le sue opzioni/configurazioni nel file yaml:
```bash
kubectl explain pods
kubectl explain pods.metadata.labelskubectl explain pods
```

Ottenere tutti gli oggetti nel namespace
```bash
kubectl get all
```

Contare numero di pod
```bash
kubectl get pods --no-headers | wc -l
```

Sostituire un pod con uno nuovo creato da file:
```bash
kubectl replace --force -f pod.yaml
```

## Comandi vari

k3s install
```bash
curl -sfL https://get.k3s.io | sh -
```

Port-forwarding via Kubectl (per raggiungere la porta di un Pod per testing):
```bash
kubectl port-forward inspircd-69b984fbcc-4xwgh 6667:6667
```

## Configurazione Windows kubectl

1. Collegarsi al Cluster Kubernetes e recuperare il file kubeconfig, es:
	`cat /etc/rancher/k3s/k3s.yaml`
2. Copiare il file sul proprio PC, e sostituire l'IP locale `127.0.0.1` con l'IP del cluster (es. `192.168.0.10`):
    ![kubeconf.png](/images/utilities_01.png)
3. Inserire come variabile `KUBECONFIG`	 il path dove hai copiato il fule kubeconf.
![a5391005a3e7a9c6b12dc067cb6ae448.png](:/8f09348abf7b4a37a45d296b8a0860a9)
4. Scaricare il binario di `kubectl` da [qui](https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/)

Service
=======

Cuscinetto che permette la comunicazione di tra componenti di una applicazione (sia tra pod, che da utenti).

Ci sono 3 tipi di servizi:
- NodePort
- ClusterIP
- LoadBalancer

Nodeport
--------

Il mio PC puo' contattare un nodo, ma non un Pod al suo interno.
Il Nodeport mappa una porta del nodo a una porta di un pod.
Ad esempio la porta 80 di un Pod web viene mappata alla porta 30008 del nodo.
Accedendo a http://nodo:30008/ posso vedere l'applicazione.

![Nodeport](/images/services_01.png)

Esempio configurazione che espone i/il pod con label `app: myapp` e `type: front-end`:
```yaml
apiVersion: v1
kind: Service
metadata:
  name: myapp-service
spec:
  type: NodePort
  ports:
  - targetPort: 80
    port: 80
    nodePort: 30008
  selector:
    app: myapp
    type: front-end
```

Cluster IP
----------

Ogni pod del front-end deve poter contattare ogni pod del backend. I pod per natura pero' cambiano non sono stabili, e cambiano spesso IP/nome.

Per evitare questo:

![Cluster IP](/images/services_02.png)

Faccio questo, usando un Cluster IP Service:

![Cluster IP](/images/services_03.png)

Ogni servizio Cluster IP avra' un nome e un IP, che verra' usato dai pod per comunicare.

E' possibile recuperare nome e IP usando:
```bash
kubectl get services
```

Esempio definition file:
```yaml
apiVersion: v1
kind: Service
metadata:
  name: back-end
spec:
  type: ClusterIP
  ports:
  - targetPort: 80
    port: 80
  selector:
    app: myapp
    type: back-end
```

Load Balancer
-------------

E' utilizzabile SOLO su piattaforme Cloud compatibili, perche' vado a usare il loro Load Balancer nativo.  
Se creo un'applicazione con pod su diversi nodi, e uso `Nodeport`, finisco per avere un punto di ingresso per ogni nodo.  

![Load Balancer](/images/services_04.png)

Io voglio avere un solo punto di ingresso. Vado quindi a creare un `Load Balancer`, che sara' l'unico punto di ingresso.

![Load Balancer](/images/services_05.png)

Esempio definition file:
```yaml
apiVersion: v1
kind: Service
metadata:
  name: myapp-service
spec:
  type: LoadBalancer
  ports:
  - targetPort: 80
    port: 80
    nodePort: 30008
```

Comandi freschi
---------------

Crea Service chiamato `redis-service` di tipo `ClusterIP` per esporre un pod redis sulla porta 6379:
```bash
kubectl expose pod redis --port=6379 --name redis-service --dry-run=client -o yaml
```

Crea un service chiamato `nginx` di tipo `NodePort` per esporre la porta 80 di un pod nginx sulla porta 30080 del nodo:
```bash
kubectl expose pod nginx --type=NodePort --port=80 --name=nginx-service --dry-run=client -o yaml
```
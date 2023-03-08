Imperative e Declarative
========================

Imperative
----------
I comandi imperative sono quelli lanciati brutalmente con kubectl senza applicare file.
Rimane traccia solo nella sessione dell'utente che li ha lanciati. Sono stonks per fare prove/andare yolo, ma non adatti a un ambiente enterprise.  
I principali comandi sono:
```
kubectl run --image=nginx nginx
kubectl create deployment --image=nginx nginx
kubectl expose deployment nginx --port 80
kubectl edit deployment nginx
kubectl scale deployment nginx --replicas=5
kubectl set image deployment nginx nginx=nginx:1.18
kubectl create -f nginx.yaml
kubectl replace -f nginx.yaml
kubectl delete -f nginx.yaml
```

I comandi imperative usati nel lab sono:
```
kubectl run --image nginx:alpine nginx-pod
kubectl run --image redis:alpine redis --labels tier=db
kubectl expose pod redis --port 6379 --name redis-service
kubectl create deployment webapp --image kodekloud/webapp-color --replicas 3
kubectl run --image nginx custom-nginx --port 8080
kubectl create ns dev-ns
kubectl create deployment redis-deploy --namespace dev-ns --image redis --replicas 2

kubectl run --image httpd:alpine httpd
kubectl expose pod httpd --port 80
[OPPURE]
kubectl run httpd --image=httpd:alpine --port=80 --expose 
```

Declarative
-----------

Approccio piu' gentile che usa i file. In casi in cui ci fossero errori/il pod esistesse gia', e' intelligente e non restituisce errori:
```
kubectl apply -f nginx.yaml
```
Pod su nodo down
----------------

Se un nodo va down, e rimane giu' per 5 minuti (default), il nodo e' considerato morto.  
Quando un nodo muore i Pod che ospitava possono finire in due modi:  
1. Replicaset: se faceva parte di un Replicaset, vengono schedulati su un altro nodo
2. Morti: se non facevano parte di un Replicaset, semplicemente muoiono.

Applicare patch a nodo con Pod su di esso
-----------------------------------------

Se dobbiamo riavviare un nodo (es. per un update) non vogliamo creare disservizi.
Se ad esempio un Pod e' ospitato come sola copia su un Nodo, e questo e' down, l'applicazione sul Pod non sara' raggiungibile.
Per evitare questi disservizi:
1. "Spostiamo" i Pod su un altro nodo
2. Rendiamo il nodo NON schedulabile
3. Riavviamo il nodo
4. Rendiamo il nodo nuovamente schedulabile

Per fare cio':
1. Lanciamo una Drain sul nodo, che uccide tutti i Pod su un nodo, li spawna su un altro nodo, e rende il nodo non schedulabile:
```bash
kubectl drain node01
```
```bash
kubectl drain node01 --ignore-daemonsets
```
2. Eseguiamo reboot del nodo
3. Rendiamo il nodo nuovamente schedulabile:
```bash
kubectl uncordon node01
```

Note
----
Per rendere un nodo NON schedulabile, ma senza uccidere i pod su di esso, esiste il comando:
```bash
kubectl cordon node01
```
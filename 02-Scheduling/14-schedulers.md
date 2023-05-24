Schedulers
==========

Custom Scheduler
----------------

Molteplici scheduler possono runnare allo stesso momento.  
E' possibile quindi creare scheduler con regole personalizzate.  

Inserendo `schedulerName` nel .yaml di un Pod e' possibile scegliere quale Scheduler usare:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spac:
  containers:
  - image: nginx
    name: nginx
  schedulerName: my-custom-scheduler
```

Scheduling Plugins & Extention Points
-------------------------------------

Normalmente lo scheduling avviene in 4 fasi:

1. Scheduling Queue  
Controlla le `PriorityClass` e da una priorita' ai Pod da schedulare.  
Possibile grazie al plugin `Priority Sort`.

2. Filtering  
Filtra i nodi in base alle Risorse richieste dal Pod, e alle Risorse disponibili dei nodi.
Possibile grazie ai plugin `NodeResourcesFit`, `NodeName` (se presente nello yaml del Pod), `NodeUnschedulable` (se presente nel Nodo settato a true).

3. Scoring  
Associa uno score ad ogni Nodo a seconda delle risorse rimanenti dopo l'assegnazione del Pod (viene scelto il nodo con lo score piu' alto).
Possibile grazie ai plugin `NodeResourceFit`, `ImageLocality` (se un Nodo ha gia' l'immagine del Pod)

4. Binding  
Il Pod viene legato al nodo.  
Possibile grazie al plugin `DefaultBinder`.

Ci sono inoltre molti altri Extention Points su cui basare Plugin, che possono essere utilizzati per creare scheduling/comportamenti personalizzati.

Esempio Custom Scheduler
------------------------

Questo scheduler ha piu' profili.
In ciascun profilo sono stati disabilitati alcuni plugin, e abilitati plugin custom.

```yaml
apiVersion: kubescheduler.config.k8s.io/v1
kind: KubeSchedulerConfiguration
profiles:
- schedulerName: my-scheduler-2
  plugins:
    score:
      disabled:
      - name: TaintToleration
      enabled:
      - name: MyCustomPluginA
      - name: MyCustomPluginB
- schedulerName: my-scheduler-3
  plugins:
    preScore:
      disabled:
      - name: '*'
      score:
      disabled:
      - name: '*'
```
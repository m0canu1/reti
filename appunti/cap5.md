###Distance Vector (DV) algorithm


**Bellman-Ford** formula:

$d_x(y) = min_v\{c(x,v) + d_v(y)\}$ dove:
 - $min_v$ riguarda **tutti i vicini** di $x$
 - $c(x,v)$ è il **costo** verso il **vicino** $v$
 - $d_v(y)$ è il **costo** dal **vicino** $v$ verso, la destinazione, $y$

**Idea chiave:**
- di tanto in tanto, ogni nodo manda il proprio ***distance vector* stimato** ai suoi vicini
- alla ricezione di un **DV**, il nodo $x$ aggiorna il proprio **DV** stimato con la formula di Bellman-Ford
  
Ciascun nodo $x$ mantiene i seguenti dati di instradamento:
- per ogni vicino $v$ il costo $c(x,v)$
- il vettore delle distanze del nodo $x$
- i vettori delle distanze di **ciascuno** dei suoi vicini.

---

###Scalable Routing

Divisione in **Autonomous Systems** (**AS**) con algoritmi di:
- *intra-routing* (host della stessa rete AS) e
- *inter-routing* (tra AS diversi)
  
**Intra-AS routing** oppure *Interior gateway protocols* (**IGP**), esempi:
- **RIP** - *Routing Information Protocol*
  - è un algoritmo basato su **DV** (distance vector) basato su broadcast
  - la **metrica** è basata sugli **hops** (massimo 15 hops)
  - i *DV* sono scambiati ogni 30 secondi
- **OSPF** - Open Shortest-Path First
  - è un algoritmo basato su **LS** (Link State)
  - usa Dijkstra
- **IGRP** - Interior Gateway Routing Protocol

**Inter-AS routing**:
- **BGP** - *Border Gateway Protocol*
  - eBGP: ottiene informazoni sui subnet raggiungibili dall'**e**sterno, cioè dagli *AS* vicini
  - iBGP: propaga le informazioni sulla raggiungibilità all'**i**nterno dell'*AS*
  - Path Vector - è un **messaggio** (con attributi) che indica che per raggiungere la rete $N$ il router successivo è $R$ e il percorso è $AS1, AS2, AS3$.
  - Funziona con i servizi TCP, con connessioni tra BGP-peers
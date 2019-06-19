### FTP (File Transfer Protocol)

- Trasferimento file da/verso host remoto
- Modello client/server
   - Client: chi inizia la richiesta di trasferimento (da/verso host remoto)
   - Server: remote host

Utilizza **due connessioni TCP parallele**:
1. **Control connection**: invio di informazioni di controllo (username, password, comandi change directory, comandi trasferimento file). Invio informazioni in FTP *out-of-band* (HTTP *in-band*, nell'header).
2. **Data connection**: usata per il vero e proprio invio dei file.
   
 **Control connection** è persistente, la **Data connection** si apre e si chiude ad ogni inizio e fine del trasferimento di un file.

---

**DNS** (Domain Name System)
 E' un servizio che **traduce** i nomi degli host nei loro indirizzi IP usando un **Database distribuito**. E' *distribuito* perche':
- unico punto di fallimento
 - alto volume di traffico
 - database centralizzato *lontano*
 - manutenzione

Altri servizi:
1. **Host aliasing**
2. **Mail aliasing**
3. **Load distribution** - usato per distribuire il carico tra server replicati, nel caso di siti con tanto traffico per cui vengono usati molteplici server (quindi ognuno con il **proprio indirizzo IP** diverso da quello degli altri). Il Database DNS contiene tutti questi indirizzi IP.

Il **DNS** è un database **distribuito e gerarchico** e ha tre classi:
1. *Root Server*
2. *TLD (top-level-domain) server* - .com, .org, .edu, ecc.
3. *Server Autoritativi* - i server *DNS* propri di un'organizzazione che mappa i propri IP agli host name

 Esiste anche il **DNS server locale**, quello fornito dagli *ISP*. E' detto anche *default name server*.
Quando un host fa una *DNS query* essa è mandata al *DNS server locale* che:
 - ha una **cache** delle recenti "traduzioni" (ma può essere vecchia).
 - fa da proxy e inoltra la query nella gerarchia.

**DNS Resolutions**:
1. ***Iterativa*** - il server contattato risponde con il nome del server da contattare
2. ***Ricorsiva*** - i server vengono contattati in modo ricorsivo. Tanto lavoro nei livelli alti della gerarchia

### Resource Record (Record di risorsa)
Sono i record memorizzati nei database e hanno i seguenti campi:
**($Name, Value, Type, TTL$)**

- $Type$ = $A$ -> **Name** = hostname, **Value** = IP dell'hostname. Es: (relay1.bar.foo.com, 145.37.93.126, A)
- $Type$ = $NS$ -> **Name** = dominio, **Value** = hostname del **DNS server autoritativo** che sa come ottenere gli indirizzi IP dell'host del dominio. Es: (foo.com, dns.foo.com, NS)
- $Type$ = $CNAME$ -> Es: (foo.com, relay1.bar.foo.com, CNAME)
- $Type$ = $MX$ -> mail server. Es: (foo.com, mail.bar.foo.com, MX)

**PTR Resource Record** - e' un record particolare che mi permette di ricevere l'**hostname** da un **indirizzo IP**. Questa operazione viene chiamata *Reverse DNS*.
Viene spesso usato per verificare l'affidabilita' di una macchina (in quanto i manager di rete assegnano domain name solo alle macchine di cui hanno fiducia).

---

### File Distribution

**Client-Server**:
- $\frac{N \times F}{U_s}$ tempo di distribuzione in secondi (non si tiene conto della capacita' di download dei client):
  - $N$ e' il **numero** di client
  - $F$ e' la **dimensione** del file
  - $U_s$ e' la velocita' di **upload del server** 
- $\frac{F}{d_{min}}$ tempo minimo di download del client
  - $F$ dimensione del file
  - $d_{min}$ e' la capacita' di download minima del client
- Tempo di distribuzione a $N$ client:
  - $D_{c-s} = min \{\frac{N \times F}{U_s},\frac{F}{d_{min}}\}$

**P2P**:
- $D_{P2P} = max \{\frac{F}{u_s},  \frac{F}{d_{min}},  \frac{N \times F}{u_s + \sum u_i },  \}$
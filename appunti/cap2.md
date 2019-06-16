###FTP (File Transfer Protocol)

>- Trasferimento file da/verso host remoto
>- Modello client/server
>   - Client: chi inizia la richiesta di trasferimento (da/verso host remoto)
>   - Server: remote host

Utilizza **due connessioni TCP parallele**:
1. **Control connection**: invio di informazioni di controllo (username, password, comandi change directory, comandi trasferimento file). Invio informazioni in FTP *out-of-band* (HTTP *in-band*, nell'header).
2. **Data connection**: usata per il vero e proprio invio dei file.
   
> **Control connection** è persistente, la **Data connection** si apre e si chiude ad ogni inizio e fine del trasferimento di un file.

---

**DNS** (Domain Name System)
> E' un servizio che **traduce** i nomi degli host nei loro indirizzi IP usando un **Database distribuito**.

Altri servizi:
1. **Host aliasing**
2. **Mail aliasing**
3. **Load distribution** - usato per distribuire il carico tra server replicati, nel caso di siti con tanto traffico per cui vengono usati molteplici serve (quindi ognuno con il **proprio indirizzo IP** diverso da quello degli altri). Il Database DNS contiene tutti questi indirizzi IP.

Il **DNS** è un database **distribuito e gerarchico** e ha tre classi:
1. root server
2. TLD (top-level-domain) server
3. server autoritativi

>Esiste anche il **DNS server locale**, quello fornito dagli *ISP*. E' detto anche *default name server*.
Quando un host fa una *DNS query* essa è mandata al *DNS server locale* che:
>- ha una **cache** delle recenti "traduzioni" (ma può essere vecchia).
>- fa da proxy e inoltra la query nella gerarchia.

####Resource Record (Record di risorsa)
Sono i record memorizzati nei database e hanno i seguenti campi:
>($Name, Value, Type, TTL$)

- $Type$ = $A$ -> Name = hostname, Value = IP dell'hostname. Es: (relay1.bar.foo.com, 145.37.93.126, A)
- $Type$ = $NS$ -> Name = dominio, Value = hostname del DNS server autoritativo che sa come ottenere gli indirizzi IP dell'host del dominio. Es: (foo.com, dns.foo.com, NS)
- $Type$ = $CNAME$
- $Type$ = $MX$
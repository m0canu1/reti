###Protocollo TCP

- Comunicazione point-to-point: 
  - un mittente, un destinatario
- Trasferimento affidabile stream-oriented:
  - Non c'è un concetto di messaggio ma si parla di stream di byte
- Modalità full duplex data:
  - Una connessione TCP offre un servizio full-duplex
- Connection-oriented:
  - prima di effettuare lo scambio dei dati, i processi devono effettuare l'**handshake** (*a tre vie*), ossia devono inviarsi reciprocamente alcuni segmenti preliminari per stabilire i parametri del successivo trasferimento dati. 
- Controllo di flusso
  - Il mittente non sovraccaricherà di pacchetti il destinatario
- Buffer di invio e **ricezione**
- Dimensione Massima del Segmento
- Segmenti TCP (**MSS** è la dimensione massima)

**Struttura segmenti TCP:**
- **Sequence number**
- **Acknowledgement number**
- **Header Length** - indica la lunghezza dell'header in parole da 32 bit
- **8 Flag**:
  - **PSH**
  - **URG**
  - **ACK**
  - **RST**
  - **SYN**
  - **FIN**
- **Receive Window (rwnd)**
- **Checksum**
- **URG Data Pointer**
- **Options**
- **Application Data**

> **RTT** (**Round Trip Time**) - 

> **RTO** (**Re-transmission Time-Out**) - Intervallo di tempo oltre il quale il mittente ritrasmette il segmento con il **più basso numero di sequenza** che non abbia ancora ricevuto *ACK*. Ogni volta che ciò accade, l'RTO raddoppia.
Nel caso degli altri due eventi (dati ricevuti dal livello applicazine o ACK ricevuto) l'**RTO** viene stimato dai più recenti valori di **EstimatedRTT e DevRTT**.

**GBN**
**SR**
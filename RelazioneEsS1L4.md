> # Relazione esercizio S1/L4

## Prefazione

L'esercizio di oggi consiste nel creare e configurare una rete di calcolatori con il tool di Cisco Packet Tracer, usando un router. 

L'obiettivo dell'esercitazione di oggi è capire come funzionano le comunicazioni a livello 2 e 3 del modello ISO/OSI con i rispettivi device di rete. 



### Svolgimento dell'esercizio

Data la prefazione, si continua con lo svolgimento dell'esercizio. Per l'implementazione iniziale dei diversi dispositivi (host e switch), fare riferimento al seguente link che porta alla repository dedicata alla spiegazione in merito: https://github.com/Jolly25/CS0425.

Il file si trova dentro UNIT_1 > S1 > L3 sotto il nome di "Relazione esercizio S1L3.pdf". 

Quest'oggi mi soffermo solo nella configurazione del router, cosa nuova, che nella relazione di cui sopra non c'è spiegazione. 

### Configurazione del router e suo funzionamento

Il ruolo fondamentale del router che andrò a implementare, è quello di **riuscire a far comunicare l'intera rete di calcolatori, nonostante abbiano IP di rete differenti**. 

Facciamo un esempio: prendiamo il caso in cui abbiamo 2 dispositivi (PC1 e PC2) collegati a uno switch A e altri due dispositivi (PC3 e PC4) collegati a un altro switch B. Cosa succede?

- PC1 e PC2 comunicano tra di loro direttamente a livello 2, usando MAC Address e lo switch si occupa di inoltrare i pacchetti. 
- Stessa cosa succede per PC3 e PC4, che comunicano senza bisogno del router. 

Morale? Finché i dispositivi stanno nella stessa rete IP, lo switch è sufficiente e non serve altro. 

Cosa facciamo se i dispositivi che vogliamo far comunicare, hanno **reti diverse**? Qui entra in gioco il router. 

Il router possiede due **interfacce** (è come se si dividesse per entrambe le parti che vogliono comunicare): una collegata allo Switch A (per esempio con rete `192.168.1.11`) e una a Switch B (per esempio con rete `192.168.2.13`). Il suo ruolo è quello di **tradurre e instradare i pacchetti a livello 3**. 



> [!NOTE]
>
> **Livello 2 e livello 3**. Cosa sono? Con "livello 2" mi riferisco al livello "Data Link" che si occupa di far arrivare i dati da un dispositivo all'altro nella stessa rete locale (LAN). Con "livello 3" mi riferisco al livello "Network", che si occupa di instradare i dati tra reti diverse.



A livello visivo, il risultato atteso della rete di calcolatori deve essere il seguente: 

<img src="/Users/iriscanole/Desktop/Screenshot 2025-09-25 alle 15.10.30.png" style="zoom:40%;" />

Come sono collegati gli switch al router? Sono collegati entrambi tramite un cavo C*opper Straight-Through* e in particolare: 

- lo switch A è collegato alla porta `GigaBitEthernet0/0/0`
- lo switch B è collegato alla porta `GigaBitEthernet0/0/1`



Una volta collegato il tutto, dobbiamo andare a **configurare il router**. Cosa ho fatto? 

Per prima cosa bisogna assegnare al router un indirizzo IP per parte, un'interfaccia verso lo switch A e un'interfaccia verso lo switch B (<u>ricordando di controllare anche la *subnet mask*</u>). 

Dall'immagine qua sotto vediamo la schermata di configurazione del router e i vari passaggi per inserire le interfacce per parte: 

<img src="/Users/iriscanole/Desktop/Screenshot 2025-09-25 alle 15.22.58.png" style="zoom:45%;" />

> [!WARNING]
>
> Fare attenzione alla "Port Status", che deve essere flaggata su ON, altrimenti non funziona.

Gli indirizzi IP che si vanno ad inserire, non sono altro che gli indirizzi di gateway dei dispositivi. 

> [!TIP]
>
> è possibile configurare il router anche tramite la CLI (Command Line Interface), cliccando sulla finestra dedicata (CLI per l'appunto) e inserendo i comandi direttamente da terminale. 



### Test e conclusioni

Una volta configurato il tutto, si può iniziare a testare l'intera connessione. 

> [!IMPORTANT]
>
> Cosa ci aspettiamo? Ci aspettiamo che i pacchetti che partono dai dispositivi collegati a una rete, arrivino ai dispositivi collegati a una rete differente. 

Il test lo eseguiamo utilizzando lo stesso tool citato nel repository citato all'inizio, ovvero la "*simulation*" di Cisco Packet Tracer. 

Il giro che ci si aspetta di vedere è il seguente: 

- PC1 (192.168.1.100 nel caso dell'immagine) crea il pacchetto IP;
- controlla la subnet, perché capisce che l'indirizzo IP del destinatario non è nella sua rete;
- incapsula il pacchetto in un frame livello 2 e manda allo Switch di pertinenza;
- Switch A inoltra il frame (legge il MAC e porta il frame al router);
- Il Router riceve il frame (legge il pacchetto IP con l'indirizzo IP del destinatario), consulta tabella di routing e manda a switch B;
- Switch B inoltra il frame e lo consegna a PC2 (192.168.2.112 nel caso dell'immagine)
- A questo punto, PC2 riceve il pacchetto - riconosce che l'indirizzo IP di destinazione è il suo e risponde creando un pacchetto inverso e lo manda al suo gateway.



> [!NOTE]
>
> - Gli IP rimangono uguali dall'inizio alla fine
> - I MAC cambiano a ogni salto, perché valgono solo nella rete locale (Livello 2)
> - Il router è l'unico che legge gli IP e decide come instradare.



Anche l'esercizio di oggi è stato testato e funziona in tutte le sue parti. Concludo lasciano degli screenshot del flusso, citato poche righe prima, dei pacchetti durante la modalità "*simulation*". 



<img src="/Users/iriscanole/Desktop/Screenshot 2025-09-25 alle 15.50.34.png" style="zoom:33%;" />

<img src="/Users/iriscanole/Desktop/Screenshot 2025-09-25 alle 15.51.07.png" style="zoom:33%;" />

<img src="/Users/iriscanole/Desktop/Screenshot 2025-09-25 alle 15.51.17.png" style="zoom:33%;" />

<img src="/Users/iriscanole/Desktop/Screenshot 2025-09-25 alle 15.52.17.png" style="zoom:33%;" />

<img src="/Users/iriscanole/Desktop/Screenshot 2025-09-25 alle 15.52.28.png" style="zoom:33%;" />
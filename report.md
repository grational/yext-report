# YEXT API
Yext mette a disposizione dei propri *Partner* le funzionalità della sua infrastruttura tramite delle REST API per permettere loro la creazione di piattaforme personalizzate per i propri *customer*.

---
#### Scan API
Permette di conoscere lo status delle informazioni riportate su di un *customer* sui vari siti di *listing*. Esiste una sola chiamata:
```
https://api.yext.com/v1/powerlistings/scan
```
Questa richiede una serie di parametri atti ad identificare in maniera il più possibile specifica l'attività (i.e., nome, indirizzo, telefono, città, stato, cap e codice internazionale del paese). Per evitare risultati non accurati si suggerisce di non tralasciare la compilazioni di nessun campo.

È possibile restringere la ricerca a specifici siti di listing utilizzando il parametro **siteId**. Per ogni paese Yext mette a disposizione uno specifico elenco di siti di *listing*. 

Per l'Italia sono disponibili: Google, Apple, AroundMe, Bing, Brownbook.net, Cylex, Factual, Here, IGlobal, Infobel, Navmii, OpenDI, Ricercare Imprese, Waze, WhereTo, Yalwa, Yelp. 

In aggiunta sono disponibili le seguenti piattaforme social: Facebook, Google+, Foursquare.

Maggiori info qui sulle piattaforme supportate da Yext per paese [qui](http://www.yext.com/products/listings/network/).

---
#### Location Manager API
Permette di controllare tutti gli aspetti di creazione, associazione e gestione relativi ad una *location* (l'effettivo oggetto di base per cui è stata costruita tutta l'infrastruttura Yext).

Dato uno specifico *customer* è possibile:
  - Ottenere una lista di tutte le *location* associate;
  - Aggiungere / Rimuovere location;

Specificando una coppia location/customer è possibile:
  - Controllare ed aggiornare le informazioni associate ad una *location*;
  - Controllare / Visualizzare le categorie di business associate ad un *customer*.

Per ogni *location* è richiesto di specificare **obbligatoriamente** almeno una categoria di business. 

I dati contenuti in una *location* sono suddivisi in gruppi:
  1. **Core Location Data**: dati di base che permettono la localizzazione dell'attività (e.g., nome, indirizzo, categoria di business, telefono, città, stato, cap);
  2. **Enhanced Content**: una tipologia di dati di arricchimento del profilo di una attività (e.g., foto, descrizione, video, categorie di business aggiuntive);
  3. **Enhanced Content List (ECL)**: menù, biografie dello staff, calendari, liste di prodotti, liste di servizi che possono essere aggiunte ai siti di *listing*;
  4. **Custom fields**: Campi aggiuntivi associati alla *location* che non saranno pubblicati ma utilizzabili per scopi interni (tipologie di dati supportate: date, URL, numeri, testo multilinea).
  5. **Profile-optimization tasks**: compiti legati alle location che i partner possono completare in autonomia o lasciare da completare ai loro clienti (e.g, aggiunta di un logo, collegamento ad uno specifico social network, and updating the Featured Message.	

Per le chiamate di aggiornamento prendiamo come esempio i dati presenti sull'opec di test "Pizza Pazza" di Milano:
  - Descrizione attività (
  - Orari di apertura
  - Categoria
  - Posizione
  - Foto
  - Video
  - Metodi di pagamento
  - Virtual Tour

3)  Aggiungere al processo la creazione della scheda google con i sottocasi
e la chiusura della stessa con aggancio tramite utilizzo delle api messe a disposizione da Yext.

4)      Aggiungere allo stesso modo Facebook ( come note, dovrebbe esserci la produzione della scheda Fb senza avere conferme del cliente se gli piace o meno, pero’ il cliente è contattato per i vari casi ). Il processo si chiude con l’aggancio della scheda come per google utilizzando Api di yext per il link

 

5)      Aggiungere il feedback di produzione verso sistemi commerciali e prima produzione verso il cliente.

 

---
#### (Power) Listing API
Le listing API sono utilizzate per ottenere informazioni sui siti di listing. In particolare:
  - conoscere i siti di listing associati ad una certa *location* (e quindi *customer*);
  - sapere quali sono i siti di listing (fra quelli disponibili per l'Italia) che sono stati rimossi dal controllo del *Powerlisting*;
  - rimuovere o aggiungere i siti di *listing* dal controllo del *Powerlisting*.

##### Listing Status
È una chiamata analoga a quella fornita dalla *Scan API* per determinare lo stato della specifica *location* all'interno dei siti di listing:

```
https://api.yext.com/v1/powerlistings/status
```

Esempio di chiamata:
```
GET /v1/powerlistings/status?api_key=xb7QeD92LnqpI&customerId=2938293&locationIds=78432&locationIds=85423&siteIds=YAHOO&siteIds=CITYSEARCH HTTP/1.1
Host: api.yext.com
```
Response
```
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
 
{"statuses": [
  {
    "locationId": "78432",
    "siteId": "YAHOO",
    "status": "LIVE",
    "url": "http://local.yahoo.com/info-79843407",
    "screenshotUrl": "http://www.yext-static.com/cms/b16e1d2b-9197-44fa-96bd-2a55c08c753b.png"
  },
  {
    "locationId": "78432",
    "siteId": "CITYSEARCH",
    "status": "LIVE",
    "url": "http://newyork.citysearch.com/profile/653931120/new_york_ny/meme_mediterranean.html",
    "screenshotUrl": "http://www.yext-static.com/cms/8a328098-0083-4ddc-8011-479efb6ba72c.png"
  },
  {
    "locationId": "85423",
    "siteId": "YAHOO",
    "status": "PROCESSING"
  },
  {
    "locationId": "85423",
    "siteId": "CITYSEARCH",
    "status": "BLOCKED",
    "blockedReasonsHtml":
      [
        "This location is already a Citysearch advertiser.",
        "Citysearch does not accept toll free numbers, please provide a local phone number"
      ]
  }
]}
```

---
#### Social API
Per il controllo delle informazioni sulle piattaforme Social (Facebook, Foursquare, Google+) è necessario soddisfare i seguenti pre-requisiti:

  1. Il *customer* a cui si desidera associare la piattaforma ha sottoscritto un pacchetto (e quindi ha un *Offer ID*) che copre la piattaforma social desiderata;
  2. Esiste almeno una location correttamente configurata per il *customer*;
  3. Il proprio account social è stato collegato a Yext tramite la Dashboard;
  4. La pagina locale dell'attività nella piattaforma social è stata collegata ad una *location* su Yext.

A questo punto sarà possibile:

  - Vedere una lista degli account social collegati a Yext;
  - Controllare le informazioni della pagina dell'attività tramite il *Powerlisting*;
  - Scrivere post/commenti e controllarne lo stato di gradimento;

##### Setup e requisiti per singole piattaforme

###### Facebook
Passaggi richiesti:
  - Avere una Facebook Local Business Page (in altri punti si accenna come sia possibile reclamare una pagina auto-generata)
  - Copertina e foto profilo già assegnate alla pagina
  - Account social con permessi *Admin* o *Editor* sulla pagina
  - Aver generato un token valido per Yext per l'edit e i post sulla pagina

**NOTA**: non viene indicata nella documentazione la procedura per il rilascio del token tramite la dashboard Yext (si suppone quindi che questo vada fatto tramite la piattaforma Facebook come di regola).

Non c'è limite al numero di pagine che un account facebook può amministrare.

###### Google+
Passaggi richiesti:
  - Avere una Google My Business listing (che ha anche una Google+ Local Page)
  - Possedere un account Google+ con status *Owner*, *Manager* o *Community Manager* per la pagina;
  - Aver generato un token OAuth valido per Yext per l'edit e i post sulla pagina

È lasciata allo *Yext Partner* la responsabilità della creazione, della richiesta e della verifica delle pagine Google+ associate alle *location* dei propri clienti. 

Dalla dashboard di Yext è possibile gestire il processo di generazione del token OAuth Google+ e rigenerarlo quando necessario.

Un account Google+ può amministrare fino a 100 pagine.

---
#### Ambiente di Test API
È disponibile un ambiente di test per le API di Yext all'indirizzo:
  - https://api-sandbox.yext.com

**NB**: le API di test richiedono delle credenziali differenti da quelle dell'ambiente di produzione (http://api.yext.com)

---
##### Glossario
* **Customer**: azienda od organizzazione che ha 1 o più *location* associate
* **Listing**: sito web che riporta informazioni sulle attività commerciali (a livello locale o globale) (e.g., Facebook, Foursquare, Google Places, Google Plus, NavMii, TomTom).
* **Location**: un business individuale che corrisponde ad un certo indirizzo, sempre associato ad un *customer*.
* **Business Category**: categoria merciologica associata obbligatoriamente ad una *location*;
* **Subscription**: iscrizione di un *customer* ad uno o più servizi offerti da Yext, richiede l'utilizzo di un *Offer ID* (i.e., iscrizione al *Powerlisting*; quest'ultimo legato obbligatoriamente ad una ed una sola location)
* **Offer ID**: codice rilasciato da Yext per quando uno dei clienti sottoscrive un pacchetto Yext (opzionalmente rivenduto dal Partner)
* **Partner ID**: codice rilasciato da Yext ai Partner assieme ad una username e password per l'utilizzo delle API
* **Api key**: token alfanumerico richiedibile tramite la dashboard Yext per l'utilizzo delle API
* **Customer ID**: codice rilasciato dalla piattaforma per identificare un singolo cliente

---
### Risorse aggiuntive

* [Facebook sync](https://partners.yext.com/community/intl/syncing-facebook-yext/)
* [Google+ sync](https://partners.yext.com/community/yext-update/how-to-enable-google-scanning-in-your-listing-scan/)
* [API comparison](https://www.brightlocal.com/2014/05/27/mozlocal-vs-yext-vs-ubl-vs-brightlocal-vs-whitspark/)
* [Tech Specs](http://www.yext.com/technical-specifications/)
* [Resources](http://www.yext.com/resources/)

[//]: # (These are reference links used in the body of this note and get stripped out when the markdown processor does its job. There is no need to format nicely because it shouldn't be seen. Thanks SO - http://stackoverflow.com/questions/4823468/store-comments-in-markdown-syntax)


   [dill]: <https://github.com/joemccann/dillinger>
   [git-repo-url]: <https://github.com/joemccann/dillinger.git>
   [john gruber]: <http://daringfireball.net>
   [@thomasfuchs]: <http://twitter.com/thomasfuchs>
   [df1]: <http://daringfireball.net/projects/markdown/>
   [marked]: <https://github.com/chjj/marked>
   [Ace Editor]: <http://ace.ajax.org>
   [node.js]: <http://nodejs.org>
   [Twitter Bootstrap]: <http://twitter.github.com/bootstrap/>
   [keymaster.js]: <https://github.com/madrobby/keymaster>
   [jQuery]: <http://jquery.com>
   [@tjholowaychuk]: <http://twitter.com/tjholowaychuk>
   [express]: <http://expressjs.com>
   [AngularJS]: <http://angularjs.org>
   [Gulp]: <http://gulpjs.com>

   [PlDb]: <https://github.com/joemccann/dillinger/tree/master/plugins/dropbox/README.md>
   [PlGh]:  <https://github.com/joemccann/dillinger/tree/master/plugins/github/README.md>
   [PlGd]: <https://github.com/joemccann/dillinger/tree/master/plugins/googledrive/README.md>
   [PlOd]: <https://github.com/joemccann/dillinger/tree/master/plugins/onedrive/README.md>


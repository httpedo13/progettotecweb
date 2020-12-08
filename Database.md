# Database

## Tabelle

- ```Users(ID, NomeUtente, PWD, Permessi);```
  - Permessi = _booleano_ -> se _true_ = admin; se _false_ = editors (ci possono essere più editors, ma un solo admin)  
  - L’admin ha il completo controllo su tutto. Può aggiungere, modificare ed eliminare un lavoro e un evento. Gli editors possono creare e modificare il contenuto e la visibilità di un lavoro ma non possono eliminarlo. Stessa cosa per gli eventi, con la sola differenza che non possono modificarne la visibilità. 

- ```Lavori(ID, Titolo, pinn, visibilita, evento, posizione);```
  - sono le categorie di lavori possibili (lauree, matrimoni, ecc…). 
  - ```pinn``` = indica le immagini pinnate, ovvero quelle che affiancano i paragrafi (in base al num. di paragrafi)
  - ```visibilita``` = indica se la categoria è mostrata oppure no
  - ```evento``` = _booleano_ -> _false_ = lavoro normale, _true_ = evento. Sono tipi di lavori che si alternano durante l’anno -> ce n’è sempre uno (e uno soltanto) -> __Eventi__: San Valentino, Festa della Donna, Festa del Papà, Pasqua, Festa della Mamma, Estate, Autunno, Festa dei Morti, Natale
  - ```posizione``` = se il record è riferito ad un _evento_ (quindi attributo ```evento = true```) il valore deve essere ```NULL```. In caso contrario, indica in che ordine verranno mostrati i vari lavori nel menù laterale nella pagina ```lavori.html```.

- ```Immagini(ID, percorso_immagine, alt, pinned, visibilita, IDlavoro);```
  - ```pinned```, _booleano_ -> _true_ = se l’immagine è una di quelle del paragrafo, _false_ = immagine della galleria. Nell’interfaccia admin/editor si decide quali pinnare e quali no.
  - ```visibilita```, _booleano_ -> _true_ = immagine visibile e solo in questo caso può essere messa (con la funzione ```random()``` nella home), _false_ = immagine non visibile. 

- ```Paragrafi(ID, contenuto_paragrafo, visibilita, IDlavoro);```
  - unico accorgimento: controllare sempre che il numero di paragrafi visibili (attributo ```visibilita```) di un lavoro è uguale al numero di immagini pinnate (attributo ```pinn``` nella tabella ```Lavori```)

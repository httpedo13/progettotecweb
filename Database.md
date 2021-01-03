# DATABASE

## TABELLE

- ```users(ID, NomeUtente, PWD, Permessi);```
  - Permessi = _booleano_ -> se _true_ = admin; se _false_ = editors (ci possono essere più editors, ma un solo admin)  
  - L’admin ha il completo controllo su tutto. Può aggiungere, modificare ed eliminare un lavoro e un evento. Gli editors possono creare e modificare il contenuto e la visibilità di un lavoro ma non possono eliminarlo. Stessa cosa per gli eventi, con la sola differenza che non possono modificarne la visibilità. 
- ```lavori(ID, Titolo, pinn, visibilita, evento, posizione);```
  - sono le categorie di lavori possibili (lauree, matrimoni, ecc…). 
  - ```pinn``` = indica le immagini pinnate, ovvero quelle che affiancano i paragrafi (in base al num. di paragrafi)
  - ```visibilita``` = indica se la categoria è mostrata oppure no
  - ```evento``` = _booleano_ -> _false_ = lavoro normale, _true_ = evento. Sono tipi di lavori che si alternano durante l’anno -> ce n’è sempre uno (e uno soltanto) -> __Eventi__: San Valentino, Festa della Donna, Festa del Papà, Pasqua, Festa della Mamma, Estate, Autunno, Festa dei Morti, Natale
  - ```posizione``` = se il record è riferito ad un _evento_ (quindi attributo ```evento = true```) il valore deve essere ```NULL```. In caso contrario, indica in che ordine verranno mostrati i vari lavori nel menù laterale nella pagina ```lavori.html```.
- ```immagini(ID, percorso_immagine, alt, pinned, visibilita, IDlavoro);```
  - ```pinned```, _booleano_ -> _true_ = se l’immagine è una di quelle del paragrafo, _false_ = immagine della galleria. Nell’interfaccia admin/editor si decide quali pinnare e quali no.
  - ```visibilita```, _booleano_ -> _true_ = immagine visibile e solo in questo caso può essere messa (con la funzione ```random()``` nella home), _false_ = immagine non visibile. 
- ```paragrafi(ID, contenuto_paragrafo, visibilita, IDlavoro);```
  - unico accorgimento: controllare sempre che il numero di paragrafi visibili (attributo ```visibilita```) di un lavoro è uguale al numero di immagini pinnate (attributo ```pinn``` nella tabella ```Lavori```)
- ```accessi(ID, IDUtente, InizioSessione);```

## CODICE

```mysql
CREATE TABLE IF NOT EXISTS `users` (
  `ID` int(11) NOT NULL PRIMARY KEY,
  `NomeUtente` varchar(25) NOT NULL,
  `PWD` varchar(100) NOT NULL,
  `Permessi` tinyint(1) NOT NULL,
  `IDUltimoAccesso` int(4) NOT NULL
);
INSERT INTO `users` (`ID`, `NomeUtente`, `PWD`, `Permessi`, `IDUltimoAccesso`) VALUES
(1, 'Admin', 'adminadmin', 1, 0),
(2, 'Editor1', 'ed1ed1', 0, 0),
(3, 'Editor2', 'ed2ed2', 0, 0);

CREATE TABLE IF NOT EXISTS `lavori` (
  `ID` int(4) NOT NULL PRIMARY KEY,
  `Titolo` varchar(25) NOT NULL,
  `pinn` int(4) NOT NULL,
  `visibilita` tinyint(1) NOT NULL,
  `evento` tinyint(1) NOT NULL,
  `posizione` int(11)
);
INSERT INTO `lavori` (`ID`, `Titolo`, `pinn`, `visibilita`, `evento`, `posizione`) VALUES
(1, 'Laurea', 4, 1, 0, 1),
(2, 'San Valentino', 3, 1, 1, NULL),
(3, 'Matrimonio', 5, 1, 0, 3),
(4, 'Festa della Mamma', 3, 0, 1, NULL);

CREATE TABLE IF NOT EXISTS `accessi` (
  `ID` int(11) NOT NULL PRIMARY KEY,
  `IDUtente` int(11) NOT NULL,
  `InizioSessione` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  FOREIGN KEY(`IDUtente`) REFERENCES users(`ID`)   
);
INSERT INTO `accessi` (`ID`, `IDUtente`, `InizioSessione`) VALUES
(1, 2, '2020-12-28 15:56'),
(2, 3, '2020-12-30 11:37'),
(3, 2, '2021-01-02 10:12'),
(4, 2, '2021-01-02 18:46'),
(5, 3, '2021-01-03 18:46');

CREATE TABLE IF NOT EXISTS `immagini` (
  `ID` int(4) NOT NULL PRIMARY KEY,
  `percorso` varchar(50) NOT NULL,
  `alt` varchar(50) NOT NULL,
  `pinned` tinyint(1) NOT NULL,
  `visibilita` tinyint(1) NOT NULL,
  `IDLavoro` int(4) NOT NULL,
  FOREIGN KEY(`IDLavoro`) REFERENCES lavori(`ID`) 
);

CREATE TABLE IF NOT EXISTS `impostazioni` (
  `ID` int(11) NOT NULL PRIMARY KEY,
  `Nome` varchar(255) NOT NULL,
  `contenuto` varchar(255) NOT NULL
);
INSERT INTO `impostazioni` (`ID`, `Nome`, `contenuto`) VALUES
(1, 'Logo', 'indirizzo percorso del logo'),
(2, 'Orario', 'Testo con orari');

CREATE TABLE IF NOT EXISTS `paragrafi` (
  `ID` int(4) NOT NULL PRIMARY KEY,
  `contenuto` varchar(255) NOT NULL,
  `visibilita` tinyint(1) NOT NULL,
  `IDLavoro` int(4) NOT NULL,
   FOREIGN KEY(`IDLavoro`) REFERENCES lavori(`ID`) 
);
```


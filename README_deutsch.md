# ASCII-Mapper-2.0

Neu, maechtiger und nutzt nun einen besseren Programmierstil!

Ich empfehle nicht die vorige Version von meinem ASCII-Mapper zu benutzen, da der Code dort wirklich grauenvoll ist!
Ich schaeme mich, da ich eine Menge von globalen Variablen dabei verbraucht habe. Aber lasst uns lieber ueber schoenere Sachen reden, wie die Version 2.0!

## Was kann ich mit dem ASCII-Mapper 2.0?

Wenn du in ein neues Gebiet gehst, dann kannst du den ASCII-Mapper 2.0 aktivieren und dieser wird deine Bewegung aufzeichnen.
Auf Abruf wird dir eine Karte von der Gegend, eingeschraenkt auf die bereits besuchten Raeume, in ASCII Art gezeichnet, wie hier:

 	 Z           
 	  \          
 	   \   |     
 	   -O--O     
 	    |\/|     
 	    |/\|     
 	   -O--O--O-



Wenn man eine kleinere Version der Karte bevorzugt, dann kann man es sich auch so darstellen lassen:


	Z       
 	 \  |   
 	 -O-O   
 	  |X|   
 	 -O-O-O-


 WARNUNG: Der Mapper funktioniert nur, wenn die folgenden Bedingungen erfuellt sind:
 - Jeder Raum muss eine eindeutige Raum ID haben
 - Die Raeume muessen auf einem Gitter liegen
 - Die Ausgaenge sind beschraenkt auf die 8 Hauptrichtungen, oben und unten.

Die zweite Bedingungen ist sehr wichtig. Wenn du nach Norden und dann wieder zurueck nach Sueden gehst, solltest du im selben Raum langen, in dem du gestartet bist. Wenn du nach Nordosten gehst, dann gehst du auch automatisch eine Einheit nach Norden und eine Einheit nach Osten gleichzeitig. Das bedeutet, dass wenn du nach Nordosten und dann Suedosten gehst, dass du zwei Einheiten nach Osten gegangen bist. Damit sollte es auch zwei Einheiten nach Westen gehen muessen um wieder im Startraum zu landen.

Die dritte Bedingung ist wichtig, da es schwer ist mit Richtungen, wie "Suedostoben" umzugehen.

## Kartenlegende
- "O" ist ein nicht markierter Raum
- "Z" ist der Raum in dem du stehst (oder der Mapper gestoppt wurde)
- "B" ist ein Raum in der du nach oben und unten gehstn kannst (um eine andere Ebene zu betreten)
- "R" ist ein Raum in dem du nach unten gehen kannst (um eine tiefere Ebene zu betreten)
- "H" ist ein Raum in dem du nach oben gehen kannst (um eine hoehere Ebene zu betreten


## Wie kann ich den ASCII-Mapper 2.0 benutzen?

Zuerst ist es wichtig, dass der Mapper ein ausgerufenes Ereignis benötigt, welches die benutze Richtung und die Raum ID sendet.
Ich bevorzuge dafuer das Nummernfeld zu nutzen und habe die Funktionen des Mappers so geschrieben, dass sie mit meinem Skript "evented navigation" (https://github.com/Mundron/Evented-Navigation) zusammen funktionieren.
Wenn du bereits dieses Skript nutzt, da es zum Beispiel ein anderes Skript von mir benoetigt, dann musst du es keinesfalls nochmal installieren oder etwas aendern. Es reicht einfach zum naechsten Schritt zu gehen:
Dann installiert man "Mapper_script_part" und "Mapper_alias_part".
Der "script part" enthaelt den Basiscode mit all den noetigen Funktionen.
Um es im Spiel zu nutzen, muss man Aliase erstellen, welche die Funktionen des Mappers aufrufen.
Dazu sind die Aliase in "alias part" ein Vorschlag, wie das umgesetzt werden kann.


## Wie kann ich den ASCII-Mapper 2.0 im Spiel bedienen?
Wenn du meinen "alias part" benutzen moechtest, dann kannst du folgende Aliase benutzen:

### \#mapper <an|aus|weiter>
Wie der Name schon vermuten laesst, kann man mit "\#mapper an" den Mapper einschalten. Dabei wird es initialisiert mit einer leeren Karten.
Desweiteren stoppt "\mapper aus" den Mapper. Du kannst jedoch weiter jeder Zeit dir die Karte anzeigen lassen, solange du den Mapper nicht neu initialisiert hast.
Schliesslich wird der Mapper mit "\#mapper weiter" wieder weiter ausgefuehrt, vorausgesetzt, dass du in dem Raum stehst, wo der Mapper angehalten wurde.

### \#map
Dies zeigt die dir doppel grosse Karte im Spiel an. Wenn du die kleine Version bevorzugst, dann musst du im Namen der Funktion, die in diesem Alias stehst, "doublesize" einfach loeschen.

### \#markiere <X>
X muss eine Ziffer oder ein Buchstaben sein. Dieses Zeichen wir auf der angezeigten Karte zu diesem Raum angezeigt.

### \#addstop <ausgang>
Das Arguement sollte ein Ausgang in diesem Raum sein. Wenn du diesen Ausgang dann benutzt, dann stoppt sich der Mapper automatisch und geht auch automatisch wieder an, wenn ein anderer Ausgang genutzt wird.

### \#delstop <ausgang>
Diese Funktion kann sowohl mit oder ohne Argument aufgerufen werden. Wenn du als Argument einen Ausgang eingibst, dann wird der entsprechende Stopper, sie "\#addstop" wieder geloescht. Wird es ganz ohne ein Argument, also "\#delstop" aufgerufen, dann werden alle existierenden Stopper geloescht.

### \#savemap <filename>
Diese Funktion speichert die Karte in einer Datei, dessen Name durch das Argument bestimmt wird. Aber um diese zu nutzen, muss einmal der bevorzugte Ordner im Skript im Element "Mapper core" in Zeile 33 (savedirectory="C:/") eingegeben werden.
Wenn du die kleine Version bevorzugst, dann musst du im Namen der Funktion, die in diesem Alias stehst, "doublesize" einfach loeschen.

## ASCII Mapper Functions

Der Mapper ist eine Art Objekt. Wenn man es benutzen will, dann initializiert man es. Man kann es aber ebenso stoppen oder weiterlaufen lassen. Wenn der Mapper aktiviert ist, dann arbeitet es eigenstaendig. Du musst keine Raeume oder Wege manuell hinzufuegen.
Jedes Mal, wenn du einen neuen Raum betrittst, aktualisiert sich der Mapper von selbst. Somit fuehrt es zu keinen Problemen, wenn du verstehentlich einen nicht vorhandenen Ausgang eingibst oder ein Ausgang von etwas geblockt wird.
Desweiteren schaltet sich der Mapper automatisch ab, wenn es einen Widerspruch zu einen der Bedingungen findet. Gibt es in Raum auch nicht zulaessige Ausgaenge, wie "suedostunten", dann gibt der Mapper nur eine Warnung. Aber wenn du einen solchen Ausgang auch benutzt, dann wird sich der Mapper automatisch abschalten und Fehler zu vermeiden.
Darueber hinaus kann man auch noch ein Paar Informationen in den Mapper einfuettern.
Zum Beispiel kann man den Raum markieren, in dem man sich gerade befindet. Die Markierung ist eine einstellige Zahl oder ein einzelner Buchstabe. Spaeter wird diese Markierung auf der angezeigten Karte eingefuegt.
Zusaetzlich kann man noch eine Art Checkpoint mit einem Ausgang hinzufuegen. Wenn du diesen Ausgang am Checkpoint benutzt, dann wird der Mapper sich automatisch abschalten und wenn du einen anderen Ausgang am Checkpoint benutzt, dann schaltet sich der Mapper auch wieder automatisch ein. Damit kann man den Beginn einer Region kennzeichnen und der Mapper wird nur innerhalb dieser Region funktionieren. Natuerlich lassen sich auch mehrere Ausgaenge an einem Checkpoint eintragen, sowie auch mehrere Checkpoints moeglich sind.
Wenn du den Client schliesst oder wenn du den Mapper in einer anderen Gegend benutzen willst und neu initiierst, dann geht die Karte leider verloren. Allerdings besetzt die Moeglichkeit die Karte in einer Textdatei speichern.

## --	ASCIIMapper:init()												 --
Wenn diese Funktion aufgerufen wird, dann wird der Mapper neu initialisiert.
Um den Mapper in einer neuen Gegend zu benutzen, muss dies aufgerufen werdne.
Aber beachte, dass dabei die alte Karte geloescht wird!
Daher solltest du diese vorher speichert, wenn du sie behalten willst.

## --	ASCIIMapper:markroom(s)										 --
Mit dieser Funktion wird eine einstellige Zahl oder ein einzelner Buchstabe als Markierung auf der Karte hinzugefuegt.

## --	ASCIIMapper:addblocker(dir)								 --
Wenn diese Funktion mit einer Richtung als Argument ausgerufen wird,
dann wird der aktuelle Raum als Checkpoint gespeichert mit der Richtung als Ausgang.
Wenn du den Raum durch den gespeicherten Ausgang verlaesst, dann schaltet
sich der Mapper automatisch ab und auch wieder automatisch an, wenn
in dem Raum ein anderer Ausgang benutzt wird.
Auf diese Weise kann man Grenzen eines Gebiets markieren, welches du
kartographieren willst.

## --	ASCIIMapper:deleteblocker(dir)						 --
Hiermit kann man einen Stopper-Ausgang (siehe obige Funktion) wieder loeschen.

## --	ASCIIMapper:deleteallblocker()						 --
Alle Checkpoints werden geloescht.

## --	ASCIIMapper:stop()
Man kann den Mapper auch manuell stoppen, wenn man etwas anderes machen
will oder fertig ist.

## --	ASCIIMapper:continue()										 --
Man kann auch den Mapper wieder weiter ausfuehren lassen.
ABER dazu muss man im selben Raum stehen, wo der Mapper ausgeschaltet wurde!

## --	ASCIIMapper:showmap()											 --
Zeigt die ganze Karte (kleine Version) einschliesslich allen Ebenen im Spiel.

## --	ASCIIMapper:showmapdoublesize()						 --
Zeigt die ganze Karte (grosse Version) einschliesslich allen Ebenen im Spiel.

## --	ASCIIMapper:savemap(filename)							 --
Speichert die Karte (kleine Version) in einer Textdatei. Der Speicherpfad muss im Code angepasst werden! Nachdem das Paket im Client importiert wurde, kann man den bevorzugten Speicherort im Skript Element "Mapper Core" in Zeile 33 eingeben.

## --	ASCIIMapper:savemapdoublesize(filename)		 --
Speichert die Karte (grosse Version) in einer Textdatei. Der Speicherpfad muss im Code angepasst werden! Nachdem das Paket im Client importiert wurde, kann man den bevorzugten Speicherort im Skript Element "Mapper Core" in Zeile 33 eingeben.

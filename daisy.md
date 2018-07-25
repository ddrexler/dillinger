My Drive

# Einbinden einer neuen Vulnerabilty

Im Zuge der �berarbeitung von Daisy wurde auch eine neue Vulnerability im System geschaffen, f�r die nat�rlich ein entsprechendes Reward-Token angelegt wurde.

## XML External Entities
Nach einiger Recherche fiel die Wahl f�r die neue Schwachstelle auf XML External Entities (XXE).

### Erkl�rung der Schwachstelle
XML (Exstensible Markup Language) ist eine Auszeichnungssprache, welche f�r standardisierte Daten�bertragung genutzt wird.

### DTD und XML-Schemas

F�r jedes XML-Dokument muss dabei eine Definition verf�gbar sein, welche festlegt, welchen Kriterien das Dokument zu entsprechen hat, um �berhaupt vom XML-Parser akzeptiert zu werden.

Hierf�r haben sich zwei Methoden etabliert:

 - Doc Type Declaration (DTD) und
 - XML Schemas

DTDs, die in Form von Metadaten am Kopf des Dokuments festgelegt werden, geben dabei nur die Basisgrammatik f�r eine XML-Datei an.

XML Schemas, welche in separaten XML-Dateien festgelegt werden und selbst �ber eine DTD verf�gen, gehen einen Schritt weiter und definieren auf detaillierte Art und Weise, welche Daten und in welcher Form eine XML enthalten und nicht enthalten kann.

Um konkreter festzulegen, wie eine XML-Datei in einem bestimmten Kontext auszusehen hat, empfiehlt es sich also, XML Schemas zu benutzen.

Dazu kommt, dass DTD zu schwerwiegenden Sicherheitsschwachstellen f�hren kann, welche im folgenden Absatz erl�utert werden.

Verf�gbare Parser in vielen Programmiersprachen erlauben in der Standardkonfiguration eine blo�e DTD mittlerweile nicht mehr, allerdings kann dies vom Programmierer nat�rlich �berschrieben werden.

### DTD - Externe Entit�ten

DTD erlaubt das Einbinden sogenannter externer Entit�ten. Entit�ten k�nnen dabei zum leichteren Verst�ndnis mit Variablen in Programmiersprachen verglichen werden. Mit Entit�ten kann ein Wert klar einem Schl�sselwort zugewiesen werden. So kann beispielsweise ein l�ngerer String welcher an mehreren Stellen im Dokument aufscheinen soll, an mehreren stellen mit dem deutlich k�rzeren Schl�sselwort referenziert werden.

### Auslesen beliebiger Dateien

Allerdings, und hier entsteht nun die Gefahr, muss der String nicht direkt im Dokument definiert werden, sondern kann beispielsweise auch mittels einem �bergebenen Pfad vom XML-Parser aus einer Textdatei ausgelesen werden.

Diese Art des Angriffs klappt nur dann, wenn der Angreifer nach dem Einspielen der XML Datei eine R�ckgabe der geparsten Datei erh�lt. Ist dies nicht der Fall, so kann mittels einer "Blind"-Variante, wo eine Datei auf einem externen Server, den der Angreifer unter seiner eigenen Kontrolle hat, referenziert wird, zumindest festgestellt werden, ob der Angriff m�glich ist.

Die Variante mit Textr�ckgabe kommt in weiterer Folge in der neuen Daisy-Schwachstelle zum Einsatz um ein Reward-Token auszulesen. Dem Angreifer wird dabei auch die geparste XML-Datei angezeigt, welche nach erfolgreichem Angriff, das Token erhalten wird.

### Remote Code Execution

Das Gefahrenpotenzial ist sogar noch gr��er, da mittels eines XXE-Exploits auch Remote Code Execution, also das Ausf�hren beliebigen Schadcodes auf den angegriffenen Systemen erm�glicht wird.

Dies ist unter anderem bei PHP-Systemen m�glich, wo beispielsweise PHP-Dateien �ber XXE ausgelesen werden k�nnen, welche Input-Funktionalit�t o.�. enthalten und dann direkt ausgef�hrt werden.

Allerdings kommt diese Variante hier nicht zum Einsatz.

## �nderungen in Daisy

Um die Schwachstelle in Daisy einbetten zu k�nnen, war klar, dass eine M�glichkeit f�r den Spieler geschaffen werden musste, eine XML-Datei hochzuladen.

Im Hinblick auf den "Shop-Kontext" von Daisy wurde entschieden, eine Upload-Maske f�r Produkte zu integrieren. Der Upload erfolgt dabei nat�rlich �ber XML, das neu zu erstellende Produkt soll in diesem Format beschrieben werden.

Die Maske ist dabei nicht f�r alle User ersichtlich. Die Rolle des zweiten Users wurde von der einer Standardrolle auf die Rolle des sogenannten "Shop Owners" ge�ndert. Der Spieler erh�lt Zugriff auf diesen zweiten User durch Ausn�tzen der Inscure Direct Object Reference aus der urspr�nglichen Implementierung.
# wie genau? XXX

Nach erfolgtem Zugrif auf die Maske kann 



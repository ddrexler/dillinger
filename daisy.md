My Drive

# Einbinden einer neuen Vulnerabilty

Im Zuge der Überarbeitung von Daisy wurde auch eine neue Vulnerability im System geschaffen, für die natürlich ein entsprechendes Reward-Token angelegt wurde.

## XML External Entities
Nach einiger Recherche fiel die Wahl für die neue Schwachstelle auf XML External Entities (XXE).

### Erklärung der Schwachstelle
XML (Exstensible Markup Language) ist eine Auszeichnungssprache, welche für standardisierte Datenübertragung genutzt wird.

### DTD und XML-Schemas

Für jedes XML-Dokument muss dabei eine Definition verfügbar sein, welche festlegt, welchen Kriterien das Dokument zu entsprechen hat, um überhaupt vom XML-Parser akzeptiert zu werden.

Hierfür haben sich zwei Methoden etabliert:

 - Doc Type Declaration (DTD) und
 - XML Schemas

DTDs, die in Form von Metadaten am Kopf des Dokuments festgelegt werden, geben dabei nur die Basisgrammatik für eine XML-Datei an.

XML Schemas, welche in separaten XML-Dateien festgelegt werden und selbst über eine DTD verfügen, gehen einen Schritt weiter und definieren auf detaillierte Art und Weise, welche Daten und in welcher Form eine XML enthalten und nicht enthalten kann.

Um konkreter festzulegen, wie eine XML-Datei in einem bestimmten Kontext auszusehen hat, empfiehlt es sich also, XML Schemas zu benutzen.

Dazu kommt, dass DTD zu schwerwiegenden Sicherheitsschwachstellen führen kann, welche im folgenden Absatz erläutert werden.

Verfügbare Parser in vielen Programmiersprachen erlauben in der Standardkonfiguration eine bloße DTD mittlerweile nicht mehr, allerdings kann dies vom Programmierer natürlich überschrieben werden.

### DTD - Externe Entitäten

DTD erlaubt das Einbinden sogenannter externer Entitäten. Entitäten können dabei zum leichteren Verständnis mit Variablen in Programmiersprachen verglichen werden. Mit Entitäten kann ein Wert klar einem Schlüsselwort zugewiesen werden. So kann beispielsweise ein längerer String welcher an mehreren Stellen im Dokument aufscheinen soll, an mehreren stellen mit dem deutlich kürzeren Schlüsselwort referenziert werden.

### Auslesen beliebiger Dateien

Allerdings, und hier entsteht nun die Gefahr, muss der String nicht direkt im Dokument definiert werden, sondern kann beispielsweise auch mittels einem übergebenen Pfad vom XML-Parser aus einer Textdatei ausgelesen werden.

Diese Art des Angriffs klappt nur dann, wenn der Angreifer nach dem Einspielen der XML Datei eine Rückgabe der geparsten Datei erhält. Ist dies nicht der Fall, so kann mittels einer "Blind"-Variante, wo eine Datei auf einem externen Server, den der Angreifer unter seiner eigenen Kontrolle hat, referenziert wird, zumindest festgestellt werden, ob der Angriff möglich ist.

Die Variante mit Textrückgabe kommt in weiterer Folge in der neuen Daisy-Schwachstelle zum Einsatz um ein Reward-Token auszulesen. Dem Angreifer wird dabei auch die geparste XML-Datei angezeigt, welche nach erfolgreichem Angriff, das Token erhalten wird.

### Remote Code Execution

Das Gefahrenpotenzial ist sogar noch größer, da mittels eines XXE-Exploits auch Remote Code Execution, also das Ausführen beliebigen Schadcodes auf den angegriffenen Systemen ermöglicht wird.

Dies ist unter anderem bei PHP-Systemen möglich, wo beispielsweise PHP-Dateien über XXE ausgelesen werden können, welche Input-Funktionalität o.Ä. enthalten und dann direkt ausgeführt werden.

Allerdings kommt diese Variante hier nicht zum Einsatz.

## Änderungen in Daisy

Um die Schwachstelle in Daisy einbetten zu können, war klar, dass eine Möglichkeit für den Spieler geschaffen werden musste, eine XML-Datei hochzuladen.

Im Hinblick auf den "Shop-Kontext" von Daisy wurde entschieden, eine Upload-Maske für Produkte zu integrieren. Der Upload erfolgt dabei natürlich über XML, das neu zu erstellende Produkt soll in diesem Format beschrieben werden.

Die Maske ist dabei nicht für alle User ersichtlich. Die Rolle des zweiten Users wurde von der einer Standardrolle auf die Rolle des sogenannten "Shop Owners" geändert. Der Spieler erhält Zugriff auf diesen zweiten User durch Ausnützen der Inscure Direct Object Reference aus der ursprünglichen Implementierung.
# wie genau? XXX

Nach erfolgtem Zugrif auf die Maske kann 



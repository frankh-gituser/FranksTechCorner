# ESP Programmierung mit Arduino

## Was ist eigentlich „Arduino“?
Um unseren Mikrocontroller zu programmieren, benötigen wir eine Entwicklungsumgebung. 
Hierzu bietet sich u.a. Arduino an, eine Open-Source-Prototyping-Plattform. 
Arduino bietet einen einfachen Zugang zum Thema „Mikrocontrolling“ an und ermöglicht die Entwicklung kreativer Projekte. 
Hierzu gibt es eine grosse Zahl an Beispielen in den Foren und Communities, 
so dass es auch für einen Einsteiger in die Thematik keine unüberwindbare Herausforderung darstellt.

## Hardware und Software
Der Begriff Arduino [link](https://de.wikipedia.org/wiki/Arduino_(Plattform) ) wird sowohl für die verschiedenen 
„Arduino-Boards“ (Hardware) als auch für die Programmierumgebung (Software) verwendet. 
In unserem Fall verwenden wir ein Arduino kompatibles Board , den ESP 8266 D1 Mini. 
Die Softwareplattform kann sowohl auf Windows, als auch zB auf MacOS installiert werden.

### Hardware
Der „Arduino“ ist ein sogenanntes Mikrocontroller-Board (im weiteren Verlauf „Board“ genannt). Also im Grunde eine Leiterplatte (Board) mit jeder Menge Elektronik rund um den eigentlichen Mikrocontroller. Die Anschlüsse an den Rändern des Boards werden als PINs bezeichnet, an die wir dann später die unterschiedlichen Geräte, Sensoren etc anschliessen. Hier nochmal wichtig den Hinweis auf das mapping zwischen Pins und GPIOs beachten, damit ihr in dem Sketch Code nicht eine 1:1 Verbindung verwendet. 
Bei den Boards gibt es verschiedene Versionen von verschiedenen Herstellern. Wichtig ist hier vorallem die Kompatibilität der Boards mit Arduino. Dann findet ihr eine grosse Auswahl an Boards/Herstellern.
￼

### Software
Die Software, mit welcher der Mikrocontroller programmiert wird, ist frei verfügbar, also Open-Source-Software und kann auf www.arduino.cc kostenlos heruntergeladen werden. Mit dieser „Arduino-Software“ schreibt man dann kleine Programme, die der Mikrocontroller später ausführen soll. Diese kleinen Programme werden „Sketch“ genannt.
Per USB-Kabel werden die fertigen Sketches dann auf den Mikrocontroller übertragen.

Zwei wichtige Einstellungen gibt es im Programm zu beachten.
a) Es muss das richtige Board ausgewählt werden: In diesem Fall wähle ich ein D1 kompatibles Board „WeMos D1“ aus
b) Es muss der richtige „Serial-Port“ ausgewählt werden, damit das Programm (Sketch) auf das Board geladen werden kann. 
Hierzu muss der Mikrocontroller mit einem USB Kabel an den Computer angeschlossen werden , 
wobei das USB Kabel neben der Stromversorgung auch für die Datenübertragung geeignet sein muss, 
d.h. ein USB Kabel kompatibel mit Arduino und Nano V3 ! 
(Hier ein Beispiel)
[Amazon link](https://www.amazon.de/AZDelivery-Arduino-Nano-Kompatibel-V3/dp/B07CP4NCH3/ref=sr_1_22_sspa?__mk_de_DE=%C3%85M%C3%85%C5%BD%C3%95%C3%91&keywords=usb+cable+esp8266&qid=1638636069&sr=8-22-spons&psc=1&smid=A1X7QLRQH87QA3&spLa=ZW5jcnlwdGVkUXVhbGlmaWVyPUExVVpETE9HVDFYVjFaJmVuY3J5cHRlZElkPUEwNTU5NzQyMjBSTlk5TDAxSzhMMyZlbmNyeXB0ZWRBZElkPUEwNDEyMzI5MUhJREQyWk8zNVhDMyZ3aWRnZXROYW1lPXNwX2J0ZiZhY3Rpb249Y2xpY2tSZWRpcmVjdCZkb05vdExvZ0NsaWNrPXRydWU= )


Nun klickt man in dem Untermenü  auf „Port“.  
Dort werden mehrere Ports zu sehen sein und der richtige Port muss dann entsprechend eingestellt werden.

<img src="{{ site.baseurl }}/images/kuerbis/Port-Arduino-Auswahl.jpg" width='350'>


### Bibliotheken zur Arduino Software hinzufügen

Bibliotheken (auch Library genannt)  vereinfachen die Programmierung, da im Code auf Funktionen aus der Bibliothek zurückgegriffen 
werden kann. Damit ersparen wir uns die eigene Programmierung von Funktionen, 
die für bestimmte Aufgaben immer wieder benötigt werden und somit nicht immer in unseren Sketches als Code eingefügt werden müssen.

Im weiteren Verlauf wird auf solche Bibliotheken zurück gegriffen. 
Bevor sie in unserem Sketch eingebunden werden können, müssen sie erst in der Arduino Software hinzugefügt werden. 
Dazu gibt es verschiedene Möglichkeiten.
Die einfachste Möglichkeit bietet sich durch die Funktion „Bibliotheken verwalten…“. 
Diese befindet sich in der Software unter „Sketch > Bibliothek einbinden > Bibliotheken verwalten…“ 
Dort kann über das Suchfeld die gewünschte Library gesucht und direkt installiert werden. 
Nach der erfolgreichen installation kann die Bibliothek direkt verwendet werden. 
Mit der Installation von Programmbibliotheken werden häufig auch gleichzeitig Beispielsketche zur Arduinosoftware hinzugefügt. 
Diese Beispiele befinden sich unter „Datei > Beispiele“ und können einen guten Einblick in die einzelnen Funktionen der jeweiligen 
Bibliothek geben. 
Die ersten Schritte mit neuen Zusatzkomponenten wie LED Ringen, OLED Displays etc werden hierdurch deutlich vereinfacht.

<img src="{{ site.baseurl }}/images/kuerbis/Arduino-Bibliotheken.jpg" width='350'>


Es gibt auch die Möglichkeit eine Bibliothek auf einer externen Seite herunterzuladen und über die 
„.ZIP Bibliothek hinzufügen…“ Funktion einzubinden. 
Diese Variante ist allerdings umständlicher als über die eingebaute Funktion in der Arduino Umgebung.

## Programmieren

Auf die Grundlagen der Programmierung gehe ich in meinem Blog nicht weiter ein, weil es auch hierzu bereits hervorragende Anleitungen
im Netz gibt. Sei es in Form von Tutorials, Blogs oder youtube etc.
Meine persönliche Erfahrung bzw mein Rat an Einsteiger ist, dass ihr mit einfachen Beispielen beginnt, die zB die Arduino 
Entwicklungsplattform mitliefert. Ein wirklich einfaches Beispiel ist es zB die onBoard LED blinken zu lassen. Dazu braucht ihr weder 
ein umfangreiches Programm / Sketch, noch benötigt ihr am Anfang zusätzliche Hardware und deren Verdrahtung.
Anhand einfacher Beispiele werdet ihr sehr schnell eigene Erfahrungen sammeln und euch damit an immer komplexere Aufgaben heran wagen.

### Grundstruktur für einen Sketch

Zum Einstieg nur eine kurze Erklärung bzgl der Grundstruktur eines Sketches. 
Ein Sketch kann zunächst in drei Bereiche eingeteilt werden:

1. Variablen benennen
Im ersten Bereich werden Elemente des Programms benannt.  Dieser Teil ist nicht zwingend erforderlich.

2. Setup (im Programm zwingend erforderlich)
Das Setup wird vom Board nur einmal ausgeführt. Hier teilt man dem Programm zum Beispiel mit, welcher Pin (Steckplatz 
für Kabel) am Mikrokontrollerboard ein Ausgang oder ein Eingang ist.

3. Loop (im Programm zwingend erforderlich)
Der Loop-Teil wird von Board kontinuierlich wiederholt. Es verarbeitet den Sketch einmal komplett bis zum Ende und beginnt dann 
erneut am Anfang des Loop-Teils.

Hier ein Beispiel , um die eingebaute LED auf dem Board blinken zu lassen
￼
<img src="{{ site.baseurl }}/images/kuerbis/simple-led-Sketch.jpg" width='350'>

Wer mehr Hintergründe und Details zur Programmierung von Mikrocontrollern lernen möchte, dem kann ich das folgende Tutorial empfehlen

[Funduino Anleitung](https://funduino.de/anleitung)

Es gibt aber auch viele andere Lernvideos auf YouTube und den verschiedenen Medien. 
Hier müsst ihr für euch entscheiden, welcher level für euch am Besten geeignet ist oder ob ihr nur eine spezielle Information 
sucht bzw lernen möchtet.


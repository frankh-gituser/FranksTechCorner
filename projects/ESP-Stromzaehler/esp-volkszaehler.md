# IR Lesekopf für Stromzähler am ESP8266 mit Tasmota


!!!! WORK IN PROGRESS !!!!!!!!



[comment]: <> (This is a comment, it will not be included)
[//]: <> (This is also a comment.) 



### [zurück zum Index](../../index.md)

IR Lesekopf für Stromzähler am ESP8266 mit Tasmota

Wie bekomme ich die Daten vom Stromzähler mit einem ESP8266 in mein OpenHAB eingebunden , um in Zusammenarbeit mit
der Balkon PV
den hausinternen Stromverbrauch zu optimieren ?
Na ja - die Voraussetzungen sind ja in den meisten Fällen durch den Einbau eines modernen SmartMeters, dh Stromzählers schon gegeben.
Mit Hilfe eines Lesekopfs können wir dann die Daten per Infrarot-Schnittstelle  vom Stromzähler auslesen und Richtung openHAB schicken.
Voraussetzung hierfür ist allerdings die Freischaltung des Stromzählers mit einer PIN, die ihr online bei eurem Netzbetreiber anfordern könnt.
Das geht schnell und einfach. Die notwendigen Schritte sind meistens je nach Stromzähler ebenfalls beim Netzbetreiber abrufbar.

Beim Lesekopf habe ich mich für den sog. Volkszähler entschieden, der sehr verbreitet ist und zu dem ihr sehr viele Anleitungen als
Videos und Blogs im Netz findet. Links füge ich  im weiteren Verlauf noch bei.

Es gibt eine Wifi Variante und eine ESP8266 Variante.
Bei der Wifi Variante sind sozusagen alle Komponenten als Einheit direkt im Lesekopf vorhanden.
Bei der TTL Variante müsst ihr den IR Lesekopf noch mit einer Steuerungseinheit, dem ESP verbinden und einige zusätzliche Arbeitsschritte selber durchführen .

Dazu erstmal ein paar grundsätzliche Worte zu Tasmota.


Tasmota ist eine Software , die auf einem Microcontroller wie zB dem D1 Mini,ESP8266 läuft . Diese Software kann man auf die Hardware , den ESP flashen.
Tasmota wird benötigt, damit der Lesekopf  mit dem Stromzähler kommunizieren kann.
Dafür könnte man sich eigentlich die Standard Release von Tasmota auf der Releasemanagement Seite

[Tasmota Github Repository](https://github.com/arendst/tasmota)

runterladen und mit einem Tool eigener Wahl auf den ESP flashen.
Das Problem ist allerdings, dass man die Funktionen / Kommandos für einen Smartmeter und speziell für deinen eingesetzten Smartmeter benötigt, 
diese aber im Standardrelease nicht enthalten sind.

[Tasmota Firmware Releases ](https://tasmota.github.io/docs/) 

<b>  Unter Features -> Smart Meter Interface </b>

[Tasmota SmartMeter Features ](https://tasmota.github.io/docs/Smart-Meter-Interface/)


![image logo](../../projects/ESP-Stromzaehler/images/Tasmota-Smartmeter-Features.png)
￼


Dh die bin enthalten nicht den nötigen Code um sich mit dem smartmeter zu verbinden und Funktionen auszuführen. 
Würden alle Funktionen in einem Standardrelease eingebunden, dann wäre der Speicherbedarf einfach zu groß, um es auf einem kleinen ESP im verfügbaren Flash 
Speicher unterzubringen.
Daher müssen wir uns eine eigene <i> bin </i> Datei zusammenstellen und compilieren. 
Dabei müssen die SML commands mit reincompiliert und in unseren Code eingebunden werden.

	Based on Tasmota's scripting language. 
	To use it you must compile your build. 
	Add the following to user_config_override.h:

	#ifndef USE_SCRIPT
	#define USE_SCRIPT
	#endif
	#ifndef USE_SML_M
	#define USE_SML_M
	#endif
	#ifdef USE_RULES
	#undef USE_RULES
	#endif


Auf der rechten Seite der Liste könnt ihr dann auch direkt nachschauen, ob euer SmartMeter in der Liste der bekannten Geräte
Enthalten ist. 


Jetzt geht es also daran , unser eigenes Tasmota System zu bauen und zu kompilieren.  Klingt dramatisch, bekommt aber 
jeder hin. Dafür brauchen wir erstmal den Quellcode von Tasmota.

Auf den link oben rechts auf der Tasmota Seite klicken , um auf die aktuellen Code Release Seiten zu kommen:

[Tasmota Release Pages ](https://github.com/arendst/tasmota)

Hier liegen alle möglichen Dateien, die nachher in unser binary kommen und somit kompiliert werden müssen.  
In dem development Branch könnt ihr sehen, dass  permanent Änderungen durchgeführt und aktualisiert werden. 
Die Einzelheiten zu dem Entwicklungszyklus von Tasmota will ich hier nicht weiter ausführen, dazu gibt es 
reichlich Material zum Lesen und sehr gute YouTube Videos.

Ihr solltet euch über die Tags die aktuelle Version raussuchen. 
Zum Zeitpunkt des Verfassers dieses Artikels ist dies die Version 12.2.0   

[Tasmota Aktuelle Release  ](https://github.com/arendst/Tasmota/tags)


![image logo](../../projects/ESP-Stromzaehler/images/Tasmota-current-Tag.png) 

￼

Nun diese Release als zip Datei oder tar downloaden. Den kompletten Quellcode dann auf eurem System in ein Verzeichnis
entpacken. Achtet darauf , dass ihr auf einen freigegeben Release/Tag zurückgreift
Dann habt ihr den kompletten Tasmota Quellcode auf eurem System
Hier müsst ihr jetzt ein paar Dinge anpassen, um die SML Funktionen mit in den Quellcode einzubinden
Würdet ihr jetzt einfach nur einen compile+build durchführen, dann hättet ihr nichts gewonnen, sondern einfach nur ein
Standard Tasmota Release gebaut, die es auf den Tasmota Seiten auch direkt zum Download gibt.

Fügen wir also die zusätzlichen SML Features hinzu. Da wir dazu im Code Änderungen vornehmen  und eine neue Version
compilieren müssen, benötigen wir eine entsprechende Entwicklungsumgebung , eine sog. IDE . Hier gibt es wieder viele
Möglichkeiten, einige sind hier https://tasmota.github.io/docs/Compile-your-build/
beschrieben , zB 

	Visual Studio Code - setup and configure Visual Studio Code with PlatformIO for Tasmota


Da ich auf meinem MacBook sowieso Visual Studio Code installiert habe, 
zeige ich das weitere Vorgehen exemplarisch auf Basis dieser IDE.
Solltet ihr euch auch für diese Variante entscheiden, findet ihren einen kostenlosen 
Download der Software hier :  

[VisualStudio Code Download Page  ](hhttps://code.visualstudio.com/download)

Aber wie gesagt, mit Gitpod und Co gibt es auch weitere Alternativen.. 


In Visual Studio müsst ihr dann Platform.io als Plugin in VSCode installieren. 
Dazu ladet ihr euch die zusätzliche Software unter

[Platform.io Download Page  ](https://tasmota.github.io/docs/PlatformIO/)

auf euren Rechner und installiert das Ganze nach Anleitung wie auf der Seite beschrieben.

Platform.io hilft uns dann dabei, die fertigen binaries auf den ESP zu flashen. Nach der Installation könnt ihr links in VSCode
auf Extensions klicken und seht dann das Symbol der die Ameise für Platform.io. 


![image logo](../../projects/ESP-Stromzaehler/images/PlatformIO-EXTENSIONS.png) 



Danach wieder oben links auf 
	-> Explorer zurückgehen 
	-> Open Folder 
	-> in das Download Verzeichnis mit den Tasmota Dateien wechseln 
	-> und das Verzeichnis öffnen. 

Um zu sehen ob das alles funktioniert hat, 
	links auf die Ameise gehen -> default -> und auf Build all gehen . 

![image logo](../../projects/ESP-Stromzaehler/images/platformio-default.png) 


mitverfolgen. Wenn das alles funktioniert hat , solltet ihr dort

![image logo](../../projects/ESP-Stromzaehler/images/platformio-buildall.png) 

sehen



Jetzt geht es wieder zurück in die Dateiansicht und wir können endlich die Änderungen 
durchführen,  um unsere eigene Tasmota Version zu bauen.

Wie das geht , ist in der Doku unter 

	Smartmeter, Compiler your own Build beschrieben

[Tasmota Firmware Compile-your-Build  ](https://tasmota.github.io/docs/Compile-your-build/)



Als Nächstes müssen die Zusätzlichen Funktionen aktiviert werden, indem die folgenden #defineCompiler-Anweisungsparameter hinzugefügt und dann die Firmware kompiliert werden. Die Parameter und die defines  
werden unter 

[Tasmota SmartMeter Interface  ](https://tasmota.github.io/docs/Smart-Meter-Interface/)

erläutert.

	#ifndef USE_SCRIPT
	#define USE_SCRIPT
	#endif
	#ifndef USE_SML_M
	#define USE_SML_M
	#endif
	#ifdef USE_RULES
	#undef USE_RULES
	#endif


Wie ihr seht, werden hier neben den SML auch die Script Funktionen aktiviert bzw. erweitert, 
da die Smartmeter Schnittstelle auf der Scriptsprache von Tasmota basiert.  

Zurück in VSCode Geht ihr wieder zum 
	-> Menüpunkt tasmota -> bis zu user_config-override.h . 

Dort könnt ihr auch weitere  Angaben machen und in eure binsaries einfügen, zB WLAN-ID 
und Passwort etc.  Wir fügen jetzt Also  unsere kopierten Zeilen unten ein und speichern 
das Ganze. 
Das sagt jetzt  der Projektdatei, „Bau mir mein binary mit den SML Erweiterungen“.
Jetzt wieder auf die Ameise klicken und 
	-> Build all

VSCode baut nun das Standard binary mit den Features, die wir eben hinzugefügt haben.
Anschließend  müssen wir das binary nur noch auf den ESP aufspielen (flashen).

Dazu gehe ich wieder auf den
	Explorer oben links 
und öffne ganz unten die Datei 
	Platforms-Override.ini 

Die hat VSCode automatisch angelegt, ansonsten müsstest du die sample-Datei selber
umbenennen und anpassen.
Hier können wir jetzt verschiedene Sachen konfigurieren. 
Da steht sehr viel drin, lasst euch dadurch aber nicht ablenken. 
Wir müssen hier nur den Upload-Port ändern, weil da steht als default sowas wie 

	 COM5

was zB auf einem Mac nicht passt. 
Welchen uploadport nutzen wir also ? 
Dazu önnen wir in ein Terminal und geben in der Shell

	ls -la /dev/cu.*


--------  bis hierher fertig ------


[Ausgabe hier reinkopieren]

Was mach ich jetzt. Ich nehme hier mein d1 Mini und Stecke das ganze einfach mal in einen meiner usb Ports. Und jetzt geben wir den Befehl nochmal aus und jetzt sehe ich den richtigen usbserial-### Port . Das ganze kopiere ich jetzt einfach und das füge ich in die Datei platformio.ini ein. 

(Bild reinkoieren)

Wir haben jetzt die Features hinzugefügt. Das binary gebaut und können’s jetzt auf unseren esp flashen. Einfach auf Upload-all klicken und das sollte funktionieren. Schreibt jetzt den Flash Speicher des esp mit unserem binary. 
Wenn das geklappt hat, sind wir mit diesem Abschnitt fertig.

Wenn wir alles richtig gemacht haben und den ESP an Strom angeschlossen haben, dann sollte wir ein WLAN finden, welches mit mit Tasmota_ anfängt. Der esp ist momentan im Access-Mode Modus , dh ich trete diesem wlan bei und nach kurzer Zeit geht ein Konfigurationsfragen auf 

Bild

In diesem Dialog kann ich erstmal meine wlan Zugangsdaten eingeben, damit der esp Teilnehmer in meinem wlan wird . Speichern, neu starten, Anzeige der IP adresse, die ihr vom dhcp Server bekommen habt. Adresse nehmen und die könnt ihr im Browser aufrufen und kommt in das tasmota config Menü.
Erstmal configuration-Modul = generic. Speichern.
Wenn ihr erstmal testen wollt, ob eure sml Funktionen auch in dem binary eingebaut  wurden, könnt ihr auf Console-edit Script gehen , wo ihr ein Script einfügen könnt.

Hier muss ich jetzt meinen Zähler konfigurieren. In meinem Fall ist das ein Apator NORAX 3D 
Hier muss ich konfigurieren, welche Daten ich denn auf welche Art und Weise bekomme und wie will ich die Abfragen. All das wird in dem Script beschrieben.
Was schreibe also hier rein ? Dafür Wechsel ich wieder auf die tasmota Seite unter Features-> smartmeter-Interface , und hier findest du hoffentlich deinen Stromzähler. In meinem Fall ist der Norax3D aufgeführt, und dort finde ich ein Script. Das kopiere ich alles , Wechsel zurück zu tasmota und füge die scriptdaten unter edit-script ein u d aktiviere das ganze
Auf der tasmota Seite kann ich noch Erklärungen finden, was mit dem script dann bereitgestellt wird:

Norax 3D+ (SML)~
This script gives also the wattage per phase. Make sure to get the PIN from your grid operator! Tested on a WeMos D1 mini with an IR Head from https://agalakhov.github.io/ir-interface connected to the RX pin (3). The meter also outputs the phase angles, but i left them out since i do not need them. You can easily find additional values by activating the debug mode ("sensor53 d1" for the first meter, switch off after a few seconds with "sensor53 d0").

>D
>B
->sensor53 r
>M 1
+1,3,s,1,9600,SML
1,77070100010800ff@1000,Total consumption,KWh,Total_in,4
1,77070100020800ff@1000,Total Feed,KWh,Total_out,4
1,77070100100700ff@1,Current consumption,W,Power_curr,0
1,77070100200700ff@1,Voltage L1,V,Volt_p1,1
1,77070100340700ff@1,Voltage L2,V,Volt_p2,1
1,77070100480700ff@1,Voltage L3,V,Volt_p3,1
1,770701001f0700ff@1,Amperage L1,A,Amperage_p1,1
1,77070100330700ff@1,Amperage L2,A,Amperage_p2,1
1,77070100470700ff@1,Amperage L3,A,Amperage_p3,1
1,77070100240700ff@1,Current consumption L1,W,Power_curr_p1,0
1,77070100380700ff@1,Current consumption L2,W,Power_curr_p2,0
1,770701004c0700ff@1,Current consumption L3,W,Power_curr_p3,0
1,770701000e0700ff@1,Frequency,Hz,frequency,0
#

Script enable -> speichern

Wenn das geklappt haben sollte, dann sollte ich im Main Menü die entsprechenden Werte sehen.

Damit das Ganze funktioniert, müsst ihr euch vom Netzbetreiber den PIN für die Freischaltung des Zählers geben lassen. Das geht sehr einfach über ein Online Formular und der PIN wird dann aus Sicherheitsgründen per Post zu euch geschickt. Das dauert ein paar Tage, hat aber in meinem Fall schnell und reibungslos funktioniert. 

Je nachdem welche Werte euch der Zähler liefert , könnt ihr das Script anpassen und Zeilen rauslöschen, weilzB nicht jede einzelne Phase als Wert geliefert wird etc.
Das Ganze ist recht einfach, weil alle Details super dokumentiert sind. Ich hoffe also, dass auch euere Zähler mit in der Liste der smartmeter enthalten ist.

All diese Werte kann ich jetzt wieder per mqtt weiterleiten, zB an meine SmartHome zentrale . Hierzu in Configuration-MQtt die entsprechenden Konfigurationen vornehmen.


Der Volkszähler Lesekopf 

ttl ir lesekopf lese-schreib-Kopf EHZ Volkszähler original-Hichi Smartmeter

https://www.ebay.de/itm/314015465828?_trkparms=amclksrc%3DITM%26aid%3D1110006%26algo%3DHOMESPLICE.SIM%26ao%3D1%26asc%3D242766%26meid%3D34b44eb109e34cc3b43298d06e1fc4b8%26pid%3D101195%26rk%3D1%26rkt%3D12%26sd%3D314152997777%26itm%3D314015465828%26pmt%3D1%26noa%3D0%26pg%3D2047675%26algv%3DSimplAMLv11WebTrimmedV3MskuAspectsV202110NoVariantSeedKnnRecallV1&_trksid=p2047675.c101195.m1851&amdata=cksum%3A31401546582834b44eb109e34cc3b43298d06e1fc4b8%7Cenc%3AAQAHAAABIMFr2e4EmAnM%252ByHZkULYKDIJ4L66fOjNL0iupgt%252BzO1%252F3AE1t3mNirUYB96NktMCicMagiS6mbeTl0xquGODv9nSajpm1aaEbsSFw0uTVvCdFa4SbTTTejhdIALH%252FMfICFmn9uxcclDxbM5y0r8z4myyvxKikwjz5jwlJAw6hlp5di%252BCZ3FC5B8BnS6VuoSzmejpuqpezh2l0g3lUGIw5ENGdD0xE19uE%252BqGTt2GsHa59UBPO%252FmSiOGQGHyfpNfF8iHoEeax%252FVso5CxW%252FCTFlzilaKSnOya31INXwB6%252B0fz5t1f4NiGPY52y27aNEXhGpWSLr%252FahPOyEy6qzYpvqeXp0%252B84C9ZiKywRw6olNBsqNvwC2weP8w3zDAxKii%252FP%252BSw%253D%253D%7Campid%3APL_CLK%7Cclp%3A2047675

* 		ausgereifte und verbreitete Platine mit Schmitt Triggern zum sicheren und störungsfreien Betrieb
* 		Baudraten bis 57600 sind möglich und getestet.
* 		TTL Version (RX / TX)
* 		3,3V - 5V Betriebsspannung
* 		geeignet für Arduino, ESP8266, ESP32, Raspberry und alles was eine UART Schnittstelle hat
* 		Platine beschriftet VCC GND RX TX
* 		Lesen und senden (manche Zähler müssen zum Senden aufgefordert werden, funktioniert nicht mit einem reinem Lesekopf, mit diesem kein Problem)



Das Ganze gibt es auch direkt als wlan Variante, was euch den ganzen Aufwand eines eigenen tasmota builds erspart.  
Jetzt müsst ihr den Lesekopf mit dem esp verbinden 







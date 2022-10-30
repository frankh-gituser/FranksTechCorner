# IR Lesekopf für Stromzähler am ESP8266 mit Tasmota


!!!! WORK IN PROGRESS !!!!!!!!



[comment]: <> (This is a comment, it will not be included)
[//]: <> (This is also a comment.) 

### [zurück zum Index](../../index.md)




IR Lesekopf für Stromzähler am ESP8266 mit Tasmota

Wie bekomme ich die Daten vom Stromzähler mit einem ESP8266 in mein OpenHAB eingebunden , um In Zusammenarbeit mit der Balkon PV den hausinternen Stromverbrauch zu optimieren ?

Bevor wir loslegen, kurz noch ein paar Erklärungen zum Tasmota Begriff.  
Tasmota ist eine Software , die auf einem mikrocontroller wie dem d1 Mini, esp8266 läuft und auf vieler anderer Hardware. Diese Software kann man auf die Hardware , den ESP flashen.
Tasmota um mit dem Lesekopf  zu kommunizieren
Dafür könnte man sich tasmota auf der Releasemanagement Seite xxxx runterladen und mit einem Tool auf die entsprechende Hardware flashen
Das Problem ist, dass man die Funktionen für einen smartester und speziell für deinen eingesetzten smartmeter benötigt

https://tasmota.github.io/docs/

Bild von Features 

https://tasmota.github.io/docs/Smart-Meter-Interface/

This Feature is Not inclusive in precompiled binaren
Dh die bin enthalten nicht den nötigen Code um sich mit dem smartmeter zu verbinden und Funktionen auszuführen
Dh wir müssen uns eine eigene binare zusammenstellen und compilieren zu einem binare
SML commands müssen mit reincompiliert werden , 

#ifndef USE_SCRIPT
#define USE_SCRIPT
#endif
#ifndef USE_SML_M
#define USE_SML_M
#endif
#ifdef USE_RULES
#undef USE_RULES
#endif

Das sind die nächsten Schritte. Klingt dramatisch, bekommt aber jeder hin
Dafür brauchen wir erstmal den Quellcode von tasmota.

Auf den link oben rechts auf der tasmota Seite klicken auf die aktuellen Code Release

https://github.com/arendst/tasmota

Hier liegen alle möglichen Dateien, die nachher in unser binare kommen und somit Compiler werden müssen. Unserem Release. In dem Dev Branche werden permanent Änderungen aktualisiert . DieEi zelheiten zu dem entwickljngszyklus von tasmota will ich hier nicht weiter ausführen, dazu gibt es reichlich Material zum lesen oder exzellente YouTube Beiträge.

Ihr sollteteuch über die Tags die aktuelle Version raussuchen, in meinem Fall aktuell die Version 12.2.0   Und die diese Release als zip Datei oder gar Downloaden. Den kompletten Quellcode dann auf eurem system entpackt in ein Verzeichnis. Achtet darauf , dass ihr auf einen freigegeben stand/Tag zurückgreift
Dann habt ihr den kompletten tasmota Quellcode auf eurem system
Hier müsst ihr jetzt etwas anpassen, um die SML Funktionen mit in den Quellcode einzubinden
Wuerdetihr jetzteinfach nur complilieren, dann hätte ihr nichts gewonnen, sondern einfach nur ein Standard tasmota Release gebaut, die es auf den tasmota Seiten auch direkt zum Download gibt.

Wir fügen jetzt neue Features hinzu. 

Da wir jetzt am Code ändern müssen und eine neue Version compilieren müssen, benötigen wir eine Entwickler ide . Hier gibt es viele Möglichkeiten.die Anker beschrieben sind

https://tasmota.github.io/docs/Compile-your-build/

Ich habe auf meinem mac Visual Studio Code installiert, so dass ich exemplarisch , damit das weitere Vorgehen zeigen werde.
Ihr könnt euch brauch kostenlos Downloaden unter 

https://code.visualstudio.com/download

Aber wie gesagt, mit Gitpod und Co gibt es auch weitere Varianten. 
In Visual Studio müsst ihr dann Platform.io als Plugin installieren

https://tasmota.github.io/docs/PlatformIO/

Das hilft uns dann dabei, die ganzen Sachen auf den ESP zu flashen
Links in Vs dann auf Extensions klicken und sehe dann die Ameise Platform.Info IDE. Danach links ein neues Menü.. wieder oben links auf explorerzurueckgehen und Open folder u d in das Download Verzeichnis gehen und öffnen. Um zu gucken ob das alles funktioniert, links auf die Ameise gehen, default, und auf Build all gehen . Der baut dann das binary , erstmal ein Test, ob das alles läuft. Success, dann sollte alles funktioniert haben.
Wieder zurück in die dateiansicht und endlich die Änderungen durchführen, um unsere eigene tasmota Version zu bauen.

Wie das geht ist in der Docu beschrieben. Unter smartmeter, Compiler your own Build
Erstmal die ganzen defines kopieren, 
Und dann zurücknimmst VsCode . Dann unter tasmota , bis zu user_config-override.h . Da kann man auch andere Angaben machen und in meine binsaries einfügen, zB wlan id und Passwort etc. Also unsere kopierten Zeilen unten einfügen und speichern.
Das sagtjetzt der Projektdateien, Bau mir mein binary mit den sml geschichten. Jetzt wieder auf
Ameise klicken und Build all
Jetzt baut er das Standard binary mit den Features, die wir eben hinzugefügt haben.
Jetzt müssen wir das binary nur noch auf den esp aufspielen, flashen 
Dafür gehe ich wieder auf Dateien oben links, ganz unten die Datei Platforms-Override.ini 
Die hat VS automatisch angelegt,, ansonsten müsstestdu die sample Datei selber umbenennen und anpassen
Hier können wir jetzt verschieden Sachen konfigurieren. Da ist sehr viel drin, lasst euch dadurch nicht ablenken. Wir müssen hier nur den Upload-Port ändern, weil da steht jetzt COM5, was auf unserem Mac nicht passt. Welchen uploadport nutzen wir also ? Dazu gehen wir in ein Terminal in eine Shell und geben 
ls -la /dev/cu.*

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







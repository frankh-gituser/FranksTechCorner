# ESP8266 vs Fritz!DECT Schaltsteckdose


[comment]: <> (This is a comment, it will not be included)
[//]: <> (This is also a comment.) 

ESP8266 vs FritzDECT Schaltsteckdose

In diesem Projekt geht es um schaltbare Steckdosen, die ins lokale WLAN Netztwerk eingebunden werden können. 

Bisher nutze ich hierfür zahlreiche FritzDECT!200 Steckdosen in meinem SmartHome. 
Die Steckdosen von AVM lassen sich sehr einfach in das eigene WLAN mit der FritzBox einbinden und können dann komfortabel über die FritzApp gesteuert werden. 
Neben einer AN/AUS Funktion , liefert die Steckdose noch die Temperatur, kann per Voice geschaltet werden und 
erfasst sowohl den aktuellen als auch den langfristigen Stromverbrauch der angeschlossenen Geräte. 
Der Nachteil der AVM Steckdosen ist, dass sie inzwischen leider sehr teuer sind. 
Selbst gebrauchte Schaltsteckdosen bei eBay-Kleinanzeigen werden zu teils absurden Preisen , weil quasi Neupreis angeboten. 
Wer nur die einfache An/Aus Funktion nutzen möchte, um zB Lampen, Pumpen etc zu schalten, 
der braucht die vielen Komfortfunktionen nicht zwingend und zahlt somit viel Geld für viel ungenutzte Funktionen. 

Daher hat mich interessiert, was es sonst noch auf dem Markt gibt und die Auswahl ist - wenig überraschend - riesig. 
Wenn ich die Suche allerdings bzgl ohne-Cloud, und flexibles Programmierinterface erweitere, dann lande ich u.a, bei den Sonoff oder Tasmota Modellen. 
Das ausgewählte Modell basiert auf einem ESP8266 Controller, 
den ich in anderen Projekten ja bereits einsetze und benötigt keine Registrierung in irgendeiner weltweiten Cloud. 
Ist also nur im lokalen Netz erreichbar. 
Das ist keine Bewertung oder Entscheidung im Sinne eines umfassenden Enscheidungsprozesses oder umfangreichen Testkriterien, 
sondern die Kombination mir bekannter Technologien, Preis, Programmiermodell etc. Da hat jeder seine eigene Präferenz, und das ist auch gut so.

Die Stecker habe ich bei Amazon erstmal im 2er Pack zum Testen bestellt. 
Aber es gibt sicher noch andere Anbieter. Hier ein link auf Amazon, ohne das ich davon einen kommerziellen Nutzen habe:



- Wlan Steckdose mit Tasmota vorgeflasht. Kleinste ESP8266 smart plug. 16A mini wifi steckdosen mit stromverbrauch stromzähler, zeitschaltuhr. 
MQTT, Domoticz, Home Assistant, Alexa. 2er pack. NOUS A1T

[Amazon link](https://www.amazon.de/vorgeflasht-stromverbrauch-stromz%C3%A4hler-zeitschaltuhr-NOUS/dp/B0054PSIDW/ref=sr_1_6?__mk_de_DE=%C3%85M%C3%85%C5%BD%C3%95%C3%91&crid=X3HACXVI2MC5&keywords=tasmoto%2Bschaltsteckdosen&qid=1659692996&sprefix=tasmoto%2Bschaltsteckdosen%2Caps%2C52&sr=8-6&th=1)

Zum Vergleich hier ein Bild der Steckdosen:


Das erste, was einem auffällt ist der deutliche Grössenunterschied.
Für kleine Stromverbraucher ist eine Fritz Steckdose schon sehr voluminös, was zB in einer 3-Fach Steckleiste Max zwei AVM Stecker zulässt.
Das Tasmota Gerät fällt sehr schlank auf, wie ihr selber sehen könnt. Das ermöglicht den Einsatz auch in Bereichen mit Platzknappheit.

Konfiguration

Die ersten Schritte mit der Tasmota Steckdose gestalten sich eigentlich ganz einfach. 
Tasmota Stecker in die Steckdose, kurz warten und der Tasmota Stecker bietet ein eigenes Tasmotoxxx WLAN Netzwerk an. 
Das Netzwerk mit dem Smartphone oder einem anderen Gerät auswählen und man wird automatisch in die erste Konfiguration weitergeleitet. 
Dort das eigene Haus WLAN auswählen , Passwort eingeben und anschliessend startet das Tasmoto Device neu.

Nun meldet ihr euch auf eurem Router an und schaut nach der 
Netzwerkadresse der Tasmota Steckdose. Mit dem Browser ruft ihr denn 
diese IP Adresse und landet im Web-Interface der Tasmota , 
wo ihr mit der Konfiguration beginnen könnt


In der Konfiguration das template entsprechend konfigurieren, 
wenn es Standardmässig noch nicht auf das richtige Template eingerichtet ist.
Die entsprechenden templates fuer das device findet ihr unter

https://templates.blakadder.com/nous_A1T.html

Hier am Beispiel der Schaltsteckdose
Configure->Configure Other-> Template einfügen (kopierte Zeile einfügen), 
activate, speichern
Bei meinem gelieferten Gerät war das richtige Template eingestellt, 
so dass ich hier nichts weiter ändern musste.



 

Nach der Änderung des templates startet das Gerät startet dann neu

Im MainMenu könnt ihr nun mit „toggle“ die Steckdose ein-, ausschalten 
und ihr seht auch die Leistungsangaben bzw Verbräuche der angeschlossenen Verbraucher. 
Die stimmen normalerweise erstmal nicht, weil die Steckdose erstmal kalibriert werden muss, 
d.h. die Grundwerte müssen eingestellt werden. 

Dazu nehmt ihr euch am Besten eine alte Glühlampe mit einem definierten 
Verbrauch bzw. einer festen Wattzahl, zB 40W
In dem folgenden Video ist das sehr schön erklärt. 
Auch hierzu habe ich keine persönlichen oder geschäftlichen Verbindungen 
oder irgendwelche Vorteile, aber es ist halt hilfreich und gut gemacht. 


https://www.youtube.com/watch?v=Gj6yR45DJFQ


Fangen wir also mit der Volt Zahl an, die normalerweise bei uns so bei 235 V liegt. 

Wie gesagt, am Besten einen bekannten Verbraucher dranhängen, 
damit man sich auf die Geundverbräuche auch verlassen kann. 
In meinem Fall habe ich einfach eine alte, klassische 40W Glühlampe eingesteckt.

Dann in der Konfiguration->Konsole (Wattzahl der Glühlampe)
>powerset 40W.0 

290 V passt nicht, im Normalfall liegt die Spannung bei 230-235 Volt
>voltageset 235

(40/235)*1000=170,212 mA
Currentset 170,2127

Danach sollte die Steckdose kalibriert sein und euch den richtigen Verbrauch der angeschlossenen Geräte anzeigen, 
in diesem Fall von einem  angeschlossenen Notebook, nicht mehr der 40W Lampe. 
Also nicht wundern über die Angaben.




Einbindung in unser OpenHAB Smarthome

So - nachdem die Steckdose nun also kalibriert ist und funktioniert, wollen wir das Ganze natürlich auch komfortabel bedienen bzw auch automatisch steuern. 
Das ist jetzt natürlich nicht so einfach wie bei der Fritz Steckdose, da es keine eigene App dazu gibt.
Dafür bietet das API , also die Kommunikationsschnittstelle der Steckdose die Möglichkeit 
u.a. mit dem uns bekannten mqtt Protokoll eine große Anzahl von Parametern und Sensorinformationen abzufragen bzw zu steuern.

MQTT Einbindung

Die Konfiguration eines OpenHAB MQTT Brokers und die Kommunikation mit dem ESP Microcontroller habe ich ja bereits an anderer Stelle beschrieben, 
daher hier nochmal ein paar Grundlagen und Community links. 
Oder einfach in meine anderen Projekte schauen 😉

Eine Beschreibung dazu findet ihr u.a. hier:

https://tasmota.github.io/docs/openHAB/#requirements
https://community.openhab.org/t/itead-sonoff-switches-and-sockets-cheap-esp8266-wifi-mqtt-hardware/15024

The "open Home Automation Bus" (openHAB) is an open source, technology agnostic home automation platform which runs as the center of your smart home. Besides more than 400 other add-ons for all kinds of technologies, openHAB provides an MQTT add-on ("binding") to interface with systems like Tasmota.
By following the guide below you'll be able to observe, control and manage your Tasmota modules from your openHAB system. If you are new to openHAB, please learn about the basic concepts and the initial setup. The below article will not cover any basics which are out of scope to the Tasmota integration.

Requirements
	•	Working openHAB installation (see documentation)
	•	Configured Tasmota device (accessible from your local network)
	•	MQTT broker available (e.g. Eclipse Mosquitto via openHABian)
	•	A basic understanding of MQTT
	•	Working and tested connection between openHAB and the MQTT broker
	•	(optional) Standalone MQTT client (e.g. MQTT Explorer) to observe and identify messages on the MQTT broker


Wie ihr seht, braucht es als erstes einen konfigurierten MQTT Broker , den ihr im OpenHAB entsprechend einrichtet.


Als Nächstes müsst ihr in der Tasmota Konfiguration die mqtt Einstellungen ändern, damit eine Verbindung zu eurem OpenHAB Broker aufgebaut werden kann.

Screenshot Tasmoto mqtt Kondiguration



Zurück in der OpenHAB Admin Konsole erstellt ihr dann ein MQTT-Generic-Thing für den Tasmota Stecker und 
konfiguriert den entsprechenden Channel mit State und command Topic für die Kommunikation zwischen OpenHAB Broker und Tasmota. 
Über ein entsprechendes Item lässt sich der Stecker dann zB an und ausschalten.

GenericMQTT Thing und item und die beiden Channels




Zwei Channels einrichten
	0.	Switch ON/OFF topic
	cmnd/tasmota2
	cmnd/tasmota2


Tasmota commands
https://tasmota.github.io/docs/Commands/#command-flow


Ob die Kommunikation über den Broker funktioniert könnt ihr mit den entsprechenden publish, subscribe Kommandos in einem Shell Terminal prüfen:

mosquitto_pub -h localhost -t cmnd/tasmota2/cmnd/POWER -m "OFF"
Lassen sich Kommandos an das Device schicken und mit

mosquitto_sub -d -t  cmnd/tasmota2/cmnd/POWER
Kann man auf dem topic mitlesen

Im Web Interface unter der IP Adresse des Tasmota Steckers könnt ihr unter
MainMenu->Console die send/request Kommunikation entsprechend mitverfolgen, die vom Tasmota abgeschickt wird bzw vom Tasmota auf dem Topic empfangen wird


A broker Thing with a Generic MQTT Thing and a few channels

https://github.com/openhab/openhab-addons/blob/main/bundles/org.openhab.binding.mqtt.generic/xtend_examples.md#converting-an-mqtt1-installation


Fuer die PowerPlugs habe ich jeweils ein GenericMQTT Thing für Power Switch und eins fuer die Sensorwerte angelegt und entsprechende MQTT topics definiert

Bei den Sensordaten wird es jetzt etwas komplizierter , weil diese Werte nicht als Einzelwerte gesendet werden, 
sondern als „Sammelinformation“ in einer Zeichenkette zur Verfügung gestellt wird

Item 'GenericMQTTThingTasmota1SensorData' changed from {"Time":"2022-08-18T09:34:27","ENERGY":{"TotalStartTime":"2022-08-12T15:35:39","Total":0.350,"Yesterday":0.067,"Today":0.050,"Period": 0,"Power": 0,"ApparentPower": 0,"ReactivePower": 0,"Factor":0.00,"Voltage": 0,"Current":0.000}} to {"Time":"2022-08-18T09:39:27","ENERGY":{"TotalStartTime":"2022-08-12T15:35:39","Total":0.350,"Yesterday":0.067,"Today":0.050,"Period": 0,"Power": 0,"ApparentPower": 0,"ReactivePower": 0,"Factor":0.00,"Voltage": 0,"Current":0.000}}


Aus diesem String müssen die Einzelwerte dann entsprechend extrahiert und einem Item bzw. einer Variablen zugeordnet werden, 
so dass wir den Wert im OpenHAB zB in einer Regel weiterverarbeiten bzw. anzeigen können
Hier können verschiedene Funktionen genutzt werden, die OpenHAB bereits mitliefert. 

Wichtig ist allerdings zu erwähnen, dass zB die JSONPath Transformation Addons im PaperUI bzw in der Admin Umgebung erst installiert werden müssen:

JSONPATH Transofrmations unter 
Settings->Tranformations in openHAB Admin Konsole installieren

Danach lassen sich die Werte über ein Topic auslesen und weiterverarbeiten. Ich zeige euch hier nur exemplarisch einige Werte, wie ihr das Thing+item mit den Channels anlegt:


Dann die item für die Weiterverabeitung der Einzelwerte in der item Datei
Tasmota.item:

Number  Tas1TotalEnergy "Tas1TotalEnergy [%.3f kWh]"
Number  Tas1CurrentEnergy "Tas1CurrentEnergy [%.3f kWh]"
Number  Tas1PowerWatt "Tas1PowerWatt [%.2f W]"


Die Einbindung in der 
Default.sitemap
Switch  item=GenericMQTTThingTasmota1PowerSwitch      label="Tasmota-1 Steckdose
        Text item=Leeres_Item label="Tasmotoa1 Energieverbrauch" {
          Text    item=Tas1PowerWatt
          Text    item=Tas1TotalEnergy
          Text    item=Tas1CurrentEnergy
        }


Und die Aufbereitung in der 

Tasmota1.rules
rule "TasmotaPlug1 On"
when
  Item GenericMQTTThingTasmota1PowerSwitch changed to ON
then
  val mqttActions = getActions("mqtt","mqtt:broker:331a9f6380")
  mqttActions.publishMQTT("cmnd/tasmota1/cmnd/POWER","ON")
  logInfo("     TASMOTA1 !!!! ON      ", "   ")
end


Und hier werden die Sensordaten mit Hilfe der JSONPATH Funktionen den Einzelwert-Items zugewiesen 


rule "TasmotaPlug1 Sensor Data"
when
  Item GenericMQTTThingTasmota1SensorData changed
then
  val mqttActions = getActions("mqtt","mqtt:broker:331a9f6380")
  logInfo("  TASMOTA1 Sensordaten:    --->>> ", GenericMQTTThingTasmota1SensorData.state.toString," ")
  
val temp1 = transform("JSONPATH", "$.ENERGY.Total", GenericMQTTThingTasmota1SensorData.state.toString)
  logInfo("  Total Energy  String :    --->>> ", temp1)
  // post the new value to the Number Item
  Tas1TotalEnergy.postUpdate( temp1 )

  val temp2 = transform("JSONPATH", "$.ENERGY.Current", GenericMQTTThingTasmota1SensorData.state.toString)
  logInfo("  Current Energy :    --->>> ", temp2)
  Tas1CurrentEnergy.postUpdate( temp2 )

  val temp3 = transform("JSONPATH", "$.ENERGY.Power", GenericMQTTThingTasmota1SensorData.state.toString)
  logInfo("  Power Watt Energy :    --->>> ", temp3)
  Tas1PowerWatt.postUpdate( temp3 )
end




Viel Spaß bei den ersten Schritten und der Weiterentwicklung eurer eigenen Anforderungen. 
Über Anregungen und gute Erkenntnisgewinne freue ich mich immer. 

Einfach per Email an mich.

# Halloween Stimmungslicht

Die Grundlagen für die Programmierung eines Mikrocontrollers habe ich in dem Beitrag 
<link auf den Beitrag> 
beschrieben. In dem folgenden Projekt habe ich einen ESP genutzt, 
um zur Halloween Zeit einen ausgehöhlten Kürbis mit einem Stimmungslicht auszuleuchten. 
Natürlich könnt ihr das Stimmungslicht auch für andere Einsätze zB zur Weihnachtszeit nutzen. 
Hierfür lassen sich dann die Farbeffekte nach eigenem Geschmack anpassen. 
Als Steuerungszentrale nutze ich in meinem Fall das SmartHome, dh meine openhab Umgebung. 
Auch hier könnt ihr im Sketch die mqtt Steuerung entfernen und das Stimmungslicht immer nach dem Start des ESP aktivieren. 
Das Ganze dann ergänzt durch eine Schaltzeituhr oder Schaltsteckdose. 
Oder aber über eine App auf dem Smartphone den ESP an/ausschalten. Die Möglichkeiten sind , wie ihr seht, vielfältig .

Werfen wir erstmal einen Blick auf das Ergebnis und die hierfür notwendigen Komponenten:



## Der Bio-Kuerbis Live 

![image logo](../projects/Halloween/images/kuerbis.jpg)

### Komponentenliste

- D1 Mini NodeMcu mit ESP8266-12F WLAN Module CH340G Lua kompatibel mit Arduino

[Amazon link](https://www.amazon.de/gp/product/B01N9RXGHY/ref=as_li_tl?ie=UTF8&tag=diesudas06-21&camp=1638&creative=6742&linkCode=as2&creativeASIN=B01N9RXGHY&linkId=d735117de710eaf041a18dcf3390f753)


<img src="{{ site.baseurl }}/images/kuerbis/D1-Mini-Anschluesse.jpg" width='250'>
![image logo](../projects/Halloween/images/kuerbis.jpg)
 
- 5V RGB LED Ring WS2812B 12-Bit 38mm kompatibel mit Arduino

[Amazon link](https://www.amazon.de/AZDelivery-WS2812B-12-Bit-Neopixel-Arduino/dp/B07TZK9DNT/ref=sr_1_1_sspa?keywords=neopixel+ring+12&qid=1639237233&sr=8-1-spons&psc=1&smid=A1X7QLRQH87QA3&spLa=ZW5jcnlwdGVkUXVhbGlmaWVyPUFNOVVETTU2U1FJSEgmZW5jcnlwdGVkSWQ9QTAzMDk0NDQyTjVNTklOVkI3MTlJJmVuY3J5cHRlZEFkSWQ9QTAyMTI0NzdPMjJGSFk5Wjk4MDQmd2lkZ2V0TmFtZT1zcF9hdGYmYWN0aW9uPWNsaWNrUmVkaXJlY3QmZG9Ob3RMb2dDbGljaz10cnVl)

<img src="{{ site.baseurl }}/images/kuerbis/Neopixel-Anschluesse-top.jpg" width='250'>
![image logo](../projects/Halloween/images/kuerbis.jpg)

Die Stromversorgung des ESP erfolgt in diesem kleinen Projekt ueber eine Powerbank oder ein USB Ladekabel. 
Da das Stimmungslicht hauptsächlich in den Abendstunden angeschaltet ist, sollte dies ausreichend sein. 
Bei mir funktioniert es jedenfalls.
Vom ESP könnt ihr dann 5V und Ground mit dem Neopixel Ring entsprechend mit  VCC (5V) und GND verbinden. 
Die Datenleitung IN verbindet ihr dann mit dem Datenpin auf dem ESP - in meinem Fall PIN6. 
Später im Arduino Code müsst ihr darauf achten, dass PIN6 je nach Board unterschiedliche GPIO PIN Werte hat.


#### ESP8266 Pinout Reference: Welche GPIO pins muss ich auswählen ?
[ESP8266 Pinout Reference](https://randomnerdtutorials.com/esp8266-pinout-reference-gpios/)

<img src="{{ site.baseurl }}/images/kuerbis/ESP-GPIO-pinout.jpg" width='350'  >
![image logo](../projects/Halloween/images/kuerbis.jpg)



### Der ESP Arduino code

Werfen wir jetzt einen Blick in das Programm bzw Sketch. 

-> Arduino Code einfuegen

In dem Code findet ihr zu den jeweiligen Arbeitsschritten die Kommentare im Sketch.
Zunächst müssen wir einige Variablen setzen, wie zB die WLAN credentials und den Namen des mqtt Servers.
In setup() wird dann eine Verbindung zu unserem WLAN aufgebaut und zu dem definierten MQTT Server. 
Um im Falle eines Verbindungsabbruchs wieder Die Funktion loop() sorgt dafür, dass auch im Fall einer 
Verbindungsunterbrechung die Verbindung wieder hergestellt wird. 
In reconnect() wird im Fall einer Verbindung dann ein subscribe auf das entsprechende topic durchgeführt, 
so dass ab dann neue Nachrichten empfangen bzw gesendet werden können.


Nachdem wir das Microcontroller Board mit dem Sketch aktiviert haben, brauchen wir jetzt eine Steuerungsfunktion, 
um das Stimmungslicht an- und auszuschalten bzw das Ganze eventgesteuert zu betreiben. In meinem Fall verwende ich 
hierzu meine SmartHome Zentrale, so dass ich im folgenden einige Hinweise geben möchte, was hierzu an Einstellungen 
notwendig ist.

### Openhab Einstellungen

Als erstes erstellen wir uns einen AN-/AUS Schalter fuer den ESP Controller in unserer sitemap:

<b> In default.sitemaps: </b>

		Switch  item=ESP2_Moodlight_OnOff label="ESP-Neo2 Stimmungslicht"

Das Item müssen wir dann später noch entsprechend dem Channel eines Thing zuordnen

Für die Kommunikation zwischen dem Mikrocontroller und dem raspberry setze ich einen MQTT Broker ein.
Um MQTT Geräte nutzen zu können, müssen wir folgende Schritte durchführen:

1. Installation des MQTT broker auf dem raspberry
2. Installation des MQTT binding in openHAB
3. MQTT Bridge Thing Konfiguration in openHAB

Ein gutes Tutorial findet ihr in der openHAB community unter folgendem link:
[MQTT Broker Installation](https://community.openhab.org/t/oh3-mqtt-setup-and-configuration/111494)


Nach der Installation und Konfiguration habt ihr eine Kommunikationsstrecke zwischen eurem raspberry (Server) 
und einem oder mehreren Devices (ESP8266), die durch den Broker zur Verfügung gestellt wird.
Den Broker würde ich vereinfacht als mehrspurige Strasse bezeichnen auf dem wir nun eine zugewiesene Verbindungsstrecke
für ein Device konfigurieren können, vergleichbar mit einer speziellen Spur, zB für Busse.

<b> Installation+Konfiguration des MQTT broker in openHAB </b>

<img src="{{ site.baseurl }}/images/kuerbis/add-new-mqtt-broker-thing.jpg" width='800'  >
![image logo](../projects/Halloween/images/kuerbis.jpg)

<b> Anschliessend wird die Verbindung zwischen openHAB mit dem MQTT Broker hergestellt </b>

OpenHAB3 ist über ein Bridge Thing mit dem MQTT-Broker verbunden. Um ein Bridge Thing zu erstellen sind folgende
Schritte erforderlich

	Settings -> Things -> Blue + -> MQTT Binding -> MQTT Broker

Die Bridge muss nun konfiguriert werden

* Unique ID: Der Wert kann auf default gelassen werden oder es wird eine eigene ID vergeben, die eindeutig sein muss
    * Einmal angelegt, kann diese ID nachträglich nicht mehr geändert werden
* Label:Hier kann ein eigener sprechender Name fuer das MQTT Bridge Thing vergeben werden
* Broker Hostname/IP: Hier die Verbindung zum Broker auf dem System angeben/einstellen

<img src="{{ site.baseurl }}/images/kuerbis/new-MQTT-Broker.jpg" width='800'  >
![image logo](../projects/Halloween/images/kuerbis.jpg)


Klicke dann auf Thing erstellen. 

Das Bridge Thing wird erstellt und erscheint in der Liste der Things. 
Nach ein paar Sekunden wird es als ONLINE angezeigt.

Wir haben nun einen funktionierenden MQTT-Broker, und openHAB ist über das Bridge Thing mit dem MQTT-Broker verbunden. 
Wir können nun beginnen, einzelne Geräte als Generic MQTT Thing hinzuzufügen.

<b> Konfiguration openHAB3 für die Verbindung mit einem Device </b>


Um zB einen Mikrocontroller zu steuern, müssen wir ein Generic MQTT Thing konfigurieren

	Settings -> Things -> Blue + -> MQTT Binding -> Generic MQTT Thing

Eingabe der initialen Konfigurationsinformationen

<i> * Unique ID: </i>
Der Wert kann auf default gelassen werden oder es wird eine eigene ID vergeben, die eindeutig sein muss.
Einmal angelegt, kann diese ID nachträglich nicht mehr geändert werden

<b> zB: espD1-02 </b> (in this case for 2nd D2 Mini)

<i>  * Label: </i>  
sprechender Name für das Device
<b> zB „ESPD1-02“ </b> an Stelle von Generic MQTT Thing

<img src="{{ site.baseurl }}/images/kuerbis/New-Generic-MQTT-Thing.jpg" width='800'>
![image logo](../projects/Halloween/images/kuerbis.jpg)

￼

Anschliessend erstellen wir einen Kanal für dieses MQTT Thing. Hier werden die Eigenschaften des Kanals definiert, 
zB ob es sich um einen Schalter handelt. 

<img src="{{ site.baseurl }}/images/kuerbis/Add-Channel-1.jpg" width='800'>
![image logo](../projects/Halloween/images/kuerbis.jpg)

Zusätzlich definieren wir hier die MQTT topics,
über die später die Informationen mit unserem Device ausgetauscht werden, zB

<i>  stat/espD1/OnOff </i> , um das Device an- oder auszuschalten (Werte ON oder OFF)


<img src="{{ site.baseurl }}/images/kuerbis/Add-Channel-2.jpg" width='800'>
![image logo](../projects/Halloween/images/kuerbis.jpg)

###  Code Beispiele

Damit sind alle wichtigen Voraussetzungen erfüllt, um Informationen zwischen dem SmartHome openHAB und 
dem Mikrocontroller auszutauschen.

Im Arduino Sketch findet ihr dann die entsprechenden Definitionen 

		const char* mqtt_server = „IP Adresse des Brokers“;

Kommando zum Schreiben von Informationen auf ein topic 

		client.publish("stat/espD1/OnOff“, „WriteString);

bzw dem Auslesen der Informationen auf der Empfängerseite.

		client.subscribe("stat/espD1/OnOff);


openHAB.rules

		val mqttActions = getActions("mqtt","mqtt:broker:ID-number)
		//  Schreibe auf das topic
		mqttActions.publishMQTT("stat/espD1/OnOff“,“OFF")



Viel Spass beim ausprobieren.

Bei Fragen/Hinweisen/Kommentaren kommt gerne auf mich zu.




# LED Streifen mit ESP8266 und openHab steuern

### [zurück zur Startseite](https://frankhoerper.github.io/FranksTechCorner)

Die Grundlagen für die Programmierung eines Mikrocontrollers habe ich in dem Beitrag
<link auf den Beitrag> beschrieben.
 
In diesem Projekt möchte ich euch vorstellen, wie ihr einen WS2812B LED Streifen mit Hilfe eines ESP8266 
Mikrocontrollers und OpenHab steuern könnt. 
Als Steuerungszentrale nutze ich mein SmartHome auf Basis einer  Openhab Umgebung.
Alternativ könnt ihr eine fertige App für euer Smartphone verwenden oder ein WebInterface , um die
LEDs nach Belieben zu steuern.
Die WS2812B Lichterkette unterscheidet sich von einfachen LED Lichterketten dadurch, dass wir bei der WS2812 jede 
einzelne LED ansteuern können. Es lassen sich auch Gruppen von LEDs zusammenfassen, die dann sowohl von den 
Effekten als auch der Farbe etc individuell gesteuert werden können. 

Beispiel für eine App:
[WLED App](https://github.com/Aircoookie/WLED-App)

Wie auch bei den anderen Projekten lasst uns zunächst einen Blick auf die notwendigen Komponenten werfen:

### Gesamtprojekt Teileübersicht
<img src="../LED-stripe/images/Gesamt-Projekt-Teileluebersicht.jpg" width='500'>
### 2
![Book logo](../LED-stripe/images/Gesamt-Projekt-Teileluebersicht.jpg)
![Book logo](/least-github-pages/assets/logo.png)
### 3
<img src="/images/LED-stripe/Gesamt-Projekt-Teileluebersicht.jpg" width='500'>


#### Gesamtverkabelung
<img src="{{ site.baseurl }}/images/LED-stripe/ESP+Gesamtverkabelung.jpg" width='300'>

#### LED Streifen

<img src="{{ site.baseurl }}/images/LED-stripe/LED-strip-BTF-Verpackung.jpg" width='200'>
<img src="{{ site.baseurl }}/images/LED-stripe/LED-strip-strip-Rolle.jpg" width='200'>

#### D1 Mini  mit ESP8266-12F WLAN Module CH340G Lua kompatibel mit Arduino
<img src="{{ site.baseurl }}/images/LED-stripe/ESP-D1-Mini-Verkabelung.jpg" width='200'>

[Amazon link](https://www.amazon.de/gp/product/B01N9RXGHY/ref=as_li_tl?ie=UTF8&tag=diesudas06-21&camp=1638&creative=6742&linkCode=as2&creativeASIN=B01N9RXGHY&linkId=d735117de710eaf041a18dcf3390f753)

#### Netzteil
<img src="{{ site.baseurl }}/images/LED-stripe/Netzteil.jpg" width='300'>


Grundlagen für Netzteil, Berechnung der notwendigen Leistung etc findet ihr zB 
[hier](https://www.xgadget.de/anleitung/ws2812b-maximale-laenge-stromversorgung/)


#### Erste Lessons learned

Bei meinen ersten Tests mit dem ESP und der Lichterkette, habe ich den ESP wie gewohnt über ein USB Kabel mit meinem Rechner verbunden,
um den Arduino Sketch anpassen und übertragen zu können. Die ersten Tests liefen allerdings gründlich schief, da die Lichterkette die Signale
des ESP scheinbar falsch übermittelt bekam. 
Erst als ich den ESP vom USB getrennt und mit dem Netzteil verbunden hatte, funktionierte die Signalübertragung wie erwartet. 
Ich bin kein Elektrotechniker, aber vermutlich werden die empfindlichen Signale des ESP durch das USB Kabel gestört. 
Dadurch passiert zwar irgendwas mit der Lichterkette, allerdings nicht die Effekte die per Kommando an den ESP übermittelt werden. 
Das nur so am Rande als lessons learned…


Um die Basisfunktionen der LED Streifen auszutesten, findet ihr in der Arduino Bibliothek einige Beispiele. 

<img src="{{ site.baseurl }}/images/LED-stripe/Arduino-ws2812fx-Beispiele.jpg" width='150'>

Das kann zB ein simpler erster Test für die verschiedenen Modi mit einem WebInterface sein. 
Zudem gibt es mit WLED eine fertige App, (siehe link oben) mit der sich sowohl per App als auch im Browser der LED Streifen steuern lässt. Einfach mal losprobieren und ihr werdet erstaunt sein, was man für tolle Effekte findet. 
Mit einiger Übung sind den eigenen kreativen Ideen dann wenige Grenzen gesetzt.

Da ich die LEDs über das openHab SmartHome steuere, nutze ich wie in dem anderen Beispiel auch, MQTT als Kommunikationsprotokoll. 
Dazu müssen wir im Sketch entsprechend eine Verbindung per Wlan zu dem Mqtt Broker aufbauen und dann die entsprechenden 
Kommandos für brightness, speed und modus von openHab an den Mikrocontroller schicken. 

Doch zunächst habe ich mir für erste Tests und den späteren produktiven Einsatz einige Steuerungselemente in der sitemap definiert.

#### Sitemap

In der sitemap habe ich items zum ein/ausschalten des ESPs und für die Einstellung
- der Helligkeit
- die Auswahl des Effekt-Modus 
- die  Farbe 
angelegt.

Für die Auswahl der Farbe bietet sich ein item vom Type Colorpicker an. 

<img src="{{ site.baseurl }}/images/LED-stripe/Colorpicker.jpg" width='150'>


Für die Auswahl des Modus würde ich normalerweise auf den item-Type Selection zurückgreifen. 
Allerdings funktioniert dann die sitemap in der aktuellen openHab App nicht mehr. 
Daher habe ich mehrere Item-Switches mit entsprechenden mappings zur Auswahl des Effekts angelegt. 
Sieht nicht so schön aus, ist aber erstmal als workaround hilfreich. 
Wer die App nicht nutzt, kann direkt beim item Type selection bleiben und dort die Modi in den Metadaten definieren.

#### MQTT Generic Things anlegen

Im nächsten Schritt erstellen wir die MQTT Generic Things für die Kommunikation des ESP mit unserem MQTT Broker. 
Die MQTT Things werden mit den entsprechenden items unserer sitemap verknüpft, so dass die Änderung der Einstellung 
je nach Steuerungselement (item) an den ESP übermittelt wird. 
Exemplarisch schauen wir uns die notwendigen Schritte für die Auswahl der Farbe mittels Colorpicker Item Type an:


#### Generic MQTT Thing und Item für Farbauswahl erstellen


1. MQTT Binding anlegen

<img src="{{ site.baseurl }}/images/LED-stripe/mqtt-color-1.jpg" width='500'>


2. Generic MQTT Thing anlegen

<img src="{{ site.baseurl }}/images/LED-stripe/mqtt-color-2.jpg" width='500'>

3. Channel anlegen

<img src="{{ site.baseurl }}/images/LED-stripe/mqtt-color-3.jpg" width='500'>

<img src="{{ site.baseurl }}/images/LED-stripe/mqtt-color-4.jpg" width='500'>

<img src="{{ site.baseurl }}/images/LED-stripe/mqtt-color-5.jpg" width='500'>



4. Item mit dem Channel verlinken

<img src="{{ site.baseurl }}/images/LED-stripe/mqtt-color-6.jpg" width='500'>

<img src="{{ site.baseurl }}/images/LED-stripe/mqtt-color-7.jpg" width='500'>


Das fertige Element sieht dann im Browser oder in der App wie folgt aus:

<img src="{{ site.baseurl }}/images/LED-stripe/sitemap-colorpicker.jpg" width='500'>


Mit den fertigen Steuerungselementen kann die LED Lichterkette nun gesteuert, dh Farbe , Helligkeit
etc geändert werden.
In einer rules Datei habe ich mir hierzu Regeln definiert, die auf Zustandsänderungen der Items reagieren
oder zu bestimmten Zeiten eine Aktion, zB Änderung der Helligkeit ausführen.
Das kann dann zB wie folgt aussehen:

	rule "ESP03 Brightness"
	when
	  Item GenericMQTTThingesp03Brightness_esp03Brightness changed	
	then
    	  val mqttActions = getActions("mqtt","mqtt:broker:331a9f6380")
    	  mqttActions.publishMQTT("stat/esp03/brightness",GenericMQTTThingesp03Brightness_esp03Brightness.state.toString)
	end

Hierbei ist zu beachten, dass der Dezimalwert als String per MQTT gesendet werden muss, dh eine Konvertierung
in den richtigen Type vor dem Senden notwendig ist.


#### Farbwerte konvertieren

Wenn eine Farbe mit dem Colorpicker Element ausgewählt wurde, muss der entsprechende HSB Wert in einen 
Hexadezimal String konvertiert werden. Dieser wird dann per MQTT an den Microcontroller gesendet und weiter 
verarbeitet:


	// *********************************************************************************************
	// Converts OpenHAB colorpicker HSB values into RRGGBB hex string for MQTT
	// see references at:
	// https://docs.openhab.org/configuration/sitemaps.html#element-type-colorpicker
	// https://community.openhab.org/t/oh2-how-to-convert-colorpicker-to-rgb-values/
	// https://community.openhab.org/t/example-convert-color-item-values-to-rgb-with-explanation/
	// *********************************************************************************************
	rule "Set HSB value of item RGBLed to RGB color value"
	when
        	Item GenericMQTTThingesp03color_esp03color changed
	then
        	val hsbValue = GenericMQTTThingesp03color_esp03color.state as HSBType
        	val brightness = hsbValue.brightness.intValue
        	val redValue = String.format("%02X", (hsbValue.red.floatValue*2.55*hsbValue.brightness.intValue/100).intValue)
        	val greenValue = String.format("%02X", (hsbValue.green.floatValue*2.55*hsbValue.brightness.intValue/100).intValue)
        	val blueValue = String.format("%02X", (hsbValue.blue.floatValue*2.55*hsbValue.brightness.intValue/100).intValue)
        	// the below one does not output two character of hex (%02X).
        	//val redValue = Integer.toHexString(hsbValue.red.intValue)
        	//val greenValue = Integer.toHexString(hsbValue.green.intValue)
        	//val blueValue = Integer.toHexString(hsbValue.blue.intValue)

        	val color = redValue + greenValue + blueValue
        	sendCommand(GenericMQTTThingesp03color_esp03color, color)
        	val mqttActions = getActions("mqtt","mqtt:broker:<broker-ID>)
        	mqttActions.publishMQTT("stat/esp03/color", color.toString)
	end


Auf der Arduino Seite sieht der Code wie folgt aus:

	// ******************************************************
	// Hier wird die Nachricht auf dem MQTT topic analysiert und 
	// die entsprechende Funktion aufgerufen
	// ******************************************************
	void callback(char* topic, byte* message, unsigned int length) {
  	  Serial.println("Message arrived on topic: ");
  	  Serial.println(topic);
  	  Serial.println(". Message: ");
  	  String messageTemp;

	for (int i = 0; i < length; i++) {
	  Serial.print((char)message[i]);
	  messageTemp += (char)message[i];
	}

	// *********************************************************
	// stat/esp03/color topic ändert die Farbe der LED Kette
	// der String muss in einen long Type umgewandelt werden
	// *********************************************************
	if (String(topic) == "stat/esp03/color") {
	  Serial.print("---> stat/esp03/color value ARRIVED:");
	  Serial.println(messageTemp);
	  Serial.println();

	  Serial.println("\n\nColor String to Hex (): ");
	  Serial.println(strtol(messageTemp.c_str(), NULL, 16)); 
	  uint32_t tmp = (uint32_t) strtol(messageTemp.c_str(), NULL, 16);
	  if(tmp >= 0x000000 && tmp <= 0xFFFFFF) {
	  Serial.print("str = D3B0AA : "); 
	  String str = "7FFFFFFF";
	  Serial.println(strtol(str.c_str(), NULL, 16));        
	  Serial.println(); 
	  Serial.print("tmp = "); 
	  Serial.println(tmp);
	  ws2812fx.setColor(tmp);
	}


Zur Zeit nutze ich erstmal die Grundfunktionen zur Steuerung des LED Streifens, aber die Möglichkeiten sind
ziemlich umfangreich. Da jede einzelne LED angesteuert werden kann, lassen sich zB Segmente definieren, die dann 
unabhängig vom Rest des LED Streifens programmiert werden können.  Zudem kann man über das Selection Item viele
vordefinierte Effekte aktivieren. Lasst euch überraschen, was hier alles möglich ist.

In den Foren und communities finden sich hierzu unzählige Code Beispiele, so dass ich mich hier auf die wesentlichen 
Informationen zum Einstieg beschränken möchte. 
Der Kreativität sind grundsätzlich erstmal keine Grenzen gesetzt. Von daher wünsche ich viel Spass beim 
Ausprobieren und Geniessen der Effekte.

# Trovemat Kiosk User Guide

## Konfigurationsdateien des Kiosks (configs)
Die Konfigurationsdateien des Kiosks werden im Verzeichnis configs gespeichert.
* config.xml - Konfiguration des Kiosks.
* menu.xml - Menüstruktur.
* operators.xml - Liste der Operatoren.
* crypto.xml - Liste der verschlüsselten Variablen.
* *_lastgood.xml - letzte erfolgreich heruntergeladene Datei.
* *_lastbad.xml - letzte erfolglos heruntergeladene Datei.
	
Beispiel:
1. Wenn config.xml korrekt ist. Nach der erfolgreichen Überprüfung wird der Kiosk sie kopieren und unter dem Namen config_lastgood.xml speichern.
1. Wenn [operators.xml](#Liste-der-Operatoren-operatorsxml) falsch ist. Nach der erfolglosen Überprüfung wird der Kiosk sie kopieren und unter dem Namen operators_lastbad.xml speichern. Der Kiosk wird weiter versuchen, die Datei operators_lastgood.xml herunterzuladen und wenn sie valid ist, wird der Kiosk die Datei operators_lastgood.xml kopieren und unter dem Namen [operators.xml](#Liste-der-Operatoren-operatorsxml) speichern

## Konfiguration des Kiosks (config.xml)
	
Ein Beispiel der config-Datei:
``` XML
<config>
    <parameters>
		<point_id>1</point_id>
		<extended_logging>false</extended_logging>
		<point_name>Trovemat kiosk #1</point_name>
	</parameters>
	<gateways>
		<jetcrypto_wallet type="trovemat" url="beta.jetcrypto.com" username="demo" password="crypto.jetcrypto_wallet_password" check="true" pay="true" tasks="3" tasks_interval="300" />
		<tox_messenger type="tox_text" >
			<friends>
				<users />
			</friends>
			<nodes>				
				<node_1 ip="144.76.60.215" port="33445" tox_id="04119E835DF3E78BACF0F84235B300546AF8B936F035185E2A8E9E0A67C8924F" />
				<node_3 ip="128.199.199.197" port="33445" tox_id="B05C8869DBB4EDDD308F43C1A974A20A725A36EACCA123862FDE9945BF9D3E09" />
				<node_4 ip="23.226.230.47" port="33445" tox_id="A09162D68618E742FFBCA1C2C70385E6679604B2D80EA6E84AD0996A1AC8A074" />
				<node_5 ip="2001:bc8:4400:2100::1c:50f" port="33445" tox_id="2C289F9F37C20D09DA83565588BF496FAB3764853FA38141817A72E3F18ACA0B" />
				<node_6 ip="178.21.112.187" port="33445" tox_id="4B2C19E924972CB9B57732FB172F8A8604DE13EEDA2A6234E348983344B23057" />
				<node_7 ip="163.172.136.118" port="33445" tox_id="2C289F9F37C20D09DA83565588BF496FAB3764853FA38141817A72E3F18ACA0B" />
			</nodes>
		</tox_messenger>	
	</gateways>
    <payments>
    	<gateway>jetcrypto_wallet</gateway>
		<currency>USD</currency>
		<limit_min>10</limit_min>
		<limit_max>3000</limit_max>
		<common_params>
			<poloniex_public_key>00000000000000000000000000000000000000000000000000000000</poloniex_public_key>
		</common_params>
    </payments>
	<interface wait_init_devices="2" lang="EN" >
		<menu vending="true" >
			<limit_max USD="3000" />
		</menu>
		<inactivity_timer data_entry="120" message="120" money_entry="120" />
	</interface>
    <terminal>
        <init>
<app>./startup.sh</app>
<timeout></timeout>
        </init>
    </terminal>
    <peripherals>
        <barcodereader model="" port="" baudrate="9600" extended_logging="config.parameters.extended_logging" show_errors="0" />
        <printer model="" port="" baudrate="9600" extended_logging="config.parameters.extended_logging" show_errors="0" charset_code_table="0" />
        <validator model="" port="" baudrate="9600" extended_logging="config.parameters.extended_logging" show_errors="0" />
    </peripherals>
    <point_info>
		<dealer_name name="" value="JetCrypto" />
		<dealer_address name="" value="Dzērbenes iela 14, Vidzemes priekšpilsēta, Rīga, LV-1006" />
		<dealer_phone name="" value="+371 66 555 098" />
		<point_address name="" value="Dzērbenes iela 14, Vidzemes priekšpilsēta, Rīga, LV-1006" />
    </point_info>
</config>
```

> Standardwerte.
Wenn ein Parameter in der Config-Datei fehlt oder den Wert "default" hat, wird ihr Standardwert genommen. 

> Verweise auf Werte.
Wenn der Parameterwert sich mit "config." beginnt, behandelt der Kiosk ihn als einen Verweis. Im obigen Beispiel ist das Attribut extended_logging des Tags barcodereader über einen Verweis auf den Wert extended_logging in parameters angegeben.

* parameters - allgemeine Parameter des Terminals.
    * extended_logging - zusätzliches Logging:
   	 > **!!! ACHTUNG ! Wenn das zusätzliche Logging aktiviert ist, kommen absolut alle auf das Gerät übergegebene Daten einschließlich geheime Daten, die für Logging nicht vorgesehen sind, in die Logdatei hinein !!!** 
		- **"false"** (Standardwert) - deaktiviert.
		- **"true"** - aktiviert.
    * point_id - Punktnummer. Der Standardwert ist 0.
    * point_name - Name des Punktes für Darstellung im Messenger als Kontaktname. Wenn dieses Attribut nicht angegeben ist, wird der Kontaktname "Trovemat kiosk #<WERT AUS DEM ATTRIBUT point_id>".
* payments - standardmäßige Parameter der Zahlungen. Diese Parameter können für jeden Operator im Schritt "money_entry" in operators.xml vorgegeben werden. Die Parameter sind im Abschnitt "Operatoren" beschrieben.
    * gateway - Standardwert - "" - Name des Gateways
    * currency - Standardwert - "USD".
    * limit_min - Standardwert - "0".
    * limit_max - Standardwert - "1000000".
    * payment_type - Standardwert - "mut_price".
    * fee - standardmäßig - nicht vorhanden.
    * check_name - Standardwert - "default.chq".
    * failed_check_name - Standardwert - "default_failed.chq".
    * common_params - Liste der Parameter, die in operators.xml wie "Felder" verwendet werden können.
* interface
    * wait_init_devices - Übergang von der Initialisierungsseite zur Menüseite.
		- **"0"** (Standardwert) - kein Warten auf die Initialisierung der Geräte.
		- **"1"** - Warten auf Beendigung der Initialisierung der Geräte mit dem Parameter "show_errors", der nicht gleich "0" ist.
		-**"2"** - Warten auf Beendigung der Initialisierung aller Geräte.
	* lang - standardmäßige Sprache der Hauptseitenschnittstelle
	* default_phone_code - standardmäßige Landeskennzahl laut ISO 3166-1 bei der Eingabe der Telefonnummer. Wenn der Attributwert leer ist, wird die standardmäßige Landeskennzahl nicht angezeigt. Wenn dieses Attribut nicht angegeben ist, ist die standardmäßige Landeskennzahl "+1"
	* menu - Tag, der den Sachverhalt der Hauptseite beschreibt:
		* vending - der Standardwert ist "true" - Flag, das Bareinzahlung auf dem Hauptbildschirm ohne Auswahl einer bestimmten Kryptowährung für den Kauf zulässt
		* limit_max - der Höchstbetrag für Einzahlung auf dem Hauptbildschirm in der angegebenen Währung.
	* inactivity_timer - Timeout der Inaktivität des Benutzers in Sekunden auf verschiedenen Bildschirmen:
		* data_entry - auf dem Bildschirm der Eingabe (Abtastung) der Geldbörsennummer
		* message - auf dem Bildschirm der Nachricht an den Kunden
		* money_entry - auf dem Bildschirm der Bargeldeinzahlung
* terminal
    * init - Tag, der die Parameter für Start der Applikation beim Start des Kiosks
        * app - Reihe - Applikation, die vom Kiosk in einem separaten Prozess beim Start angelassen wird. Argumente können nach Leerstellen hinzugefügt werden.
        * timeout - Ganzzahl - Wartezeit des Starts und der Beendigung der Applikation in Millisekunden. Wenn der Wert von -1 angegeben ist, wird das Timeout nicht kontrolliert.
* peripherals    
    * model - Modell des Gerätes. Wenn kein Gerät vorhanden ist, lassen Sie den Wert leer. Der Standardwert ist "".
    * port - Nummer des Serienports, der von diesem Gerät verwendet wird. Der Standardwert ist "".
    * baudrate - Baudrate des COM-Ports, der von diesem Gerät verwendet wird. Der Standardwert ist "9600".
    * extended_logging - zusätzliches Logging, einschließlich aller in den COM-Port ans Gerät übergegebenen Befehle.  
		> **!!! ACHTUNG ! Wenn das zusätzliche Logging aktiviert ist, kommen absolut alle auf das Gerät übergegebene Daten einschließlich geheime Daten, die für Logging nicht vorgesehen sind, in die Logdatei hinein !!!**  
		- **"false"** (Standardwert) - deaktiviert.
		- **"true'** - aktiviert.
    * show_errors - falls Mängel in diesem Gerät gefunden sind, den Bildschirm "Kiosk funktioniert nicht" anzeigen.
		- **"0"** (Standardwert) - deaktiviert.
		- **"1"** - nur wenn keine aktive Zahlungen oder Restbargeld vorhanden sind.
		- **"2"** - sofort umschalten.
* validator - Unterstützte Modelle (wird im model-Attribut angegeben):
    - **"mei_ebds"** - MEI-Geldscheinempfänger. Umprogrammieren beim Kiosk wird unterstützt. Beim Start des Kiosks wird Anwesenheit der Datei 'validator.bin' (Firmware) im Wurzelverzeichnis nachgeprüft und, falls diese Datei anwesend ist, wird die Aktualisierung der Firmware versucht. Nach der erfolgreichen Firmware-Aktualisierung wird die Datei gelöscht. Unterstützt die folgenden Parameter:
		- **enabled_currencies** - Liste der Währungen, die der Geldscheinempfänger akzeptieren kann. Ein leerer Wert bedeutet Akzeptieren aller Währungen, die durch die Firmware des Geldscheinempfängers unterstützt werden. Der Standardwert ist eine leere Zeichenfolge. 2 Aufzeichnungsformate werden unterstützt:
			1. Zeichenfolge mit Trennzeichen "," (Komma), in der Zeichenfolge werden dreistellige Währungsbezeichnungen nach ISO-4217 durch Trennzeichen angegeben. Beispiel: "RUB,USD" - Währungen "Russischer Rubel" (RUB, 643), "US-Dollar" (USD, 840) sind zulässig.
			1. Zeichenfolge mit Trennzeichen "," (Komma), in der Zeichenfolge werden bestimmte zulässige Banknoten durch Trennzeichen angegeben. Banknoten sind im folgenden Format angegeben: "CUR:NOM". CUR ist dreistellige Währungsbezeichnung der Banknote nach ISO-4217, NOM ist Nennwert der Banknote in Grundeinheiten der angegebenen Währung. Beispiel: "RUB:10,RUB:1000,USD:10,USD:100" - Banknoten "10 Russische Rubel" (RUB, 643, 10RUB), "1000 Russische Rubel" (RUB, 643, 1000RUB), "10 US-Dollar" (USD, 840, 10USD), "100 US-Dollar" (USD, 840, 100USD) sind zulässig.
    - **"test_validator"** - software-emulierter Test-Geldscheinempfänger für Entstörung.
* printer - Unterstützte Modelle (wird im model-Attribut angegeben):
	- **"tg2480"** - Belegdrucker CUSTOM TG2480. In einigen Fällen (abhängig von der Version der Firmware für den Drucker) wird auch der Drucker Custom VKP 80 II mit den gleichen Einstellungen unterstützt.
	- **"np-f3092d"** - Belegdrucker Nippon NP-F3092D. Für diesen Drucker ist die Funktion der automatischen Suche nicht implementiert, die Einstellungen für Verbindung sind manuell einzugeben.
	- **"test_printer"** - software-emulierter Test-Drucker für Entstörung.
    * charset_code_table - Nummer der beim Drucken verwendeten Symboltabelle. Der Standardwert ist "0".
        * Für tg2480:
- **"0"** - CP437 (US)
- **"2"** - CP850 (Multilingual)
- **"3"** - CP860 (Portuguesse)
- **"4"** - CP863 (Canadian-French)
- **"5"** - CP865 (Nordic)
- **"17"** - CP866 (Cyrillic)
- **"19"** - CP858 (for Euro symbol 213)
* dispenser - Unterstützte Modelle (wird im model-Attribut angegeben):
    - **"puloon_lcdm"** - Geldscheinspender Poloon LCDM1000/2000 (entsprechend 1 und 2 Kassetten).
    - **"test_dispenser"** - software-emulierter Test-Geldscheinspender für Entstörung (2 Kassetten).
    * Anzahl der folgenden Attribute soll der Anzahl der Kassetten im Spender entsprechen:
    * cassette_0 - Nennwert der Geldscheine der 0. Kassette, zum Beispiel "100 USD".
    * default_capacity_0 - standardmäßige Anzahl der Geldscheine für die 0. Kassette (dieser Wert kann bei Einkassierung geändert werden), zum Beispiel "1000".
		
* point_info - in diesem Anteil kann eine unbegrenzte Anzahl von Systemfeldern mit beliebigen Namen vorgegeben werden. Diese Felder können beim Drucken der Belege und für Abfragen an Gateways verwendet werden. "Id" des Feldes wird als _INFO_* eingegeben, wo * ist der Name des Feldes in Großbuchstaben. Der Name und Wert können mit Attribute "name" und "value" eingegeben werden. Beispiel: <dealer_name name="Händler" value="Name der Gesellschaft"></dealer_name> verwandelt sich in ein Systemfeld mit id="_INFO_DEALER_NAME", name="Händler" und value="Name der Gesellschaft".

* gateways In diesem Abschnitt können Sie eine unbegrenzte Anzahl von Tags eingeben, die die Verbindung zu verschiedenen Servern beschreiben.
Name des verschachtelten Tags ist der Name des Gateways, der in Schritten verwendet werden kann (siehe Tag "step" Attribut "gateway"). Für jedes Gateway werden die folgende Attribute angegeben:
	- type - Name des Typs des Gateways. Verfügbare Typen: 
		- **"trovemat"** - Gateway für den Zugriff zu api.jetcrypto.com.
		Für dieses Gateway werden folgende zusätzliche Attribute angegeben:
			- url - Adresse des Servers		
			- username - Konto für den Zugriff
			- password - Passwort für den Zugriff zu API
			- tasks_interval - Intervall der Abfrage des Gateways für neue Aufgaben
		- **"tox_text"** - Gateway für den Zugriff zum Terminal über beliebigen tox-Messenger.
			* Für dieses Gateway werden folgende zusätzliche Attribute angegeben:
				- udp_enabled - Verwendung des UDP-Protokolls. Der Standardwert ist "true".
				- local_discovery_enabled - Suche nach andere tox-Einheiten innerhalb des lokalen Netzwerkes. Der Standardwert ist "true".
				- hole_punching_enabled - Verwendung der "hole-punching" Methode für Suche nach andere tox-Einheiten. Der Standardwert ist "true".
				- Alle drei Attribute (udp_enabled, local_discovery_enabled, hole_punching_enabled) - können für Datenverkehrsreduzierung abgeschaltet werden, in diesem Fall aber ist die tcp-Verbindung zur Einheit notwendig, die tcp relay unterstützt.
			* Für dieses Gateway werden folgende zusätzliche Tags angegeben:
				- users - Liste der vertrauenswürdigen tox-Konten. Name des verschachtelten Tags ist der Name dieses Kontos.
					- tox_id - Kennzeichnung des tox-Kontos
					- message_queue_limit - Größe der Schlange der nicht gesendeten Nachrichten (wenn das Terminal oder der Benutzer offline ist). Der Standardwert ist "100".
				- nodes - Liste der Einheiten für Verbindung ans tox-Netzwerk. Name des verschachtelten Tags ist der Name dieser Einheit.
					- ip - IP-Adresse der Einheit
					- port - Port der Einheit
					- tox_id - öffentlicher Schlüssel der Einheit
					- tcp_relay - Verbindung an die Einheit über TCP. Macht nur Sinn, wenn die Einheit dieses Feature unterstützt und UDP für das Gateway abgeschaltet ist. Der Standardwert ist "false".
		- Eine Anzahl der Attribute wird im Wurzeltag für Gateways ohne Benutzer (z. B. "trovemat") und in Tags, die Benutzer für Gateways mit Benutzern (z. B. "tox_text") beschreiben, angegeben:
			- check - Erlaubnis für Verwendung dieses Gateways für Informationsanforderungen aus dem Kiosk. Der Standardwert ist "false".
			- pay - Erlaubnis für Verwendung dieses Gateways für Zahlungsanforderungen aus dem Kiosk. Der Standardwert ist "false".
			- tasks - Ebene der Befehle, die für Ausführung von der Seite des Gateways erlaubt sind (die angegebene Ebene und niedriger).
				- **"-1"** (Standardwert) - alle sind verboten.
				- **"0"** - Anzeigen der Informationen, die die Arbeit nicht beeinträchtigen.
				- **"1"** - Service-Befehle (Abschaltung, Neustart usw.).
				- **"2"** - Änderung der Terminal-Einstellungen.
				- **"3"** - Ausführung von Skripts als Administrator.
			- info_task - Ebene der Befehle, bei deren Ausführung von anderen Benutzern Sie Benachrichtigungen bekommen (angegebenes Niveau und höher). Der Standardwert ist "2".
			- info_factor - Erhalten der Benachrichtigungen über Zufügung/Entfernung der neuen Zustandsfaktoren. Der Standardwert ist "false".
			- info_full_state - Erhalten der Benachrichtigungen über Änderung des vollkommenen Zustandes des Terminals (bestehend aus der Summe aller aktuellen Zustandsfaktoren). Der Standardwert ist "false".
			- info_state - Erhalten der Benachrichtigungen über Änderung des einfachen Zustandes des Terminals (Ok, Error, usw.). Der Standardwert ist "false".
			- info_bill - Erhalten der Benachrichtigungen über den Erhalt der Geldscheine vom Geldscheinempfänger. Der Standardwert ist "false".
			- info_payment - Erhalten der Benachrichtigungen über Zahlungen. Der Standardwert ist "false".
    
## Menü (menu.xml)
Das Menü besteht aus Gruppen und Operatoren, also nur Tags der Art 'group' oder 'operator' sind in der Config erlaubt. Das Attribut 'type' ist obligatorisch für alle Elemente. Alle Operatoren, die im Menü angegeben sind, sollen auch in der Liste der Operatoren eingegeben werden [operators.xml](#Liste-der-Operatoren-operatorsxml). Operator in der Datei [operators.xml](#Liste-der-Operatoren-operatorsxml) wird nach dem Namen seines Tags bestimmt.

Attribute:
* "type" - Typ des Elements. Mögliche Werte sind: "group" (Gruppe von Operatoren), "operator" (ein bestimmter Operator).
* "name" - Name der Gruppe. Der Wert des Attributs ist ein Schlüssel des Satzes aus einer Datei mit lokalisierten Nachrichten interface/js/language.js für das aktuell ausgewählte Gebietsschema

Beispiel der menu.xml Datei für ein Menü, das aus 8 Operatoren auf der Hauptseite besteht:
``` XML
<?xml version="1.0" encoding="utf-8"?>
<menu type="group" name="choose_your_currency" >
	<bitcoin type="operator" />
	<ethereum type="operator" />
	<dash type="operator" />
	<ripple type="operator" />
	<monero type="operator" />
	<ethereum_classic type="operator" />
	<litecoin type="operator" />
	<nem type="operator" />
</menu>
```

## Liste der Operatoren (operators.xml)

* operator - Operator. Beschreibt das Schema der Zahlung. Dieser Tag kann jeden Namen haben, der den Regeln für Benennung der XML-Tags nicht widerspricht.
	* step - Schritt der Zahlung.
		* type - Typ des Zahlungsschritts. Erforderlicher Parameter. Kein Standardwert ist vorhanden. Zulässige Werte:			
			- **"data_entry"** - Eingabe der Zahlungsdaten vom Benutzer.
			- **"change_currency"** - Auswahl der Währung. Der Benutzer wählt eine Währung aus der Liste. 
				* mode - Mögliche Werte (Wege für Bildung einer Liste der Währungen):
					1. **"USD,RUB,EUR"** - Auflisten der Währungen, durch Kommas getrennt.
					1. **"NOTE_VAL"** (Standardwert) - alle Währungen, die vom Geldscheinempfänger unterstützt sind.			
			- **"money_entry"** - Einzahlung vom Benutzer. Hier werden auch Parameter für Zahlungen angegeben, für die die Standardwerte in conig.xml -> payments angegeben sind.
				* payment_type - Typ der Zahlung. Mögliche Werte:
					- **"mut_price"** - der Preis ist nicht fixiert.
					- **"fix_price"** - der Preis ist fixiert.
				* price - Preis (für fixierte Zahlung). Kein Standardwert ist vorhanden. Erforderlich einzugeben wenn **type="fix_price"**.
				* currency - Zahlungswährung, dreistelliger Code nach ISO 4217.
				* limit_min - minimaler Betrag für Bargeldeinnahme.
				* limit_max - maximaler Betrag für Bargeldeinnahme.
				* fee - Kommission, wird in Intervallen im Bereich des eingenommenen Geldes eingegeben.
					* part - ein Bereich; für jeden Bereich wird eine eigene Kommission eingegeben.
						* min - Schwellenwert, von dem die Kommission eingezogen wird.
						* percent - Zinsanteil der Kommission.
						* fix - fester Anteil der Kommission.
						
					Beispiel:
					Von 0 bis 50 Bezugseinheiten - eine Kommission von 2% + 2.01 Bezugseinheiten wird eingezogen.
					Von 50 und mehr - eine Kommission von 1% wird eingezogen.
					``` XML
					<fee>
						<part_1 min="0" percent="2" fix="2.01" />
						<part_2 min="50" percent="1" fix="0" />
					</fee>
					```
			- **"check"** - Abfrage an das Gateway ohne Zahlung.
				* server - Name des Servers, an das die Abfrage zu senden ist. Bestimmte Parameter für den Zugriff zum Server befinden sich in conig.xml -> network -> <NAME DES SERVERS>
				* path - Abfragezeichenfolge (Adresse). Zum Beispiel: "/api/Method"
				* method - Methode der Abfrage. Mögliche Werte:
					- **"POST"** - eine POST-Abfrage wird gesendet. Der Körper der Abfrage wird ein JSON-Objekt enthalten, bestehend aus Feldern, die in Tags "request_field" in diesem Schritt beschrieben sind.
					- **"GET"** - eine GET-Abfrage wird gesendet. Der Körper der Abfrage wird leer sein. Die URL der Abfrage wird aus Feldern, die in Tags "request_field" in diesem Schritt beschrieben sind, zusammengesetzt.
			- **"pay"** - Abfrage an das Gateway und Zahlung. Die Parameter dieses Schrittes sind ähnlich wie die Parameter für Schritte mit dem Typ "check"
			- **"print"** - Beleg drücken.
				* check_name - Name der Datei mit Belegmuster, Standardwert in config.xml.
			- **"message"** - Nachricht an den Benutzer.
		- request_field - Tag - wird für Schritte mit dem Typ "check" und "pay" als Abfragefeld verwendet:
			- id - Kennzeichnung des Feldes. Wenn sie das Zeichen "." (Punkt) enthält - wird dieser Parameter in der Abfrage mit dem Typ method="POST" als verschachtelte Elemente des JSON-Objektes gespeichert. Zum Beispiel, wenn der Parameter parameters.token angegeben ist, wird das folgende JSON-Objekt im Körper der Abfrage gesendet: {"parameters" : {"token" : "123123123123"}}
			- value - enthält den Wert des Abfrageparameters. Innerhalb des Wertes kann ein "Link" auf Daten eingetragen werden, die in einem anderen Schritt bekommen sind (vom Benutzer oder vom Gateway). Der Link auf das vom Client eingegebene Feld wird wie folgt beschrieben: "|fi_\<Wert des Attributs id des Tags field\>|". Der Link auf das vom Gateway erhaltene Feld wird wie folgt beschrieben: "|\<NAME_DES_PARAMETERS\>|". Zum Beispiel, wenn der Schritt mit dem Typ **"data_entry"** ausgeführt ist und der Client in diesem Schritt den Wert für das Feld mit id="2" eingegeben hat, kann der Wert wie "001-|fi_2|" angezeigt werden - in diesem Fall, wenn der Client den Wert "123" eingegeben hat, wird die Zeichenfolge "001-123" an den Server gesendet.
		- receive_field - das Feld, das vom Gateway in der Rückmeldung auf die Abfrage gesendet wird.
			- id - Name des Parameters in der Rückmeldung vom Server. Ein Parameter mit diesem Namen wird auch in der aktuellen Client-Session generiert. Dieser Name wird nur innerhalb der aktuellen Client-Session relevant sein. Dieser Name kann im Link verwendet werden. Zum Beispiel, das Gateway sendet in der Rückmeldung ein Feld mit dem Namen "buy". Im Schritt wird Folgendes angegeben: "...<receive_field id="buy" name="COURSE" />...". In jedem weiteren Schritt, z.B. im Tag "request_field" kann der Wert dieses Parameters durch Anzeigen der Zeichenfolge "|buy|" verwendet werden - statt dieser Zeichenfolge wird der bestimmte vom Gateway erhaltene Wert eingesetzt.
			- name - Name des Parameters innerhalb dieses Skripts. 

Beispiel der Datei operators.xml, bestehend aus zwei Operatoren:
``` XML
<?xml version="1.0" encoding="utf-8"?>  
<operators>
	<bitcoin name="bitcoin" long="bitcoin" short="BTC" image="btc.png" >
		<step_0 type="check_printer" />
		<step_1 type="money_entry" />
		<step_2 type="data_entry" >
			<field_wallet name="Wallet" barcode_title="scan_your_wallet" title="enter_wallet_address" input_type="barcode" keyboard="text" regexp="^[13][a-km-zA-HJ-NP-Z1-9]{25,34}$" validChars="[a-km-zA-HJ-NP-Z1-9]" control_buttons="SHIFT DEL" language="EN" />
		</step_2>
		<step_3 type="data_entry" >
			<field_phone name="Phone number" title="enter_your_phone_number" keyboard="numbers" regexp="(9[976]\d|8[987530]\d|6[987]\d|5[90]\d|42\d|3[875]\d|2[98654321]\d|9[8543210]|8[6421]|6[6543210]|5[87654321]|4[987654310]|3[9643210]|2[70]|7|1)\d{1,14}$" />
		</step_3>
		<step_4 type="check" path="/api/trovemat/Phone/verify" method="GET" >
			<phoneNumber type="request_field" value="|field_phone|" />
			<message type="request_field" value="Trovemat verification code: {0}" />
			<verifyCode type="receive_field" name="VERIFYCODE_RECEIVED" />
		</step_4>
		<step_5 type="data_entry" >
			<field_code name="Verification code" title="enter_verification_code" keyboard="numbers" print="0" regexp="[0-9]{4,}" />
		</step_5>
		<step_6 type="check" path="validate_verification_code" >
			<VERIFYCODE_SRC type="request_field" value="|verifyCode|" />
			<VERIFYCODE_DST type="request_field" value="|field_code|" />
		</step_6>
		<step_7 type="pay" path="/api/trovemat/Payment" method="POST">
			<operatorId type="request_field" value="31" />
			<params type="request_field" secure="true" value="{&quot;currency&quot;:&quot;BTC&quot;,&quot;address&quot;:&quot;|field_wallet|&quot;,&quot;secretKey&quot;:&quot;|_CRYPTO_poloniex_secret_key|&quot;,&quot;publicKey&quot;:&quot;|_COMMON_poloniex_public_key|&quot;,&quot;withdrawalAmount&quot;:&quot;|WITHDRAWAL_AMOUNT|&quot;,&quot;phoneNumber&quot;:&quot;|field_phone|&quot;}" />
			<currencyId type="request_field" value="|_CURRENCY_CODE|" />
			<amount type="request_field" value="|_ACCEPTED_MINOR_UNIT|" />
		</step_7>
		<step_8 type="print" check_name="poloniex.chq" failed_check_name="poloniex_failed.chq" />
		<step_9 type="message" />
	</bitcoin>
	<ethereum name="ethereum" long="ethereum" short="ETH" image="ethereum.png" >
		<step_0 type="check_printer" />
		<step_1 type="money_entry" />
		<step_2 type="data_entry" >
			<field_wallet name="Wallet" barcode_title="scan_your_wallet" title="enter_wallet_address" input_type="barcode" keyboard="text"  regexp="^(0x)?([0-9a-f]{40})|([0-9A-F]{40})$" validChars="[a-fA-FxX0-9]" control_buttons="SHIFT DEL" language="EN" />
		</step_2>
		<step_3 type="data_entry" >
		<field_phone name="Phone number" title="enter_your_phone_number" keyboard="numbers" regexp="(9[976]\d|8[987530]\d|6[987]\d|5[90]\d|42\d|3[875]\d|2[98654321]\d|9[8543210]|8[6421]|6[6543210]|5[87654321]|4[987654310]|3[9643210]|2[70]|7|1)\d{1,14}$" />
		</step_3>
		<step_4 type="check" path="/api/trovemat/Phone/verify" method="GET" >
			<phoneNumber type="request_field" value="|field_phone|" />
			<message type="request_field" value="Trovemat verification code: {0}" />
			<verifyCode type="receive_field" name="VERIFYCODE_RECEIVED" />
		</step_4>
		<step_5 type="data_entry" >
			<field_code name="Verification code" title="enter_verification_code" keyboard="numbers" print="0" regexp="[0-9]{4,}" />
		</step_5>
		<step_6 type="check" path="validate_verification_code" >
			<VERIFYCODE_SRC type="request_field" value="|verifyCode|" />
			<VERIFYCODE_DST type="request_field" value="|field_code|" />
		</step_6>
		<step_7 type="pay" path="/api/trovemat/Payment" method="POST">
			<operatorId type="request_field" value="31" />
			<params type="request_field" secure="true" value="{&quot;currency&quot;:&quot;ETH&quot;,&quot;address&quot;:&quot;|field_wallet|&quot;,&quot;secretKey&quot;:&quot;|_CRYPTO_poloniex_secret_key|&quot;,&quot;publicKey&quot;:&quot;|_COMMON_poloniex_public_key|&quot;,&quot;withdrawalAmount&quot;:&quot;|WITHDRAWAL_AMOUNT|&quot;,&quot;phoneNumber&quot;:&quot;|field_phone|&quot;}" />
			<currencyId type="request_field" value="|_CURRENCY_CODE|" />
			<amount type="request_field" value="|_ACCEPTED_MINOR_UNIT|" />
		</step_7>
		<step_8 type="print" check_name="poloniex.chq" failed_check_name="poloniex_failed.chq" />
		<step_9 type="message" />
	</ethereum>
</operators>
```

## Belegmuster (*.chq)

In Belegmustern können Variablen verwendet werden.

Regeln für Eingabe der Variablen:
1. Name der Variable ist in Prozentzeichen "%" eingefügt.
1. Im Namen der Variablen können lateinische Buchstaben, Ziffern und Unterstrich verwendet werden.
1. Nach dem Variablennamen können Parameter der Variable folgen.
	Regeln für Eingabe der Parameter der Variablen:
	1. Vor jedem Parameter soll ein Doppelpunkt ":" stehen.
	1. Die Parameter sollen exakt in der unten aufgeführten Reihenfolge geordnet sein.

Verfügbare Parameter:
1. Lateinische Großbuchstabe "N" oder "V". Gibt den Namen ("N") oder der Wert ("V") des Feldes soll ausgegeben werden. Standardwert: "V".
1. Die Zahl gibt die Länge des Ausgabewertes. Der Wert der Variable wird abgeschnitten oder mit Leerzeichen auf die angegebene Länge ergänzt.
1. Lateinische Großbuchstabe "R" oder "L". Gibt ein, wo - rechts ("R") oder links ("L") - soll das Feld bearbeitet werden (unnötige Zeichen entfernen oder Leerzeichen auf die gewünschte Länge hinzufügen). Standardwert: "L".

Beispiel für Bearbeitung und Ausgabe der Variablen.  
Muster:
```
--------------------------------
%SUM%
%SUM:N%: %SUM:V%
Wert des Feldes Betrag: %SUM:V%
%SUM:N:3:R%.: %SUM:V:5%
%SUM:20%
--------------------------------
```
Wenn das folgende Feld eingegeben ist:  
id="SUM" name="Betrag" value="1234567890"
Das Ergebnis wird dann wie folgt aussehen:
```
--------------------------------
1234567890
Betrag: 1234567890
Wert des Feldes Betrag: 1234567890
Betr.: 67890
          1234567890
--------------------------------
```
Benutzerdefinierte Variablen.
1. Können nicht mit einem Unterstrich "_" beginnen.
1. In den benutzerdefinierten Variablen werden alle Felder von Operatoren (field) hinzugefügt. Die Kennzeichnungen werden als "fi_id" (fi_23 für field id="23") angegeben.

Systemvariablen.
Variablen, die mit Unterstrich "_" beginnen, sind systembedingt. Die folgenden Variablen sind verfügbar:
* _FIELDS_FOR_PRINT - zeigt alle benutzerdefinierte Variablen, wobei der Wert "print" ist gleich "1", in Form einer Reihe von Zeilen: "Name_der_Variable: Wert_der_Variable".
* _INFO_* - Felder, die im Abschnitt config.xml -> config -> point_info angegeben sind.
* _TERMNUMBER - Terminalnummer (config.xml -> config -> parameters -> point_id)
* _DATETIME - Datum und Uhrzeit
* _ACCEPTED - Betrag des eingenommenen Bargeldes
* _COMISSION - Kommission
* _ENROLLED - Betrag des auf das Konto des Kunden einzuzahlenden Bargeldes
* _CURRENCY - dreistelliger Code der Währung der Zahlung
* _OPNAME - Name des Zahlungsoperators

Nur für den Scheck der Einkassierung:
* _INCS_CASSETTE - Nummer der aktuellen Bargeldkassette (zur Zeit ersetzt mit einer Leerzeile)
* _INCS_NEXT_CASSETTE - Nummer der neuen Kassette (zur Zeit ersetzt mit einer Leerzeile)
* _INCS_LAST_INCASSATION_TIME - vorherige Zeit der Einkassierung
* _INCS_INCASSATION_TIME - Zeit der aktuellen Einkassierung
* _INCS_MONEY_INFO - detaillierte Information über die eingenommene Geldscheine

## Protokolle (Logs/*.log)

Protokolle werden im Verzeichnis logs gespeichert, das sich auf der gleichen Ebene mit der Applikation befindet. Der Dateiname enthält ein Logging-Objekt und das Datum. Jede Zeile in der Datei besteht aus "Nachrichtentyp", Zeit, Stromkennzeichnung, Codezeilennummer und Nachrichtentext.

Logging-Objekte:
* Kiosk - Protokoll mit grundlegenden Informationen.
* Main - wichtige Informationen über Funktion des Programms, leer in der Abwesenheit von Fehlern.
* Device_* - Geräteprotokolle, wo * ist der Name des Geräts.
	
Nachrichtentyp:
* INF - einfache Nachricht, kann ignoriert werden, bis alles gut geht.
* WRN - die Nachricht ist zu beachten, vielleicht etwas geht nicht so, wie es geplant war.
* ERR - ein Fehler ist aufgetreten.
* EXT - erweiterte Informationen, wird nur angezeigt, wenn die entsprechende Einstellung in der Config des Kiosks aktiviert ist.
	
## Emulation der Geräte

Für Software-Testung kann Emulation statt echten Geräten verwendet werden. Um das zu tun, ist es notwendig, den bestimmten Namen in der Einstellungen des Typs des Gerätemodells anzugeben. Für Emulation der Ereignisse sollen zwei Tasten im Bereich (F1 - F12) konsequent auf der Tastatur gedrückt werden. Der erste Tastendruck wählt das Gerät, mit dem zweiten Tastendruck wird das Ereignis dieses Gerätes gewählt. 

* Validator.
    * model - "test_validator"
    * Taste - "F9"
    * Ereignisse:
        * F1 - das Gerät ist ausgefallen.
        * F2 - das Gerät funktioniert.
        * F3 - die Kassette ist entfernt.
        * F7 - 1 Bezugseinheit einfügen. (1 Kopeke für RUB)
        * F8 - 10 Bezugseinheiten einfügen. (10 Kopeken für RUB)
        * F9 - 100 Bezugseinheiten einfügen. (1 Rubel für RUB)
        * F10 - 500 Bezugseinheiten einfügen. (5 Rubel für RUB)
        * F11 - 1000 Bezugseinheiten einfügen. (10 Rubel für RUB)
        * F12 - 5000 Bezugseinheiten einfügen. (50 Rubel für RUB)
		
* Printer - Ausdrucken der Belege in Dateien (test_check_*.txt)
    * model - "test_printer"
    * Taste - "F11"
    * Ereignisse:
        * F1 - das Gerät ist ausgefallen.
        * F2 - das Gerät funktioniert.
	
* Dispenser.
    * model - "test_dispenser"
    * Taste - "F12"
    * Ereignisse:
        * F1 - das Gerät ist ausgefallen.
        * F2 - das Gerät funktioniert.
        * F3 - die Kassette ist entfernt.
        * F4 - den Geldbetrag, der den Nennwert der Geldscheine der 0. Kassette gleich ist, ausgeben.	

## Service-Tastenkombinationen
* F1 + F1 - die Trovemat-Software für den Zugriff auf das System-Menü sperren.
* F1 + F2 - Übergang ins Service-Menü der Trovemat-Software.
* F1 + F3 - Neustart der Trovemat-Software ohne Umladung des Betriebssystems.
* F1 + F4 - Start des Dienstprogramms  für Kalibrierung des Sensorbildschirms.
* F9 + ... - virtueller Geldschein-Validator (Geldscheinempfänger) - siehe [Emulation der Geräte](#Emulation-der-Geräte).
* F10 + ... - virtueller Barcode-Scanner - siehe [Emulation der Geräte](#Emulation-der-Geräte).
* F11 + ... - virtueller Belegdrucker - siehe [Emulation der Geräte](#Emulation-der-Geräte).
* F12 + ... - virtueller Geldscheinspender - siehe [Emulation der Geräte](#Emulation-der-Geräte).

## Besonderheiten der Funktion der Applikation im Demo-Modus
1. SMS wird über den Server [jetcrypto.com](https://jetcrypto.com/) von "Trovemat" gesendet.
1. Die Zahlungen werden über den Server [jetcrypto.com](https://jetcrypto.com/) ausgeführt.
1. Auf dem Bildschirm über der Applikation erscheint eine Überschrift, dass es eine Demo-Version ist.
1. Die Applikation läuft 10 Minuten, dann wird abgeschaltet (erweiterbar auf Anfrage an sales@trovemat.com).

## Besonderheiten der Funktion der Applikation
1. Die Telefonnummer ist im internationalen Format ohne "+"-Zeichen am Anfang und ohne Landeskennzahl eingegeben. Ein bestimmtes Land wird aus der Liste vor dem Feld für Eingabe der Telefonnummer gewählt. Beispielsweise für Russland ist "9261234567" einzugeben.
1. Die Wechselkurse auf der Hauptseite stammen von der Seite cryptocompare.com
1. Zutritt zu Börsen erfolgt durch das Token, das in der Datei [configs/operators.xml](#Liste-der-Operatoren-operatorsxml) für das entsprechende Feld request_field vorgegeben werden soll:
	1. BitLish: im Schritt mit id="0" und im Schritt mit id="7" beim Parameter "request_field" id="parameters.token" wird das Token für den Zugriff zu API angegeben.		
	1. EXMO: 
		- im Schritt mit id="0" beim Parameter "request_field" id="parameters.publicKey" wird API key angegeben, im Parameter "request_field" id="parameters.secretKey" wird API secret angegeben.
		- im Schritt mit id="7" beim Parameter "request_field" id="params" im Parameter wird API key im Attribut "publicKey" des JSON-Objektes angegeben, API secret wird im Attribut "secretKey" des JSON-Objektes angegeben.
	1. Poloniex: im Schritt mit id="7" beim Parameter "request_field" id="params" im Parameter wird POLONIEX API Key im Attribut "publicKey" des JSON-Objektes angegeben, POLONIEX API Secret wird im Attribut "secretKey" des JSON-Objektes angegeben.

## Kalibrierung des Sensorbildschirms (touch screen)

Kalibrierung des Sensorbildschirms kann erforderlich sein, wenn der Sensorbildschirm die Klicken-Koordinaten unrichtig erfasst. Das wirkt sich darin aus, dass Klicken auf den Sensorbildschirm schwierig sind (es ist schwierig, die Bedientasten der Schnittstelle der Trovemat-Software zu „treffen“), oder die Funktion der Schnittstelle der Trovemat-Software unkorrekt ist. 
Wenn nach der Kalibrierung des Sensorbildschirms das Problem nicht erlöst ist, die Rande des Sensorbildschirms können verschmutzt sein und erfordern spezialisierte Reinigung für Zahlungskiosks und Geldautomaten, oder der Sensorbildschirm kann defekt sein.

### Dienstprogramm für Kalibrierung

Das Dienstprogramm für Kalibrierung kann durch konsequentes Drücken der Tasten F1 und F4 auf der physischen Tastatur bei der laufenden Trovemat-Software auf der Hauptseite gestartet werden.

### Manuelles Starten der Kalibrierung des Sensorbildschirms aus der Befehlszeile

Den Sensorbildschirm kalibrieren: 
1. Den folgenden Befehl ausführen: 
    ```Shell
    sudo apt install xinput-calibrator && xinput_calibrator --list
    ```
    und id des Touchscreens anzeigen.
1. Den folgenden Befehl ausführen: 
    ```Shell 
    xinput_calibrator --device <DIE IM ERSTEN SCHRITT FESTGESTELLTE KENNZEICHNUNG DES GERÄTES>
    ```
1. Um die Kalibrierung zu speichern, müssen Sie die nach der Kalibrierung erhaltene Informationen (beginnend mit dem Abschnitt "InputClass" und endend mit EndSection, einschließlich diese Sätze) kopieren
1. Den folgenden Befehl ausführen: 
    ```Shell 
    sudo nano /usr/share/X11/xorg.conf.d/99-calibration.conf
    ```
    Drücken der Taste „Y“ kann eventuell für Bestätigung der Aktion benötigt sein, der entsprechende Hinweistext wird auf dem Bildschirm des Terminals erscheinen.
1. Die kopierte Information einfügen, den Eintrag speichern (CTRL + "O") und mit ENTER bestätigen, dann beenden (CTRL + X)

## Konfiguration der Liste der Administratoren zum Verwalten des Kiosks

Für Bedienung der Trovemat-Software können beliebige Messenger, die das Protokoll [Tox](https://tox.chat/) unterstützen, verwendet werden. Die Liste der Benutzer, die mit dem Kiosk kommunizieren können, wird wie folgt verwaltet:
1. In den Service-Modus übergehen (siehe [Service-Tastaturkürzel](#Service-Tastaturkürzel))
1. Die Taste "Liste der Administratoren" klicken
1. Den QR-Code mit der tox-id des Benutzers, der zur Liste der Kiosk-Administratoren hinzugefügt wird, öffnen.
1. Den offenen QR-Code im Kiosk scannen
1. Die Anfrage auf Hinzufügung zur Kontaktliste von der Trovemat-Software im Administrator-Messenger bestätigen.

Bedienung der Trovemat-Software mit Messenger:
1. Der folgende Befehl kann gesendet werden
	```
	help
	```
	der Kiosk wird eine Liste mit Befehlen, die er ausführen kann, zurücksenden
1. Für eine detaillierte Beschreibung eines Befehls soll eine Nachricht der folgenden Art gesendet werden
	```
	help 'BEFEHLNAME'
	```
	Zum Beispiel, der Befehl:
	```
	help 'status'
	```
	Als Ergebnis dieses Befehls wird die Trovemat-Software eine detaillierte Beschreibung des Befehls "status" senden

## Konfigurieren der Interaktion mit Börsen

### [Poloniex](https://poloniex.com)

API Key wird im Parameter "config.payments.common_params.poloniex_public_key" gespeichert

Secret wird im Parameter "crypto.poloniex_secret_key" gespeichert

### [Bitfinex](https://www.bitfinex.com/)

API key wird im Parameter "config.payments.common_params.bitfinex_public_key" gespeichert

API key secret wird im Parameter "crypto.bitfinex_secret_key" gespeichert

### [Bittrex](https://bittrex.com)

API key wird im Parameter "config.payments.common_params.bittrex_public_key" gespeichert

API key secret wird im Parameter "crypto.bittrex_secret_key" gespeichert

### [EXMO](https://exmo.com)

Public key wird im Parameter "config.payments.common_params.exmo_public_key" gespeichert

Secret key wird im Parameter "crypto.exmo_secret_key" gespeichert

## Starten der Befehlszeile

1. Die Trovemat-Software beenden (siehe [Service-Tastaturkürzel](#Service-Tastaturkürzel))
1. ALT + F4 drücken
1. Im angezeigten Dialogfeld auf "Beenden" ("Logout") klicken
1. Nach Beendigung der Funktion im Konsolen-Modus den "exit"-Befehl für Starten der Trovemat-Software eingeben bzw. das Betriebssystem neustarten

# Anweisungen zur Installation der Trovemat Kiosk-Software
Folgende Software-Optionen sind für den Kiosk möglich:
1. [Linux Mint Cinnamon x64](https://linuxmint.com/edition.php?id=237) - Version für Testieren und Demonstration, enthält einen Desktop und kann von Benutzern mit minimaler Erfahrung (oder auch ohne Erfahrung) der Arbeit mit Linux-Betriebssystem leicht bedient werden. Es wird empfohlen, diese Option zu verwenden, wenn die Installation der Trovemat-Software für Bekanntmachung erforderlich ist.
1. [Ubuntu 16.04 Server x64](http://releases.ubuntu.com/16.04/ubuntu-16.04.3-server-amd64.iso) - Version für den Einsatz im Industriebetrieb. Enthält alle für eine sichere Bedienung der Trovemat-Software notwendigen Einstellungen.

## Anweisungen zur Installation der Trovemat Kiosk-Software auf Linux Mint Cinnamon x64
1. Installation der Trovemat-Software erfolgt im Betriebssystem Linux Mint Cinnamon x64.
1. Trovemat installieren (den Befehl im Terminal eingeben)  
    
    > !!! ACHTUNG  
    > !!! DIESER BEFEHL INSTALLIERT EINE DEMO-VERSION DER KIOSK-SOFTWARE  
    > !!! UM EINE LIZENZIERTE VERSION OHNE EINSCHRÄNKUNGEN ERHALTEN  
    > !!! BESUCHEN SIE BITTE sales@trovemat.com  
  
    ``` SHELL 
    wget https://trovemat.com/trovemat_online_installer.sh -O /tmp/trovemat_online_installer.sh && sudo bash /tmp/trovemat_online_installer.sh $USER
    ```
1. Um den Touchscreen zu kalibrieren: im Terminal den folgenden Befehl eingeben:
    ``` SHELL 
    sudo apt install xinput-calibrator && xinput_calibrator --device {id}
    ```
    Touchscreen-Id (Eingabe ohne geschweifte Klammern) kann nach der Installation von Trovemat in der Liste der Geräte angezeigt werden.
1. Das System neustarten, > Trovemat wird automatisch gestartet, wenn das System geladen wird.
1. Trovemat-Software laut dem Dokument ["Anweisung zur Bedienung der Trovemat Kiosk-Software"](https://github.com/trovemat/docs/blob/master/Kiosk/en/User%20Guide%20%5Ben%5D.md) konfigurieren

## Anweisungen zur Installation der Trovemat Kiosk-Software auf Ubuntu 16.04 Server x64
1. Installation der Trovemat-Software erfolgt im Betriebssystem Ubuntu 16.04 Server x64.
    1. Stellen Sie sicher während der Installation, dass die Option "OpenSSH Server" gewählt ist.
    1. Installation des Betriebssystems erfolgt mit Standardeinstellungen.
1. Trovemat installieren (den Befehl im Terminal eingeben)  
    
    1. Installation der Demo-Version der Trovemat-Software
    > !!! ACHTUNG  
    > !!! DIESER BEFEHL INSTALLIERT EINE DEMO-VERSION DER KIOSK-SOFTWARE  
    > !!! UM EINE LIZENZIERTE VERSION OHNE EINSCHRÄNKUNGEN ERHALTEN  
    > !!! BESUCHEN SIE BITTE sales@trovemat.com  
  
    ``` SHELL 
    wget https://trovemat.com/trovemat_online_installer_ubuntu.sh
    chmod +x trovemat_online_installer_ubuntu.sh
    sudo ./trovemat_online_installer_ubuntu.sh $USER
    ```
    
    1. Installation der lizenzierten Version der Trovemat-Software
        
        Um die lizenzierte Version zu installieren, ist es notwendig, eine URL zu erhalten, die die lizenzierte Version zur Verfügung stellen wird. Nachdem diese URL erhalten ist, erfolgt die Installation der lizenzierten Version mittels folgender Befehle im Terminal:

			wget https://trovemat.com/trovemat_online_installer_ubuntu.sh
			chmod +x trovemat_online_installer_ubuntu.sh
			sudo ./trovemat_online_installer_ubuntu.sh $USER GEBEN_SIE_DIE_NACH_DER_ZAHLUNG_FÜR_DIE_TROVEMAT-SOFTWARE_ERHALTENE_URL_HIER_EIN

			Ein Beispiel des Befehls  für Installation der lizenzierten Version von der URL https://files.bytewerk.com/files/11111111111/trovemat-latest.tar.xz:
			wget https://trovemat.com/trovemat_online_installer_ubuntu.sh
			chmod +x trovemat_online_installer_ubuntu.sh
			sudo ./trovemat_online_installer_ubuntu.sh $USER https://files.bytewerk.com/files/11111111111/trovemat-latest.tar.xz
    
1. Nach der Installation von Trovemat wird der Computer neu gestartet. Nach dem Systemneuladen wird die Trovemat-Software automatisch gestartet.
1. Trovemat-Software laut dem Dokument ["Anweisung zur Bedienung der Trovemat Kiosk-Software"](https://github.com/trovemat/docs/blob/master/Kiosk/en/User%20Guide%20%5Ben%5D.md) konfigurieren

## Typenliste der Befehle, die im Messenger TOX für Trovemat-Konfigurieren gesendet werden

Der Terminal wird durch Senden von Befehlen von einem beliebigen Tox-Klient eingestellt.
Bevor einen Befehl zu senden, müssen Sie Ihre "tox id" in Trovemat hinzufügen:
1. Gehen Sie zu Service-Menü (F1 und F2 auf der Tastatur nacheinander tasten)
1. Klicken Sie auf die Schaltfläche "Administrator hinzufügen" - der erste hinzugefügte Administrator wird mit vollen Rechten erstellt, ohne Möglichkeit, ihn aus dem Service-Menü zu entfernen)
1. Im Tox-Klient die Freundschaftsanfrage akzeptieren und warten bis Trovemat in Tox online ist.

Nachfolgend finden Sie die wichtigsten Befehle für die Ersteinrichtung des Terminals (dieses Dokument enthält Testdaten für Einstellungen). Für ein genaueres Verständnis der Einstellungen verwenden Sie das Dokument ["Anweisung zur Bedienung der Trovemat Kiosk-Software"](https://github.com/trovemat/docs/blob/master/Kiosk/en/User%20Guide%20%5Ben%5D.md) Anführungszeichen, die in den Befehlen verwendet werden können - alle Arten der Anführungszeichen, die sowohl für Mobiltelefonen als auch für Desktop-Betriebssysteme üblich sind (einschließlich Apostrophen).

    !!! ACHTUNG  
    !!! Nachdem alle Einstellungen fertig sind, soll der Kiosk mit dem folgenden Befehl neugestartet werden: service restart

In diesen Befehlen werden die Daten in spitzen Klammern angegeben, statt deren bestimmte Werte eingefügt werden sollen (d.h. z.B. die Zeichenfolge in der Form "<GANZZAHL - TERMINALKENNZEICHNUNG>" wird im Befehlstext wie "2" angezeigt).

1. Den Namen des Punktes ändern:

		settings set "config.parameters.point_name" "<NAME EINES BESTIMMTEN TERMINALS>"
1. Die Terminalkennzeichnung ändern:

		settings set "config.parameters.point_id" "<GANZZAHL - TERMINALKENNZEICHNUNG>"
1. Benutzernamen und Passwort des Kontos Jetcrypto Wallet speichern:
    1. Benutzername:

			settings set "config.gateways.jetcrypto_wallet->username" "<BENUTZERNAME DER GELDBÖRSE JetCrypto Wallet>"
    1. Passwort:

			settings set -secure "crypto.jetcrypto_wallet_password" "<PASSWORT DER GELDBÖRSE JetCrypto Wallet>"
1. Die Schlüssel der gewählten Börse speichern (als Beispiel werden die Parameter für Poloniex Börse verwendet - für Einstellung anderer Börsen siehe ["Anweisung zur Bedienung der Trovemat Kiosk-Software"](https://github.com/trovemat/docs/blob/master/Kiosk/en/User%20Guide%20%5Ben%5D.md#settings-for-crytpocurrency-exchange)):
    1. Offener Schlüssel:
    
			settings set "config.payments.common_params.<NAME DES PARAMETERS, WO DER OFFENE SCHLÜSSEL DER GEWÄHLTEN BÖRSE GESPEICHERT IST>" "<OFFENER SCHLÜSSEL DER GEWÄHLTEN BÖRSE>"
			
			Ein Beispiel für Poloniex Börse:
			settings set "config.payments.common_params.poloniex_public_key" "00000000-00000000-00000000-00000000"
    1. Privater Schlüssel:
    
			settings set -secure "crypto.<NAME DES PARAMETERS, WO DER PRIVATE SCHLÜSSEL DER GEWÄHLTEN BÖRSE GESPEICHERT IST>" "<PRIVATER SCHLÜSSEL DER GEWÄHLTEN BÖRSE>"
			
			Ein Beispiel für Poloniex Börse:
			settings set -secure "crypto.poloniex_secret_key" "00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000"

1. Einstellung der Grenzwerte für den maximalen und minimalen Bargeldbetrag in einer einzigen Zahlung (standardmäßig beträgt der minimale Bargeldbetrag 1, der maximale Betrag ist 15.000) in aktueller Währung:
    1. Minimaler Grenzwert:
    
			settings set "config.payments.limit_min" "<MINIMALER BARGELDBETRAG>"
		
			Beispiel: Einstellung des minimalen Bargeldbetrags 200 in aktueller Währung: 
			settings set "config.payments.limit_min" "200"
    1. Maximaler Grenzwert:

			settings set "config.payments.limit_max" "<MAXIMALER BARGELDBETRAG>"
    1. Maximaler Grenzwert für das einbezahlte Bargeld auf der Hauptseite (Standardwert ist 15.000) in der aktuellen Währung. Wird gesondert für jede vom Geldscheinempfänger unterstützte Fiatwährung angegeben:

			settings set "config.interface.menu.limit_max-><CODE DER FIATWÄHRUNG, FÜR DIE DER GRENZWERT EINGEFÜHRT WIRD>" "<MAXIMALER BARGELDBETRAG>"
		
			Beispiel: Einstellung des maximalen Grenzwertes von 14.999 für die Währung "RUB": 
			settings set "config.interface.menu.limit_max->RUB" "14999"
1. Einstellung der Währung, in der Preise für die Kryptowährung auf dem Hauptbildschirm der Trovemat-Software angezeigt werden:

		settings set "config.payments.currency" "<CODE DER FIATWÄHRUNG, IN DER DIE KURSE DER KRYPTOWÄHRUNGEN ANGEZEIGT WERDEN>"
	
		Beispiel: Kurse der Kryptowährungen in US-Dollar (USD) auf dem Hauptbildschirm anzeigen:
		settings set "config.payments.currency" "USD"
1. Einstellung der Liste der zur Einführung verfügbaren Geldscheine (standardmäßig sind alle vom Geldscheinempfänger unterstützte Geldscheine verfügbar):

		settings set "config.peripherals.validator->enabled_currencies" "<LISTE ALLER ZUR EINFÜHRUNG ZULÄSSIGEN WÄHRUNGEN BZW. GELDSCHEINE>"
	
		Beispiel: Rubel-Banknoten mit Nennwert von 100, 500, 1000 und 5000 für Einführung zulassen:
		settings set "config.peripherals.validator->enabled_currencies" "RUB:100,RUB:500,RUB:1000,RUB:5000"
1. Einstellung der Kommission:
    1. Einstellung der Bedingung für Anwendung der Kommission bei Einzahlung eines Betrags ab 0 in aktueller Währung:
    		
			settings set "config.payments.fee.part->min" "<MINIMALER BETRAG DES AKZEPTIERTEN BARGELDES, AB DEM DIESE REGEL FÜR BERECHNUNG DES KOMMISSIONBETRAGS VERWENDET WIRD>"
			
			Beispiel: Hinzufügen einer Regel für Anwendung der Kommission ab dem einbezahlten Betrag "0":
			settings set "config.payments.fee.part->min" "0"
    1. Einstellung der Kommission in % vom einbezahlten Bargeld:
    		
			settings set "config.payments.fee.part->percent" "<GANZZAHL - PROZENTSATZ DER KOMMISSION>"
		
			Beispiel: Einstellung einer Kommission von 7% vom einbezahlten Geld:
			settings set "config.payments.fee.part->percent" "7"
    1. Einstellung eines festen Bestandteils der Kommission in der aktuellen Währung:
			
			settings set "config.payments.fee.part->fix" "<GANZZAHL - FESTER BESTANDTEIL DER KOMMISSION>"
		
			Beispiel: Einstellung eines festen Teils der Kommission von 50 in aktueller Währung:
			settings set "config.payments.fee.part->fix" "50"
1. Änderung der Terminalparameter (Name, Adresse usw.):
    1. Händlername:
    
			settings set "config.point_info.dealer_name->value" "<BEZEICHNUNG DES TERMINALEIGENTÜMERS>"
    1. Adresse des Händlers:
    
    		settings set "config.point_info.dealer_address->value" "<SITZ DES TERMINALEIGENTÜMERS>"
    1. Telefonnummer des Händlers:
    
    		settings set "config.point_info.dealer_phone->value" "<TELEFONNUMMER DES TERMINALEIGENTÜMERS>"
    1. Adresse des Punkts, wo sich das Terminal befindet:
    
    		settings set "config.point_info.point_address->value" "<TATSÄCHLICHE ADRESSE DES TERMINALS>"  
1. Änderung der standardmäßigen Landeskennzahl bei Eingabe der Telefonnummer:

		settings set "config.interface->default_phone_code" "<LANDESKENNZAHL LAUT ISO 3166-1>" 
		
		Beispiel: Einstellung der standardmäßigen Landeskennzahl +7 (Russland):
		settings set "config.interface->default_phone_code" "RU"
1.  Neustarten des Kiosks für Anwendung aller Einstellungen: 
		
		service restart

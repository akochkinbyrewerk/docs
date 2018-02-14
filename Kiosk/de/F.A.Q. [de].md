# Häufig gestellte Fragen

## Wie funktioniert dieses Gerät?
       
Arbeitsprozess: - Bareinzahlung - Feststellung der Adresse der Geldbörse des Käufers:
    
    - Scannen Sie die Adresse von einer bestehenden Geldbörse 
    - Erstellen Sie eine neue Papiergeldbörse (wenn möglich) auf dem Terminal
- Drucken von Schecks für den Kunden (einschließlich Papiergeldbörse, falls sie erstellt wurde)

Link zu einem Video, das Trovemat in Aktion zeigt: https://youtu.be/6f3cR620obs

Technische Informationen finden Sie auf unserer Website: https://trovemat.com

Anweisungen zur Installation der Demo-Version zum Testen: ["Anweisungen zur Installation der Trovemat Kiosk-Software"](https://github.com/trovemat/docs/blob/master/Kiosk/de/Install%20[de].md)

Arbeitsprozess:
- Bareinzahlung (Bargeld kann sowohl auf dem Hauptbildschirm als auch auf der Cash-Einzahlungsseite getätigt werden)
!["Hauptmenü"](https://github.com/trovemat/docs/blob/master/Kiosk/de/img/1.png)
!["Geld einzahlen"](https://github.com/trovemat/docs/blob/master/Kiosk/de/img/2.png)
- Ermitteln der Adresse der Brieftasche des Käufers

  - Scannen Sie den QR-Code der Adresse einer vorhandenen Brieftasche
!["Scannen Sie eine Brieftasche"](https://github.com/trovemat/docs/blob/master/Kiosk/de/img/3.png)
  - Erstellen Sie eine neue Papiergeldbörse (wenn möglich) auf dem Terminal
  - Geldbörsenadresse manuell eingeben
  
 - Eine Telefonnummer eingeben
 !["Eine Telefonnummer eingeben"](https://github.com/trovemat/docs/blob/master/Kiosk/de/img/4.png)
 - Geben Sie den Bestätigungscode ein
 !["Geben Sie den Bestätigungscode ein"](https://github.com/trovemat/docs/blob/master/Kiosk/de/img/5.png)
 - Nachricht über das Ergebnis der Beendigung des Vorgangs
 !["Ergebnis der Operation"](https://github.com/trovemat/docs/blob/master/Kiosk/de/img/6.png)
 
 ## Was soll ich tun, wenn die Transaktion zum Kauf einer Kryptowährung nicht erfolgreich war?
 
 - Wenn der Vorgang zum Kaufen einer Krypto-Währung nicht erfolgreich war, können Sie sich an den Support wenden. Dadurch wird eine Überprüfung des Ergebnisses der Operation gedruckt.
 !["Fehlgeschlagene Operation"](https://github.com/trovemat/docs/blob/master/Kiosk/de/img/7.png)
 - Der Scheck wird auf jeden Fall ausgedruckt.

## Wie verläuft der Vorgang des Kryptowährungskaufs? Bitte beschreiben Sie ausführlich oder geben Sie ein Vorgangsdiagramm.

Der Kaufvorgang verläuft wie folgt: 1. Der Kunde zahlt Bargeld beim Terminal 1 ein. Der Kunde scannt die Adresse der Geldbörse des Empfängers oder gibt sie manuell ein, oder erstellt eine neue Papiergeldbörse 1. Der Kunde gibt Telefonnummer ein, um SMS-Bestätigungscode 1 zu erhalten. Die Terminal-Software sendet eine Anfrage an Server api.jetcrypto.com für Sendung einer SMS-Mitteilung mit dem Bestätigungscode an die im vorherigen Schritt 1 angegebene Telefonnummer des Kunden. Der Kunde gibt den per SMS erhaltenen Bestätigungscode beim Terminal 1 ein. Die Terminal-Software führt die Anfrage an die gewählte Börse aus für Überweisung der gewählten Kryptowährung von der Geldbörse des Terminaleigentümers an die Geldbörse des Käufers 1. Die Börse führt Überweisung 1 aus. Die Terminal-Software druckt eine Quittung für den Kunden mit Festlegung des Betrags der an die angegebene Adresse der Geldbörse 1 überwiesenen Kryptowährung. Nach dem Erstellen einer Transaktion erhält der Kunde (wenn möglich) eine SMS-Mitteilung mit transactionId für Prüfung der Transaktion im blockchain

## Wie ist das Geschäft für den Verkauf der Kryptowährungen über den Kiosk überhaupt organisiert? Können Sie die Mittelbewegung in diesem Vorgang beschreiben?

!["Trovemat-Diagramm Flow of Funds (Mittelbewegung)"](https://github.com/trovemat/docs/blob/master/Kiosk/ru/img/Trovemat%20flow%20of%20funds.png)

1. Der Trovemat-Eigentümer füllt sein Konto auf einer beliebigen Börse auf, die von der Trovemat-Software (egal ob in Fiat- oder Kryptowährung) unterstützt wird.
1. Im persönlichen Konto der Börse sollen die Schlüssel für Trovemat generiert werden, um die Überweisungen der Kryptowährung zu ermöglichen
1. Der Trovemat-Eigentümer speichert die Börsenzugriffsschlüssel in der Trovemat-Software
1. Der Kunde zahlt Bargeld bei Trovemat ein
1. Nachdem der Kunde verifiziert und die Adresse für Überweisung der Kryptowährung festgestellt ist, sendet Trovemat einen Befehl an die Börse für Überweisung der Geldmittel von der Geldbörse des Trovemat-Eigentümers an die vom Kunden angegebene Adresse
1. Die Börse führt die Überweisung der Kryptowährung im angegebenen Betrag an die angegebene Adresse des Kunden aus
1. Der Trovemat-Eigentümer erfüllt Einkassierung des Kiosks je nach Befüllung des Geldscheinempfängers.

## Welche Börsen werden verwendet?
    
Folgende Börsen werden derzeit unterstützt: 
- https://exmo.com 
- https://poloniex.com 
- https://bittrex.com 
- https://bitfinex.com 
- https://jetcrypto.com

Im Prinzip ist es möglich, unser Zahlungs-Service zur Unterstützung der gewünschten Börsen nachzuarbeiten. Voraussetzungen für solche Nachbearbeitung sind separat zu besprechen.

## Welche Währungen können gekauft werden? Wo wird der Umrechnungskurs gerechnet?
    
Unterstützt werden alle Währungen, die von der bestimmten Börse unterstützt sind. Die Ankaufskurse werden von der Terminal-Software bestimmt. Die Kurse zur Darstellung für den Kunden können bei der bestimmten gewählten Börse angefragt werden, dabei ist es notwendig, das bestimmte von der entsprechenden Börse unterstützte Währungspaar anzugeben. Oder können Aggregatoren wie coinmarketcap.com verwendet werden.

## Kann der Kunde jede Geldbörse mit dem Gerät wieder auffüllen? 

Ja, der Kunde kann jede Geldbörse mit den am Terminal vorgestellten Währungen auffüllen.

## Welche Spezialisten (Ebene, Qualifikation) sind erforderlich, um die Arbeit des Systems zu unterstützen? Welche Ausbildung ist erforderlich?
    
Zur Unterstützung ist es nur notwendig, die Konfigurationsdatei der Software nach Bedarf zu erneuern. Überwachung und Steuerung der Geräte erfolgt durch Senden von Befehlen in jedem Messenger, das das Tox-Protokoll unterstützt. Keine speziellen Kenntnisse sind erforderlich. Die wichtigste Fähigkeit ist Verständnis der Struktur von XML-Dateien.

## Wie ist die Leistungsaufnahme des Kiosks?
    
Leistungsaufnahme: 200 W – 500 W, je nachdem, welche Geräte derzeit im Szenario beteiligt sind. Zum Beispiel: Terminal im Schlafbetrieb - 200 W, Monitor, Geldscheinempfänger und Drucker funktionieren gleichzeitig - 500 W.

## Welche Hardware wird von der Trovemat-Software unterstützt?
    
Geldscheinempfänger: 1. MEI Advance SC (Anschluss über COM-Port, Betrieb über das Protokoll MEI EBDS)

Drucker: 
1. CUSTOM VKP-80II (Anschluss über COM-Port und USB) 
1. CUSTOM TG-2480 (Anschluss über COM-Port und USB) 
1. Nippon NP-F3092D

Touchscreen: Jeder Touchscreen, der vom Betriebssystem unterstützt wird

## Zahlung
    
Wir arbeiten gegen Vorauszahlung. Nach Abstimmung und Ausstellung der Anforderung wird für Ihnen eine Zahlungsrechnung gestellt. Der Vorauszahlungsbetrag ist 100%. Die Zahlung für die Ausrüstung wird auf das Konto der Gesellschaft überwiesen.

## Lieferung
    
Wir liefern Anlagen weltweit, die Liefertermine für bestimmte Städte/Länder sind gesondert genau zu bestimmen. Die Lieferung kann vom beliebigen Transportunternehmen nach Ihrem Wahl durchgeführt werden. Die Lieferung erfolgt auf Kosten des Kunden. Die Speditionsdienstleistungen können wir in die Rechnung aufnehmen.

## Wie erfolgt das Garantie-Upgrade der Software?
    
Die Trovemat-Software wird während der Garantiezeit automatisch erneuert (falls sie mit dem Server api.jetcrypto.com verbunden ist)

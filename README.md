#RPI_eduroam_whs
#EDUROAM FÜR DAS RRASPBERRY PI - WHS EDITION
#Erarbeitet aus Wlan Einrichtung RPI eduroam der Uni Kiel & WHS-WLAN Wiki
c. Lukas Wöhle
Als ROOT ausführen oder sudo (empfohlen)



#Schritt 1:
Wlan-Dongle checken (USB): $ lsusb 
Falls da, direkt weiter machen, sonst Treiber installieren

#Schritt 2:
Zertifikat Herunterladen (https://wiki.w-hs.de/wlan)
Ablegen im Verzeichnis: /etc/wpa_supplicant/
[Befehl zum direkten kopieren, dafür im Download Verzeichnis sein!:
	 sudo cp deutsche-telekom-root-ca-2.crt /etc/wpa_supplicant/deutsche-telekom-root-ca-2.crt

#Schritt 3:
in Verzeichnis /etc/wpa_supplicant wechseln und wpa_supplicant.conf öffnen:
sudo nano wpa_supplicant.conf

#Schritt 4:
Bearbeiten der wpa_supplicant.conf -ERSTE ZEILE STEHEN LASSEN
erweitern um WLAN-Informationen der WHS-Seite und eigene Zugangsdaten:

network={
	ssid="eduroam"
	password= W-HS passwort	
	key_mgmt=WPA-EAP
	proto=WPA2
	eap=PEAP
	identity="WHSBenutzername@w-hs.de"
	anonymous_identity="anonymous@w-hs.de"
	ca_cert="/etc/wpa_supplicant/deutsche-telekom-root-ca-2.crt"
	phase2=auth=MSCHAPV2"
}


#Schritt 4:
Testen der Verbindung : 
sudo wpa_supplicant -c /etc/wpa_supplicant/wpa_supplicant.conf -D wext -i wlan0 -d
Bei erfolgreicher Verbindung kommt irgendwann:
EAPOL: startWhen-->0
Außerdem erscheint in der Menü leiste das WIFI-Symbol

#Schritt 5:
Anwendung im Hintergrund starten: 
sudo wpa_supplicant -c /etc/wpa_supplicant/wpa_supplicant.conf -D wext -i wlan0 -B

#Schritt 6:
Schützen der eigenen Informationen: Passwort & Username: 
sudo chmod 0600 /etc/wpa_supplicant/wpa_supplicant.conf	
RPI_eduroam_whs



#++++NOTES++++
Manchmal bricht das WLAN zusammen. Dann einfach wlan adapter ausschalten:
sudo ifdown wlan0
Und wieder in der wpa_supplicant.config nachschauen, ob sich prot und  key_mgnt
geändert haben! KOMMT VOR DASS DIE AUF NONE GESETZT WERDEN (noch keine Lösung).


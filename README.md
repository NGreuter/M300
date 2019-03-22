# M300 - Plattformübergreifende Dienste in ein Netzwerk integrieren
Noah Greuter

Version 2.0, 20.03.2019

#### Voraussetzungen für Servicenutzung

* PC mit min. 8 GB freiem RAM und ca. 20 GB freiem Harddisk.
* Ein schneller Netzwerk- (Kabel!) und Internet-Anschluss

### Inhaltsverzeichnis

* [Auftrag und Planung]()
* [Vorbereitung]()
* [VLC Stream Server]()
* [Teamspeak Server]()


### Allgemeine Hinweise

Die Inhalte sind auf das Modul 300 Plattformübergreifende Dienste in ein Netzwerk integrieren zugeschnitten.

Link: https://cf.ict-berufsbildung.ch/modules.php?name=Mbk&a=20101&cmodnr=300&noheader=1

## Auftrag und Plannung

Unser Auftrag ist es Dienste im Netzwerk zur Verfügung zu stellen und diese per 'Infrastructure as Code' zu realisieren. Mit einem Vagrantfile soll dies realisiert werden. Somit kann man diese Dienste mit einem Befehl zu Verfügung stellen und wieder abschalten.

Nishan Kotuwattegedera und Ich überlegten uns was wir für Service as Code realisieren wollen.

Unsere ersten Ideen:
* Apache Web Server mit Zusatzfunktion
* Ein Backup Service
* Monitoring Tool
* VLC Stream Server
* Teamspeak Server

Schlussendlich haben wir uns dazu entschieden das wir einen Teamspeak Server (erarbeitet durch Hr. Kotuwattegedera) und einen VLC Stream Server  (erarbeitet durch Hr. Greuter).

## Vorbereitung
Folgende Vorbereitung mussten getroffen werden um die Umgebung für die 'Service as Code' zu ermöglichen.
* Github
* Git-Bash
* Virtual Box
* Vagrant
* Visual Studio Code

### Github
Als erstes mussten wir einen Account bei Github erstellen und die eingegebene Mail verifizieren. Danach einloggen und ein Neues Projekt auf der Hauptseite erstellen. Diese Projekte nennen sich Repository und gilten als Ablage für ausgeführte Arbeiten. Dieses setzten wir auf Public um anderen Zugriff auf unser Projekt zu verschaffen. Mit der README Datei kann man das Projekt dokumentieren und versionieren.

Githublink zu meinem Projekt: https://github.com/NGreuter/M300.git

Als nächstes muss man einen private SSH Key generieren und dem Github Account zuweisen. Dazu braucht man eine Bash Terminal, welches auf einem Windows Computer noch zusätzlich installiert werden muss.

### Git-Bash / Git-Client
Folgender Befehl muss in dem Bash Terminal eingegeben werde : ssh-keygen -t rsa -b 4096 -C "Muster@Hans.ch".

Es muss die Mail Adresse des Github Accounts verwendet werden um diese zu verknüpfen. Man sollte dieser SSH Key mit einem Passwort hinterlegen damit man dies nur einmalig eingeben muss. Folgende Datei wird erstellt "%HOME%/.ssh/id_rsa.pub" diese sollte man in die Zwischenablage kopieren. Diese Datei muss man unter den GitHub Account Settings einfügen unter NEW SSH Key kopieren.

Zusätztlich muss ein Git-Client installiert werden um Lokal mit den Daten von dem GitHub Repository zu arbeiten. Damit ermöglicht man das arbeiten von mehreren Personen an dem Projekt.
Mit dem Befehl git pull kann man die Daten vom Repository herunterladen. Danach kann man in das Verzeichnis wechseln in dem das Projekt überarbeitet wurde und diese mit git add -A dem Upload hinzufügen. Diese Upload müssen mit git commit -m "Kommentar" bestätigt werden und mit dem Befehl git push werden die Daten ins Repository übernomen.

### VirtualBox
VirtualBox wird als Tool benutzt um die Virtuellen Maschinen zu erstellen nach den Vorgaben des Vagrantfiles.

Virtual Box: https://www.virtualbox.org/

### Vagrant

Vagrant wird unser Programm sein welches den Code in eine Virtuelle Maschine umwandelt. Vagrant benutzt keine ISO's sondern sogennante Boxes welche im Internet zur Verfügung stehen.

Sofern man ein Vagrantfile hat kann man in dessen Verzeichnis wechseln im Bash Terminal und eine VM Starten mit dem Befehl vagrant up. Löschen kann man diese Virtuelle Maschine mit dem Befehl vagrant destroy.

### Visual Studio Code

Als Texteditor wir Visual Studio Code verwendet, dies aus dem Grund da Visual Studio Code sehr kompatible für verschiedene Programmiersprachen sowie Syntax von Dateitypen. Auch kann man mit diesem Programm die Daten direkt ins Repository hochladen ohne ein Terminal zu benutzen.

## VLC Stream Server

Unsere Idee war es ein VLC Stream Server zu erstellen in welchem man bei dem Host die Videos in ein Ordner tun kann und diese dann auf dem Streaming service vorfinden kann.

So sieht das Vagrantfile aus.
![Bild 1](/images/vlc.JPG)

In dem ersten Abschnitt des Files wird die Erstelllung der VM gemacht und den Provider VirtualBox gewählt und der Ordner Media in dem Projekt Ordner wird dem Server übergeben. Somit kann der Datenaustausch der Medien geregelt werden. Ausserdem wird dem Host den Port 8080 auf den Port 8080 weitergegeben um darauf zuzugreifen.

In dem Zweiten Abschnitt wird die Installation von dem VLC Server gemacht sowie die Installation meherer Codex zur Wiedergabe verschiedener Dateformate. Mit dem Letzten Befehl wird der Stream gestartet auf den Port 8080.

Den Streaming Server kann man so testen indem man auf den Host oder innerhalb des Hostnetzwerkes auf die IP des Host zugreift mit dem Port 8080 oder direkt mit dem VLC Player.


![Bild 2](/images/vlcopen.JPG)

Danach sieht man das gestreamte Video, in diesem Fall das Media.mp4.


![Bild 3](/images/vlcstream.JPG)

## Teamspeak Server
Die Teamspeak Vagrantfile und Shell Scripts sind in dem Ordner Teamspeak.
Bevor man mit der Erstellung der Maschine beginnt, erstellt man mit GitBash eine lokale Ablage (hier "teamspeakvm"). Danach GitBash starten, mit dem Kommand "cd teamspeakvm" in die Ablage wechseln.

Nun wird vagrant mit dem Kommand "vagrant init" gestartet und eine Erstumgebung initialisiert.

Nachdem man die Umgebung erstellt hat, muss das vagrant File angepasst werden, sodass es unseren Bedürfnissen entspricht. In diesem Fall einem Teamspeak- Server.

![Bild2](/images/vfile.PNG)

* Die VM wird provisioniert auf die TSS Instance.
* Eine Box CentOS7 wird erstellt
* Der Hostname wird gesetzt auf ts-server
* Der Provider wird definiert.
* Die Spezifikationen der VM werden vorgegeben.
* Die IP wird gesetzt. Hier durch DHCP.
* Schluss endlich werden noch die nötigen Zusatzsoftwares installiert über Shell Commands.

(Installation des TSS Services, der Firewall und des eigentlichen unterstützten Servers.)

(Notiz:Der VM Name in Virtualbox darf nicht gesetzt werden, da dies zu einem unerwarteten Error führt.)

#### Shells und Service
Auf dem Server müssen verschiedene Dienste zusätzlich installiert werden, damit nebst der Sicherheit, auch der eigentlich Dienst funktioniert.

Folgende drei Dienste müssen definiert und installiert werden:

* Firewall
* Serverinfrastruktur
* Server- Systemdienste

Diese werden über Shell Commands in das Vagrantfile eingespielt, sodass diese beim Start der VM automatisch verwirklichen.

![Bild3](/images/addFirewall.PNG)

![Bild4](/images/installApp.PNG)

![Bild5](/images/addSystem.PNG)

## Start des Servers/ IP

Der Server muss im NAT Modus gestartet werden. Sobald der Server komplett durchgestartet ist, kann man sich mit dem Kommand "vagrant ssh" auf das Server Interface verbinden.

Um die IP des Servers zu ermitteln, gibt man den Kommand "ip address" ein. Die Server IP ist unter dem eth1 herauszufiltern.

## Client und Connect

Um sich auf den Server zu verbinden, muss ein ein Teamspeak Client heruntergeladen werden.

Unter der Option "Connections/ Verbinden" kann man die nun bekannte Server IP eingeben, sich einen Nicknamen setzen und Verbinden. Der Server wurde so definiert, dass kein Server- Passwort gesetzt erstellt wurde.

![Bild6](/images/tsclient.JPG)

--------------------

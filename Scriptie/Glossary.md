# 7. Glossary
**6LowPAN**: (IPv6 over Low Power Personal Area Network) Elke node krijgt een IPv6 adres, er wordt via open standaarden gecommuniceerd over het netwerk. 

**ANT+**: Gebruikt ook de 2.4GHz band. ANT+ heeft een reikwijdte van om en bij de 50m, een overdrachtsnelheid van ongeveer 1Mbit/s, en is bedoelt om weinig data op te sturen (enkele bytes, zoals text messages). Het nadeel ten opzichte van BLE is dat ANT+ minder support heeft onder moderne devices en dus vaak een dongle nodig is.

**API**: Application Programming Interface. Een API is een groep functies en objecten die worden gebruikt om een applicatie op te bouwen die communiceert met een ander systeem. Ze komen vaak in de vorm van *libraries*. Een voorbeeld hiervan is de Google Maps API, die ook in dit project gebruikt werd.

**Baud rate**: de snelheid waarop data binnenkomt in een systeem. Het wjst op ofwel het aantal signaalwisselingen of symbolen per seconde op een kanaal. Het is niet gelijk aan het aantal bits per seconde, al wordt dit vaak gedacht.

**Bluetooth Low Energy**: Gebruikt evenals de band van 2.4GHz, en bezit een gelijkaardige reikwijdte/overdrachtsnelheid van 50m/1Mbit/s. BLE transfereert maximum pakketjes van 20 bytes. Het voordeel is dat er veel support voor is. De grote bedrijven staan er allemaal achter. BLE heeft ook een laag energieverbruik.

**DASH7 Alliance Protocol**: Er kan gecommuniceerd worden van meters tot kilometers.
* B.L.A.S.T. Principe:
    * *Blasty*: Data-overdracht is abrupt en bevat geen inhoud zoals video of audio.<br>
    * *Light*: Voor de meeste applicaties zijn de pakketgroottes beperkt tot 256 bytes.<br/>
    * *Asynchronous*: De command-reactie vereist door het ontwerp geen "handshake" of synchronisatie tussen apparaten.<br/>
    * *Stealthy*: DASH7 maakt niet gebruik van discovery beacons.<br/>
    * *Transitional*: DASH7 is upload-centric, niet download-centric, dus de apparaten moeten niet uitgebreid beheerd worden door vaste infrastructuren.<br>

**Endianness**: De volgorde van bytes. Bijvoorbeeld: in een *Big Endian* hexadecimale string zal de *big end* van de string eerst zetten en dus de meest significante byte eerst geplaatst (ABCD). Big Endian is het vaakst voorkomend. Bij Little Endian zal de minst significante byte eerst geplaatst worden (DCBA).

**LoRa**: (Long Range Radio) is een netwerk dat bestaat uit zendmasten waarnaar zeer korte pakketjes verstuurd kunnen worden. Het is bedoeld voor toestellen die bv enkel statusberichtjes hoeft te verzenden en niet een constante verbinding nodig hebben. De snelheid is beperkt tot maximum 50kb/s en de reikwijdte van 2.5 tot 15km. 

**MQTT**: Messaging protocol voor bij TCP/IP. Werkt op basis van publish-subscribe (afzender "publisht"; dus stuurt het bericht de wereld in en "subscribers" krijgen berichten aan die ze willen zien).

* Methoden:
    * *Connect*: Wachten op connectie met de server. <br/>
    * *Disconnect*: Wacht totdat MQTT klaar is met taak, en tot de TCP/IP sessie gedaan is. <br/>
    * *Subscribe*: Wacht totdat de Subscribe of Unsubscribe methode klaar is. <br/>
    * *Unsubscribe*: Vraagt aan de server om te unsubscriben van een of meer onderwerpen. <br/>
    * *Publish*: Stuur een bericht naar de server en keert terug als het afgeleverd is. <br/>
* Broker: de broker is verantwoordelijk voor het verdelen van de berichten naargelang het topic.

**SIGFOX**: SIGFOX is een bedrijf dat zich specialiseert in draadloze netwerken, niet anders dan LoRa. Hun bedoeling is low-energy apparaten te laten communiceren. De apparaten sturen dan constant korte berichtjes. De reikwijdte van een SIGFOX base station is ongeveer 40-50km en de pakketjes zijn ongeveer 12 bytes groot. Al concurreren SIGFOX en LoRa op het Internet of Things gebied, bestaan er dual modules die beide netwerken ondersteunen. 

**UART**: Universal Asynchronous Receiver/Transmitter. Een UART is een onderdeel van een systeem dat data omzet van parallel naar serieel of omgekeerd. Het wordt vaak in microcontrollers gebruikt om data (asynchroon) naar een seriële poort op een computer te versturen.

**VOC**: Volatile Organic Compound (Nederlands: VOS) is een verzamelnaam voor koolwaterstoffen die gemakkelijk verdampen. Ze worden gezien als milieuonvriendelijk, en kunnen schadelijke effecten hebben op mensen. Een VOC-Sensor is dus een sensor die deze stoffen opspoort in de lucht.

**Z-WAVE**: Wordt gebruikt in domotica. Kan tot tussen de 50 en 150m gebruikt worden over two-way communicatie. Betrouwbaar: als het pakketje niet aankomt probeert de sender het opnieuw. Lukt het weer niet wordt een andere route gezocht. Kostprijs ligt hoog.
# Voortgangsverslag **Kevin Van de Mieroop**
# (Voorlopige) Titel onderwerp:
## Promotors
* Stagecoordinator: Tim Dams
* Stagebegeleider: Maarten Luyts
* Stagementor: Glenn Ergeerts
* Stagepromotors: Thomas van Loon, Didier Verachtert, Jens Vanhooydonck

## Abstract
De opdracht die ik tijdens mijn loopbaan bij de Universiteit Antwerpen moet afkrijgen is een full-stack applicatie (Node.js, ExpressJS, AngularJS en MongoDB) die de metingen van een development board en bijhorende sensoren moet verwerken en visualiseren in de vorm van grafieken.

## Technische omschrijving
Bij het onthaal heb ik een presentatie gekregen over het development board waar de opdracht rond draait: de Octa. Voor dit bordje worden verschillende shields ontworpen die elk een communicatieprotocol behandelen. 
Ik heb een kleine cheat sheet gemaakt waarin de verschillende protocols worden uitgelegd:
    ANT+: Gelijkaardig aan Bluetooth Low Energy; gebruikt ook de 2.4GHz band. ANT+ heeft een reikwijdte van om en bij de 50m, een overdrachtsnelheid van ongeveer 1Mbit/s, en is bedoelt om weinig data op te sturen (enkele bytes, zoals text messages). Het nadeel ten opzichte van BLE is dat ANT+ minder support heeft onder moderne devices en dus vaak een dongle nodig is.
	Bluetooth Low Energy: Gebruikt evenals de band van 2.4GHz, en bezit een gelijkaardige reikwijdte/overdrachtsnelheid van 50m/1Mbit/s. BLE transfereert maximum pakketjes van 20 bytes. Het voordeel is dat er veel support voor is. De grote bedrijven staan er allemaal achter. BLE heeft ook een laag energieverbruik.
	6LowPAN: (IPv6 over Low Power Personal Area Network) Elke node krijgt een IPv6 adres, er wordt via open standaarden gecommuniceerd over het netwerk.
	Z-WAVE: Wordt gebruikt in domotica. Kan tot tussen de 50 en 150m gebruikt worden over two-way communicatie. Betrouwbaar: als het pakketje niet aankomt probeert de sender het opnieuw. Lukt het weer niet wordt een andere route gezocht. Kostprijs ligt hoog.
	DASH7: 	
    Er kan gecommuniceerd worden van meters tot kilometers.
    B.L.A.S.T. Principe** <br>
    Blasty*: Data-overdracht is abrupt en bevat geen inhoud zoals video of audio.<br/>
	Light*: Voor de meeste applicaties zijn de pakketgroottes beperkt tot 256 bytes.<br/>
	Asynchronous*: De command-reactie vereist door het ontwerp geen "handshake" of synchronisatie tussen apparaten.<br/>
	Stealthy*: DASH7 maakt niet gebruik van discovery beacons.<br/>
	Transitional*: DASH7 is upload-centric, niet download-centric, dus de apparaten moeten niet uitgebreid beheerd worden door vaste infrastructuren.
	MQTT: 
    Messaging protocol voor bij TCP/IP. Werkt op basis van publish-subscribe (afzender "publisht"; dus stuurt het bericht de wereld in en "subscribers" krijgen berichten aan die ze willen zien).
    Methoden** <br>
    	*Connect*: Wachten op connectie met de server. <br/>
    	*Disconnect*: Wacht totdat MQTT klaar is met taak, en tot de TCP/IP sessie gedaan is. <br/>
    	*Subscribe*: Wacht totdat de Subscribe of Unsubscribe methode klaar is. <br/>
    	*UnSubscribe*: Vraagt aan de server om te unsubscriben van een of meer onderwerpen. <br/>
    	*Publish*: Stuur een bericht naar de server en keert terug als het afgeleverd is. <br/>
     Broker: de broker is verantwoordelijk voor het verdelen van de berichten naargelang het topic.

Mijn project is een full stack applicatie ontwerpen die de data binnenhaalt via een gateway board, de gegevens in een database steken en ze kan visualiseren. Praktisch: een client die de gateway koopt en aansluit om de gegevens door te sturen naar de backend waar ze worden verwerkt.
Bij de eerste meeting werd besproken welke talen/frameworks gebruikt zouden worden in de applicatie. Hier is een schema van de architectuur: ![Schema architectuur](https://i.imgur.com/Fogabnr.jpg)
Aanvankelijk was het idee om de gateway in Python te schrijven. De enige wijziging die is gebeurd na dit schema is dat de gateway eveneens wordt behandeld door een Node-client aangezien er langs mijn kant meer ervaring was en ik dus geen tijd moest besteden om python onder de knie te krijgen. 
Ik had nog nooit met MQTT gewerkt dus hier moest ik enige research over doen. MQTT is een messaging protocol voor bij TCP/IP. Het werkt op basis van publish-subscribe: de afzender "publisht" een bericht en stuurt het dus de wereld in. De subscribers krijgen ze aan. De publisher zet de berichten in een "topic" en de subscriber krijgt enkel berichten van de topic waarop hij subscribet. 
     * MQTT methoden: <br>
    	*Connect*: Wachten op connectie met de server. <br/>
    	*Disconnect*: Wacht totdat MQTT klaar is met taak, en tot de TCP/IP sessie gedaan is. <br/>
    	*Subscribe*: Wacht totdat de Subscribe of Unsubscribe methode klaar is. <br/>
    	*UnSubscribe*: Vraagt aan de server om te unsubscriben van een of meer onderwerpen. <br/>
    	*Publish*: Stuur een bericht naar de server en keert terug als het afgeleverd is. <br/>
     * MQTT broker: de MQTT broker is verantwoordelijk voor het verkrijgen en verdelen van de berichten. De berichten worden op een topic geplaatst. De voornamelijkste MQTT brokers zijn HiveMQ (service) en Mosquitto (open source software om zelf MQTT broker te runnen).
Als eerste heb ik een korte MQTT demo geschreven om bekend te worden met het protocol. De broker heb ik zelf lokaal gehost met Mosquitto (die de MQTT versies 3.1 en 3.1.1 implementeert). De MQTT demo heb ik gemaakt naargelang een tutorial die ik online heb gevonden: http://thejackalofjavascript.com/getting-started-mqtt/ . Het maakt gebruik van 1 Node.js client. De publisher en subscriber in 1 app. De publisher subscribet op een topic "presence"; stuurt een berichtje erin naar de MQTT broker op localhost:1883. De subsriber subscribet op het topic "presence" en schrijft het ontvangen berichtje automatisch weg naar de console, om zo een succesvolle overdracht aan te tonen. Ik heb het een beetje verder uitgewerkt door simpelweg de subscriber en publisher in aparte Node.js bestanden weg te schrijven.  ![MQTT Publisher/Subscriber Node.js](http://i.imgur.com/U6anSqj.png)
Dit is uiteindelijk de allerlaagste basis van de uiteindelijke applicatie en geeft de werkelijke communicatie (data-overdracht) tussen client en server weer.
Ik heb op mezelf nog verder geexperimenteerd met de Qualities of Service (QoS) van MQTT:
     * QoS 0: "At most once"; bericht wordt eenmaal verstuurd vanaf publisher, subscriber kan het ontvangen
     * QoS 1: "At least once"; bericht wordt bijgehouden door de afzender totdat een acknowledgment is teruggekregen (PUBACK)
     * QoS 2: "Exactly once"; Publish --> PUBREC(eived) --> PUBREL(eased) --> PUBCOMP(leted)
Er is voorgesteld om websockets te implementeren in de applicatie om zo een constante verbinding te hebben tussen client en server. Hiervoor zijn de plannen nog niet van tafel en kan een implementatie nog van volgen. Er bestaat een Node.js module die dit behandelt dus het is zeker een mogelijkheid.
Er is onderzoek gedaan naar wat de beste backend zou vormen. Server-side zou een andere Node.js applicatie de data verwerken. Database-gewijs kon het 2 kanten op: een SQL database of een non-relationele database. Al heb ik meer ervaring met een SQL database, het leek me beter om een MongoDB op te starten en met JSON te werken omdat het flexibeler is en lichter. De collega's gingen hier mee akkoord. Dit wou wel zeggen dat ik wat tijd zou moeten steken in het perfectioneren van de database en de beste structuur. 
De data zou worden geparsed bij ofwel het opslaan ofwel het ophalen ervan. Na dit nader besproken te hebben was de beste oplossing om een dataparser te implementeren bij het ophalen zodat hij er makkelijker uitgeschreven kon worden moest hij uitendelijk bijvoorbeeld gewijzigd worden.
Andere tutorial die ik gevolgd heb om inspiratie te krijgen: https://utbrudd.bouvet.no/2015/01/11/voice-controlling-a-robot-using-arduino-node-js-mqtt-websockets-johnny-five-and-html5-speech-recognition/ . Deze demo gebruikt de Johnny Five module (een JavaScript robotics programming platform); deze is voor mij ongebruikelijk aangezien hij op bepaalde development boards fixeert zoals Arduino.
De laatstgenoemde tutorial bracht me wel op het idee om een demo opstelling te maken met een Arduino die dummy data kon genereren. 
De Arduino opstelling die ik toen heb gemaakt wordt nog steeds gebruikt en bestaat uit een lichtsensor en 3 ledjes om de hoeveelheid licht weer te geven (weinig licht <600 is rood, meer licht 600-800 is geel en boven de 800 is groen). Hij geeft deze data om de seconde door via seriele communicatie.
Demo applicatie geeft verkregen seriele data weer in console: [Console vensters MQTT](http://i.imgur.com/hyCMOM1.png)
Het volgende dat moest gebeuren was de 2 demo applicaties combineren en dus het MQTT protocol aan de applicatie toevoegen. De datastream wordt automatisch gepost op het topic "lightmeasuring" wanneer het binnenkomt door de poort. De andere Node.js client die zich subscribet op het topic "lightmeasuring" krijgt de data een voor een binnen zolang er gepublishd wordt/gesubscribed wordt. 

    sp.on('open', function(){
      console.log('Serial Port Opened');
      sp.on('data', function(data){
          client.publish('lightmeasuring', data);
          console.log(data);

De sp variabele wijst op de serialportmodule die hier geopend wordt met sp.on. De binnenkomende datastream wijst op parameter "data" en binnen deze functie wordt ook de MQTT opgeroepen met de client.publish functie. Hierbij wordt ook de data meegegeven als parameter.

De aangekregen data wordt succesvol weergegeven in een apart consolevenster. Deze subscriber moest dan uiteindelijk de verkregen data automatisch wegschrijven in een database. Aangezien we met MongoDB gingen werken was het eerste dat moest gebeuren een installatie hiervan.
Een simpele implementatie volgde hierop en de datastream werd zogezegd doorgegeven door de subscriber aan de database. Er werden geen foutmeldingen weergegeven maar in het consolevenster werd gezegd dat de collectie leeg was. 
Hier ben ik dus wat moeilijkheden tegen het lijf gelopen en ben ik verplicht geweest om een simpele demo applicatie te maken die gebruik maakt van MongoDB om ook hier familiair mee te worden. Ik heb een tutorial gevolgd (https://scotch.io/tutorials/build-a-restful-api-using-node-and-express-4) om een RESTful API te creeeren. Hierin wordt beschreven hoe een modeldb te maken en deze door te geven met Mongoose. Hierna kan met postman de data worden opgehaald (GET), toegevoegd (POST), gewijzigd (PUT), of verwijderd (DELETE). Dit heeft mezelf veel bijgeleerd ivm RESTful APIs en het gebruik van MongoDB. Veel van de code is nu niet meer relevant aangezien ik geen gebruik meer maak van een model of Mongoose. De DELETE en PUT functies heb ik voorlopig als comments gezet omdat ze evenmin relevant zijn aangezien de data enkel opgehaald moet worden. De api die ik heb opgesteld wordt nog wel steeds gebruikt. 

    router.route('/data')     
        .get(function(req, res) {
        Light.find(function(err, data) {
            if (err)
                res.send(err);
            res.json(data);
        });
    });

In de code snippet hierboven wordt het belangrijkste van de GET functie weergegeven. Light is de variabele voor het model db.js, en wat doorgegeven wordt is de data in json formaat.
Ik ben uiteindelijk verder gegaan met enkel de mongo module in npm in plaats van mongoose. Hiervoor heb ik commands opgezocht om de officiele tutorial van MongoDB (https://docs.mongodb.org/manual/reference/method/db.collection.update/).
De collectie updaten kan door een blok waarvan hier een code snippet:

      collection.update(  
    { _id:"ArduinoLightSensor" },
    { $push: { events: { value:datastream, when:new Date()} } } ,
    { upsert:true },
    function(err,docs) {
      if(err) { console.log("Insert fail"); }
    }

De collectie waarnaar wordt gepushed heet "lights" en wordt aangemaakt in een command line venster. Wanneer mongod actief is moet eerst gewisseld worden naar de juiste database:

    use lightdb

Hierna kan gecheckt worden welke collecties aanwezig zijn in de actieve database:

    show collections

Een nieuwe collectie kan aangemaakt worden

    db.createCollection(lights, options)

met eventuele opties, bv. max aantal objecten.

Wanneer dit allemaal klaar stond maakte ik nog even gebruik van postman en de voorafgaande GET functie om de collectie en de link met de database te testen. Met postman kon ik een willekeurig object in de database steken en opnieuw oproepen dus de collectie was actief.
Hierna gebeurde de migratie met de MQTT app. De subscriber app subscribed en schrijft de data weg in dezelfde functie om het compact te houden en zodat de variabelen meteen konden doorgegeven worden. De MQTT subscriber krijgt data (message) aan en schrijft ze meteen weg als "value". Hierachter wordt door de update functie ook een timestamp geplakt onder de waarde "when" met new Date(). Hieronder een code snippet:

      function setupCollection(err,db) {  
    if(err) throw err;
    collection=db.collection("lights");
    client.subscribe('lightmeasuring')
    client.on('message', function(topic, message) {
      var datastream = message.toString();
      console.log(datastream);
      collection.update(  
    { _id:"ArduinoLightSensor" },
    { $push: { events: { value:datastream, when:new Date()} } } ,
    { upsert:true },
    function(err,docs) {
      if(err) { console.log("Insert fail"); }
    }
    )
    });
    }
![Inhoud database via Postman](http://i.imgur.com/oqwMJDY.png)

Het volgende punt was de frontend maken. Dit moet in AngularJS gebeuren samen met Bootstrap om het wat esthetischer te maken. 
Hiervoor heb ik mijn AngularJS opnieuw moeten opfrissen. Na nader inzicht had ik meteen moeten overschakelen op Angular 2 aangezien hier meer aantrekkelijke functies in zetten (bv geen gebruik meer van $scope en variabelen worden automatisch getransfereerd naar andere controllers) maar de applicatie werkt naar behoren in AngularJS 1. Om Angular opnieuw op te frissen heb ik snel een cursus gevolgd op codecademy: https://www.codecademy.com/learn/learn-angularjs. Uiteindelijk heb ik een eigen controller en view opgesteld voor de applicatie. De controller geeft een error in de chrome console als hij extern gelinked wordt. Voorlopig is hij nog niet al te uitgebreid dus kan ik heb intern in de view steken en werkt het perfect.
De data wordt opgehaald met een http request en automatisch in de $scope gestoken. In een ng-repeat tabel wil ik de verkregen data weergeven maar tevergeefs. Het lijkt erop dat de structuur van de database niet optimaal is voor gebruik in AngularJS. Hiervoor heb ik de collectie even leeggemaakt:

    db.lights.drop()

Dit verwijdert de collectie maar hij wordt automatisch opnieuw aangemaakt.
Door middel van trial and error (het json object wordt weergegeven in de chrome console dus het juiste pad naar de gegevens werd uiteindelijk wel gevonden) wordt uiteindelijk de volledige inhoud van de database weergegeven in een tabel in de view. 

![Screenshot van view draft 1](http://i.imgur.com/qXDqG8S.png)
![Structuur van database draft 1 (via Postman)](http://i.imgur.com/BBC8DD3.png)

Het volgende plan was de view automatisch laten updaten en eventueel een chart of meerdere in de view verwerken.

De week erop heb ik de volledige applicatie nog eens overlopen, de code opgeschoond en enkele comments bij de code gezet om hem verstaanbaarder te maken. Ik heb het probleem ook bekeken waardoor de controller en andere javascript files niet extern gelinked konden worden. 

    Unexpected token "<" op lijn 1 van de index.html

Na Stack Overflow te raadplegen bleek dit een serverside probleem. De correcte files werden niet door express doorgegeven dus enkele lijnen moesten toegevoegd worden aan de server app: 

    app.use(express.static('public'));
    app.use('/bower_components', express.static(__dirname + '/bower_components'));

Hierdoor wordt de public folder doorgegeven (waarin de view en de javascript folders zitten verwerkt). Ook de bower components wordt doorgegeven maar dit is voor een chart die later wordt uitgelegd. 

Ik heb research gevoerd naar verschillende Angular functies die ik zou kunnen gebruiken om de applicatie responsief te maken

    * $scope.$apply: een functie die opgeroepen kan worden om bindings te updaten
    * $scope.$watch: een functie die steeds kijkt naar bindings (scope) om te zien of de gegevens veranderd zijn; zoja wordt er een functie uitgevoerd.
    * $scope.$timeout: een functie wordt uitgevoerd na een bepaalde tijdsduur
    * $scope.$interval: een functie wordt uitgevoerd met intervallen ertussen

Ook heb ik bekeken hoe de data best wordt overgedragen van controller naar controller: 

    * $scope.$broadcast: zendt de scope van de ene controller naar alle andere controllers, die er op hun beurt op kunnen "subscriben" indien ze de data nodig hebben.
    * Services: overkoepelende "controllers" waarin de data wordt opgeroepen en alle childs de data kunnen overnemen.

Ik ben gegaan voor de broadcast functie omdat zo de data opnieuw kan gebruikt worden als er een nieuwe controller wordt aangemaakt die niet afstamt van een overkoepelende controller, of een die afstamt van een andere irrelevante controller.

De broadcast wordt opgehaald in de andere controller door middel van $on. Ik test de functie door in de andere controller de opgehaalde data meteen de loggen in de console. Het object wordt 2x weergegeven in de console (1x in de oorspronkelijke controller, 2de keer in de subscriber controller):

Hier de code van de broadcaster:

    $scope.lightdata = json;
    $rootScope.$broadcast('lightcast', $scope.lightdata);

De http request krijgt de data als json aan en steekt ze in de scope onder de variabele lightdata. Daaronder wordt de broadcast functie opgeroepen en broadcast het de scope variabele lightdata onder het topic lightcast.

$scope.$on('lightcast', function lightCast(events, args){
    $scope.lightdata = args;
      //for(var i = 0; i < 25; i++)
          //{
          //    $scope.datapoint = parseInt($scope.lightdata.data[0].events[i].value);
          //    $scope.dataarray.push($scope.datapoint);
          //    $scope.datepoint = $scope.lightdata.data[0].events[i].when;
          //    $scope.datearray.push($scope.datepoint);
          //    if ($scope.dataarray.length > 25)
          //        {
          //            $scope.dataarray.shift($scope.dataarray[0]);
          //            $scope.datearray.shift($scope.dataarray[0]);
          //       }
          // }
      console.log($scope.dataarray);

De functie $on subscribed op de broadcast "lightcast" die wordt aangemaakt in de originele controller. De data wordt verkregen onder het argument "args". Deze wordt meteen in de $scope van de nieuwe controller gestoken. De code die hierboven in comments staat is nog niet relevant en hoort bij de chart code.

Nadat dit werkte heb ik onderzoek gedaan naar de opties om een plot op te stellen in de view aangezien de lijst met data niet overzichtelijk genoeg is. Er waren verschillende mogelijkheden: eventueel met een bootstrap template werken en een dashboard gebruiken die de gegevens visualiseert ofwel een javascript library gebruiken voor angularjs en de data zelf weergeven. Ik heb voor het laatste geopteerd omdat een meer customization opties voor zijn en ik later meer kanten op kan met de frontend.
De library die ik heb gekozen is de angular-chart.js library (http://jtblin.github.io/angular-chart.js/). De charts die hierbij gemaakt kunnen worden zijn gemakkelijk te customizen maar vooral ook responsief, wat gebruikelijk is omdat de gegevens om de seconde (zou moeten veranderen naar om de minuut) veranderen.
Eerst en vooral werd de library geinstalleerd met bower. De grafiek werd correct weergegeven maar voorlopig met hardcoded data in de controller.
De data die gevisualiseerd wordt moet in een array zitten. 2 arrays om nauwkeuriger te zijn: 1 array met data (int) en 1 array voor de labels (strings). De eerste array bevat dus de waarden (value) en de 2de array bevat de timestamps (when). 
Ik heb een for loop geschreven die de afzonderlijke arrays creeert (max 25 entries). Dit zal altijd kloppen aangezien er altijd evenveel timestamps als data zal zijn. Ook heb ik de parseInt() gebruikt om de waarden om te zetten van strings naar int. 

    $scope.datapoint = parseInt($scope.lightdata.data[0].events[i].value);
              $scope.dataarray.push($scope.datapoint);
              $scope.datepoint = $scope.lightdata.data[0].events[i].when;
              $scope.datearray.push($scope.datepoint);
              if ($scope.dataarray.length > 25)
                  {
                      $scope.dataarray.shift($scope.datearray[0]);
                      $scope.datearray.shift($scope.dataarray[0]);
                  }

Hierboven de forloop die de arrays aanmaakt.
Hetgeen ik hierna wou realiseren was het automatisch updaten van de view.
Dit heb ik gedaan door een $timeout functie die om de seconde een nieuwe http request doet. 
Het probleem is dat dit alle data gewoonweg bij de array voegt en het dus een eindeloos lange array wordt en de chart past zich hieraan aan. Ik heb dus de max lengte van de array naar 25 objecten gezet en maak gebruik van de shift functie om de eerste entry te verwijderen uit de desbetreffende array. Zo blijft de array altijd 25 objecten lang en de chart verschuift ook elke seconde naar links als er een nieuwe datapoint verkregen wordt.<br>
![Chart van light data)](http://i.imgur.com/zyvEGfT.png)<br>
Wat nu nog moet gebeuren: de charts moeten mooier worden en een oplossing moet gevonden worden dat niet telkens de volledige database opgehaald moet worden en een authenticatie moet geimplementeerd worden zodat mensen met hun device id kunnen inloggen en de data op een eigen topic wordt gezet. Zo moet men enkel subscriben op het eigen topic en krijgt men enkel de eigen data binnen.



## Extra informatie
### Bijscholingen
Codecademy

### Nieuwe contacten
Jens Vanhooydonck,
Thomas Van Loon,
Didier Verachtert,
Glenn Ergeerts,
Dragan Subotic,
Maarten Weyn

### Literatuur
Codecademy (http://www.codecademy.com) <br>
Stack Overflow (http://www.stackoverflow.com) <br>
Build a RESTful API using Node and Express 4 (https://scotch.io/tutorials/build-a-restful-api-using-node-and-express-4) <br>
Getting started with MQTT - The Jackal of JavaScript (http://thejackalofjavascript.com/getting-started-mqtt/) <br>
MongoDB Manual (https://docs.mongodb.org/manual) <br>
Google (www.google.com)


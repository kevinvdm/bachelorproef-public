## 4.4 Gebruikte tools

Hieronder zullen de verschillende tools opgenoemd en uitgelegd worden die gebruikt werden om deze applicatie te verwezenlijken. Hier moet bij gezegd worden dat elke tool zijn voor- en nadelen heeft. Deze worden telkens toegelicht zodat een persoonlijke voorkeur gemaakt kan worden.

### 4.4.1 Brackets

Brackets is een open-source IDE vooral bedoeld voor web development. Het programma zelf is ook geschreven in HTML, CSS en JavaScript. Het is ontwikkeld door Adobe als een code editor gedreven door de community, en is cross-platform beschikbaar voor Mac-, Windows- en Linux-systemen. <br>

Het voordeel aan Brackets is de customization. Er kunnen thema's toegevoegd worden en de juiste syntax-highlighting kan gebruikt worden die best bij jou past. Het is echter vooral ook volledig gratis voor gebruik.

Nadeel is dat er niet al te veel mogelijke plugins voor zijn. Het programma is wel open-source, dus moesten er noodzakelijke dingen aan toegevoegd worden kan dit altijd door de gebruiker zelf worden gedaan. Er zijn echter betere mogelijkheden.<br>

### 4.4.2 Atom

Atom is een beter alternatief. Het is eveneens een open-source text editor, maar met veel meer mogelijkheden dan Brackets. Relatief recent is Atom ontwikkeld door het team van GitHub zelf, dus ze weten wat nodig is in een IDE. Hun slogan voor Atom is: *Hackable to the core*. Customizations kunnen dus worden toegevoegd naar keuze, maar het is van het begin af aan toegankelijk zonder zelfs een configuratiebestand aan te raken. Zo zijn er ontzettend veel plugins te gebruiken voor alle mogelijke programmeertalen en doeleinden. Atom kan dienen voor webtechnologieen (het is zelf geschreven met het Electron framework in JavaScript), maar ook als Markdown editor en zo verder.

Een van de meest populaire (snelste) text editors is Sublime. Hiervoor kan een package manager geinstalleerd worden om plugins te installeren maar dit is minder gebruiksvriendelijk om op te stellen.
Atom heeft een geintegreerde package manager met enorm veel keuzes, die elk op zich nog aparte configuraties bieden.
Net iets trager dan Sublime, maar ontzettend gebruiksvriendelijk is Atom zeer sterk aan te raden als algemene text editor.

### 4.4.3 Node.JS (Command Prompt)

Node.js is een software omgeving waarop applicaties geschreven in JavaScript ontwikkeld kunnen worden. De omgeving biedt een eigen runtime waarop de applicaties draaien, en biedt dan server-side services aan voor iedereen die er gebruik van maakt. Het grote voordeel van Node.js is dat ze cross-platform zijn, en dus zowel op Mac, Windows als Linux kunnen draaien.

In de Node.js runtime zit een reeks command-line tools, die gebruikt worden om de ontwikkelde applicaties te starten, te stoppen, te configureren en te debuggen.

### 2.4.4 MongoDB

De MongoDB runtime is eveneens een reeks command-line tools die zorgen voor het hosten van een eigen database. Als de database op een third party service wordt gehost moet er niets op de computer zelf geinstalleerd worden.
Indien het wel nodig is, dienen de tools om databases aan te maken, te configureren of de toegankelijkheid (rechten) van databases of collecties aan te passen.

### 4.4.5 Google Chrome

Om het project te debuggen werd vooral Google Chrome gebruikt. Dit omdat het de nuttigste en overzichtelijkste debug tools bevat met een duidelijke console. 

## 4.5 Third-party dashboard services

Het laatste dat wordt aangekaart is een set van externe dashboards die gemakkelijk en gratis gebruikt kunnen worden. Hier zit authenticatie reeds in verwerkt maar de personalisatie laat soms wat te wensen over. 

### 4.5.1 Dashing

Dashing is een framework om kleurvolle dashboards aan te maken. Het is gebouwd op Sinatra, een web-applicatie library in Ruby dat een alternatief biedt voor bijvoorbeeld Ruby on Rails (maar het gebruikt niet het MVC-model van RoR). Features van Dashing:

* Maak gebruik van widgets die vooraf gemaakt zijn of schrijf ze zelf in sccs (sassy CSS), html en coffeescript (taal die in JavaScript compileert, maar het overzichtelijker en korter maakt)
* Dashing heeft een eigen API die gebruikt kan worden om data naar het dashboard te pushen 
* Charts kunnen versleept worden

Het wordt vooral gebruikt voor Ruby projecten. Het is te vinden op http://dashing.io/.

### 4.5.2 Freeboard.io

Freeboard is een open source real-time dashboard platform voor allerlei Internet of Things projecten. Het gebruik ervan is volledig gratis, en in het platform zit alles, inclusief authenticatie, reeds ingebouwd. Wanneer er wordt ingelogd kan elke gebruiker zijn eigen dashboard personaliseren en widgets toevoegen. Aan elke widget wordt een databron toegevoegd, die deze data automatisch ophaalt. Een voorbeeld van zulk een databron is JSON. Op te merken indien een externe JSON wordt geladen in freeboard: de connectie moet veilig zijn en dus moet het gehost worden op een *https* server. Hiervoor moet poort 443 open staan op de server.

* Authenticatie ingebouwd
* Widgets gebruiksvriendelijk, enkel databron moet toegevoegd worden

Het werkt goed samen met dweet.io. Dit is een service die zorgt voor de opslag en routing van binnenkomende datapoints. Hier hangt echter een maandelijkse kost aan vast als ze opgeslagen moeten worden. Het is te vinden op http://freeboard.io/.

### 4.5.3 Blynk

Blynk is een mobile-only IoT dashboard dat gebruikt kan worden met vrijwel alle IoT projecten. Er moet een library worden geinstalleerd in de code van het development board die de nodige pins verbindt met het dashboard. De app wordt dan gedownload op de smartphone en met token-based authentication wordt er verbonden met het project. De data verschijnt dan automatisch in het dashboard. Het is te vinden op de App Store en Google Play. De website van Blynk is http://www.blynk.cc/.

* Mobile-only
* Enorm gebruiksvriendelijk
# LSP x Nuts met GF

## Introductie project

Binnen de zorg wordt tussen verschillende soorten zorgaanbieders data
uitgewisseld. Zo is er het Landelijk Schakel Punt (LSP) wat gebruikt wordt voor
bijvoorbeeld uitwisseling van medicatie gegevens tussen ziekenhuizen, apotheken
en huisartsen. Ook is er de Nuts infrastructuur die bijvoorbeeld veel gebruikt
wordt in de verpleeg-, verzorgingshuizen en thuiszorg. Doordat niet alle
zorgaanbieders op het LSP dan wel op Nuts zijn aangesloten is het niet mogelijk
gegevens uit te wisselen tussen alle zorgaanbieders in Nederland.

Om brede uitwisseling in de zorg mogelijk te maken zijn er vier opties:

1. Alle zorgaanbieders sluiten direct aan op het LSP

2. Alle zorgaanbieders leveren een Nuts connectie

3. LSP en Nuts met elkaar verbinden met gebruik making van Generiek Functies

4. Nieuwe infrastructuur opzetten voor het delen van medische gegevens

Binnen dit project is gekozen voor optie 3 om uitwisseling mogelijk te maken.
Dit zal gedaan worden door gebruik te maken van de componenten uit de Generieke
Functies. Naast technische specifcaties zullen ook allerlei organisatorische afspraken gemaakt moeten worden. Het idee is dat al deze technische en organisatorische afspraken zullen landen in (een nieuwe versie van) het Twiin afsprakenstelsel.

Uiteindelijk zijn zorgverleners zijn de beoogde gebruikers van de oplossing. 
Het doel van dit project is om de Generieke Functies te kunnen beproeven voor 
het realiseren van de koppeling tussen het LSP en Nuts.

## Context en scope

Dit project beoogd het LSP (VZVZ), Nuts (Nuts community) en de voor de Generieke
Functies (VWS en community) ontwikkelde modules in een beproeving aan elkaar te
verbinden. Ook
vraagt het aanpassingen in de zorgaanbieders gebruikte informatie systemen (XIS
etc.). 

Bij uitwisseling tussen zorgaanbieders op LSP en Nuts zijn twee richtingen
mogelijk:

1. Nuts vraagt informatie op bij LSP

2. LSP vraagt informatie op bij Nuts

Beide uitwisselrichtingen zijn in-scope. Voor de eerste wordt het actueel
medicatie overzicht als casus gebruikt. Hierbij kan een, volgens de autorisatie
richtlijnen geauthenticeerde, arts in via de Nuts node welke verbonden is met
het LSP een overzicht krijgen van de voorgeschreven medicatie.

Het twee scenario heeft nog geen concrete casus. Hierbij wordt in de PoC gewerkt
met het ophalen van de Patient resource aangevuld met de Observervation
resource. Dit is puur een demonstratie van technische werking en mogelijkheden.

## Architectuur samenvatting

Deze PSA is opgesteld door het project team. Dit zijn VWS met iRealisatie 
als technische ondersteuning, VZVZ
voor LSP en Nuts. Daarnaast is dit PSA ook bedoelt voor de stakeholders: beleid
bij VWS, IT leveranciers en juridische ondersteuning.

De oplossing maakt nieuwe functies beschikbaar voor zorgverleners. 

De beoogde oplossing voorziet in het digitaal ondertekenen van allerlei attesten/credentials voor identificatie
en authenticatie, lokalisatie, autorisatie en adressering. Deze credentials moeten vaak door een eindgebruiker(sorganisatie) en/of door een vertrouwde uitgever digitaal ondertekend worden. De organisatie en technologie hiervoor is voor een groot deel van de partijen nog niet beschikbaar. Het bouwen en implementeren van de nieuw te maken technische en organisatorische hiervoor heeft n grote technische en organisatorische impact  op de systemen van de eindgebruikers. De verwachting is dat het nog jaren kan duren voordat alle partijen hieraan (kunnen) voldoen zal de oplossing ook een deze overgangssituatie moeten kunnen ondersteunen. Het concept van (Gekwalificeerde Twiin) knooppunten (GtK) helpt om een brug te slaan tussen de partijen en interoperabiliteit te bewerkstelligen. 

Voor de aan AORTA deelnemende zorgaanbieders en -leveranciers is het uitgangspunt dat deze geen of slechts beperkte
aanpassingen kunnen gaan doen voor deze PoC. Waar mogelijk zullen benodigde wijzigingen voor de aansluiting op de Generieke Functies via het LSP gedaan worden. Het LSP fungeert hier dan als GtK. De verwachting is dat een dergelijke constructie van het gebruik van een GtK ook van toepassing zal zijn voor andere Twiin deelnemers.

 Het gebruik van knooppunten heeft wel impact op het (niet) kunnen toepassen van cryptografie. Knooppunten kunnen namelijk niet digitaal ondertekenen wat eigenlijk een zorgaanbieder/-verlener zou moeten doen. Vol vertrouwen op de technlogie is daarom voorlopig niet (altijd) mogelijk. Een deel van het te creeëren vertrouwen wat je eigenlijk met cryptografie ingevuld/ondersteund zou willen zien zal noodgedwongen nog organisatorisch moeten worden ingericht.  

 Om te zorgen/borgen dat de benodigde aanvullende organisatorische afspraken worden op de beoogde manier worden ingevuld stelt Twiin eisen aan de GtK's en de achterliggende deelnemers. Om de gegevensuitwisselingen tussen de knoopunten interoperabel te maken stelt Twiin wél technische eisen aan de interfaces van de GtK's. Om een groeipad te faciliteren is het idee dat de beoogde technlogie van voor de generieke functies tussen de knoopunten al wel gebruikt wordt, maar de bijbeorende cryptografie op bepaalde punten tijdelijk op een andere manier wordt ingericht. In een dergelijk model kunnen de partijen die de beoogde cryptografie eigenljk moeten toepassen dit meer in hun eigen tempo doen, zonder dat er sprake moet zijn van een 'big bang overgang'.

Het project heeft als doel technisch oplossingen te ontwikkelen en beproeven in 
een PoC. De PoC richt zich op de technische realisatie van de 
uitwisselmogelijkheid. De juridische grondslagen en andere voorwaardelijke 
onderdelen voor een pilot fase zijn buiten scope van dit project.

## Huidige situatie

In de huidige situatie zijn het LSP en Nuts twee gescheiden omgevingen. Elk
hebben hun eigen oplossingen voor identificatie & authenticatie (I&A),
autorisatie (controle op toestemming), lokalisatie, adressering, logging en bij
LSP conversie van standaarden.

### LSP

#### I&A

##### XIS

Het LSP houdt in haar applicatieregister bij welke XIS’en gevalideerd zijn voor
welke gegevensuitwisselingen (welke versies van berichten gestuurd en ontvangen
mogen/kunnen worden). Hiervoor dient een XIS bij Nictiz een kwalificatie voor
een bepaalde zorgtoepassing te hebben behaald en door VZVZ geaccepteerd zijn
voor de invulling van de generieke functies zoals ingevuld voor AORTA.

##### Zorgaanbieder

Identificatie van de zorgaanbieder gebeurt op basis van het
UZI-registerabonneenummer (URA). Authenticatie daarvan geschiedt via het
opzetten van de TLS-verbinding met het UZI-servercertificaat en (nieuw) het
signen van de transactie met datzelfde UZI-servercertificaat.

#### I&A zorgverlener

Identificatie van de zorgverlener gebeurt op basis van het Unieke Zorgverlener
Identificatie (UZI)-nummer. Authenticatie hiervan geschiedt op het hoogste
betrouwbaarheidsniveau (eIDAS hoog) op basis van gesignde SAML2-tokens van de
UZI-pas. Hierbij zijn er twee opties:

1. De gebruiker signet de transactie met de eigen UZI-pas (al dan niet onder
   mandaat van een arts).

2. De uiteindelijk verantwoordelijke arts heeft een zogenaamd mandaattoken
   getekend. Hiermee kan het systeem (icm een transactietoken en een
   inschrijftoken) gegevens kan raadplegen.

### Autorisatie

Het LSP autoriseert binnenkomende en uitgaande verzoeken op basis van het
afgesproken autorisatieprotocol. Voor de meeste gegevensuitwisselingen die nu op
AORTA draaien is een autorisatie op basis van UZI-rolcode afgesproken.

De zorgkoepels, die
over de autorisatieregels gaan, hebben gesteld dat alleen een beperkte set van
BIG-rollen geautoriseerd is om medicatiegegevens te raadplegen/versturen (aan de
hand van de UZI-rolcodes). Daarom wordt binnen dit project enkel met deze rollen
gewerkt.

### Lokalisatie

Lokalisatie van gegevens hangt nauw samen met toestemming.

- In het geval een brondossierhouder de toestemmingen in het XIS beheerd wordt
  alleen met toestemming de verwijsindex _van_ het LSP gevuld met
  lokalisatiemetadata

- In het geval de brondossierhouder de toestemmingen in Mitz beheert wordt
  altijd het actualiteitenregister _in_ het LSP met lokalisatiemetadata gevuld,
  maar wordt dit pas vrijgegeven als Mitz hiervoor een toestemming heeft
  afgegeven.

### Adressering

Als het LSP zelf een applicatie moet adresseren geschiedt dit door het al
genoemde interne applicatieregister te raadplegen. Als een zorgverlener een
andere zorgverlener zoekt/wil adresseren gebruikt met hiervoor het ZORG-AB
adresboek (waarin ook een kopie zit van het applicatieregister).

### Logging

Zowel agerend als reagerend XIS en het LSP houden allemaal een log bij

### Conversie van standaarden (FHIR)

Het LSP beschikt over een berichten transformatiedienst die HL7v3 berichten kan
omzetten naar HL7 FHIR en vice versa op basis van door Nictiz aangeleverde XSLT
vertaal algoritmes.

### Nuts

#### I&A

##### Zorgaanbieder

Nuts maakt voor verificatie van zorgaanbieders gebruik van verifiable
credentials. De wijze waarop deze gemaakt worden is via een UZI-server
certificaat.

##### Authenticeren zorgverlener

Binnen Nuts wordt er gewerkt met verifiable credentials voor het authenticeren van
de zorgverlener. Nuts kent drie wijzen van het authenticeren van zorgverleners:

1. Zorgverleners met een UZI-pas kunnen ZorgID gebruiken voor het maken van een credential.
2. Voor zorgverleners zonder persoonslijke (UZI) authenticatie middelen wordt gebruik
   gemaakt van de door de zorgaanbieder uitgegeven credentials
   (NutsEmployeeCredential).
3. Zorgverleners kunnen persoonlijk authenticeren via de Yivi app en hun bank.
   Dit levert enkel gegevens op over de persoon (niet dat dit een zorgverlener
   is).

#### Autorisatie

Applicaties die gebruik maken van Nuts hebben eigen autorisatie logica die
gebruik maakt van de eerder genoemde credentials.

#### Lokalisatie

Er zijn twee wijzen waarop binnen Nuts gelokaliseerd wordt.

1. Voor lokalisatie wordt per instelling een zorgnetwerk geregistreerd. Dit
vaste netwerk wordt bij lokalisatie bevraagd op beschikbaarheid van gegevens.

2. Een shared care plan kan gebruikt worden om samenwerkende organisaties te
   bepalen. Deze kunnen daarmee bevraagd worden bij lokalisatie.

#### Adressering

Raadplegers kunnen op 2 manieren de technische adressen van bronhouders vinden:

1. Via een per toepassing in te richten "discovery" dienst. Deze dienst wordt vervolgens gebruikt door de deelnemers van de toepassing.
2. Via het gedistribueerde Nuts-netwerk. Deze optie wordt uitgefaseerd.

#### Logging

Logging vindt plaats bij de bronhouders en is buiten scope van Nuts.

#### Conversie van standaarden (FHIR)

Het bieden van endpoints zoals FHIR is buiten scope van Nuts. Dit wordt ingevuld
door de toepassingen die gebruik maken van de Nuts node(s). Binnen de Nuts
gebruikers wordt daarom per toepassing afgesproken welke standaarden hiervoor te
hanteren.

## Eindresultaat

In dit hoofdstuk wordt het beoogde eindresultaat beschreven.

### Plateau 1: PoC

#### I&A

##### Authenticeren zorgverlener

Voor het authenticeren van de zorgverlener zal gewerkt worden met Dezi die de
Generieke Functie Identificatie en Authenticatie invult. Op basis van door Dezi
uitgeven attesten kunnen de communicerende partijen elkaar op het juiste
betrouwbaarheidsniveau authenticeren. Binnen de PoC zal gewerkt worden met een
stub-implementatie van Dezi of een alternatieve (tijdelijke) oplossing voor
authenticatie.

Omdat het nog jaren kan duren voordat alle zorgaanbieders hun systemen en processen hebben aangepast om de nieuwe Dezi manier van authenticatie geïmplmenteerd te hebben is het voorstel om op aan de knooppunten de technlogische eisen te stellen voor authenticatie maar de partijen achter het knoopunt -binnen bepaalde kaders- een groeipad naar de gewenste situatie te ondersteunen. 

##### Authenticeren zorgaanbieder

Het authenticeren van de zorgaanbieder zal vanuit Nuts naar het LSP gaan op basis
van een verifiable credential wat (evt. indirect via een certificaat) via het
UZI register tot stand komt. Het LSP valideert dit vervolgens. Systemen die op
het LSP aangesloten zijn vertrouwen het LSP en zullen daarom geen additionele
verificatie doen.

Voor de communicatie vanuit het LSP naar Nuts zal de vragende zorgaanbieder een
verifiable credential gebruiken. Dit kan het LSP doorsturen naar de Nuts node
van de bron houder. Het credential zal "handmatig" opgebouwd worden via het
UZI-servercertificaat of een andere oplossing (zie hoofdstuk 
"Gap huidige met eindresultaat").

### Toestemming

Bij de op het LSP aangesloten zorgaanbieder zal op de huidige wijze bepaald
worden of er expliciete toestemming verleend is. Dit gebeurt lokaal bij 
het bronsysteem of via Mitz.

#### Lokalisatie

In de eindsituatie zal er gewerkt worden volgens de Generieke Functie
lokalisatie. Hierbij wordt gewerkt met een lokalisatie index op basis van de in
de lokalisatie werkgroep uitgewerkte opzet. Deze index wordt ook wel de
Nationale Verwijs Index (NVI) genoemd. In de PoC wordt hiervoor gewerkt met een
referentie implementatie. Voor een pilot/productiesituatie zouden alle partijen de NVI ingebruik genomen moeten hebben. Dit zal ook een proces van vermoedelijk enkele jaren zijn voordat het zover is. Wellicht zouden knooppunten de NVI namens hun zorgaanbieders kunnen vullen of is een combinatie met Mitz mogelijk waarin het abonnementenregister van Mitz met de NVI wordt gescynchroniseerd.

#### Adressering

Voor adressering zal in gebruik gemaakt worden van de Generieke Functie
Adressering. Hierbij zal er een IHE mCSD Directory worden ingericht met de
endpoint gegevens van de in de PoC betrokken systemen. Hierbij wordt nog niet
gewerkt met registratie via het LRZa.

#### Logging

Het LSP logt op de wijze die het nu al doet in voor het AORTA-afsprakenstelsel. Voor
vragen in de andere richting (LSP naar Nuts) zal dit werken op de wijze zoals
dit nu ook gebeurd bij Nuts-Nuts bevragingen.

#### Conversie van standaarden (FHIR)

Het LSP draagt zorg voor de conversie van standaarden bij bevraging van VVT naar
MSZ. De Nuts omgeving kan gebruik maken van FHIR R3 voor het opvragen van
medicatiegegevens. De data vanuit de VVT is beschikbaar op endpoints die conform
FHIR Stu3 werken. Het LSP maakt, waar nodig, conversies naar andere standaarden
voor de op haar aangesloten zorgaanbieder. Voor medicatiegegevens gebruiken de
meeste bronsystemen nu nog HL7v3.

## Gap huidig met eindresultaat

Het eindresultaat bevat een aantal onderdelen die nog niet gerealiseerd zijn. Deze
sectie beschrijft per onderdeel wat er mist / aangevuld moet worden.

### NVI

De NVI bestaat op dit moment zowel in concept als in een basis implementatie.
Deze basis implementatie mist nog volwaardige autorisatie. In een PoC zou dit
(ten dele) weggelaten kunnen worden. Het is echter wel gewenst om een 
(beperkte) vorm van authenticatie te hebben.

### Adressering

Voor adressering is een er een referentie implementatie van IHE mCSD. De
referentie implementatie kan gebruikt worden voor het beschikbaar maken van de
endpoints. De huidige versie mist authenticatie functionaliteit. Dit is voor een
PoC niet blokkerend.

### Dezi

Op het moment van schrijven heeft Dezi nog geen mogelijkheid tot het
ondersteunen van authenticatie over meerdere systemen heen. Hiermee wordt
bedoelt dat een zorgverlener bij zorgaanbieder in haar ECD inlogt en dat
dit ECD vervolgens de authenticatie kan "doorsturen" naar een bron-systeem voor
bevraging van medische gegevens. Zowel de technische specificatie als een (demo)
oplossing is nodig om een PoC uit te kunnen voeren. Ook moeten de binnen de Nuts
en LSP gebruikte systemen voor de PoC aangepast worden om dit
mechanisme te ondersteunen.

Afhankelijk van de keuzes binnen de GF I&A en de bijbehorende impact kan voor 
de PoC gekozen worden voor een tussentijdse oplossing voor het authenticatie
vraagstuk. Twee mogelijke routes zijn:

- Het LSP zet het de gedane zorgverlenerauthenticatie door het LSP om in een (VC)
  formaat. Het Nuts-netwerk dient dan het LSP als uitgever te vertrouwen. Dit past bij het GtK-model van Twiin
- Het LSP stuurt authenticatiebewijs (SAML2/ JWT token) door naar het Nuts 
  netwerk. De ontvangende Nuts node gecontroleert dit zelf.

Het kan zijn dat de hierboven geschetste alternatieven voor Dezi tegen juridische
obstakels lopen wanneer deze voor een pilot/productie fase ingezet moeten worden.
Binnen dit project is dat vraagstuk buiten scope.

Naast deze routes is het mogelijk dat binnen de PoC nog andere oplossingen bedacht 
worden voor de authenticatie vraag.

### LSP en Nuts I&A

Het ontwerp gaat uit van het uitbreiden van de het LSP met functionaliteit om
Nuts te kunnen bevragen en bevraagd te worden door Nuts nodes. Hiervoor dienen
de authenticatie gegevens (Dezi en zorgaanbieder) volgens de Nuts wijze
verstuurd en ontvangen te worden. Ook zullen deze, binnen het LSP, vertaald
moeten worden naar het LSP eigen formaat.

Voor de communicatie vanuit de eerste lijn (via het LSP) naar de VVT zal de 
betreffende zorgaanbieder zichzelf moeten authenticeren. In het 
AORTA-afsprakenstelsel gebeurt dat nu o.b.v. het aanbieden van het 
UZI-servercertificaat waar de TLS-verbinding mee wordt opgezet en door het
ondertekenen van een transactietoken. 

Voor authenticatie vanuit LSP naar Nuts zijn er de volgende mogelijkheden:

1. De op LSP aangesloten systemen gaan VC's gebruiken voor authenticatie
   (significante change voor ECD's).
2. Het LSP zet de zorgaanbiederauthenticatie om in een (VC) formaat. De
   ontvangende Nuts node dient dan het LSP als uitgever te vertrouwen. Dit past bij het model van Twiin waarin de GtK's vertrouwd worden en Twiin kadervormende eisen stelt aan de zorgaanbieders achter het GtK om de gebruikersauthenticatie op eIDAS niveau hoog te doen.
3. Het LSP stuurt het authenticatiebewijs (nu een SAML2 token) door naar
   de Nuts node. Deze kan dan dan controleren of dit klopt.

## Verantwoordelijkheden

De binnen de PoC geïdentificeerde gaps (te realiseren functies) zullen
gezamenlijk (VWS, VZVZ, Nuts) ingevuld worden. Per onderdeel is de
hoofdverantwoordelijke:

- VWS
  - NVI
  - Adressering
  - Dezi
- VZVZ
  - LSP Nuts connectiviteit
  - Dezi aansluiting
- Nuts
  - Dezi aansluiting
- Twiin
   - opname afspraken in Twiin afsprakenstelsel (via het ontwikkelsupplement)

## Architectuur principes

Bij het uitwerken van de oplossing worden de volgende architectuur principes
gebruikt.

### Beperkte aanpassingen bij LSP deelnemer

Voor de aan LSP AORTA deelnemende zorgaanbieder en -leveranciers is het
uitgangspunt dat deze beperkte aanpassingen zullen gaan doen voor deze PoC. Dit
betekent dat aanpassingen, voor zover mogelijk, bij het LSP, Nuts of de
Generieke Functies zullen plaatsvinden. Hiervan wordt enkel afgeweken wanneer de
toekomstige visie voor de zorg / Generieke Functies al duidelijk is of dat dit 
met oog op de beproeving wenselijk is om mee te nemen in de PoC.

## Openstaande punten en aannames

### Uitbreiding Dezi

Een van de vereisten bij het leveren van medischegegevens aan een zorgverlener
van een andere organisatie is informatie over die persoon. Dit gaat dan om zaken
als het BIG nummer en de rolcode.

Om dit mogelijk te maken voor een uitwisseling tussen LSP en Nuts moet deze
informatie op een betrouwbare wijze gedeeld kunnen worden. Het plan is om
hiervoor een uitbreiding te realiseren in Dezi. De aanname is dat er een
oplossing is waar draagvlak voor is bij het CIBG. Ook gaat dit project er vanuit
dat er tijdens de PoC een stub (namaak) versie van Dezi gebruikt kan worden die
deze functie heeft. Tussen de knooppunten kan wel afvast een duurzame implemnetatie afgesproken worden voor het authenticeren van zorgverleners die ook een groeimodel ondersteunt. Dat zou inhouden dat wel de gewenste VC's tussen de knooppunten worden uitgewisseld maar dat de digitale ondertekening daarvan op verschillende manieren nog gedaan mag worden, waaronder ondertekening van het knooppunt zelf. 

### Veilig netwerk

Het LSP maakt gebruik van een gesloten netwerk voor communicatie met haar
deelnemers. Nuts gebruikt hiervoor het reguliere internet. De huidige opzet van
het per zorgaanbieder afsluiten van een contract om op het gesloten LSP is niet
gewenst in de VVT sector vanwege de additionele kosten die dit met zich
meebrengt. Voor nu wordt er daarom uitgegaan van een aansluiting van de VVT via
het publieke internet. Binnen VWS wordt in het kader van het Landelijk Dekkend
Netwerk (LDN) project gewerkt aan een zorgbrede richtlijn. Binnen dit project
wordt de aanname gedaan dat de uitkomsten hiervan geen negatieve invloed op de
keuzes binnen het project hebben.

Voor de PoC wordt gewerkt met het publieke internet. Een definitieve oplossing
die voldoet aan de juridische en andere eisen met betrekking tot veilig netwerk
is buiten de scope van deze PoC, maar is voor een pilot/productie situatie wel randvoorwaardelijk.

### Toestemming

Binnen dit project wordt uitgegaan van de registratie van toestemming bij de
Generieke Functie Toestemmingen op basis van een Online Toestemmingsvoorziening
(OTV). Dit is nodig omdat de lokalisatie index (NVI) anders
niet kan bepalen of de toegang geautoriseerd kan worden. In de PoC fase kan
hiervoor een stub ingezet worden of worden uitgegaan van toestemming 
(autorisatie op NVI is dan buiten scope). Voor een productiefase zou de NVI toestemming bij Mitz kunnen controleren of met de partijen die lokaal toestemming registreren afspreken dat die toestemming ook voor de NVI wordt gevraagd.

Binnen dit project zal gewerkt worden met VCs voor het verwerken van
(toegangs)bewijzen (credentials). Voor de PoC zal gekozen worden voor danwel
bestaande oplossingen binnen Nuts, danwel oplossingen die voldoen aan de EIDAS
richtlijnen die nu in ontwikkeling zijn.

Voor de LSP zijde gelden andere keuzes. Zie hiervoor de eerder beschreven
punten met betrekking tot authenticatie.

### Autorisatie richtlijnen

Binnen de VVT zijn veel zorgverleners werkzaam die geen BIG registratie hebben.
Bestaande richtlijnen die van toepassing zijn op de ontsluiting van medische
gegevens (zoals [die van de
KNMP](https://www.knmp.nl/richtlijnen/overdracht-van-medicatiegegevens-de-keten))
eisen echter dat zorgverleners die deze gegevens raadplegen BIG geregistreerd
zijn. 

De technische PoC zal enkel met fictieve data werken. Hierdoor is het mogelijk
om van de richtlijnen af te wijken. Binnen dit project zal waar mogelijk in de
test scenario's gewerkt worden met voorbeelden die binnen de richtlijnen passen.
Indien dit niet kan (omdat de richtlijn bepaalde communicatie niet toestaat)
zal dit duidelijk worden aangegeven om verwarring over de inzetbaarheid te
beperken.

### Onderdelen uit het trust-over-ip model

Voor dit project zal enkel gewerkt worden aan de Technology aspecten (laag 1 tot
en met 4) uit het [trust-over-ip model](https://trustoverip.org/toip-model/
"https://trustoverip.org/toip-model/"). De Governance en Ecosystem onderdelen
worden buiten dit project opgepakt.

## Architectuur risico’s

### Juridisch

De uitwisseling van medische gegevens is gebonden aan wetten, richtlijnen en
andere juridische kaders. Het is mogelijk dat sommige functies of oplossingen
juridisch grondslagen missen. Binnen deze PoC wordt hier geen activiteit op
ondernomen. Wel kunnen ontwerpen en implementaties voorgelegd worden ter
beoordeling op juridische en organisatorische haalbaarheid.

In de situatie waarin knooppunten vertrouwd worden om bepaalde zaken namens de zorgaanbieders/-verleners invuld zijn soms schuren met wat er in de NEN7512 wordt vereist en in het geval er sprake is van een gegevensuitwisseling met een elektronisch uitwisselingsysteem is het voldoen aan de NEN7512 een wettelijk vereiste. 

### Generieke functies

De specificaties voor de generieke functies zijn nog in ontwikkeling. Voor de
PoC kan gestart worden met concept versies. Eventuele aanpassingen in
specificaties kunnen gevolgen hebben voor zowel de Generieke Functie componenten
als het LSP, Nuts en de aangesloten XIS systemen. We zullen onder ogen moeten zien dan het volledig implemneteren van de Generiek Functies een proces van jaren kan zijn. In de tussentijd zal een groeimodel ondersteund moeten worden om gegegvensuitwisseling op brede schaal mogelijk te maken. 

## Advies en goedkeuring

Dit document wordt aangeboden voor akkoord aan het projectteam LSP x Nuts. In
het specifiek wordt gevraagd om een akkoord van:

- Opdrachtgever vanuit VWS
- Project leider VZVZ
- Project leider Nuts

Het advies is om, naast het PoC project, een juridisch traject op te starten om de
openstaande punten op juridisch vlak te adresseren.

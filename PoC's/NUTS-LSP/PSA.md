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

3. LSP en Nuts met elkaar verbinden

4. Nieuwe infrastructuur opzetten voor het delen van medische gegevens

Binnen dit project is gekozen voor optie 3 om uitwisseling mogelijk te maken.
Dit zal gedaan worden door gebruik te maken van de componenten uit de Generieke
Functies.

Zorgverleners zijn de beoogde gebruikers van de oplossing. De zorgkoepels, die
over de autorisatieregels gaan, hebben gesteld dat alleen een beperkte set van
BIG-rollen geautoriseerd is om medicatiegegevens te raadplegen/versturen (aan de
hand van de UZI-rolcodes). Daarom wordt binnen dit project enkel met deze rollen
gewerkt.

Het doel van de oplossing is om zorgverleners over meer en betere gegevens te
laten beschikken waardoor deze efficiënter en effectiever zorg kunnen verlenen.

## Context en scope

Dit project beoogd het LSP (VZVZ), Nuts (Nuts community) en de voor de Generieke
Functies (VWS en community) ontwikkelde modules aan elkaar te verbinden. Ook
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

Deze PSA is opgesteld voor de het project team wat betrokken is bij de uitvoer
van LSP x Nuts. Dit zijn VWS met iRealisatie als technische ondersteuning, VZVZ
voor LSP en Nuts. Daarnaast is dit PSA ook bedoelt voor de stakeholders: beleid
bij VWS, IT leveranciers en juridische ondersteuning.

De oplossing maakt nieuwe functies beschikbaar voor zorgverleners. Hiervoor
moeten wel aanpassingen gemaakt worden in de technische laag voor identificatie
en authenticatie, lokalisatie, autorisatie en adressering. Voor de aan AORTA
deelnemende zorgaanbieder en -leveranciers is het uitgangspunt dat deze geen
aanpassingen zullen gaan doen voor deze PoC.

Het project heeft als doel via een PoC door te gaan naar een Pilot. De PoC richt
zich op de technische realisatie van de uitwisselmogelijkheid. Parallel hieraan
(buiten de PoC) zal gewerkt worden aan de juridische grondslagen en voorwaarden
om, na afronden van de PoC, door te kunnen naar een Pilot.

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

#### Autorisatie

Autorisatie geschied op basis van de autorisatierichtlijn waarin voor iedere
transactie bepaald is welke UZI-rolcodes het bericht mogen versturen.

#### Lokalisatie

Lokalisatie van gegevens hangt nauw samen met toestemming.

- In het geval een brondossierhouder de toestemmingen in het XIS beheerd wordt
  alleen met toestemming de verwijsindex _van_ het LSP gevuld met
  lokalisatiemetadata

- In het geval de brosdossierhouder de toestemmingen in Mitz beheert wordt
  altijd het actualiteitenregister _in_ het LSP met lokalisatiemetadata gevuld,
  maar wordt dit pas vrijgegeven als Mitz hiervoor een toestemming heeft
  afgegeven.

#### Adressering

Als het LSP zelf een applicatie moet adresseren geschiedt dit door het al
genoemde interne applicatieregister te raadplegen. Als een zorgverlener een
andere zorgverlener zoekt/wil adresseren gebruikt met hiervoor het ZORG-AB
adresboek (waarin ook een kopie zit van het applicatieregister).

#### Logging

Zowel agerend als reagerend XIS en het LSP houden allemaal een log bij

#### Conversie van standaarden (FHIR)

Het LSP beschikt over een berichten transformatiedienst die HL7v3 berichten kan
omzetten naar HL7 FHIR en vice versa op basis van door Nictiz aangeleverde XSLT
vertaal algoritmes.

### Nuts

#### I&A

##### Zorgaanbieder

Nuts maakt voor verificatie van zorgaanbieders gebruik van verifiable
credentials. De wijze waarop deze gemaakt worden is via een UZI-server
certificaat.

##### Vaststellen zorgverlener

Binnen Nuts wordt er gewerkt met verifiable credentials voor het vaststellen van
de zorgverlener. Nuts kent drie wijzen van het authenticeren van zorgverleners:

1. Zorgverleners met een UZI-pas kunnen ZorgID gebruiken voor het maken van een credential.
2. Voor zorgverleners zonder persoonslijke (UZI) authenticatie middelen wordt gebruik
   gemaakt van de door de zorgaanbieder uitgegeven credentials
   (NutsEmployeeCredential).
3. Zorgverleners kunnen persoonlijk authenticeren via de Yivi app en hun bank.
   Dit levert enkel gegevens op over de persoon (niet dat dit een zorgverlener
   is).

#### Autorisatie

Applicaties die gebruik maken van Nuts hebben eigen autorisatie logica die gebruik maakt van de eerder genoemde credentials.

#### Lokalisatie

Er zijn twee wijzen waarop binnen Nut gelokaliseerd wordt.

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

## Eindsituatie van dit project

In de eindsituatie worden naast componenten uit het LSP en Nuts ook de Generieke
Functies ingezet.

### I&A

#### Vaststellen zorgverlener

Voor het vaststellen van de zorgverlener zal gewerkt worden met Dezi die de
Generieke Functie Identificatie en Authenticatie invult. Op basis van door Dezi
uitgeven attesten kunnen de communicerende partijen elkaar op het juiste
betrouwbaarheidsniveau authenticeren.

#### Vaststellen zorgaanbieder

Het vaststellen van de zorgaanbieder zal vanuit Nuts naar het LSP gaan op basis
van een verifiable credential wat (evt. indirect via een certificaat) via het
UZI register tot stand komt. Het LSP valideert dit vervolgens. Systemen die op
het LSP aangesloten zijn vertrouwen het LSP en zullen daarom geen additionele
verificatie te doen.

Voor de communicatie vanuit de MSZ naar de VVT zal de MSZ applicatie een
verifiable credential maken?

### Autorisatie

Bij de op het LSP aangesloten zorgaanbieder zal op de huidige wijze bepaald
worden of er expliciete toestemming verleend is.

### Lokalisatie

In de eindsituatie zal er gewerkt worden volgens de Generieke Functie
lokalisatie. Hierbij wordt de NVI ingezet.

### Adressering

Voor adressering zal in de eindsituatie gebruik gemaakt worden van de Generieke
Functie Adressering. Hierbij zal het LSP voor haar deelnemers een adresgegevens
bron beschikbaar maken. Voor de gebruikers van Nuts nodes zal de adresgegevens
bron ingebouwd worden in de Nuts node?

### Logging

Het LSP logt op de wijze die het nu al doet bij aanvragen van VVT naar MSZ. Voor
vragen in de andere richting (LSP naar Nuts) zal dit werken op de wijze zoals
dit nu ook gebeurd bij Nuts-Nuts bevragingen.

### Conversie van standaarden (FHIR)

Het LSP draagt zorg voor de conversie van standaarden bij bevraging van VVT naar
MSZ. De Nuts omgeving kan gebruik maken van FHIR R3 voor het opvragen van
gegevens. De data vanuit de VVT is beschikbaar op endpoints die conform FHIR
Stu3 werken. Het LSP maakt, waar nodig, conversies naar andere standaarden voor
de op haar aangesloten zorgaanbieder.

## Gap huidig met eindsituatie

De eindsituatie bevat een aantal onderdelen die nog niet gerealiseerd zijn. Deze
sectie beschrijft per onderdeel wat er mist / aangevuld moet worden.

### NVI

De NVI bestaat op dit moment zowel in concept als in een basis implementatie.
Deze basis implementatie mist nog volwaardige autorisatie. Dit is nodig in de
pilot fase. In een PoC zou dit (ten dele) weggelaten kunnen worden.

### Adressering

Voor adressering is een er een referentie implementatie van IHE mCSD. De
referentie implementatie kan gebruikt worden voor het beschikbaar maken van de
endpoints. De huidige versie mist authenticatie functionaliteit. Dit is voor een
PoC niet blokkerend.

### Dezi

Op het moment van schrijven heeft Dezi nog geen mogelijkheid tot het
ondersteunen van authenticatie over meerdere systemen heen. Hiermee wordt
bedoelt dat een zorgverlener bij zorgaanbieder in haar ECD authenticeert en dat
dit ECD vervolgens de authenticatie kan "doorsturen" naar een bron-systeem voor
bevraging van medischegegevens. Zowel de technische specificatie als een (demo)
oplossing is nodig om een PoC uit te kunnen voeren. Ook moeten de binnen de Nuts
en LSP gebruikte systemen voor de PoC en pilot aangepast worden om dit
mechanisme te ondersteunen.

### LSP en Nuts connectiviteit

Het ontwerp gaat uit van het uitbreiden van de het LSP met functionaliteit om
Nuts te kunnen bevragen en bevraagd te worden door Nuts nodes. Hiervoor dienen
de authenticatie gegevens (Dezi en zorgaanbieder) volgens de Nuts wijze
verstuurd en ontvangen te worden. Ook zullen deze, binnen het LSP, vertaald
moeten worden naar het LSP eigen formaat. Er is in dit project voorzien in de
ontwikkeling van een koppelpunt wat deze functies levert.

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

## Architectuur principes

Bij het uitwerken van de oplossing worden de volgende architectuur principes
gebruikt.

### Geen aanpassingen bij LSP deelnemer

Voor de aan LSP AORTA deelnemende zorgaanbieder en -leveranciers is het uitgangspunt
dat deze geen aanpassingen zullen gaan doen voor deze PoC. Dit betekent dat
aanpassingen, indien nodig, bij het LSP, Nuts of de Generieke Functies zullen
plaatsvinden.

## Openstaande punten en aannames

### Uitbreiding Dezi

Een van de vereisten bij het leveren van medischegegevens aan een zorgverlener
van een andere organisatie is informatie over die persoon. Dit gaat dan om zaken
als het BIG nummer en de rol code.

Om dit mogelijk te maken voor een uitwisseling tussen LSP en Nuts moet deze
informatie op een betrouwbare wijze gedeeld kunnen worden. Het plan is om
hiervoor een uitbreiding te realiseren in Dezi. De aanname is dat er een
oplossing is waar draagvlak voor is bij het CIBG. Ook gaat dit project er vanuit
dat er tijdens de PoC een stub (namaak) versie van Dezi gebruikt kan worden die
deze functie heeft.

### Veilig netwerk

Het LSP maakt gebruik van een gesloten netwerk voor communicatie met haar
deelnemers. Nuts gebruikt hiervoor het reguliere internet. De huidige opzet van
het per zorgaanbieder afsluiten van een contract om op het gesloten LSP is niet
gewenst in de VVT sector vanwege de additionele kosten die dit met zich
meebrengt. Voor nu wordt er daarom uitgegaan van een aansluiting van de VVT via
het publieke internet. Binnen VWS wordt in het kader van het Landelijk Dekkend
Netwerk (LDN) project gewerkt aan een zorg brede richtlijn. Binnen dit project
wordt de aanname gedaan dat de uitkomsten hiervan geen negatieve invloed op de
keuzes binnen het project hebben.

Voor de PoC wordt gewerkt met het publieke internet. De aanname is dat _voor_
aanvang van de pilot, buiten dit project om, besloten is hoe te voldoen aan de
eisen op een wijze die voor de LSP en Nuts gebruikers werkzaam is.

### Toestemming

Binnen dit project wordt uitgegaan van de registratie van toestemming bij de
Generieke Functie Toestemmingen op basis van een Online Toestemmingsvoorziening
(OTV). Dit is nodig omdat de lokalisatie index (NVI) anders in de pilot fase
niet kan bepalen of de toegang geautoriseerd kan worden. In de PoC fase kan
hiervoor een stub ingezet worden. Bij pilot zal hiervoor een aansluiting op Mitz
nodig zijn.

### Logging

Voor logging zijn in dit project geen extra eisen ten opzichte van de huidige
situatie.

### Standaarden voor wallets

Binnen dit project zal gewerkt worden met wallets voor het verwerken van
(toegangs)bewijzen (credentials). Voor de PoC zal gekozen worden voor danwel
bestaande oplossingen binnen Nuts, danwel oplossingen die voldoen aan de EIDAS
richtlijnen die nu in ontwikkeling zijn.

### Autorisatie richtlijnen

Binnen de VVT zijn veel zorgverleners werkzaam die geen BIG registratie hebben.
Bestaande richtlijnen die van toepassing zijn op de ontsluiting van medische
gegevens (zoals [die van de
KNMP](https://www.knmp.nl/richtlijnen/overdracht-van-medicatiegegevens-de-keten "https://www.knmp.nl/richtlijnen/overdracht-van-medicatiegegevens-de-keten"))
eisen echter dat zorgverleners die deze gegevens raadplegen BIG geregistreerd
zijn. Binnen dit project wordt daarom in de voorbeelden enkel met BIG
geregistreerde gebruikers gewerkt. De aanname is dat de technische oplossingen
in de toekomst op dezelfde wijze gebruikt kunnen worden indien het vraagstuk
rondom thuiszorg en andere VVT medewerkers is opgelost.

### Onderdelen uit het trust-over-ip model

Voor dit project zal enkel gewerkt worden aan de Technology aspecten (laag 1 tot
en met 4) uit het [trust-over-ip model](https://trustoverip.org/toip-model/ "https://trustoverip.org/toip-model/"). De Governance en Ecosystem onderdelen
worden buiten dit project opgepakt.

## Architectuur risico’s

### Juridisch

De uitwisseling van medische gegevens is gebonden aan wetten, richtlijnen en
andere juridische kaders. Het is mogelijk dat sommige functies of oplossingen
juridisch grondslagen missen. Daarom wordt parallel aan het realisatie-project
een juridische verkenning gedaan om eventuele problemen in kaart te brengen. Ook
kan hierdoor tijdig met aanpassingen in richtlijnen of andere juridische kaders
gestart worden.

## Advies en goedkeuring

Dit document wordt aangeboden voor akkoord aan het projectteam LSP x Nuts. In
het specifiek wordt gevraagd om een akkoord van:

- Opdrachtgever vanuit VWS
- Project leider VZVZ
- Project leider Nuts

Het advies is om, naast het PoC project, een juridisch traject op te starten om de
openstaande punten op juridisch vlak te adresseren.

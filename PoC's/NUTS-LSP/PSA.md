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

Het twee scenario heeft nog geen concrete casus. Hierbij zou mogelijk een van de
onderstaande opties gebruikt kunnen worden:

- Huisarts die inzage wil bij de VVT

- Avond, nacht en weekend zorg die bij de VVT informatie wil ophalen

> **Note:** Probleem hierbij is dat er (naar mijn weten) nog geen
kwaliteitsrichtlijnen (de juridische grondslag) noch informatiestandaarden voor
zijn (behalve het AMO).

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
  alleen met toestemming de verwijsindex *van* het LSP gevuld met
  lokalisatiemetadata

- In het geval de brosdossierhouder de toestemmingen in Mitz beheert wordt
  altijd het actualiteitenregister *in* het LSP met lokalisatiemetadata gevuld,
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

#### Vaststellen zorgaanbieder

#### Autorisatie

#### Lokalisatie

#### Adressering

#### Logging

#### Conversie van standaarden (FHIR)

## Eindsituatie van dit project

In de eindsituatie worden naast componenten uit het LSP en Nuts ook de Generieke
Functies ingezet.

#### I&A

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

#### Autorisatie

Bij de op het LSP aangesloten zorgaanbieder zal op de huidige wijze bepaald
worden of er expliciete toestemming verleend is.

#### Lokalisatie

In de eindsituatie zal er gewerkt worden volgens de Generieke Functie
lokalisatie. Hierbij wordt de NVI ingezet.

#### Adressering

Voor adressering zal in de eindsituatie gebruik gemaakt worden van de Generieke
Functie Adressering. Hierbij zal het LSP voor haar deelnemers een adresgegevens
bron beschikbaar maken. Voor de gebruikers van Nuts nodes zal de adresgegevens
bron ingebouwd worden in de Nuts node?

#### Logging

Het LSP logt op de wijze die het nu al doet bij aanvragen van VVT naar MSZ. Voor
vragen in de andere richting (MSZ naar VVT) zal ....

#### Conversie van standaarden (FHIR)

Het LSP draagt zorg voor de conversie van standaarden bij bevraging van VVT naar
MSZ. De Nuts omgeving kan gebruik maken van FHIR R3 voor het opvragen van
gegevens. De data vanuit de VVT is beschikbaar op endpoints die conform FHIR
Stu3 werken. Het LSP maakt, waar nodig, conversies naar andere standaarden voor
de op haar aangesloten zorgaanbieder.

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

... Credentials ...

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

> **Note:** Voordat VZVZ een productie pilot kan doen moet het vraagstuk rondom
het veilige netwerk opgelost zijn. Bij de PoC is het reguliere internet nog
acceptabel. Bij een pilot moet de oplossing voldoen aan de wetgeving. VZVZ stelt
dat dit betekent dat er eisen gesteld moeten worden aan de netwerkleverancier
(waaronder de NEN7512 eis dat de communicatie binnen de EER moet blijven). Met
publiek internet kan die garantie niet gegeven worden. Of LDN komt met een
alternatief voor GZN dat aan alle wet- en regelgeving voldoet of er is -ook voor
de pilot- een blokkade.

### Toestemming

Binnen dit project wordt uitgegaan van de registratie van toestemming bij de
Generieke Functie Toestemmingen op basis van een Online Toestemmingsvoorziening
(OTV). Dit is nodig omdat de lokalisatie index (NVI) anders in de pilot fase
niet kan bepalen of de toegang geautoriseerd kan worden. In de PoC fase kan
hiervoor een stub ingezet worden. Bij pilot zal hiervoor een aansluiting op Mitz
nodig zijn.

#### Logging?

### Standaarden voor wallets

Binnen dit project zal gewerkt worden met wallets voor het verwerken van
(toegangs)bewijzen (credentials). Voor de PoC zal gekozen worden voor danwel
bestaande oplossingen binnen Nuts, danwel oplossingen die voldoen aan de EIDAS
richtlijnen die nu in ontwikkeling zijn.

### Autorisatie richtlijnen

Binnen de VVT zijn veel zorgverleners werkzaam die geen BIG registratie hebben.
Bestaande richtlijnen die van toepassing zijn op de ontsluiting van medische
gegevens (zoals [die van de
KNMP](https://www.knmp.nl/richtlijnen/overdracht-van-medicatiegegevens-de-keten
"https://www.knmp.nl/richtlijnen/overdracht-van-medicatiegegevens-de-keten"))
eisen echter dat zorgverleners die deze gegevens raadplegen BIG geregistreerd
zijn. Binnen dit project wordt daarom in de voorbeelden enkel met BIG
geregistreerde gebruikers gewerkt. De aanname is dat de technische oplossingen
in de toekomst op dezelfde wijze gebruikt kunnen worden indien het vraagstuk
rondom thuiszorg en andere VVT medewerkers is opgelost.

### Onderdelen uit het trust-over-ip model

Voor dit project zal enkel gewerkt worden aan de Technology aspecten (laag 1 tot
en met 4) uit het [trust-over-ip model](https://trustoverip.org/toip-model/
"https://trustoverip.org/toip-model/"). De Governance en Ecosystem onderdelen
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

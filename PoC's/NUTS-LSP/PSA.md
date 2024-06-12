# PSA PoC NUTS-LSP
[beproeving medicatieoverdracht VVT-1e lijn (NUTS-LSP)]

# Voorwoord
Dit document beschrijft de visie en realisatiemogelijkheden van NUTS, TWIIN, VZVZ en VWS op de invoering van een medicatiegegevens-uitwisseling als onderdeel van het landelijk vertrouwensstelsel van waarborgen voor landelijke gegevensuitwisseling.
Het tijdig kunnen beschikken over actuele medicatiegegevens biedt meerwaarde voor zorgverlener en patiënt doordat er invulling kan worden gegeven aan hun informatiebehoefte. Op dit moment kunnen medicatiegegevens niet gestructureerd en structureel worden uitgewisseld tussen VVT en 1e lijn.
Dit document dient: 

- als praatstuk om ervoor te zorgen dat we in gezamenlijkheid de beste oplossing kunnen vaststellen en om te borgen dat de verschillende stakeholders allen dezelfde stip op de horizon voor ogen krijgen. 

- om te bepalen welke GAPS dan wel OVERLAPS er tussen huidige situatie en gewenste/toekomstige situatie zijn om te kunnen bepalen welke acties er nodig zijn om in lijn met de doelstellingen voor eind 2025 te komen. 

# Inleiding
## Begrippen
Stub:
 Een stub is een simulatieprogramma dat een programma/software vervangt, inclusief de bijbehorende in- en uitvoerstromen, en wordt aangeroepen door het te testen object.
 Het is niet de uiteindelijke software maar een simulatie daarvan om delen van de functionaliteit in complexere ketens (met ketenafhankelijkheden) te kunnen testen zonder dat     alle benodigde functies er al zijn. 
 Stubs emuleren een functie die wordt aangeroepen.
Driver:
  Een driver is een simulatieprogramma dat een programma/software vervangt, inclusief de bijbehorende in- en uitvoerstromen, en roept een te testen object aan.
  Het is niet de uiteindelijke software maar een simulatie daarvan om delen van de functionaliteit in complexere ketens (met ketenafhankelijkheden) te kunnen testen zonder dat     alle benodigde functies er al zijn. 
  Drivers emuleren een aanroep van een functie.
Verifieerbare verklaring:
 een  claim/statement of (meta)data die middels cryptografische technieken onweerlegbaar is gemaakt, waarvan cryptografisch te bewijzen is wie deze heeft uitgegeven en waarvan    de integriteit cryptografisch is gewaarborgd.
## Werkwijze
Tijdlijnen behoren niet in een PSA. We kiezen voor een agile aanpak. Beginnen met stubs/drivers (tijdelijke componenten die het gedrag nabootsen) die we langzaam gaan vervangen voor werkelijke functionaliteit zodat het een lerende uitrol wordt. Per stap maken we een nieuwe transitie architectuur. On de fly zullen we bepalen wat wel/niet stubben. We beginnen fullstub en gaan dan invullen met bestaande dan wel te ontwikkelen producten  die we in het window voor de beproeving kunnen invullen. 

## Aanleiding
De wens van VVT-instellingen en Actiz, eerste lijnszorgaanbieders, VZVZ, Nictiz en VWS om medicatiegegevens uit te kunnen wisselen. Deze PSA beschrijft welke wijzigingen nodig zijn om,  in het kader van de zorgtoepassing Medicatieoverdacht (MO), LSP en Nuts te verbinden , waarbij de implementatie van de generieke functies in lijn met het Landelijk Vertrouwens Stelsel (LVS) worden toegepast.

## Stakeholders
Stakeholders binnen dit project zijn: 
- Burgers (patiënten/cliënten)
- Zorgverleners
- Zorgaanbieders
- Koepels
- Leveranciers en beheerders
- NUTS
- VZVZ
- VWS:
-   Programma Landelijk Vertrouwensstelsel (LVS)
-   Programma Generieke Functies (GF)
-   Programma Landelijk Dekkend Netwerk (LDN)

## Doelstelling
Er is in een Proof of Concept omgeving met een koppeling tussen een aantal stub NUTS nodes (vertegenwoordigd per node één VVT-instelling) die medicatiegegevens kan uitwisselen met op de LSP-testomgeving aangesloten bronnen.  Hierin worden twee stappen onderscheiden: De eerste stap is één VVT-instelling (stub) die AMO gegevens kan opvragen van één BSN bij meerdere LSP testbronnen. De tweede stap is één LSP-testbron die AMO gegevens van één BSN kan ophalen bij meerdere VVT-instellingen (stubs). De tweede stap zal vermoedelijk de meeste PoC ervaring opleveren. Daarbij worden ook de uitgangspunten/inrichtingskeuzes van de generieke functies meegenomen. De opgedane lessen en ervaringen worden bij het programma Twiin ingebracht in een volgende versie van het afsprakenstelsel. De afspraken zullen opgelijnd zijn met het groeipad naar het gebruik van de Generieke Functies volgens het LVS. De  PoC is onderdeel van het concept ‘vooraf vastleggen, meegeven en toetsen van verifieerbare verklaringen voor landelijke medische gegevensuitwisseling’ . 

## Lijst met aannamen
Top down ideeën waarvan we vermoeden dat deze gaan werken, maar waarvan we niet 100% zeker (kunnen) zijn, beproeven we bottom-up , om te zien of die aannames correct zijn. De combinatie van deze top-down en bottom-up aanpak leert ons wat hierna te doen. (We weten hoe we kroketten en bitterballen moeten maken maar we weten niet precies hoe heet de olie moet zijn, hoe dik de paneermeellaag en hoe de samenstelling van die paneermeellaag moet zijn.) We zijn er van overtuigd dat we jaren kunnen praten en  documenten kunnen schrijven, maar dat het sneller is om het gewoon te doen, te beproeven en iteratief verbeteringen aan te brengen. Hierom gaan we uit van de volgende aannamen:

### 0. Trust over IP (ToIP)
Het vertrouwen willen we inrichten volgens het ToIP-framework met de daarbij behorende lagen. Hierom doen we aannames over de verwachtte ontwikkelingen op elk van deze 4 lagen, in ogenschouw nemende de ontwikkeling van generieke functies, zodat we bepaalde keuzes op basis van die aannames kunnen gaan maken.

### 1. I&A: attributen. 

We gaan ervan uit dat voor het vullen en beschikbaarstellen van identificatie attributen in een wallet het OpenID4VC protocol wordt gebruikt. Deze aanname loopt vooruit op de nog vrij te geven guidelines eIDAS 2.0 (verwachte Q4 2024). Ook al is de eIDAS 2 verordening met bijbehorende ARF nog niet 100% klaar, we weten op welke ontwikkelingen de wetgeving gebaseerd is. In die zin volgt de wet de ontwikkelingen op wetenschappelijk en praktisch niveau te weten een volgbare evolutie naar attribute bases authentication en cryptografisch verifieerbare attesten. De evolutie in bijbehorende standaarden is, (van oud naar nieuw): SAML, OAuth, OIDC, OpenID4VC. 1.	Aanname  is dat er eind 2025 ook vanuit eIDAS2/NEN7518/WDO/DIAZ al formele processen en procedures komen (QEAA) over o.a. uitrol van attributen. Een andere aanname is dat er taakcodes (of rolcodes??) komen voor de taken die zorgverleners moeten kunnen uitvoeren (zoals toediener) zonder dat ze direct (bv via diploma) door het CIBG kunnen worden gekwalificeerd. Deze taakcodes zijn, naast de rolcodes, nodig om te kunnen autoriseren.
In het kort zijn de aannamen:

    -We verwachten dat vanuit de de Europese ontwikkeling mbt authenticatiemiddelen de OpenID4VC standaarden gebruikt gaan worden. DIT IS NOG PREMATUUR GEGEVEN DE DISCUSSIE BINNEN I&A. MOETEN WE NIET TRANSLEREN VAN LSP NAAR NUTS, DE STANDAARD DIE WE VANUIT I&A WEL ONDERSTEUNEN EN DE GAP DAARIN DAN BENOEMEN?
    -We verwachten dat er diverse attributen beschikbaar komen voor zorgverleners zoals bijvoorbeeld taakcodes (of rolcodes) en een zorgidentiteit
    -We verwachten dat er eind 2025 vanuit eIDAS2/NEN7518/WDO/DIAZ al formele processen en procedures komen (QEAA) over o.a. uitgifte van deze attributen


### 2. Autorisatie
We gaan er vanuit dat de dossierhouder op raadplegende zorgaanbiedertype (is dit in lijn met de autorisatierichtlijn die nu wordt gehanteerd in de eerste lijn én de VVT? Met andere woorden, is er met de VVT al overleg over dit paradigma van autoriseren?) autoriseert en verantwoordelijk is voor Attribute-based access control (ABAC) waarvoor de hard af te dwingen regels/policies in de nieuwe governance zijn vastgesteld en dat de raadpleger de Role Based Access Control (RBAC) verantwoordelijkheid krijgt gebaseerd op een in de nieuwe governance vastgestelde 'pas-toe-of-leg-uit' autorisatieregels. Deze regels zijn aan de raadplegende kant dus minder hard omdat er aan de raadplegende kant van kan worden afgeweken op een voor een auditor uitlegbare manier. Deze aanname loopt vooruit op dialoog in de werkgroep de NEN-7520 norm autorisatie. Redenen hiervoor zijn: complexiteitsreductie, het nu niet herbruikbaar zijn van al uitgegeven autorisaties in andere context en de ontwikkelingen naar het uitwisselen in kleinere datasets (bouwstenen of zelfs data-elementen: databeschikbaarheid). Zorginstellingen kunnen dan op type geautoriseerd worden, mits deze aan allerlei kwaliteitscriteria voldoen (b.v. aantoonbaar NEN-7510 certificaat), andere partijen, die nog niet zo volwassen zijn zullen dan nog op formele zorgverlenerollen geautoriseerd moeten wordenomdat ze in dit geval geen NEN-7510 verklaring kunnen overleggen.

### 3. Twiin/Nuts
We willen landelijk één plek hebben waar we de geleerde lessen kunnen vastleggen in valideerbare (kwalificeren/accepteren) afspraken. Het Twiin afsprakenstelsel is in het leven geroepen om overbruggende afspraken tussen vertrouwensdomeinen te maken op relatief korte termijn (IZA). De huidige versie van het afsprakenstelsel beschrijft werkende oplossingen, ook voor generieke functies en de geleerde lessen zullen daarom in een toekomstige versie van het afsprakenstelsel moeten landen waarin ook de generieke functies van VWS een plaats krijgen. Het is daarom handig om naast een huidige versie van het afsprakenstelsel parallel ook te werken aan een afsprakenstelsel voor de toekomst, waarin op basis van aannames alvast gewerkt wordt aan nieuwe afspraken en specificaties. Dit geldt naast Twiin ook voor het 'afsprakenstelsel' Nuts waar de afspraken in moeten worden opgenomen. 

### 4. Wallets
Ook authenticatie met attributen vanuit wallets moet voldoen aan de NEN-7512.
Aanname: Hierbij dienen Nuts en LSP te vertrouwen op het afsprakenstelsel rondom en gebruik van de digitale wallets.

## Beoogde resultaten

1.	Een volgende meer volwassen versie van deze 'high-level-doelarchitectuur NUTS-LSP' voor  de lange termijn met daarin o.a. plaats voor generieke functies en een te realiseren transitie-architectuur bestaande uit een afsprakenstelsel en Technical Agreement (ondersteund door een Solution Architectural Description (SAD)) voor het verbinden van LSP en NUTS, die invulling geven aan de (functionele) communicatie patronen waarmee (minimaal) de use cases vanuit Medicatieoverdracht gedekt worden.  [Gericht bevragen, ongericht bevragen, verzenden]
2.	Een herbruikbare open source GtK referentie-implementatie 
3.	Concept Enrollment proces dat gebruikt kan worden voor de uitrol van attributen in wallets op het juiste betrouwbaarheidsniveau. 
4.	Conceptafspraken over gebruik taakcodes  (naast rolcode) voor taak: toediener (VVT). Taakcodes zijn essentieel voor de VVT sector, omdat er veel zorgprofessionals werken die niet kunnen worden gekwalificeerd door het CIBG (want geen diploma of opname in register)(er wordt nog gewerkt aan rolcodes voor niet BIG-geregistreerden, daarmee kun je ook de aanname doen dat die rolcodes er gaan komen en daarmee de PoC alleen op rolcode uitvoeren ipv ook met taakcodes, check). Uitgangspunt is dat de koepels met een autorisatierichtlijn komen. Contact met programma MO om te kijken of de conceptafspraken die we maken op basis van de opgedane ervaring kunnen laten landen in de MO governance. Dit kan het programma Generieke Functies verzorgen.
5.	Een technische PoC zonder daadwerkelijke patiëntgegevens die opschaalbaar is naar pilot (met werkelijke patiëntgegevens).

## Het gaat om een beproeving met:
1.	Authenticatie:
Drie wallet apps. Twee  zorgverlener wallets en een zorgaanbieder XIS-wallet (wallet-agent). Hiermee kunnen we aantonen dat het niet uitmaakt welke wallet app een ZV kiest. Het gaat hierbij om het idee dat  meerdere wallet apps naast en door elkaar gebruikt kunnen worden (Dit staat nog los vd eisen die eraan moeten worden gesteld). 
2.	Autorisaties:  
a. Die uitgegeven worden voor role-based accesscontrol aan de raadplegende kant door de opvragende zorgaanbieder aan zijn zorgprofessionals op basis van een landelijke RBAC autorisatieafspraak.  
b.	Die dossierhouders in staat stellen gegevens vrij te geven obv een OAuth/OpenID4VP service die accestokens kan uitgeven op basis van landelijke autorisatieafspraak waarbinnen een dossierhouder gegevens vrijgeeft op basis van ABAC (waaronder een attribuut met raadplegende zorgaanbiedertype).    
3.	Toestemmingen:
Een Toestemmingsvoorziening (TV) met een PDP (policy decision point) in de OTV danwel in het knooppunt waar toestemmingen kunnen worden gechecked: We gebruiken hiervoor de gesloten autorisatievraag van Mitz. Op een testomgeving van Mitz?
4.	Lokalisatie:
Een Lokalisatieregister (LMR) waar obv de toestemming de organisatie-id’s kunnen worden opgehaald van (relevante) dossierhouders voor AMO. We gebruiken hiervoor de open autorisatievraag van Mitz. Dit zien wij liever niet zo uitgewerkt, maar op basis van een LMR voor AMO. We kunnen daarvoor wellicht de LMR benaderen die bij iRealisatie wordt opgezet voor Beeldbeschikbaarheid die we dan moeten vullen met AMO-gegevens. 
5.	Adressering:
Een AdresBoek (AB) waar de relevante technische endpoint van de organisatie-id’s kunnen worden opgevraagd. Dit zal een –bij gebrek aan een landelijk afsprakenstelsel over adressering- een combinatie/integratie zijn van het Nuts-register aan de ene kant en ZORG-AB/LSP-applicatieregister aan de andere kant. Deze snap ik, even kijken wanneer de PSA daadwerkelijk wordt goedgekeurd en hoe het er dan uitziet. 
6.	Transformatie  
a. Een seurity token service (STS) die tokens kan omzetten (JWT in SAML).   
b. Een transformatieservice (TS) die HL7v3 kan omzetten in HL7 FHIR.

## Scope
### Buiten scope
Omschrijf hier expliciet wat er buiten de scope van het traject valt. Het gaat hier om die zaken waar twijfel over zou kunnen bestaan.
-	Polymorfe Pseudoniemen
-	Eisen aan wallet app's
-	Eisen aan authenticatiemiddelen om wallets te ontsluiten
-	Autorisatieregels medicatieoverdracht. Autorisatie op basis van rol zoals de huidige autorisatierichtlijn voor medicatieoverdracht is opgesteld en vastgesteld door de koepels.   Voor de niet-BIG-geregistreerde zorgprofessionals in de VVT-sector dient dan een oplossing gevonden te worden (zie taakcode in paragraaf over I&A attributen). Zie eerdere opmerking over rolcodes voor niet BIG-geregistreerden. 


## Samenhang/afhankelijkheden met andere trajecten
Project toekomstbestendig maken UZI. Het project toekomst bestendig maken UZI beproeft het plaatsen van UZI-attributen in wallet apps. Deze functionaliteit is nodig voor de POC die we willen uitvoeren.
MP9
Programma's GF, LVS en LDN


# IST
De uitdaging is dat er nu twee vertrouwensdomeinen zijn (NUTS en LSP) die beide andere keuzes hebben gemaakt ten aanzien van de juridische, organisatorische en technische onderwerpen uit het  vertrouwensmodel. Vanwege deze in het verleden gemaakte keuzes is er geen interoperabele gegevensuitwisseling mogelijk tussen zorgaanbieders aangesloten op het LSP en zorgaanbieders aangesloten op NUTS.  Op dit moment zijn er per onderwerp uit het vertrouwensmodel verschillende afspraken gemaakt wat heeft geresulteerd in het ontstaan van twee verschillende vertrouwensdomeinen.

![image](https://github.com/minvws/generiekefuncties-beproevingen/assets/123090714/48348ce2-6cff-46a4-a56d-44d63f60d1d3)

Zowel NUTS als het Landelijk Schakelpunt (LSP) vereisen onweerlegbaarheid bij de uitwisseling van medische gegevens. Hiervoor worden verifieerbare verklaringen (SAML tokens bestaande uit SAML-assertions bij LSP, Verifiable Presentations met daarin Verifiable credentials. VP(VCs) bij NUTS), meegestuurd tijdens de uitwisseling die de betrouwbaarheid van de uitwisseling op het hoogste niveau waarborgen. De LSP tokens (verifieerbare verklaringen) zijn conform de SAML standaard gemaakt en de NUTS VC’s zijn conform de gelijknamige W3C standaard gemaakt. Tevens ’ gebruiken bronsystemen die zijn aangesloten op het LSP (nu nog) vooral HL7v3 als communciatieprotocol, terwijl bronsystemen in de VVT sector gebruik maakt van het HL7 FHIR (versie RX) communicatieprotocol. Medicatieoverdracht wordt o.b.v. HL7 FHIR ontwikkeld. Het LSP en (initieel een beperkt aantal) XIS’en zullen dit ook gaan ondersteunen. Is de PoC voor 2 richtingen, dus van eerste lijn naar VVT, maar ook andersom?

## Probleemomschrijving
In de huidige situatie kunnen geen medicatiegegevens worden opgevraagd/uitgewisseld tussen VVT-instellingen, huisartsen en apothekers. Dit komt omdat de vertrouwensmodellen van de onderliggende “infrastructuren” van elkaar verschillen en andere tokens gebruiken om de onweerlegbaarheid aan de andere partij aan te tonen. Een ander knelpunt is dat  op dit moment de generieke functies onvoldoende zijn ingericht of niet beschikbaar zijn, waardoor de werking van deze generieke functies nog niet in de processen  en programmatuur van de uitwisselingen is vorm  gegeven. Om uitwisseling van gegevens in plateau 1 van de NVS, eind 2025, voor medicatiegegevens mogelijk te maken moeten  deze generieke functies in het proces worden opgenomen.


# Visie op NUTS-LSP uitwisselingen

![image](https://github.com/minvws/generiekefuncties-beproevingen/assets/123090714/95c3df45-07f9-4e3b-a217-9aef161a859e)
Doelarchitectuur NUTS-LSP uitwisselingen

# SOLL

## Gewenste situatie op conceptueel niveau

Een belangrijke wens is om Generieke functies zoveel als mogelijk te beproeven gebruik makend van oplossingen die reeds beschikbaar zijn, bijvoorbeeld op het gebied van Identificatie en Authenticatie, danwel componenten uit de doelarchitectuur te stubben.

De gewenste situatie voor de beproeving:

![image](https://github.com/minvws/generiekefuncties-beproevingen/assets/123090714/783c11af-4cf4-4904-85bd-1ea528ebc48b)

![image](https://github.com/minvws/generiekefuncties-beproevingen/assets/123090714/0b730a16-6cbb-4c3b-9fbb-ffaaa9f4bc94)

## Afwijkingen t.o.v. de doelarchitectuur LSP-NUTS
Polymorfe Pseudonimisering wordt niet beproeft. Tevens zijn er nu nog geen NEN-7518 gecertificeerde middelen die we kunnen beproeven maar we kiezen voor MS authenticator en ZORG-ID smart als twee wallets om mee te beproeven.  

## Afwijkingen t.o.v. het AORTA vertrouwensmodel
-De authenticatie van zorgverleners zal niet altijd meer plaatsvinden op basis van de huidige UZI-pas (zie ontwikkelingen tav DEZI). 
-De authenticatie van zorgaanbieders zal niet altijd meer plaatsvinden op basis van het UZI-servercertificaat (Zie ontwikkelingen tav DEZI). 
	Deelnemers aan het AORTA-afsprakenstelsel dienen akkoord te zijn met de wijzigingen van het AORTA-vertrouwensmodel. Uitgangspunt hierbij is dat het vertrouwensniveau/            betrouwbaarheidsniveau hetzelfde blijft.

## Lijst met stubs/drivers/bestaande software
to do

# referentie:
Trust over IP framework:
https://trustoverip.org/

OpenID for Verifiable Presentations:
https://openid.net/specs/openid-4-verifiable-presentations-1_0.html

OpenID for Verifiable Credential Issuance:
https://openid.net/specs/openid-4-verifiable-credential-issuance-1_0.html


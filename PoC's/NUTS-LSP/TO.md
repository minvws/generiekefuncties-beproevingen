
### 0. Trust over IP (ToIP)
Het vertrouwen willen we inrichten volgens het ToIP-framework met de daarbij behorende lagen. Hierom doen we aannames over de verwachtte ontwikkelingen op elk van deze 4 lagen, in ogenschouw nemende de ontwikkeling van generieke functies, zodat we bepaalde keuzes op basis van die aannames kunnen gaan maken.

### 1. I&A: attributen. 

We gaan ervan uit dat voor het vullen en beschikbaarstellen van identificatie attributen in een wallet het OpenID4VC protocol wordt gebruikt. Deze aanname loopt vooruit op de nog vrij te geven guidelines eIDAS 2.0 (verwachte Q4 2024). Ook al is de eIDAS 2 verordening met bijbehorende ARF nog niet 100% klaar, we weten op welke ontwikkelingen de wetgeving gebaseerd is. In die zin volgt de wet de ontwikkelingen op wetenschappelijk en praktisch niveau te weten een volgbare evolutie naar attribute bases authentication en cryptografisch verifieerbare attesten. De evolutie in bijbehorende standaarden is, (van oud naar nieuw): SAML, OAuth, OIDC, OpenID4VC. Aanname is dat er eind 2024 ook vanuit eIDAS2/NEN7518/WDO/DIAZ al formele processen, procedures en technische standaarden komen over o.a. uitrol van attributen (QEAA). Een andere aanname is dat er taakcodes (of rolcodes??) komen voor de taken die zorgverleners moeten kunnen uitvoeren (zoals toediener) zonder dat ze direct (bv via diploma) door het CIBG kunnen worden gekwalificeerd. Deze taakcodes zijn, naast de rolcodes, nodig om te kunnen autoriseren.
In het kort zijn de aannamen:

    -We sluiten op termijn aan op de standaarden die vanuit de de Europese ontwikkeling mbt authenticatiemiddelen gebruikt gaan worden en tot die tijd maken we gebruik van de standaarden die vanuit het DEZI-stelsel worden aangeboden. 
    -We verwachten dat er diverse attributen beschikbaar komen voor zorgverleners zoals bijvoorbeeld taakcodes (of rolcodes) en een zorgidentiteit
    -We verwachten dat er eind 2025 vanuit eIDAS2/NEN7518/WDO/DIAZ al formele processen en procedures komen (QEAA) over o.a. uitgifte van deze attributen


### 2. Autorisatie
We gaan er vanuit dat de dossierhouder op raadplegende zorgaanbiedertype en rolcode (fictief voor rol 'toediener') autoriseert en verantwoordelijk is voor Attribute-based access control (ABAC) waarvoor de hard af te dwingen regels/policies in de nieuwe governance zijn vastgesteld en dat de raadpleger de Role Based Access Control (RBAC) verantwoordelijkheid krijgt gebaseerd op een in de nieuwe governance vastgestelde 'pas-toe-of-leg-uit' autorisatieregels. Deze regels zijn aan de raadplegende kant dus minder hard omdat er aan de raadplegende kant van kan worden afgeweken op een voor een auditor uitlegbare manier. Deze aanname loopt vooruit op dialoog in de werkgroep de NEN-7520 norm autorisatie. Redenen hiervoor zijn: complexiteitsreductie, het nu niet herbruikbaar zijn van al uitgegeven autorisaties in andere context en de ontwikkelingen naar het uitwisselen in kleinere datasets (bouwstenen of zelfs data-elementen: databeschikbaarheid). Zorginstellingen kunnen dan op type geautoriseerd worden, mits deze aan allerlei kwaliteitscriteria voldoen (b.v. aantoonbaar NEN-7510 certificaat), andere partijen, die nog niet zo volwassen zijn zullen dan nog op formele zorgverlenerollen geautoriseerd moeten worden omdat ze in dit geval geen NEN-7510 verklaring kunnen overleggen.

### 3. Twiin/Nuts
We willen landelijk één plek hebben waar we de geleerde lessen kunnen vastleggen in valideerbare (kwalificeren/accepteren) afspraken. Het Twiin afsprakenstelsel is in het leven geroepen om overbruggende afspraken tussen vertrouwensdomeinen te maken op relatief korte termijn (IZA). De huidige versie van het afsprakenstelsel beschrijft werkende oplossingen, ook voor generieke functies en de geleerde lessen zullen daarom in een toekomstige versie van het afsprakenstelsel moeten landen waarin ook de generieke functies van VWS een plaats krijgen. Het is daarom handig om naast een huidige versie van het afsprakenstelsel parallel ook te werken aan een afsprakenstelsel voor de toekomst, waarin op basis van aannames alvast gewerkt wordt aan nieuwe afspraken en specificaties. Vanuit Twiin zullen de gemaakte afspraken moeten voortvloeien naar het 'afsprakenstelsel' AORTA en Nuts waar de afspraken in moeten worden opgenomen. 

### 4. Wallets
Ook authenticatie met attributen vanuit wallets moet voldoen aan de NEN-7512.
Aanname: Hierbij dienen Nuts en LSP te vertrouwen op het afsprakenstelsel rondom het gebruik van de digitale wallets.


## Het gaat om een beproeving met:
1.	Authenticatie:
Drie wallet apps. Twee  zorgverlener wallets en een zorgaanbieder XIS-wallet (wallet-agent). Hiermee kunnen we aantonen dat het niet uitmaakt welke wallet app een ZV kiest. Het gaat hierbij om het idee dat  meerdere wallet apps naast en door elkaar gebruikt kunnen worden (Dit staat nog los vd eisen die eraan moeten worden gesteld). 
2.	Autorisaties:  
a. Die uitgegeven worden voor role-based accesscontrol aan de raadplegende kant door de opvragende zorgaanbieder aan zijn zorgprofessionals op basis van een landelijke RBAC autorisatieafspraak.  
b.	Die dossierhouders in staat stellen gegevens vrij te geven obv een OAuth/OpenID4VP service die accestokens kan uitgeven op basis van landelijke autorisatieafspraak waarbinnen een dossierhouder gegevens vrijgeeft op basis van ABAC (waaronder een attribuut met raadplegende zorgaanbiedertype).    
3.	Toestemmingen:
Een Toestemmingsvoorziening (TV) met een PDP (policy decision point) in de OTV danwel in het knooppunt waar toestemmingen kunnen worden gechecked: We gebruiken hiervoor de gesloten autorisatievraag van Mitz (in een testomgeving).
4.	Lokalisatie:
Een Lokalisatieregister (LMR) waar obv de toestemming de organisatie-id’s kunnen worden opgehaald van (relevante) dossierhouders voor AMO. 
5.	Adressering:
Een AdresBoek (AB) waar de relevante technische endpoint van de organisatie-id’s kunnen worden opgevraagd. Dit zal een –bij gebrek aan een landelijk afsprakenstelsel over adressering- een combinatie/integratie zijn van het Nuts-register aan de ene kant en ZORG-AB/LSP-applicatieregister aan de andere kant.
6.	Transformatie  
a. Een seurity token service (STS) die tokens kan omzetten (JWT in SAML).   
b. Een transformatieservice (TS) die HL7v3 kan omzetten in HL7 FHIR.




# IST
De uitdaging is dat er nu twee vertrouwensdomeinen zijn (NUTS en LSP) die beide andere keuzes hebben gemaakt ten aanzien van de juridische, organisatorische en technische onderwerpen uit het  vertrouwensmodel. Vanwege deze in het verleden gemaakte keuzes is er geen interoperabele gegevensuitwisseling mogelijk tussen zorgaanbieders aangesloten op het LSP en zorgaanbieders aangesloten op NUTS.  Op dit moment zijn er per onderwerp uit het vertrouwensmodel verschillende afspraken gemaakt wat heeft geresulteerd in het ontstaan van twee verschillende vertrouwensdomeinen.

![image](https://github.com/minvws/generiekefuncties-beproevingen/assets/123090714/48348ce2-6cff-46a4-a56d-44d63f60d1d3)

Zowel NUTS als het Landelijk Schakelpunt (LSP) vereisen onweerlegbaarheid bij de uitwisseling van medische gegevens. Hiervoor worden verifieerbare verklaringen (SAML tokens bestaande uit SAML-assertions bij LSP, Verifiable Presentations met daarin Verifiable credentials. VP(VCs) bij NUTS), meegestuurd tijdens de uitwisseling die de betrouwbaarheid van de uitwisseling op het hoogste niveau waarborgen. De LSP tokens (verifieerbare verklaringen) zijn conform de SAML standaard gemaakt en de NUTS VC’s zijn conform de gelijknamige W3C standaard gemaakt. Tevens ’ gebruiken bronsystemen die zijn aangesloten op het LSP (nu nog) vooral HL7v3 als communciatieprotocol, terwijl bronsystemen in de VVT sector gebruik maakt van het HL7 FHIR (versie RX) communicatieprotocol. Medicatieoverdracht wordt o.b.v. HL7 FHIR ontwikkeld. Het LSP en (initieel een beperkt aantal) XIS’en zullen dit ook gaan ondersteunen. In de eersre fase is de PoC voor één richting, dus datastroom van eerste lijn naar VVT en in tweede fase ook andersom.
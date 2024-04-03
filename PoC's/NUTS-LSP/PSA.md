# PSA PoC NUTS-LSP
[beproeving medicatieoverdracht VVT-1e lijn (NUTS-LSP)]

# Voorwoord
Dit document beschrijft de visie en realisatiemogelijkheden van NUTS, TWIIN, VZVZ en VWS op de invoering van een medicatiegegevens uitwisseling als onderdeel van het landelijk vertrouwensstelsel van waarborgen voor landelijke gegevensuitwisseling.
Het tijdig kunnen beschikken over actuele medicatiegegevens biedt meerwaarde voor zorgverlener en patiënt doordat er invulling kan worden gegeven aan hun informatiebehoefte. Op dit moment kunnen medicatiegegevens niet gestructureerd en structureel worden uitgewisseld tussen VVT en 1e lijn.
Dit document dient: 

- als praatstuk om ervoor te zorgen dat we in gezamenlijkheid de beste oplossing kunnen vaststellen en om te borgen dat de verschillende stakeholders allen dezelfde stip op de horizon voor ogen krijgen. 

- om te bepalen welke GAPS dan wel OVERLAPS er tussen huidige situatie en gewenste/toekomstige situatie zijn om te kunnen bepalen welke acties er nodig zijn om in lijn met de doelstellingen voor eind 2025 te komen. 

# Inleiding

## Aanleiding
2.1	Aanleiding
De wens van VVT-instellingen en Actiz, eerste lijnszorgaanbieders en VZVZ om medicatiegegevens uit te kunnen wisselen. Deze PSA beschrijft welke wijzigingen nodig zijn om,  in het kader van de zorgtoepassing Medicatieoverdacht (MO), LSP en Nuts te verbinden , waarbij de implementatie van de generieke functies in lijn met het Landelijk Vertrouwens Stelsel (LVS) worden toegepast.

## Stakeholders
•	Stakeholders binnen dit project zijn: 
•	Burgers (patiënten/cliënten)
•	Zorgverleners
•	Zorgaanbieders
•	Koepels
•	Leveranciers en beheerders
•	NUTS
•	VZVZ
•	VWS
•	?

## Doelstelling
Er is in een Proof of Concept omgeving met een koppeling tussen een aantal stub NUTS nodes (vertegenwoordigd per node één VVT-instelling) die medicatiegegevens kan uitwisselen met op de LSP-testomgeving aangesloten bronnen.  Hierin worden twee stappen onderscheiden: De eerste stap is één VVT instelling (stub) die AMO gegevens kan opvragen van één BSN bij meerdere LSP testbronnen. De tweede stap is één LSP-testbron die AMO gegevens van één BSN kan ophalen bij meerdere VVT-instellingen (stubs). De tweede stap zal vermoedelijk de meeste PoC ervaring opleveren. De opgedane lessen en ervaringen worden bij het programma Twiin ingebracht en in een volgende versie van het afsprakenstelsel. . De afspraken zullen opgelijnd zijn met het groeipad naar het gebruik van de Generieke Functies volgens het LVS.

## Lijst met aannamen
Top down ideeën waarvan we vermoeden dat deze gaan werken, maar waarvan we niet 100% zeker (kunnen) zijn, beproeven we bottom-up , om te zien of die aannames correct zijn. De combinatie van deze top-down en bottom-up aanpak leert ons wat hierna te doen. (We weten hoe we kroketten en bitterballen moeten maken maar we weten niet precies hoe heet de olie moet zijn, hoe dik de paneermeellaag en hoe de samenstelling van die paneermeellaag moet zijn.) We zijn er van overtuigd dat we jaren kunnen praten en  documenten kunnen schrijven, maar dat het sneller is om het gewoon te doen, te beproeven en iteratief verbeteringen aan te brengen. Hierom gaan we uit van de volgende aannamen:

### 1. I&A: attributen. 

Alternatief: We gaan ervan uit dat voor het vullen en beschikbaarstellen van identificatie attributen in een wallet het OpenID4VC protolcol wordt gebruiky. Dez aanname loopt vooruit op de nog vrij te geven guidelines eIDAS 2.0 (verwachte Q4 2024). Ook al is de eIDAS 2 verordening met bijbehorende ARF   nog niet 100% klaar, we weten op welke ontwikkelingen de wetgeving gebaseerd is. In die zin volgt de wet de ontwikkelingen op wetenschappelijk en praktisch niveau te weten een volgbare evolutie naar attribute bases authentication en verifierbare crypto. De evolutie in bijbehorende standaarden is, (van oud naar nieuw): SAML, OAuth, OIDC, OIDC VC.



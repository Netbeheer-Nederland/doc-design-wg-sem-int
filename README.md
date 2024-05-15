# Inleiding

Dit document leunt sterk op doelen en principes beschreven in de *Visie* en
*Doelarchitectuur* Datadelen. Affiniteit aan de kant van de lezer met deze
documenten is niet vereist, maar geeft meer diepgang aan de voorgestelde
oplossing.

# Executive summary

> TODO

# Achtergrond

Vanuit de CDO's van de netbeheerders van elektriciteitsnetten binnen Nederland
is in 2023 een nieuwe visie rondom datadelen gepubliceerd. Deze visie
beschrijft de ambitie om gestandaardiseerd en herbruikbaar data te delen vanuit
netbeheerders met derden zoals overheden, marktpartijen en klanten.

Ook vanuit Europa staat deze ambitie hoog op de agenda. Momenteel werkt de *EU
DSO Entity* samen met de ENTSO-E aan meerdere *Implementing Regulations* rondom
*demand/response*, *smart metering* en *supplier switching*. Al deze IRs delen
een focus op data en de bijbehorende standaardisatie van het beschrijven en
uitwisselen van deze data.

Om de visie vorm te geven en te implementeren is in 2023 een transitiethema
gestart vanuit Netbeheer Nederland, van waaruit de Werkgroepen
*Doelarchitectuur Datadelen* en *Semantische Interoperabiliteit* invulling
(hebben) gegeven aan de visie. De doelarchitectuur beschrijft een manier van
(meta)data beschrijven en uitwisselen vanuit het perspectief van
*waardestromen* en *dataproducten*, daar waar *Semantische Interoperabiliteit*
zich bezig houdt met de betekenis van data.

## Dataproduct

Voor alle data-uitwisseling binnen de scope van de visie is expliciet gekozen
voor het denken over data in de vorm van dataproducten. Een dataproduct
combineert de semantische-, technische- en gebruiksaspecten van
data-uitwisseling. Om dit invulling te geven bestaat een dataproduct uit de
volgende componenten:

* dataset: de daadwerkelijke gegevens die worden uitgewisseld. Zie een dataset
  als een tabel met gegevens, waarbij de kolommen beschrijven wat er op elke
  rij van de data aan gegevens wordt geleverd. Een dataset voor aansluitingen
  zal minstens een kolom "Aansluitingsnummer" bevatten, waarbij elke rij in de
  dataset een aansluiting beschrijft;
* dataservice: de technische manier van verspreiden van de dataset. Dit gaat
  over hoe de data ontsloten wordt (als bestand, API, database of *file
  server*);
* dataproduct: het combineren van de *dataset* en *dataservice*, verrijkt met
  voorwaarden voor gebruik. Er kunnen o.a. voorwaarden liggen op
  beschikbaarheid, kwaliteit, classificatie en doelbinding bij gebruik van het
  dataproduct.

## Betekenis en structuur

Eén van de grootste vragen rondom data-uitwisseling gaat over de betekenis en
structuur van een dataproduct. Dit vraagstuk raakt primair de *dataset*, waar
elke kolom een specifieke betekenis geeft aan de bijbehorende rijen. De
structuur bepaalt welke kolommen er worden geleverd als onderdeel van de
dataset. Elke informatievraag die één of meerdere dataproducten vereist om op
te lossen moet invulling aan de dataset geven. Ter illustratie: alle lopende
*epics* binnen de pRO voor het maken van dataproducten vereisen een antwoord op
de vraag "welke data is nodig voor mijn dataproduct?"

Gaat het om kleine, op zichzelf staande dataproducten is het bepalen van de
structuur van de dataset triviaal. Gaat het echter om complexere onderwerpen
zoals het modelleren van nettopologie of het modelleren van de functie van het
elektriciteitsnet voor netrekenen wordt de structuur eveneens complexer. Bij
inachtname van de eis op herbruikbaarheid bovenop de voorgenoemde complexiteit
is er behoefte aan een gestandaardiseerde aanpak rondom het beschrijven en
uitwisselen van dataproducten. Dit versnelt de implementatie van nieuwe
dataproducten doordat bestaande dataproducten hergebruikt worden.

De oplossing voor hergebruik en versnelling van de implementatie van
dataproducten is het inzetten van referentiestandaarden. Wereldwijd is de
geldende referentiestandaard voor het vastleggen van de betekenis en structuur
van componenten van een elektriciteitsnet de *IEC/ISO 61970*-standaard, ofwel
het *Common Information Model*: CIM.

## Common Information Model (CIM)

Het Common Information Model (CIM) is een internationaal gestandaardiseerd
model voor gegevensuitwisseling in de energiesector, ontwikkeld door de
*International Electrotechnical Commission* (IEC). Het CIM faciliteert de
interoperabiliteit tussen verschillende energiesystemen en -applicaties door
een uniforme *datataal* te bieden, wat essentieel is voor het efficiënt beheren
van complexe netwerkoperaties en het ondersteunen van de integratie van
hernieuwbare energiebronnen en nieuwe technologieën zoals *smart grids*.

Het CIM bestaat uit drie standaarden met verschillende doelen, die de betekenis
van componenten van het elektriciteitsnet beschrijven:

![Common Information Model](assets/common_information_model-20240514.png)

* *IEC 61970*: bedrijfsvoering & netontwerp;
* *IEC 61968*: ondersteuning van de bedrijfsvoering, zoals asset- en
  werkordermanagement;
* *IEC 62325*: marktfacilitering voor US- en EU-energiemarkten.

Deze drie standaarden beschrijven *klassen* van data zoals *Onderstation*,
*Transformator* en *Kabel*, welke attributen er vastgelegd worden per klasse en
de relaties tussen klassen. Het zijn deze klassen die de betekenis van "dingen"
in de echte wereld uitdrukken voor een specifiek doel.

Daarnaast wordt het [Netbeheer Nederland
Begrippenmodel](https://begrippen.netbeheernederland.nl/) gehanteerd als basis
voor begrippen die al in gebruik zijn binnen de energiesector.

Niet alle klassen zijn altijd nodig voor het invullen van een specifieke
informatievraag. Wanneer alleen een lijst met onderstations nodig is, hoeven er
geen klassen die betrekking hebben met marktfacilitering mee worden genomen.
Deze keuze is het *structureren* van data: bepalen wat de structuur van de data
is die nodig is voor een specifieke informatievraag. Het CIM beschrijft naast
de betekenis van data een proces om tot een specifieke structuur te komen:
*profileren*. Dit proces volgt op hoofdlijnen de volgende stappen:

1. Bepalen wat de informatievraag is, e.g. "welke onderstations zijn er in
   Nederland die onderdeel uitmaken van het elektriciteitsnet?";
2. Bepalen welke klassen uit het CIM passen bij de informatievraag, en welke
   attributen er per klasse nodig zijn. Dit is het daadwerkelijk profileren,
   dus structureren van de data;
3. De klassen onderbrengen in profielen, waarmee een beheersbaar
   informatiemodel wordt gemaakt;

Sommige profielen zijn dermate breed toepasbaar dat deze ook zelf weer een
standaard worden, dan wel als enkel profiel of een verzameling van profielen:
een *Profile Group*. De *Common Grid Model Exchange Standard* (CGMES) is een
voorbeeld van een gestandaardiseerde Profile Group, waarbij de TSO's binnen
Europa de onderliggende profielen gebruiken op operationele informatie over hun
netten met elkaar uit te wisselen.

Samenvattend: het toepassen van het CIM is het vertalen van de betekenis van
componenten van het elektriciteitsnet naar een passende, herbruikbare structuur
voor het invullen van een informatievraag.

# Doelstellingen

Vanuit de visie en doelarchitectuur datadelen zijn meerdere doelen en principes
gedefinieerd rondom het beschikbaar stellen van data. Dit document legt de
nadruk op de invulling van drie van deze doelen:

1. het [FAIR](https://nl.wikipedia.org/wiki/FAIR-principes) beschikbaar stellen
   van data (*Findable*, *Accessible*, *Interoperable*, *Reusable*: FAIR),
   waarbij de nadruk op herbruikbaarheid ligt;
2. het gebruik van dataproducten voor uitwisseling van data;
3. het versnellen van de implementatie van nieuwe dataproducten, en
   herbruikbaarheid van bestaande dataproducten.

Om invulling te geven aan de bovenliggende doelen positioneren we de volgende
oplossing:

1. het opstellen en beheren van een sectorbreed informatiemodel, in de vorm van
   een *CIM Profile Group*. Hierin wordt de structuur van data gerelateerd aan
   bedrijfsvoering, ondersteunend activiteiten en marktfacilitering beschreven.
   De *CIM Profile Group* wordt beheerd door de *Werkgroup Semantische
   Interoperabiliteit*;
2. het uitvoeren van de processen rondom het opstellen en beheren van de CIM
   Profile Group;
3. (ondersteunen bij) het opstellen en beheren van dataproducten op basis van
   de *CIM Profile Group* voor specifieke informatievragen;
4. uitdragen van het nut en de noodzaak van de *CIM Profile Group*;
5. het geven van trainingen rondom het toepassen van de *CIM Profile Group*.

In het hoofdstuk [Implementatie](#implementatie) wordt de bovenstaande
oplossing inhoudelijk verder uitgewerkt.

# Aanpak

> TODO: koppeling met lopende epics pRO

## Structuur
## Stakeholders
## Fasering
## Tijdlijn

# Implementatie

De meest effectieve manier om de in de visie gestelde doelen in te vullen is
het hanteren van een informatiemodel voor de energiesector, waarbij voor het
beschrijven van het elektriciteitsnet gebruik wordt gemaakt van het Common
Information Model (CIM). Om de structuur van de data beheersbaar en
herbruikbaar te maken kiezen we voor het perspectief van verschillende
profielen, samengevoegd tot een *CIM Profile Group*.

## Opzet

De meeste informatievragen rondom het elektriciteitsnet draaien rondom
bedrijfsvoering (nettoplogie, congestie en *power flow*), netontwerp
(nettopologie met een temporaal aspect) en marktfacilitering (meterstanden,
flexibiliteit).

### Betekenis

Begrippen worden overgenomen uit en opgevoerd op [Begrippenmodel Netbeheer
Nederland](https://begrippen.netbeheernederland.nl/).  Alle begrippen zijn
uniek identificeerbaar op basis van *URI*, waarmee de structuur (onderstaand)
direct refeert naar het corresponderende begrip.

> [!IMPORTANT]
> Dit vereist een URI-strategie voor het eenduidig identificeren van
> entiteiten.

### Structuur: CIM Profile Group

Om invulling te geven aan de informatievragen wordt de volgende structuur voor
de *CIM Profile Group* gehanteerd:

![CIM Profile Group](assets/cim_profile_group-20240514.png)

De *CIM Profile Group* is gemodelleerd op twee bestaande *Profile Groups*:

* *Common Grid Model Exchange Standard* (CGMES): De *CIM Profile Group* beheerd
  en verplicht gesteld door de ENTSO-E. CGMES is specifiek voor TSOs
  ontwikkeld, maar is ook inzetbaar voor DSOs. ENTSO-E is de toezichthouder
  voor TSO's binnen de EU;
* *Long Term Development Statement* (LTDS): additionele profielen en extensies
  op bestaande profielen vanuit CGMES. De LTDS is ontwikkeld door *OFGEM*, de
  toezichthouder op de Britse energiemarkt. LTDS heeft o.a. extensies voor
  het beschrijven van capaciteit en congestie en laagspanningsnetten.

Aanvullend worden de profielen *Assets* en *European Style Market Profile*
vanuit het CIM geprofileerd en de *Implementing Regulations* rondom
*Demand/Response* en *Metering* overgenomen, respectievelijk.

Onderstaand een toelichting van de profielen en hun toepasbaarheid:

|Profiel                      |Toelichting|Voorbeeld|
|-----------------------------|-----------|---------|
|Assets                       |Fysieke eigenschappen van componenten in het net|AssetInfo, WireInfo|
|Capacity Heatmap             |Netcomponenten nodig voor een capaciteitskaart|Substation, Line|
|Diagram                      |Tekenen van elektrische diagrammen: Single Line Diagrams (SLD)|Diagram, DiagramObject|
|Equipment                    |Functionele eigenschappen van componenten in het net|Substation, Switch, Breaker|
|European Style Market Profile|Marktfacilitering binnen EU-markten|MarketParticipant, MarketAgreement|
|Operation                    |Operationele meetwaarden|Measurement, Analog, Accumulator|
|State Variables              |Resultaat van loadflow- en kortsluitberekeningen|svStatus, svSwitch|
|Steady State Hypothesis      |Loadflow en kortsluitgegevens|ProtectedSwitch, Jumper|
|System Capacity              |Systeemcapaciteit|ShortCircuitResult, FirmCapacity|
|Topology                     |Nettopologie: node/breaker v.s. bus/branch|TopologicalNode, ReportingGroup|

### Processen

Het bouwen en beheren van profielen vereist een aantal processen om deze
activiteiten beheersbaar te houden:

![Layered View](assets/layered_view-20240523.png)

|# |Bedrijfsproces       |Beschrijving|
|-:|---------------------|------------|
|1.|Onderhoud het profiel|Modelleeractiviteiten die een minimale hoeveelheid werk vereisen|
|2.|Plan het profiel     |Ophalen van requirements, ontwerpen van het profiel|
|3.|Bouw het profiel     |Datamodelleren en het  beschrijven van de betekenis en structuur|
|4.|Beoordeel het profiel|Goedkeuring van de verandering in het profiel. Vereist voordat het profiel gepubliceerd wordt|

Het proces voor het aanpassen van een profiel start met een concrete vraag
vanuit een stakeholder om betekenis of structuur toe te kennen aan een
informatievraag. Dit wordt beschreven als een *Onderhoudsverzoek*. Voor kleine
aanpassingen wordt het Onderhoudsverzoek uitgevoerd (1) en wordt het
resulterende nieuwe profiel opnieuw beoordeeld (4) voor publicatie.  Voor
grotere aanpassingen worden één of meerdere *Inhoudelijk Expert(s)* betrokken
(2) en wordt de aanpassing uitgevoerd (3). Weer wordt er een beoordeling
uitgevoerd (4) op het nieuwe profiel en wordt het profiel gepubliceerd.
Uitkomst van het proces is een nieuw profiel met daarin de nieuwe betekenis
en/of structuur.

Uitzonderingen in het proces vallen terug op de voorgaande processtap en worden
daar opgelost.

## Bestaan

Om de processen concreet in te vullen worden er keuzes gemaakt over het bestuur
van de *CIM Profile Group* en de technologische ondersteuning:

* de *CIM Profile Group* als geheel wordt beheerd door de *Werkgroep Semantische
  Interoperabiliteit*. Dit houdt in dat:
  * de processen worden uitgevoerd door de Werkgroep;
  * de Werkgroup verantwoordelijk is voor publicatie en ondersteuning bij
    gebruik van de *CIM Profile Group*;
* voor het beschrijven van de profielen wordt [LinkML](https://linkml.io/)
  gebruikt:
  * elk profiel wordt conform het LinkML metamodel beschreven in YAML;
  * elk profiel wordt als *repository* in de [Netbeheer
    Nederland](https://github.com/Netbeheer-Nederland) GitHub organisatie
    ondergebracht;
  * het profiel wordt als online documentatie (HTML) gepubliceerd op
    *https://netbeheer-nederland.github.io*, of op een nader te bepalen
    subdomein onder *netbeheernederland.nl*
* voor versiebeheer op de profielen wordt het versiebeheermechanisme van GitHub
  gebruikt, waarbij wijzigingen als Git Pull Requests worden beheerd en
  goedgekeurd;
* voor elk profiel wordt een conceptueel model (*Entity Relationship Diagram*)
  beschreven als onderdeel van de publicatie van het profiel (HTML pagina met
  diagram en beschrijving). De structuur (*klassen*) verwijzen naar de
  betekenis via broad/narrow mapping;
* het conceptuele model bevat via URI identificeerbare klassen.

## Werking

> TODO: Continuous Improvement, Training & ondersteuning

# Risicomanagement

> TODO

# Conclusie

> TODO

# Begrippen en definities

* *CDO*: Chief Data Officer;
* *Common Information Model*: een *canonical data model* voor het beschrijven
  van het elektriciteitsnet.
* *Data & informatie*: data zijn gegevens zonder context, informatie voegt
  context toe;
* *DSO*: Distribution System Operator, e.g. Stedin;
* *ENTSO-E*: vertegenwoordigt Europese TSOs;
* *EU DSO Entity*: vertegenwoordigt Europese DSOs;
* *FAIR*: principes op gebruik van data;
* *Informatiemodel*: beschrijving van de betekenis en structuur van informatie
  en data;
* *Informatievraag*: een verzoek tot het beschrijven van de betekenis en
  structuur van een specfieke behoefte rondom informatie;
* *pRO*: ProgrammaRegieOrginsatie, een samenwerkingsverband tussen Netbeheer
* *TSO*: Transmission System Operator, e.g. TenneT; Nederland en EDSN;

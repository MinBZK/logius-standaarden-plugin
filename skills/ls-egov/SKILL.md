---
name: ls-egov
description: "Gebruik deze skill wanneer de gebruiker vraagt over 'Terugmelding', 'Terugmelden', 'Digimelding', 'basisfactuur', 'basisorder', 'e-procurement', 'inkoopstandaarden overheid', 'factuurstandaard', 'PEPPOLBIS', 'elektronisch bestellen'."
model: sonnet
allowed-tools:
  - Bash(gh api *)
  - Bash(gh issue list *)
  - Bash(gh pr list *)
  - Bash(gh search *)
  - Bash(curl -s *)
  - Bash(npx markdownlint *)
  - Bash(npx @axe-core/cli *)
  - WebFetch(*)
---

# E-Government Services

De E-Government standaarden omvatten diensten voor terugmelding op basisregistraties, Digimelding, en e-procurement (elektronisch bestellen en factureren) voor de Nederlandse overheid. Deze standaarden worden beheerd door Logius en vormen een essentieel onderdeel van de digitale overheidsdienstverlening.

## Repositories

| Repository | Beschrijving | Publicatie |
|-----------|-------------|-----------|
| [Terugmelding](https://github.com/logius-standaarden/Terugmelding) | Standaard voor terugmelden op basisregistraties | [Lees online](https://logius-standaarden.github.io/Terugmelding/) |
| [Terugmelden-API](https://github.com/logius-standaarden/Terugmelden-API) | REST API specificatie (OpenAPI 3.1.0) voor terugmelden | [Lees online](https://logius-standaarden.github.io/Terugmelden-API/) |
| [Digimelding-Koppelvlakspecificatie](https://github.com/logius-standaarden/Digimelding-Koppelvlakspecificatie) | Koppelvlakspecificatie voor Digimelding | [Lees online](https://logius-standaarden.github.io/Digimelding-Koppelvlakspecificatie/) |
| [basisfactuur-rijk](https://github.com/logius-standaarden/basisfactuur-rijk) | Standaard basisfactuur voor de Rijksoverheid | [Lees online](https://logius-standaarden.github.io/basisfactuur-rijk/) |
| [basisorder-rijk](https://github.com/logius-standaarden/basisorder-rijk) | Standaard basisorder voor de Rijksoverheid | [Lees online](https://logius-standaarden.github.io/basisorder-rijk/) |
| [e-procurement](https://github.com/logius-standaarden/e-procurement) | Overkoepelende e-procurement standaarden | [Lees online](https://logius-standaarden.github.io/e-procurement/) |

---

## Terugmelding & Digimelding

### Kernbegrippen

- **Terugmelding** - Een melding dat gegevens in een basisregistratie mogelijk onjuist zijn. Burgers, bedrijven en overheidsorganisaties kunnen een terugmelding indienen wanneer zij vermoeden dat een gegeven niet klopt.
- **Basisregistraties** - De authentieke overheidsregistraties waarop teruggemeld kan worden:
  - **BRP** - Basisregistratie Personen
  - **BAG** - Basisregistratie Adressen en Gebouwen
  - **BRK** - Basisregistratie Kadaster
  - **BGT** - Basisregistratie Grootschalige Topografie
  - **BRT** - Basisregistratie Topografie
  - En overige basisregistraties binnen het stelsel
- **Bronhouder** - De beheerder van een basisregistratie die verantwoordelijk is voor het afhandelen van terugmeldingen. De bronhouder onderzoekt de melding en past indien nodig de registratie aan.
- **Digimelding** - De centrale voorziening waarmee terugmeldingen op basisregistraties worden ingediend en afgehandeld. Digimelding fungeert als koppelpunt tussen melder en bronhouder.

### Technische Architectuur: Annotatie-framework

De berichtenarchitectuur van Digimelding is gebaseerd op een annotatie-framework. Dit framework structureert terugmeldingen als hiërarchische annotaties:

#### Annotatie-hiërarchie

```
Root-annotatie (oorspronkelijke terugmelding)
├── Leaf-annotatie (reactie bronhouder)
│   └── Leaf-annotatie (vervolgreactie melder)
├── Leaf-annotatie (statusupdate)
└── Leaf-annotatie (afhandeling)
```

- **Root-annotatie** - De initiële terugmelding die door de melder wordt aangemaakt. Bevat de kerngegevens van de melding: welk gegeven, welke registratie, en de motivatie.
- **Leaf-annotaties** - Vervolgberichten die aan de root-annotatie worden gehangen. Elke leaf refereert terug naar het UUID van de root-annotatie.
- **Annotatiebasis** - Elke annotatie (root of leaf) bevat een annotatiebasis met metadata zoals tijdstip, auteur, en berichttype.
- **Annotatiebomen** - De volledige boom van root + leafs vormt het complete dossier van een terugmelding, inclusief alle communicatie en statuswijzigingen (multi-level feedback).

#### Berichttypen in de annotatieboom

| Type | Richting | Beschrijving |
|------|----------|-------------|
| Terugmelding | Melder -> Bronhouder | Initiële melding (root-annotatie) |
| Ontvangstbevestiging | Bronhouder -> Melder | Bevestiging van ontvangst |
| Informatieverzoek | Bronhouder -> Melder | Verzoek om aanvullende informatie |
| Aanvulling | Melder -> Bronhouder | Reactie op informatieverzoek |
| Statusupdate | Bronhouder -> Melder | Wijziging in verwerkingsstatus |
| Afhandeling | Bronhouder -> Melder | Definitieve afhandeling met resultaat |

### Terugmelden-API

De Terugmelden-API is een REST API gespecificeerd in **OpenAPI 3.1.0**. Deze API is gebaseerd op de Digimelding-functionaliteit en biedt een moderne interface voor het indienen en opvragen van terugmeldingen.

Kenmerken van de API:

- RESTful architectuur conform de API Design Rules (ADR)
- JSON-gebaseerde request/response berichten
- OpenAPI 3.1.0 specificatie voor machineleesbare documentatie
- Ondersteuning voor het aanmaken, opvragen en bijwerken van terugmeldingen

---

## Basisfactuur Rijk (e-Invoicing)

De Basisfactuur Rijk definieert de minimum data-eisen waaraan facturen aan de Rijksoverheid moeten voldoen. Het doel is om de factuurverwerking te standaardiseren en automatiseren.

### Bereik en Dekking

- Dekt circa **90% van alle overheidsfacturen** (standaard inkoop- en dienstverleningsfacturen)
- De resterende 10% betreft uitzonderingsgevallen zoals creditnota's met afwijkende structuren of specifieke sectorvereisten

### Ondersteunde Standaarden

| Standaard | Beschrijving |
|-----------|-------------|
| **Peppol BIS 3** | Europese factuurstandaard via het Peppol-netwerk |
| **NLCIUS** | Nederlandse invulling (Core Invoice Usage Specification) van de Europese norm EN 16931 |
| **UBL-OHNL** | UBL-profiel specifiek voor de Nederlandse overheid |

### Verplichte Elementen

Facturen die de verplichte velden missen worden **automatisch afgewezen**. De volgende elementen zijn verplicht:

#### 1. Factuuridentificatie en -datering
- Uniek factuurnummer
- Factuurdatum
- Factuurtypeaanduiding (factuur, creditnota, etc.)
- Valutacode

#### 2. Leverancieridentificatie
- Naam en adresgegevens leverancier
- KvK-nummer of vergelijkbaar identificatienummer
- BTW-identificatienummer
- Bankgegevens (IBAN en eventueel BIC)

#### 3. Ontvanger (overheidsorganisatie)
- Naam van de ontvangende organisatie
- OIN (Overheids Identificatie Nummer) of vergelijkbaar
- Adresgegevens

#### 4. Regelitems
- Omschrijving van het geleverde product of de dienst
- Hoeveelheid en eenheid
- Prijs per eenheid
- Totaalbedrag per regel
- BTW-percentage en BTW-bedrag per regel

#### 5. Order- en referentie-informatie
- Ordernummer (inkoopordernummer van de overheidsorganisatie)
- Contractreferentie (indien van toepassing)
- Projectreferentie (indien van toepassing)

#### 6. Betalingsvoorwaarden
- Betalingstermijn
- Betalingscondities
- Bankgegevens voor betaling (IBAN)

#### 7. Bijlagen
- Maximaal **9 bijlagen** toegestaan
- Totale grootte van alle bijlagen mag niet groter zijn dan **20 MB**
- Ondersteunde formaten conform Peppol-specificaties (PDF, afbeeldingen, etc.)

### Best Practices voor Leveranciers

- **Correct ordernummer** - Gebruik altijd het juiste inkoopordernummer van de overheidsorganisatie. Een fout ordernummer leidt tot afwijzing.
- **Minimaliseer nulwaarden** - Vermijd regels met hoeveelheid 0 of bedrag 0 tenzij strikt noodzakelijk.
- **Gebruik UBL/XML** - Lever facturen aan in UBL 2.1 XML-formaat voor optimale automatische verwerking.
- **Valideer voor verzending** - Controleer de factuur tegen het NLCIUS-validatieschema voordat deze wordt ingediend.
- **Eenduidige BTW-berekening** - Zorg dat BTW-bedragen consistent zijn op regel- en factuurniveau.

---

## Basisorder Rijk

De Basisorder Rijk definieert een gestandaardiseerd inkooporderformaat voor de Rijksoverheid. Hiermee worden inkooporders op een uniforme manier opgesteld en elektronisch verzonden naar leveranciers.

Kenmerken:

- Gebaseerd op UBL 2.1 Order-berichttype
- Sluit aan op Peppol-orderberichten
- Bevat gestandaardiseerde velden voor orderidentificatie, leverancier, artikelen, prijzen en leveringsvoorwaarden
- Maakt end-to-end automatisering mogelijk van het inkoopproces: van order tot factuur (purchase-to-pay)

---

## E-Procurement

E-Procurement is het overkoepelende domein voor elektronisch bestellen en factureren bij de overheid. Het omvat zowel de Basisfactuur Rijk als de Basisorder Rijk en is gericht op het digitaliseren van het volledige inkoopproces conform EU-richtlijnen.

### Scope

- **Elektronisch factureren (e-invoicing)** - Verplicht voor leveranciers aan de Rijksoverheid conform EU-richtlijn 2014/55/EU
- **Elektronisch bestellen (e-ordering)** - Gestandaardiseerde inkooporders via het Peppol-netwerk
- **Peppol-infrastructuur** - Gebruik van het Peppol-netwerk als transportlaag voor berichten tussen overheid en leveranciers
- **Compliance** - Naleving van de Europese norm EN 16931 voor elektronische facturering

### Samenhang Standaarden

```
E-Procurement (overkoepelend)
├── Basisfactuur Rijk
│   ├── Peppol BIS Billing 3.0
│   ├── NLCIUS (NL-specifieke invulling EN 16931)
│   └── UBL-OHNL
└── Basisorder Rijk
    ├── Peppol BIS Order 3.0
    └── UBL 2.1 Order
```

---

## Tests & Validatie

### Terugmelden-API OpenAPI Spec
```bash
# OpenAPI spec ophalen
gh search code "openapi" --repo logius-standaarden/Terugmelden-API

# Valideer de OpenAPI spec met Spectral
gh api repos/logius-standaarden/Terugmelden-API/contents --jq '[.[] | select(.name | test("\\.(yaml|json)$"))] | .[].name'
```

### Basisfactuur Validatie
```bash
# Bekijk de structuur van de basisfactuur specificatie
gh api repos/logius-standaarden/basisfactuur-rijk/contents --jq '.[].name'

# Zoek naar validatieregels
gh search code "verplicht" --repo logius-standaarden/basisfactuur-rijk
```

### Centrale Checks
```bash
# WCAG check
npx @axe-core/cli https://logius-standaarden.github.io/Terugmelding/ --tags wcag2aa

# Markdown lint
npx markdownlint-cli '**/*.md'
```

## Handige Commando's

```bash
# Laatste wijzigingen Terugmelding
gh api repos/logius-standaarden/Terugmelding/commits --jq '.[:5] | .[] | "\(.commit.committer.date) \(.commit.message | split("\n")[0])"'

# Open issues per repository
gh issue list --repo logius-standaarden/Terugmelding
gh issue list --repo logius-standaarden/Terugmelden-API
gh issue list --repo logius-standaarden/basisfactuur-rijk
gh issue list --repo logius-standaarden/basisorder-rijk

# Basisfactuur inhoud
gh api repos/logius-standaarden/basisfactuur-rijk/contents --jq '.[].name'

# Basisorder inhoud
gh api repos/logius-standaarden/basisorder-rijk/contents --jq '.[].name'

# Zoek naar e-procurement documentatie
gh search code "PEPPOLBIS" --owner logius-standaarden
gh search code "NLCIUS" --owner logius-standaarden
gh search code "UBL" --owner logius-standaarden
```

## Gerelateerde Skills

| Skill | Relatie |
|-------|---------|
| `/ls-api` | Terugmelden-API volgt de API Design Rules |
| `/ls-dk` | Digimelding kan via Digikoppeling koppelvlakken |
| `/ls-pub` | ReSpec tooling voor documentatie |
| `/ls` | Overzicht alle standaarden |

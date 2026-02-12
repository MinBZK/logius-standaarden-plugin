# E-Government Services - Achtergrondinfo

Deze reference bevat achtergrondkennis en encyclopedische details voor de E-Government standaarden. Voor implementatie-instructies zie [SKILL.md](SKILL.md).

---

## Kernbegrippen

### Terugmelding & Digimelding

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

---

## Technische Architectuur: Annotatie-framework

De berichtenarchitectuur van Digimelding is gebaseerd op een annotatie-framework. Dit framework structureert terugmeldingen als hiërarchische annotaties:

### Annotatie-hiërarchie

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

### Berichttypen in de annotatieboom

| Type | Richting | Beschrijving |
|------|----------|-------------|
| Terugmelding | Melder -> Bronhouder | Initiële melding (root-annotatie) |
| Ontvangstbevestiging | Bronhouder -> Melder | Bevestiging van ontvangst |
| Informatieverzoek | Bronhouder -> Melder | Verzoek om aanvullende informatie |
| Aanvulling | Melder -> Bronhouder | Reactie op informatieverzoek |
| Statusupdate | Bronhouder -> Melder | Wijziging in verwerkingsstatus |
| Afhandeling | Bronhouder -> Melder | Definitieve afhandeling met resultaat |

---

## Best Practices voor Leveranciers

### Basisfactuur Rijk

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

### Bereik en Dekking

- Dekt circa **90% van alle overheidsfacturen** (standaard inkoop- en dienstverleningsfacturen)
- De resterende 10% betreft uitzonderingsgevallen zoals creditnota's met afwijkende structuren of specifieke sectorvereisten

### Ondersteunde Standaarden

| Standaard | Beschrijving |
|-----------|-------------|
| **Peppol BIS 3** | Europese factuurstandaard via het Peppol-netwerk |
| **NLCIUS** | Nederlandse invulling (Core Invoice Usage Specification) van de Europese norm EN 16931 |
| **UBL-OHNL** | UBL-profiel specifiek voor de Nederlandse overheid |

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

---

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

---

## Gerelateerde Skills

| Skill | Relatie |
|-------|---------|
| `/ls-api` | Terugmelden-API volgt de API Design Rules |
| `/ls-dk` | Digimelding kan via Digikoppeling koppelvlakken |
| `/ls-pub` | ReSpec tooling voor documentatie |
| `/ls` | Overzicht alle standaarden |

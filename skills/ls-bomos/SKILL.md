---
name: ls-bomos
description: "Gebruik deze skill wanneer de gebruiker vraagt over 'BOMOS', 'Beheer- en Ontwikkelmodel', 'open standaarden beheer', 'standaardisatie governance', 'beheermodel', 'community management standaarden', 'linked data standaarden', 'open source governance', 'stelselstandaarden'."
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

# BOMOS - Beheer- en Ontwikkelmodel voor Open Standaarden

BOMOS biedt een raamwerk voor het beheer en de ontwikkeling van open standaarden. Het beschrijft processen, rollen en instrumenten die nodig zijn om een standaard succesvol te beheren en door te ontwikkelen. Het model is ontwikkeld door Logius en wordt breed toegepast binnen de Nederlandse overheid voor het structureren van standaardenbeheer.

## Repositories

| Repository | Beschrijving | Publicatie |
|-----------|-------------|-----------|
| [BOMOS-Fundament](https://github.com/logius-standaarden/BOMOS-Fundament) | Kernmodel: activiteiten, rollen, structuur | [Lees online](https://logius-standaarden.github.io/BOMOS-Fundament/) |
| [BOMOS-Verdieping](https://github.com/logius-standaarden/BOMOS-Verdieping) | Verdiepende beschrijving van BOMOS activiteiten | [Lees online](https://logius-standaarden.github.io/BOMOS-Verdieping/) |
| [BOMOS-Aanvullende-Modules](https://github.com/logius-standaarden/BOMOS-Aanvullende-Modules) | Aanvullende modules voor specifieke aspecten | [Lees online](https://logius-standaarden.github.io/BOMOS-Aanvullende-Modules/) |
| [BOMOS-LinkedData](https://github.com/logius-standaarden/BOMOS-LinkedData) | Module: beheer van Linked Data standaarden | [Lees online](https://logius-standaarden.github.io/BOMOS-LinkedData/) |
| [BOMOS-OpenSource](https://github.com/logius-standaarden/BOMOS-OpenSource) | Module: open source governance | [Lees online](https://logius-standaarden.github.io/BOMOS-OpenSource/) |
| [BOMOS-Stelsels](https://github.com/logius-standaarden/BOMOS-Stelsels) | Module: beheer van stelsels van standaarden | [Lees online](https://logius-standaarden.github.io/BOMOS-Stelsels/) |
| [BOMOS-Beheermodel](https://github.com/logius-standaarden/BOMOS-Beheermodel) | Beheermodel voor BOMOS zelf | [Lees online](https://logius-standaarden.github.io/BOMOS-Beheermodel/) |
| [BOMOS-voorbeeld-beheermodel](https://github.com/logius-standaarden/BOMOS-voorbeeld-beheermodel) | Voorbeelddocument voor een beheermodel | [Lees online](https://logius-standaarden.github.io/BOMOS-voorbeeld-beheermodel/) |
| [BOMOS-Community](https://github.com/logius-standaarden/BOMOS-Community) | Community-aspecten van standaardenbeheer | [Lees online](https://logius-standaarden.github.io/BOMOS-Community/) |
| [Logius-Beheermodel](https://github.com/logius-standaarden/Logius-Beheermodel) | Overkoepelend beheermodel van Logius | [Lees online](https://logius-standaarden.github.io/Logius-Beheermodel/) |

## Het BOMOS Activiteitenmodel

Het activiteitenmodel is het hart van BOMOS en beschrijft vijf hoofdlagen die samen alle aspecten van standaardenbeheer afdekken. Elke laag heeft specifieke activiteiten en verantwoordelijke rollen.

### 1. Strategie

De strategielaag bepaalt de koers en randvoorwaarden voor het beheer van de standaard.

- **Governance** - Inrichting van de besluitvormingsstructuur, samenstelling van gremia, en vaststelling van procedures
- **Visie** - Langetermijnvisie op de standaard: doelgroep, toepassingsgebied, en gewenste marktpositie
- **Financiering** - Structurele financiering van beheeractiviteiten, begrotingsmodel, en verantwoording

**Rollen:** Eigenaar, Financier

### 2. Tactiek

De tactieklaag vertaalt strategie naar concrete beheerafspraken en kwaliteitsnormen.

- **Architectuur** - Technische en functionele architectuurkeuzes, samenhang met andere standaarden
- **Specificatiebeheer** - Versiebeheer van de standaard, onderhoud van de specificatiedocumenten
- **Wijzigingsbeheer** - Proces voor het indienen, beoordelen, en doorvoeren van wijzigingsverzoeken (RFC-procedure)
- **Adoptiestrategie** - Beleid voor het bevorderen van adoptie: verplichte open standaarden lijsten, comply-or-explain
- **Kwaliteitsbeleid** - Kwaliteitscriteria voor de standaard zelf en het beheerproces
- **Rechtenmanagement** - Intellectueel eigendom, licentiemodel, en gebruiksvoorwaarden

**Rol:** Autorisator

### 3. Operationeel

De operationele laag omvat het daadwerkelijke dagelijkse werk van standaardontwikkeling en -onderhoud.

- **Initiatie** - Opstarten van nieuwe ontwikkeltrajecten, scoping van wijzigingen
- **Wensen en eisen verzamelen** - Inventarisatie van behoeften bij gebruikers, implementeerders en stakeholders
- **Ontwikkeling** - Technische uitwerking van wijzigingen, review, en oplevering van nieuwe versies
- **Uitvoering** - Doorvoeren van goedgekeurde wijzigingen in de operationele omgeving
- **Documentatie** - Bijhouden van alle documentatie: specificaties, release notes, handleidingen

**Rollen:** Functioneel Beheerder, Technisch Beheerder

### 4. Implementatieondersteuning

Deze laag ondersteunt organisaties bij de daadwerkelijke implementatie van de standaard. Hierbij is het cruciaal om een balans te vinden tussen activiteiten van de beheerorganisatie en die van commerciele partijen.

- **Helpdesk** - Minimale ondersteuning voor implementatievragen (laagdrempelig, altijd nodig)
- **Opleidingen** - Zorgvuldig aanbieden: niet concurreren met de markt, alleen waar de markt niet voorziet
- **Open source modules** - Referentie-implementaties en tooling, alleen inzetten bij geconstateerde implementatieproblemen
- **Pilots** - Bij introductie van nieuwe versies, versterkt innovatief imago van de standaard
- **Validatie en certificatie** - Eerst intern toepassen, later publiek maken. Overweeg naming & shaming als instrument

### 5. Communicatie

De communicatielaag zorgt voor zichtbaarheid en betrokkenheid rond de standaard.

- **Promotie** - Actieve promotie van de standaard bij potentiele gebruikers en stakeholders
- **Community management** - Opbouwen en onderhouden van een actieve community van gebruikers en ontwikkelaars
- **Events** - Organiseren van bijeenkomsten, werkgroepen, en plugfests
- **Distributie** - Publicatie en verspreiding van de standaard en bijbehorende documentatie

**Rol:** Distributeur

## Rollen en Verantwoordelijkheden

BOMOS definieert een rollenmodel dat de verantwoordelijkheden binnen het standaardenbeheer structureert.

| Rol | Verantwoordelijkheid |
|-----|---------------------|
| **Eigenaar** | Eindverantwoordelijk voor de ontwikkeling van de standaard, bepaalt scope en principes |
| **Financier** | Draagt financieringsverantwoordelijkheid voor het beheer en de doorontwikkeling |
| **Autorisator** | Goedkeuringsautoriteit voor wijzigingen (kan een individu of groep/commissie zijn) |
| **Functioneel Beheerder** | Procesverantwoordelijk voor de ontwikkeling en het onderhoud van de standaard |
| **Technisch Beheerder** | Verantwoordelijk voor de technische omgeving (repositories, publicatieplatform, tooling) |
| **Distributeur** | Verantwoordelijk voor publicatie en verspreiding van de standaard |
| **Expert** | Levert domeinspecifieke expertise in bij de ontwikkeling en beoordeling van de standaard |
| **Gebruiker** | Directe of indirecte gebruiker van de standaard; levert input via wensen en eisen |

Een persoon kan meerdere rollen vervullen, en een rol kan door meerdere personen worden ingevuld. De exacte invulling hangt af van de omvang en volwassenheid van de standaard.

## Levensfasen van een Standaard

Elke standaard doorloopt een levenscyclus waarbij per fase andere accenten in het beheer worden gelegd:

1. **Creatie/Ontwikkeling** - De standaard wordt ontworpen en de eerste versie wordt opgeleverd. Focus op inhoudelijke kwaliteit, brede consultatie, en draagvlak.
2. **Introductie** - De standaard wordt gepubliceerd en bij de doelgroep geintroduceerd. Focus op promotie, early adopters, en implementatieondersteuning.
3. **Implementatie/Groei** - Brede adoptie komt op gang. Focus op helpdesk, opleidingen, validatie, en community building.
4. **Volwassenheid** - De standaard is breed geadopteerd en stabiel. Focus op onderhoud, wijzigingsbeheer, en interoperabiliteit met aanpalende standaarden.
5. **Uitfasering** - De standaard wordt vervangen of buiten gebruik gesteld. Focus op migratieondersteuning, communicatie, en archivering.

De beheerorganisatie moet de activiteiten uit het activiteitenmodel aanpassen aan de huidige levensfase van de standaard.

## Conformiteit

Een beheerorganisatie werkt conform BOMOS wanneer een beheerdocument is gepubliceerd dat alle componenten uit het BOMOS Activiteitendiagram beschrijft. Dit betekent dat voor elke activiteit uit het model een beschrijving is opgenomen van hoe de organisatie deze invult, inclusief de relevante rollen, processen en instrumenten. Het beheerdocument hoeft niet voor elk onderdeel uitgebreid te zijn -- bij sommige activiteiten volstaat een bewuste keuze om deze (nog) niet in te vullen.

## Pressure Cooker Model

BOMOS beschrijft een versneld ontwikkelmodel voor situaties waarin snel een standaard nodig is:

1. **Intensieve workshop** (1 week) - Minimaal 15 deelnemers werken in een week-lange pressure cooker aan de eerste versie van de standaard
2. **Expert-verfijning** (2 weken) - Externe experts verfijnen en verbeteren het resultaat
3. **Community review** (2 weken) - Bredere community beoordeelt en geeft feedback
4. **Afronding en governance** - Formele vaststelling en inrichting van structureel beheer

Totale doorlooptijd: circa 2 maanden van initiatie tot governance. Dit model is geschikt voor standaarden waar urgentie en breed draagvlak samenkomen.

## Praktijkvoorbeelden

### Geonovum

Geonovum past BOMOS toe voor het beheer van geo-standaarden. In 2014 ontving Geonovum het predicaat "uitstekend beheer" voor de manier waarop zij hun standaarden conform BOMOS beheren. Dit toont aan dat BOMOS een bewezen model is voor professioneel standaardenbeheer.

### TOOI - Thesaurus en Ontologie voor OverheidsInformatie

TOOI (Thesaurus en Ontologie voor OverheidsInformatie) gebruikt BOMOS als basis voor hun beheerplan. Het beheermodel van TOOI beschrijft per BOMOS-activiteit hoe deze wordt ingevuld, waarmee het een concreet voorbeeld biedt van een BOMOS-conform beheerdocument.

## Linked Data Module

De Linked Data module biedt specifieke richtlijnen voor het beheer van semantische standaarden zoals ontologieen en vocabulaires. Deze module behandelt onderwerpen als URI-strategie, versiebeheer van ontologieen, en publicatie als Linked Data.

## Open Source Module

De Open Source module beschrijft governance voor open source componenten die bij standaarden horen. Dit omvat licentiekeuze, contributor agreements, en het beheer van referentie-implementaties.

## Tests & Validatie

BOMOS repos gebruiken de centrale `Automatisering` workflows:

```bash
# WCAG check
npx @axe-core/cli https://logius-standaarden.github.io/BOMOS-Fundament/ --tags wcag2aa

# Markdown lint
npx markdownlint-cli '**/*.md'
```

## Handige Commando's

```bash
# Alle BOMOS repos
gh api orgs/logius-standaarden/repos --paginate --jq '[.[] | select(.name | startswith("BOMOS"))] | sort_by(.name) | .[].name'

# Laatste wijzigingen fundament
gh api repos/logius-standaarden/BOMOS-Fundament/commits --jq '.[:5] | .[] | "\(.commit.committer.date) \(.commit.message | split("\n")[0])"'

# Open issues
gh issue list --repo logius-standaarden/BOMOS-Fundament

# BOMOS Fundament inhoud
gh api repos/logius-standaarden/BOMOS-Fundament/contents --jq '.[].name'

# Zoek naar specifieke BOMOS concepten
gh search code "adoptie" --owner logius-standaarden --filename "*.md"
```

## Gerelateerde Skills

| Skill | Relatie |
|-------|---------|
| `/ls-pub` | Publicatie-tooling wordt gebruikt voor BOMOS documenten |
| `/ls-dk` | Digikoppeling Beheermodel is gebaseerd op BOMOS |
| `/ls-api` | ADR Beheermodel is gebaseerd op BOMOS |
| `/ls` | Overzicht alle standaarden |

# API Design Rules - Bronconflicten

Geconstateerd: 2026-02-21

Dit document beschrijft bekende discrepanties tussen GitHub-repository tags en de gepubliceerde versies op [gitdocumentatie.logius.nl](https://gitdocumentatie.logius.nl). De **gepubliceerde versie op gitdocumentatie.logius.nl is leidend** voor DEF-versies.

## Discrepanties

| Repository | Gepubliceerde versie (DEF) | Laatste GitHub-tag | Discrepantie |
|-----------|---------------------------|-------------------|-------------|
| [API-mod-geospatial](https://github.com/logius-standaarden/API-mod-geospatial) | [v1.0.3](https://gitdocumentatie.logius.nl/publicatie/api/mod-geo/1.0.3/) | `1.0.2` | Tag achter: publicatie v1.0.3 vs tag 1.0.2 |
| [ADR-Beheermodel](https://github.com/logius-standaarden/ADR-Beheermodel) | [v1.0](https://gitdocumentatie.logius.nl/publicatie/api/adr-beheer/1.0/) | `10r4` | Tag-formaat afwijkend: publicatie v1.0 vs tag `10r4` |

### Details API-mod-geospatial

- **Alle bekende tags:** `1.0.2`, `1.0.1`, `1.0.0`
- **Geen GitHub Releases** aangemaakt
- De tag `1.0.2` bestaat op GitHub, maar de gepubliceerde versie op gitdocumentatie is `1.0.3`
- Er is geen tag `1.0.3` in de repository
- Dit wijst erop dat de publicatie v1.0.3 is doorgevoerd zonder een bijbehorende Git-tag aan te maken

### Details ADR-Beheermodel

- **Enige tag:** `10r4` — afwijkend formaat, geen semantic versioning
- De repository is **gearchiveerd** en vervangen door API-Standaarden-Beheermodel
- De gepubliceerde DEF-versie op gitdocumentatie is `v1.0`
- Het tag-formaat `10r4` komt waarschijnlijk uit een ouder versiebeheersysteem

## Transport Security module: dubbelzinnige status

**Opgelost in ADR v2.2.0 (vastgesteld 2 juni 2026).**

In v2.1.0 had de Transport Security module (API-mod-transport-security) een dubbelzinnige status: de inhoud was ingebed in de hoofdspecificatie, de repo was gearchiveerd, maar de leeswijzer vermeldde module v1.0 nog als normatief.

In v2.2.0 is die dubbelzinnigheid weg. De specificatie bevat geen leeswijzer-vermelding en geen verwijzing naar `API-mod-transport-security` meer; de transport-security-eisen staan in sectie 2.11 met regels `/core/transport/*`. De module komt alleen nog voor in de historische changelog-regel bij v2.0.0.

### Keuze in SKILL.md

De SKILL.md beschrijft de eisen als onderdeel van de hoofdspecificatie en vermeldt de gearchiveerde repo als historische context. Geen openstaand conflict meer.

## ADR werkversie-nummering op draft-pagina

De draft-pagina op GitHub Pages ([logius-standaarden.github.io/API-Design-Rules](https://logius-standaarden.github.io/API-Design-Rules/)) loopt in de ReSpec-configuratie achter op de vastgestelde versie. Dit is een bekend patroon bij Logius-standaarden: de ReSpec `defined_in` configuratie wordt pas bijgewerkt wanneer een nieuwe versie wordt vastgesteld. Leidend is altijd `gitdocumentatie.logius.nl`, waar v2.2.0 sinds 2 juni 2026 als DEF staat.

### Keuze in SKILL.md

De SKILL.md beschrijft de werkversie als "lopende ontwikkeling richting de volgende release" zonder een specifiek versienummer voor de draft. Als de ReSpec-configuratie wordt bijgewerkt naar een nieuw versienummer, moet deze beschrijving opnieuw worden beoordeeld.

## Keuze in SKILL.md

De skill gebruikt versienummers van gitdocumentatie.logius.nl als bron van waarheid voor vastgestelde (DEF) versies. De GitHub-tag van API-mod-geospatial loopt één patchversie achter; dit is hetzelfde patroon als bij andere Logius-standaarden repos (zie ook [ls-dk/conflicts.md](../ls-dk/conflicts.md)). ADR-Beheermodel heeft een afwijkend tag-formaat (`10r4` vs DEF v1.0). Transport Security wordt beschreven met de feitelijke drieledige status (ingebed, leeswijzer, gearchiveerd). Als tags worden bijgewerkt of de leeswijzer wordt aangepast, moeten deze conflicten opnieuw worden beoordeeld.

## Licentie-status GitHub-repositories

Sommige repositories bevatten een W3C Software License (afkomstig van het ReSpec-template), terwijl de gepubliceerde standaarden CC-BY-4.0 vermelden via de ReSpec-configuratie. Andere repositories hebben geen expliciet `LICENSE`-bestand.

| Repository | GitHub LICENSE | Gepubliceerde licentie | Status |
|-----------|--------------|----------------------|--------|
| [ADR-Beheermodel](https://github.com/logius-standaarden/ADR-Beheermodel) | Geen | CC-BY-4.0 (ReSpec) | Discrepantie |
| [API-Standaarden-Beheermodel](https://github.com/logius-standaarden/API-Standaarden-Beheermodel) | Geen | CC-BY-4.0 (ReSpec) | Discrepantie |
| [API-mod-geospatial](https://github.com/logius-standaarden/API-mod-geospatial) | Geen | CC-BY-4.0 (ReSpec) | Discrepantie |
| [API-mod-transport-security](https://github.com/logius-standaarden/API-mod-transport-security) | W3C Software License | CC-BY-4.0 (ReSpec) | Afwijkend |
| [API-mod-signing](https://github.com/logius-standaarden/API-mod-signing) | Geen | CC-BY-4.0 (ReSpec) | Discrepantie |
| [API-mod-encryption](https://github.com/logius-standaarden/API-mod-encryption) | Geen | CC-BY-4.0 (ReSpec) | Discrepantie |
| [zaakgericht-werken-api](https://github.com/logius-standaarden/zaakgericht-werken-api) | W3C Software License | CC-BY-4.0 (ReSpec, overgeërfd van organisatie-config) | Afwijkend |

### Aandachtspunt

De W3C Software License is afkomstig van het ReSpec-template en dekt de tooling-code. De gepubliceerde standaard-inhoud valt onder CC-BY-4.0 (vermeld in de ReSpec-configuratie). Voor repositories zonder LICENSE-bestand verdient het aanbeveling om expliciet een licentie toe te voegen, relevant voor artikel 15n Auteurswet (text and data mining exceptie).

## Referenties

- Publicatie API-mod-geospatial: https://gitdocumentatie.logius.nl/publicatie/api/mod-geo/1.0.3/
- GitHub repo: https://github.com/logius-standaarden/API-mod-geospatial
- GitHub tags: https://github.com/logius-standaarden/API-mod-geospatial/tags
- Publicatie ADR-Beheermodel: https://gitdocumentatie.logius.nl/publicatie/api/adr-beheer/1.0/
- GitHub ADR-Beheermodel: https://github.com/logius-standaarden/ADR-Beheermodel
- GitHub ADR-Beheermodel tags: https://github.com/logius-standaarden/ADR-Beheermodel/tags
- GitHub API-mod-transport-security: https://github.com/logius-standaarden/API-mod-transport-security
- Publicatie ADR v2.2.0: https://gitdocumentatie.logius.nl/publicatie/api/adr/2.2.0/
- Publicatie ADR v2.1.0 (vorige DEF): https://gitdocumentatie.logius.nl/publicatie/api/adr/2.1.0/
- Forum Standaardisatie ADR: https://www.forumstandaardisatie.nl/open-standaarden/rest-api-design-rules

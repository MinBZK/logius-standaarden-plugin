# E-Government - Bronconflicten

Geconstateerd: 2026-02-21

Dit document beschrijft bekende discrepanties en bewuste keuzes voor de E-Government-skill.

## GitHub-tags vs. gepubliceerde versies

Alle E-Government repositories beheerd door Logius hebben alleen werkversies. Er zijn geen vastgestelde (DEF) versies en geen GitHub-tags.

| Repository | Gepubliceerde versie (DEF) | Laatste GitHub-tag | Discrepantie |
|-----------|---------------------------|-------------------|-------------|
| [Terugmelding](https://github.com/logius-standaarden/Terugmelding) | _(geen DEF)_ | _(geen tags)_ | N.v.t. — alleen werkversies |
| [Terugmelden-API](https://github.com/logius-standaarden/Terugmelden-API) | _(geen DEF)_ | _(geen tags)_ | N.v.t. — alleen werkversies |
| [Digimelding-Koppelvlakspecificatie](https://github.com/logius-standaarden/Digimelding-Koppelvlakspecificatie) | _(geen DEF)_ | `1.3` | N.v.t. — alleen werkversies |
| [basisfactuur-rijk](https://github.com/logius-standaarden/basisfactuur-rijk) | _(geen DEF)_ | `v1.0.8` | **Gearchiveerd** augustus 2026. Tags aanwezig (`v1.0.8`, `v1.0.6`) maar geen DEF op gitdocumentatie |
| [basisorder-rijk](https://github.com/logius-standaarden/basisorder-rijk) | _(geen DEF)_ | _(geen tags)_ | **Gearchiveerd** augustus 2026. Alleen werkversies, nooit een DEF gepubliceerd |
| [e-procurement](https://github.com/logius-standaarden/e-procurement) | _(geen DEF)_ | _(geen tags)_ | **Gearchiveerd** augustus 2026. Alleen werkversies, nooit een DEF gepubliceerd |

## Forum Standaardisatie: externe standaarden

De volgende standaarden worden op het Forum Standaardisatie vermeld in de context van e-government, maar worden **niet door Logius beheerd**. Ze verschijnen daarom niet als Logius-repositories in de SKILL.md repo-tabel:

| Standaard | Forum-versie | Beheerder |
|-----------|-------------|-----------|
| [NLCIUS](https://www.forumstandaardisatie.nl/open-standaarden/nlcius) | v1.03 | SIMPLERINVOICING (extern) |
| [Peppol BIS](https://www.forumstandaardisatie.nl/open-standaarden/peppol-bis) | v3.08 | OpenPeppol (extern) |

### Analyse

NLCIUS en Peppol BIS staan in de SKILL.md als Forum-standaarden met hun status (verplicht), maar niet als Logius-repositories. Logius beheert de Terugmeld- en Digimelding-standaarden; de e-procurement standaarden worden extern beheerd.

## Keuze in SKILL.md

Logius-repositories hebben alleen werkversies. NLCIUS en Peppol BIS staan in de skill vanwege hun Forum Standaardisatie-status, niet als Logius-repositories. Als er DEF-versies worden vastgesteld voor de Terugmeld-repositories, moet dit document worden bijgewerkt.

## Licentie-status GitHub-repositories

Sommige repositories bevatten een W3C Software License (afkomstig van het ReSpec-template), terwijl de gepubliceerde standaarden CC-BY-4.0 vermelden via de ReSpec-configuratie. Andere repositories hebben geen expliciet `LICENSE`-bestand.

| Repository | GitHub LICENSE | Gepubliceerde licentie | Status |
|-----------|--------------|----------------------|--------|
| [Terugmelding](https://github.com/logius-standaarden/Terugmelding) | W3C Software License | CC0-1.0 (ReSpec) | Afwijkend |
| [Terugmelden-API](https://github.com/logius-standaarden/Terugmelden-API) | Geen | N.v.t. | Geen licentie |
| [Digimelding-Koppelvlakspecificatie](https://github.com/logius-standaarden/Digimelding-Koppelvlakspecificatie) | Geen | CC-BY-4.0 (ReSpec) | Discrepantie |
| [basisfactuur-rijk](https://github.com/logius-standaarden/basisfactuur-rijk) | W3C Software License | CC0-1.0 (ReSpec) | Afwijkend |
| [basisorder-rijk](https://github.com/logius-standaarden/basisorder-rijk) | W3C Software License | CC0-1.0 (ReSpec) | Afwijkend |
| [e-procurement](https://github.com/logius-standaarden/e-procurement) | Geen | CC-BY-4.0 (ReSpec) | Discrepantie |

### Aandachtspunt

De W3C Software License is afkomstig van het ReSpec-template en dekt de tooling-code. De gepubliceerde standaard-inhoud valt onder CC-BY-4.0 (vermeld in de ReSpec-configuratie). Voor repositories zonder LICENSE-bestand verdient het aanbeveling om expliciet een licentie toe te voegen, relevant voor artikel 15n Auteurswet (text and data mining exceptie).

## Referenties

- GitHub Terugmelding: https://github.com/logius-standaarden/Terugmelding
- GitHub Terugmelden-API: https://github.com/logius-standaarden/Terugmelden-API
- Forum Standaardisatie Digikoppeling (Digimelding valt hieronder): https://www.forumstandaardisatie.nl/open-standaarden/digikoppeling
- Forum Standaardisatie NLCIUS: https://www.forumstandaardisatie.nl/open-standaarden/nlcius
- Forum Standaardisatie Peppol BIS: https://www.forumstandaardisatie.nl/open-standaarden/peppol-bis

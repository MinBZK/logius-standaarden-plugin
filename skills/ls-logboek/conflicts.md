# Logboek Dataverwerkingen - Bronconflicten

Geconstateerd: 2026-02-21
Bijgewerkt: 2026-04-14

Dit document beschrijft bekende discrepanties en bewuste keuzes voor de Logboek-skill.

## GitHub-tags vs. gepubliceerde versies

De normatieve hoofdspecificatie heeft sinds 9 april 2026 een vastgestelde versie v1.0.0 op `gitdocumentatie.logius.nl/publicatie/logboek/dataverwerkingen/1.0.0/`. De GitHub-repo `logboek-dataverwerkingen` heeft nog geen tag die overeenkomt met deze publicatie — dit is een bekend beheerproces-issue. Alle overige repositories hebben nog alleen werkversies en geen tags.

| Repository | Gepubliceerde versie (DEF) | Laatste GitHub-tag | Discrepantie |
|-----------|---------------------------|-------------------|-------------|
| [logboek-dataverwerkingen](https://github.com/logius-standaarden/logboek-dataverwerkingen) | [v1.0.0](https://gitdocumentatie.logius.nl/publicatie/logboek/dataverwerkingen/1.0.0/) | _(geen tags)_ | Publicatie loopt voor op tag |
| [logboek-extensie-lezen](https://github.com/logius-standaarden/logboek-extensie-lezen) | _(geen DEF)_ | _(geen tags)_ | N.v.t. — alleen werkversies |
| [logboek-extensie-nen7513](https://github.com/logius-standaarden/logboek-extensie-nen7513) | _(geen DEF)_ | _(geen tags)_ | N.v.t. — alleen werkversies |
| [logboek-extensie-object](https://github.com/logius-standaarden/logboek-extensie-object) | _(geen DEF)_ | _(geen tags)_ | N.v.t. — alleen werkversies |
| [logboek-extensie-template](https://github.com/logius-standaarden/logboek-extensie-template) | _(geen DEF)_ | _(geen tags)_ | N.v.t. — alleen werkversies |
| [logboek-dataverwerkingen-inleiding](https://github.com/logius-standaarden/logboek-dataverwerkingen-inleiding) | _(geen DEF)_ | _(geen tags)_ | N.v.t. — alleen werkversies |
| [logboek-dataverwerkingen-juridisch-beleidskader](https://github.com/logius-standaarden/logboek-dataverwerkingen-juridisch-beleidskader) | _(geen DEF)_ | _(geen tags)_ | N.v.t. — alleen werkversies |
| [logboek-dataverwerkingen-demo](https://github.com/logius-standaarden/logboek-dataverwerkingen-demo) ⚠️ | _(geen DEF)_ | _(geen tags)_ | **Gearchiveerd** augustus 2026, README meldt dat de demo verouderd is. Demo/referentie-implementatie |

## Keuze in SKILL.md

De hoofdspecificatie `logboek-dataverwerkingen` wordt vermeld met versie v1.0.0 (DEF) op basis van `gitdocumentatie.logius.nl`. Overige onderdelen blijven als werkversies vermeld totdat zij ook een DEF-publicatie krijgen. Bij een nieuwe DEF-publicatie van extensies of inleiding moet dit document worden bijgewerkt.

## Licentie-status GitHub-repositories

Sommige repositories bevatten een W3C Software License (afkomstig van het ReSpec-template), terwijl de gepubliceerde standaarden CC-BY-4.0 vermelden via de ReSpec-configuratie. Andere repositories hebben geen expliciet `LICENSE`-bestand.

| Repository | GitHub LICENSE | Gepubliceerde licentie | Status |
|-----------|--------------|----------------------|--------|
| [logboek-dataverwerkingen](https://github.com/logius-standaarden/logboek-dataverwerkingen) | W3C Software License | CC-BY-4.0 (ReSpec) | Afwijkend |
| [logboek-extensie-lezen](https://github.com/logius-standaarden/logboek-extensie-lezen) | CC-BY-4.0 | CC-BY-4.0 (ReSpec) | Opgelost |
| [logboek-extensie-nen7513](https://github.com/logius-standaarden/logboek-extensie-nen7513) | W3C Software License | CC-BY-4.0 (ReSpec) | Afwijkend |
| [logboek-extensie-object](https://github.com/logius-standaarden/logboek-extensie-object) | W3C Software License | CC-BY-4.0 (ReSpec) | Afwijkend |
| [logboek-extensie-template](https://github.com/logius-standaarden/logboek-extensie-template) | W3C Software License | CC-BY-4.0 (ReSpec) | Afwijkend |
| [logboek-dataverwerkingen-inleiding](https://github.com/logius-standaarden/logboek-dataverwerkingen-inleiding) | W3C Software License | CC-BY-4.0 (ReSpec) | Afwijkend |
| [logboek-dataverwerkingen-juridisch-beleidskader](https://github.com/logius-standaarden/logboek-dataverwerkingen-juridisch-beleidskader) | W3C Software License | CC-BY-4.0 (ReSpec) | Afwijkend |

**Opmerking:** [logboek-dataverwerkingen-demo](https://github.com/logius-standaarden/logboek-dataverwerkingen-demo) heeft wél een expliciete EUPL-1.2 licentie in GitHub en is een demo/referentie-implementatie (geen ReSpec-document), en is daarom niet opgenomen in bovenstaande tabel.

### Aandachtspunt

De W3C Software License is afkomstig van het ReSpec-template en dekt de tooling-code. De gepubliceerde standaard-inhoud valt onder CC-BY-4.0 (vermeld in de ReSpec-configuratie). Voor repositories zonder LICENSE-bestand verdient het aanbeveling om expliciet een licentie toe te voegen, relevant voor artikel 15n Auteurswet (text and data mining exceptie).

## Referenties

- GitHub repo: https://github.com/logius-standaarden/logboek-dataverwerkingen
- GitHub extensies: https://github.com/logius-standaarden?q=logboek-extensie
- Werkversie: https://logius-standaarden.github.io/logboek-dataverwerkingen/

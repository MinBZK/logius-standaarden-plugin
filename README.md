# Logius Standaarden - Claude Code Plugin

[![EUPL-1.2](https://img.shields.io/badge/licentie-EUPL--1.2-blue.svg)](LICENSE)
[![versie](https://img.shields.io/badge/versie-1.0.0-green.svg)](CHANGELOG.md)

Claude Code plugin voor het werken met de 88 GitHub repositories van [Logius standaarden](https://github.com/logius-standaarden) voor Nederlandse overheidsinteroperabiliteit.

## Installatie

```bash
git clone https://github.com/MinBZK/logius-standaarden-plugin.git
claude --plugin-dir ./logius-standaarden-plugin
```

## Wat doet deze plugin?

De plugin biedt 10 skills die een AI-agent helpen bij:

- **Implementeren** van standaard-conforme code (OAuth, Digikoppeling, CloudEvents, etc.)
- **Navigeren** door de 88 repositories, gegroepeerd in 9 domeinen
- **Ophalen** van actuele content via `gh api` en `WebFetch`
- **Valideren** met Spectral linter, WCAG checks, markdown linting (via `/ls-pub`)
- **Kiezen** van het juiste profiel of protocol via beslisbomen en keuzematrices

### Agent-instructive aanpak

Skills zijn geschreven als **agent-instructies**, niet als encyclopedie. Elke SKILL.md bevat:

1. Wanneer de skill gebruikt moet worden
2. Repository-overzicht
3. Beslisbomen en keuzematrices
4. Werkende implementatievoorbeelden
5. Domein-specifieke foutafhandeling

Achtergrondkennis (architectuur, protocollen, concepten) staat in `reference.md` bestanden die de agent kan raadplegen wanneer nodig.

## Skills

| Skill | Domein | Repos | Beschrijving |
|-------|--------|-------|-------------|
| `/ls` | Overzicht | - | Meta-skill: routeert naar het juiste domein |
| `/ls-dk` | Digikoppeling | 17 | Beveiligde gegevensuitwisseling (REST, ebMS2, WUS, GB), OIN |
| `/ls-api` | API Design Rules | 10 | NL GOV API-standaard, Spectral linter, referentie-implementaties |
| `/ls-iam` | Identity & Access Management | 8 | OAuth 2.0 NL, OpenID Connect, AuthZEN, SAML |
| `/ls-fsc` | Federated Service Connectivity | 7 | Federatief netwerk: inway/outway, service directory |
| `/ls-logboek` | Logboek Dataverwerkingen | 9 | AVG-logging van dataverwerkingen, Docker demo |
| `/ls-notif` | CloudEvents & Notificaties | 4 | NL GOV CloudEvents profiel, pub/sub |
| `/ls-bomos` | BOMOS Governance | 10 | Beheer- en Ontwikkelmodel voor Open Standaarden |
| `/ls-egov` | E-Government Services | 6 | Terugmelding, Digimelding, e-procurement |
| `/ls-pub` | Publicatie & Tooling | 8 | ReSpec, GitHub Actions, WCAG checks, markdown lint |

## Voorbeeldvragen

```
Welke Logius standaarden zijn er?
Wat zijn de API Design Rules?
Toon de laatste wijzigingen aan de Digikoppeling architectuur
Run de Spectral linter op mijn OpenAPI spec
Hoe implementeer ik OAuth 2.0 NL profiel?
Wat is het verschil tussen ebMS2 en REST koppelvlak?
Hoe stel ik een BOMOS beheermodel op?
Run een WCAG check op mijn document
```

## Structuur

```
.claude-plugin/
  plugin.json              # Plugin manifest met versie en upstream tracking
skills/
  ls/SKILL.md              # Meta/overzicht skill
  ls-api/
    SKILL.md               # Agent-instructies en implementatievoorbeelden
    reference.md           # Achtergrondkennis en repo-exploratie
  ls-dk/
    SKILL.md
    reference.md
  ...                      # Idem voor alle 9 domein-skills
```

## Content strategie

Skills bevatten compacte agent-instructies, beslisbomen en implementatievoorbeelden. Encyclopedische achtergrondkennis staat in `reference.md`. Actuele content wordt on-demand opgehaald via `gh api` en `WebFetch` - de GitHub repos zelf zijn de bron van waarheid.

## Versioning

De hele plugin wordt als 1 eenheid geversioned (semver):

- **Patch** (`1.0.1`): Typos, broken links, kleine correcties
- **Minor** (`1.1.0`): Content sync met upstream standaard-wijzigingen, nieuwe voorbeelden
- **Major** (`2.0.0`): Nieuwe skills, verwijderde skills, breaking changes in structuur

Upstream tracking van de bronreposities staat in `plugin.json` onder `upstreamSync`.

## Vereisten

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) CLI
- [GitHub CLI](https://cli.github.com/) (`gh`) - voor het ophalen van repo-content
- Node.js - voor ReSpec, Spectral en andere npm-tools (optioneel, voor validatie)

## Licentie

[EUPL-1.2](LICENSE) - European Union Public Licence

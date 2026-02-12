---
name: ls
description: "Overzicht van alle Logius standaarden. Gebruik deze skill wanneer de gebruiker vraagt over 'Logius standaarden', 'Nederlandse overheidsstandaarden', 'interoperabiliteit', 'welke standaarden zijn er', 'standaarden overzicht', of een vraag heeft die niet duidelijk bij één domein past."
model: sonnet
allowed-tools:
  - Bash(gh api *)
  - Bash(gh issue list *)
  - Bash(gh pr list *)
  - Bash(gh search *)
  - Bash(curl -s *)
  - WebFetch(*)
---

# Logius Standaarden - Overzicht

Logius beheert Nederlandse overheidsstandaarden voor interoperabiliteit. De 88 GitHub repositories onder [github.com/logius-standaarden](https://github.com/logius-standaarden) zijn verdeeld over 9 domeinen.

## Domein Overzicht

| Skill | Domein | Repos | Beschrijving |
|-------|--------|-------|-------------|
| `/ls-dk` | Digikoppeling | 17 | Standaard voor beveiligde gegevensuitwisseling tussen overheidsorganisaties (MSP, MSH, MSL). Architectuur, koppelvlakken (REST, ebMS2, WUS, GB), beveiliging, OIN |
| `/ls-api` | API Design Rules | 10 | Nederlandse API-standaard (NL GOV). Ontwerp, beveiliging, geospatial module, Spectral linter met 30+ regels, 5 referentie-implementaties |
| `/ls-iam` | Identity & Access Management | 8 | OAuth 2.0 NL profiel, OpenID Connect NL GOV, AuthZEN, SAML. Authenticatie en autorisatie voor overheids-APIs |
| `/ls-fsc` | Federated Service Connectivity | 7 | Federatieve dienstverlening: core protocol, logging, properties, regulated areas. Eigen test suite |
| `/ls-logboek` | Logboek Dataverwerkingen | 9 | Transparante logging van dataverwerkingen door overheid. REST API, juridisch kader, NEN 7513 extensie, Docker demo |
| `/ls-notif` | CloudEvents & Notificaties | 4 | NL GOV profiel voor CloudEvents, notificatieservices, abonnementen |
| `/ls-bomos` | BOMOS Governance | 10 | Beheer- en Ontwikkelmodel voor Open Standaarden. Fundament, verdieping, linked data, open source modules |
| `/ls-egov` | E-Government Services | 6 | Terugmelding, Digimelding, basisfactuur/basisorder, e-procurement |
| `/ls-pub` | Publicatie & Tooling | 8 | ReSpec documentatie-tooling, GitHub Actions workflows (build, check, publish), tech radar |

## Hoe te gebruiken

**Stel een vraag over een specifiek domein** en de juiste skill wordt automatisch geactiveerd. Voorbeelden:

- "Wat is Digikoppeling?" → `/ls-dk`
- "Hoe werken de API Design Rules?" → `/ls-api`
- "Wat is het OAuth NL profiel?" → `/ls-iam`
- "Hoe werkt FSC?" → `/ls-fsc`
- "Wat is het Logboek Dataverwerkingen?" → `/ls-logboek`
- "Hoe werken CloudEvents?" → `/ls-notif`
- "Wat is BOMOS?" → `/ls-bomos`
- "Hoe werkt Terugmelding?" → `/ls-egov`
- "Hoe publiceer ik een standaard?" → `/ls-pub`

## Tests & Validatie

Alle standaarden gebruiken gedeelde GitHub Actions workflows uit de `Automatisering` repo:
- **build.yml** - ReSpec HTML/PDF generatie
- **check.yml** - WCAG (Axe Core), markdown linting, link validatie (Muffet)
- **publish.yml** - Publicatie naar centrale `publicatie` repo

Sommige domeinen hebben aanvullende test-tooling. Zie de individuele domein-skills voor details.

## Handige Commando's

```bash
# Alle repos van logius-standaarden
gh api orgs/logius-standaarden/repos --paginate --jq '.[].name' | sort

# Zoek in alle repos
gh search code "zoekterm" --owner logius-standaarden

# Recent gewijzigde repos
gh api orgs/logius-standaarden/repos --paginate --jq 'sort_by(.pushed_at) | reverse | .[:10] | .[] | "\(.pushed_at) \(.name)"'

# Open issues over alle repos
gh search issues --owner logius-standaarden --state open --sort created

# Alle workflows bekijken
gh api repos/logius-standaarden/Automatisering/contents/.github/workflows --jq '.[].name'
```

## Gerelateerde Skills

| Skill | Domein |
|-------|--------|
| `/ls-dk` | Digikoppeling - beveiligde gegevensuitwisseling |
| `/ls-api` | API Design Rules - NL GOV API-standaard |
| `/ls-iam` | OAuth 2.0, OpenID Connect, AuthZEN |
| `/ls-fsc` | Federated Service Connectivity |
| `/ls-logboek` | Logboek Dataverwerkingen |
| `/ls-notif` | CloudEvents & Notificaties |
| `/ls-bomos` | BOMOS Governance |
| `/ls-egov` | Terugmelding, Digimelding, e-procurement |
| `/ls-pub` | Publicatie & Tooling |

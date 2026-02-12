---
name: ls-logboek
description: "Gebruik deze skill wanneer de gebruiker vraagt over 'Logboek Dataverwerkingen', 'dataverwerkingen logging', 'transparantie dataverwerkingen', 'NEN 7513', 'logging API overheid', 'logboek extensie', 'AVG logging', 'GDPR logging', 'verwerkingenlogging'."
model: sonnet
allowed-tools:
  - Bash(gh api *)
  - Bash(gh issue list *)
  - Bash(gh pr list *)
  - Bash(gh search *)
  - Bash(curl -s *)
  - Bash(npx markdownlint *)
  - Bash(npx @axe-core/cli *)
  - Bash(docker compose *)
  - Bash(make *)
  - WebFetch(*)
---

# Logboek Dataverwerkingen

Het Logboek Dataverwerkingen is de standaard voor het transparant vastleggen van dataverwerkingen door overheidsorganisaties. Het biedt een REST API voor het registreren en raadplegen van verwerkingsactiviteiten, in lijn met de AVG/GDPR verantwoordingsplicht.

## Repositories

| Repository | Beschrijving | Publicatie |
|-----------|-------------|-----------|
| [logboek-dataverwerkingen](https://github.com/logius-standaarden/logboek-dataverwerkingen) | Hoofdspecificatie met REST API definitie | [Lees online](https://logius-standaarden.github.io/logboek-dataverwerkingen/) |
| [logboek-dataverwerkingen-inleiding](https://github.com/logius-standaarden/logboek-dataverwerkingen-inleiding) | Introductie en achtergrond | [Lees online](https://logius-standaarden.github.io/logboek-dataverwerkingen-inleiding/) |
| [logboek-dataverwerkingen-juridisch-beleidskader](https://github.com/logius-standaarden/logboek-dataverwerkingen-juridisch-beleidskader) | Juridisch en beleidskader | [Lees online](https://logius-standaarden.github.io/logboek-dataverwerkingen-juridisch-beleidskader/) |
| [logboek-dataverwerkingen-demo](https://github.com/logius-standaarden/logboek-dataverwerkingen-demo) | Docker demo-omgeving met meerdere services | - |
| [logboek-extensie-lezen](https://github.com/logius-standaarden/logboek-extensie-lezen) | Extensie: leesoperaties op het logboek | [Lees online](https://logius-standaarden.github.io/logboek-extensie-lezen/) |
| [logboek-extensie-nen7513](https://github.com/logius-standaarden/logboek-extensie-nen7513) | Extensie: NEN 7513 (logging in de zorg) | [Lees online](https://logius-standaarden.github.io/logboek-extensie-nen7513/) |
| [logboek-extensie-object](https://github.com/logius-standaarden/logboek-extensie-object) | Extensie: objectgegevens bij verwerkingen | [Lees online](https://logius-standaarden.github.io/logboek-extensie-object/) |
| [logboek-extensie-template](https://github.com/logius-standaarden/logboek-extensie-template) | Template voor nieuwe extensies | [Lees online](https://logius-standaarden.github.io/logboek-extensie-template/) |
| [logboek-dataverwerkingen_Juridisch-beleidskader](https://github.com/logius-standaarden/logboek-dataverwerkingen_Juridisch-beleidskader) | Alternatieve juridisch beleidskader repo | - |

## Kernconcepten

- **Verwerkingsactiviteit** - Een operatie op persoonsgegevens die gelogd moet worden
- **Register van verwerkingsactiviteiten** - AVG Art. 30 verplicht register
- **Logboek API** - REST API voor het schrijven en lezen van log entries
- **Extensies** - Modulaire uitbreidingen (lezen, NEN 7513, object)
- **NEN 7513** - Norm voor logging in de zorg, specifieke extensie
- **Verantwoordingsplicht** - AVG-verplichting om verwerkingen te kunnen verantwoorden
- **Doelbinding** - Verwerking alleen voor het vastgestelde doel
- **Betrokkene** - De persoon wiens gegevens worden verwerkt
- **Verwerkingsverantwoordelijke** - Organisatie verantwoordelijk voor de verwerking
- **Verwerker** - Partij die namens de verantwoordelijke verwerkt

## Tests & Validatie

### Docker Demo-omgeving
Complete demo met meerdere microservices:

```bash
# Demo repo structuur bekijken
gh api repos/logius-standaarden/logboek-dataverwerkingen-demo/contents --jq '.[].name'

# Docker compose configuratie
gh api repos/logius-standaarden/logboek-dataverwerkingen-demo/contents/docker-compose.yml -H "Accept: application/vnd.github.raw"

# Demo starten (na klonen)
gh repo clone logius-standaarden/logboek-dataverwerkingen-demo /tmp/logboek-demo
cd /tmp/logboek-demo && docker compose up -d

# Of via Makefile
make build && make run
```

Services in de demo: currus, grafana, lamina, munera.

### Centrale Checks
```bash
# WCAG check
npx @axe-core/cli https://logius-standaarden.github.io/logboek-dataverwerkingen/ --tags wcag2aa

# Markdown lint
npx markdownlint-cli '**/*.md'
```

## Handige Commando's

```bash
# Laatste wijzigingen hoofdspec
gh api repos/logius-standaarden/logboek-dataverwerkingen/commits --jq '.[:5] | .[] | "\(.commit.committer.date) \(.commit.message | split("\n")[0])"'

# Alle logboek repos
gh api orgs/logius-standaarden/repos --paginate --jq '[.[] | select(.name | startswith("logboek"))] | sort_by(.name) | .[].name'

# Open issues
gh issue list --repo logius-standaarden/logboek-dataverwerkingen

# API specificatie ophalen
gh search code "openapi" --repo logius-standaarden/logboek-dataverwerkingen
```

## Gerelateerde Skills

| Skill | Relatie |
|-------|---------|
| `/ls-iam` | Authorization Decision Log als aanvulling op logboek |
| `/ls-fsc` | FSC transactielogging raakt aan logboek |
| `/ls-dk` | Digikoppeling MSL-logging |
| `/ls-pub` | ReSpec tooling voor documentatie |
| `/ls` | Overzicht alle standaarden |

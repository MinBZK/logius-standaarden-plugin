---
name: ls-api
description: "Gebruik deze skill wanneer de gebruiker vraagt over 'API Design Rules', 'ADR', 'REST API standaard', 'API richtlijnen', 'NL GOV API', 'Spectral linter', 'API linter', 'OpenAPI validatie', 'API beveiliging', 'transport security', 'API signing', 'API encryption', 'geospatial API', of 'api-linter'."
model: sonnet
allowed-tools:
  - Bash(gh api *)
  - Bash(gh issue list *)
  - Bash(gh pr list *)
  - Bash(gh search *)
  - Bash(curl -s *)
  - Bash(npx @stoplight/spectral-cli *)
  - Bash(npx markdownlint *)
  - Bash(npx @axe-core/cli *)
  - Bash(node *)
  - Bash(uv run *)
  - WebFetch(*)
---

# API Design Rules (NL GOV)

De API Design Rules (ADR) zijn de Nederlandse standaard voor het ontwerpen van RESTful APIs bij de overheid. Ze zijn verplicht onder het "pas-toe-of-leg-uit" regime van het Forum Standaardisatie.

## Repositories

| Repository | Beschrijving | Publicatie |
|-----------|-------------|-----------|
| [API-Design-Rules](https://github.com/logius-standaarden/API-Design-Rules) | Hoofdspecificatie met alle ontwerp- en technische regels | [Lees online](https://logius-standaarden.github.io/API-Design-Rules/) |
| [ADR-Beheermodel](https://github.com/logius-standaarden/ADR-Beheermodel) | Beheermodel specifiek voor de ADR standaard | [Lees online](https://logius-standaarden.github.io/ADR-Beheermodel/) |
| [API-Standaarden-Beheermodel](https://github.com/logius-standaarden/API-Standaarden-Beheermodel) | Overkoepelend beheermodel voor alle API-standaarden | [Lees online](https://logius-standaarden.github.io/API-Standaarden-Beheermodel/) |
| [API-mod-transport-security](https://github.com/logius-standaarden/API-mod-transport-security) | Module: Transport Security (TLS, certificaten) | [Lees online](https://logius-standaarden.github.io/API-mod-transport-security/) |
| [API-mod-signing](https://github.com/logius-standaarden/API-mod-signing) | Module: HTTP Message Signing | [Lees online](https://logius-standaarden.github.io/API-mod-signing/) |
| [API-mod-encryption](https://github.com/logius-standaarden/API-mod-encryption) | Module: Encryption (JWE) | [Lees online](https://logius-standaarden.github.io/API-mod-encryption/) |
| [API-mod-geospatial](https://github.com/logius-standaarden/API-mod-geospatial) | Module: Geospatial (GeoJSON, CRS) | [Lees online](https://logius-standaarden.github.io/API-mod-geospatial/) |
| [api-linter-impactanalyse](https://github.com/logius-standaarden/api-linter-impactanalyse) | Python tool: test Spectral regels tegen echte OpenAPI specs uit het API-register | - |
| [Respec-example-API-Designrules](https://github.com/logius-standaarden/Respec-example-API-Designrules) | ReSpec template voorbeeld voor ADR documentatie | - |
| [zaakgericht-werken-api](https://github.com/logius-standaarden/zaakgericht-werken-api) | API-specificatie voor zaakgericht werken | - |

## Kernconcepten

- **Normatieve regels** - Verplichte regels gemarkeerd met `/core/` of `/naming/` prefixes
- **Modules** - Uitbreidingsmodules voor specifieke domeinen (transport security, signing, encryption, geospatial)
- **Spectral linter** - Geautomatiseerde validatie van OpenAPI specs tegen de ADR regels met 30+ custom rules
- **Pas-toe-of-leg-uit** - Verplicht voor overheidsorganisaties via Forum Standaardisatie
- **Semantic versioning** - Regels zijn individueel geversioned en refereerbaar
- **OpenAPI 3.x** - Specs moeten voldoen aan OpenAPI 3.0 of 3.1
- **JSON/HAL** - Aanbevolen response formaten
- **Pagination** - Standaard paginering-patterns
- **Error handling** - RFC 7807 Problem Details for HTTP APIs
- **Naming conventions** - camelCase voor JSON velden, kebab-case voor URLs

## Tests & Validatie

### Spectral Linter (30+ regels)
De `API-Design-Rules` repo bevat een Spectral ruleset voor geautomatiseerde OpenAPI validatie:

```bash
# Lint een OpenAPI spec tegen de ADR regels
gh api repos/logius-standaarden/API-Design-Rules/contents/linter/spectral.yml -H "Accept: application/vnd.github.raw" > /tmp/adr-spectral.yml
npx @stoplight/spectral-cli lint <jouw-spec.yaml> --ruleset /tmp/adr-spectral.yml

# Bekijk beschikbare regels
gh api repos/logius-standaarden/API-Design-Rules/contents/linter/spectral.yml -H "Accept: application/vnd.github.raw" | grep "nlgov:"

# Linter testcases draaien
gh api repos/logius-standaarden/API-Design-Rules/contents/linter/testcases --jq '.[].name'
```

### Voorbeeldimplementaties
5 referentie-implementaties in verschillende talen, elk met een `build-and-check-project.sh`:

```bash
# Lijst voorbeeldimplementaties
gh api repos/logius-standaarden/API-Design-Rules/contents/examples --jq '.[].name'
# Verwacht: aspnet, express, go, python, quarkus
```

### Impact Analyse Tool
Python tool die Spectral regels test tegen echte OpenAPI specs uit het API-register:

```bash
# Kloon en draai de impact analyse
gh repo clone logius-standaarden/api-linter-impactanalyse /tmp/api-linter
cd /tmp/api-linter
uv run run-spectral-for-single-rule.py --rule nlgov:paths-no-trailing-slash
```

### Centrale Checks
```bash
# WCAG accessibility check
npx @axe-core/cli https://logius-standaarden.github.io/API-Design-Rules/ --tags wcag2aa

# Markdown lint
npx markdownlint-cli '**/*.md'
```

## Handige Commando's

```bash
# Laatste wijzigingen aan de ADR
gh api repos/logius-standaarden/API-Design-Rules/commits --jq '.[:5] | .[] | "\(.commit.committer.date) \(.commit.message | split("\n")[0])"'

# Open issues
gh issue list --repo logius-standaarden/API-Design-Rules

# Open pull requests
gh pr list --repo logius-standaarden/API-Design-Rules

# Inhoud van een specifiek bestand
gh api repos/logius-standaarden/API-Design-Rules/contents/README.md -H "Accept: application/vnd.github.raw"

# Zoek naar een specifiek onderwerp
gh search code "pagination" --repo logius-standaarden/API-Design-Rules
```

## Gerelateerde Skills

| Skill | Relatie |
|-------|---------|
| `/ls-iam` | OAuth/OIDC profielen voor API-authenticatie |
| `/ls-dk` | Digikoppeling REST-API koppelvlak gebruikt ADR |
| `/ls-pub` | ReSpec tooling voor ADR documentatie |
| `/ls` | Overzicht alle standaarden |

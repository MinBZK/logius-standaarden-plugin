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

De E-Government standaarden omvatten diensten voor terugmelding op basisregistraties, Digimelding, en e-procurement (elektronisch bestellen en factureren) voor de Nederlandse overheid.

## Repositories

| Repository | Beschrijving | Publicatie |
|-----------|-------------|-----------|
| [Terugmelding](https://github.com/logius-standaarden/Terugmelding) | Standaard voor terugmelden op basisregistraties | [Lees online](https://logius-standaarden.github.io/Terugmelding/) |
| [Terugmelden-API](https://github.com/logius-standaarden/Terugmelden-API) | REST API specificatie (OpenAPI 3.1.0) voor terugmelden | [Lees online](https://logius-standaarden.github.io/Terugmelden-API/) |
| [Digimelding-Koppelvlakspecificatie](https://github.com/logius-standaarden/Digimelding-Koppelvlakspecificatie) | Koppelvlakspecificatie voor Digimelding | [Lees online](https://logius-standaarden.github.io/Digimelding-Koppelvlakspecificatie/) |
| [basisfactuur-rijk](https://github.com/logius-standaarden/basisfactuur-rijk) | Standaard basisfactuur voor de Rijksoverheid | [Lees online](https://logius-standaarden.github.io/basisfactuur-rijk/) |
| [basisorder-rijk](https://github.com/logius-standaarden/basisorder-rijk) | Standaard basisorder voor de Rijksoverheid | [Lees online](https://logius-standaarden.github.io/basisorder-rijk/) |
| [e-procurement](https://github.com/logius-standaarden/e-procurement) | Overkoepelende e-procurement standaarden | [Lees online](https://logius-standaarden.github.io/e-procurement/) |

## Kernconcepten

- **Terugmelding** - Melding dat gegevens in een basisregistratie mogelijk onjuist zijn
- **Basisregistraties** - Authentieke overheidsregistraties (BRP, BAG, BRK, etc.)
- **Digimelding** - Voorziening voor het doen van terugmeldingen
- **Bronhouder** - Beheerder van een basisregistratie die terugmeldingen afhandelt
- **Basisfactuur** - Gestandaardiseerd factuurformaat voor Rijksoverheid (conform EU-richtlijn)
- **Basisorder** - Gestandaardiseerd inkooporderformaat
- **PEPPOLBIS** - Europese standaard voor e-procurement berichten
- **UBL** - Universal Business Language, basis voor factuur- en orderberichten
- **OpenAPI 3.1.0** - Terugmelden-API is gespecificeerd in OpenAPI 3.1.0

## Tests & Validatie

### Terugmelden-API OpenAPI Spec
```bash
# OpenAPI spec ophalen
gh search code "openapi" --repo logius-standaarden/Terugmelden-API

# Valideer de OpenAPI spec met Spectral
gh api repos/logius-standaarden/Terugmelden-API/contents --jq '[.[] | select(.name | test("\\.(yaml|json)$"))] | .[].name'
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

# Open issues
gh issue list --repo logius-standaarden/Terugmelding
gh issue list --repo logius-standaarden/Terugmelden-API

# Basisfactuur inhoud
gh api repos/logius-standaarden/basisfactuur-rijk/contents --jq '.[].name'

# Zoek naar e-procurement documentatie
gh search code "PEPPOLBIS" --owner logius-standaarden
```

## Gerelateerde Skills

| Skill | Relatie |
|-------|---------|
| `/ls-api` | Terugmelden-API volgt de API Design Rules |
| `/ls-dk` | Digimelding kan via Digikoppeling koppelvlakken |
| `/ls-pub` | ReSpec tooling voor documentatie |
| `/ls` | Overzicht alle standaarden |

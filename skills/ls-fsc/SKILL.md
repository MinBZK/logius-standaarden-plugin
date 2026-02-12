---
name: ls-fsc
description: "Gebruik deze skill wanneer de gebruiker vraagt over 'FSC', 'Federated Service Connectivity', 'federatieve dienstverlening', 'inway', 'outway', 'service directory', 'regulated area', 'external contract', 'fsc-core'."
model: sonnet
allowed-tools:
  - Bash(gh api *)
  - Bash(gh issue list *)
  - Bash(gh pr list *)
  - Bash(gh search *)
  - Bash(curl -s *)
  - Bash(npx markdownlint *)
  - Bash(npx @axe-core/cli *)
  - Bash(node *)
  - Bash(docker compose *)
  - WebFetch(*)
---

# Federated Service Connectivity (FSC)

FSC is de standaard voor federatieve dienstverlening binnen de Nederlandse overheid. Het definieert hoe organisaties op een gestandaardiseerde manier services kunnen aanbieden en afnemen via een federatief netwerk, met inway/outway componenten en een service directory.

## Repositories

| Repository | Beschrijving | Publicatie |
|-----------|-------------|-----------|
| [fsc-core](https://github.com/logius-standaarden/fsc-core) | Core specificatie: architectuur, protocol, componenten | [Lees online](https://logius-standaarden.github.io/fsc-core/) |
| [fsc-logging](https://github.com/logius-standaarden/fsc-logging) | Logging specificatie voor FSC transacties | [Lees online](https://logius-standaarden.github.io/fsc-logging/) |
| [fsc-properties](https://github.com/logius-standaarden/fsc-properties) | Metadata-properties voor FSC services | [Lees online](https://logius-standaarden.github.io/fsc-properties/) |
| [fsc-regulated-area](https://github.com/logius-standaarden/fsc-regulated-area) | Regulated Area specificatie (governance zones) | [Lees online](https://logius-standaarden.github.io/fsc-regulated-area/) |
| [fsc-external-contract](https://github.com/logius-standaarden/fsc-external-contract) | External Contract specificatie (afspraken tussen partijen) | [Lees online](https://logius-standaarden.github.io/fsc-external-contract/) |
| [fsc-extensie-template](https://github.com/logius-standaarden/fsc-extensie-template) | Template voor FSC extensies | [Lees online](https://logius-standaarden.github.io/fsc-extensie-template/) |
| [fsc-test-suite](https://github.com/logius-standaarden/fsc-test-suite) | Integratietests en componenttests | - |

## Kernconcepten

- **Inway** - Component waarmee een organisatie services aanbiedt aan het netwerk
- **Outway** - Component waarmee een organisatie services van andere organisaties afneemt
- **Service Directory** - Register van beschikbare services in het federatief netwerk
- **Manager** - Beheertool voor het configureren van inways, outways en services
- **Regulated Area** - Governance-zone met specifieke regels en afspraken
- **External Contract** - Formele afspraken tussen dienstaanbieder en -afnemer
- **Transaction Log** - Logging van alle transacties voor audit
- **Peer** - Een deelnemende organisatie in het FSC netwerk
- **Delegation** - Mechanisme voor gemachtigde toegang

## Tests & Validatie

### FSC Test Suite
Dedicated test suite met integratie- en componenttests:

```bash
# Bekijk test suite structuur
gh api repos/logius-standaarden/fsc-test-suite/contents --jq '.[].name'

# Integratie test specificatie
gh api repos/logius-standaarden/fsc-test-suite/contents/integration.md -H "Accept: application/vnd.github.raw"

# Manager service tests
gh api repos/logius-standaarden/fsc-test-suite/contents/manager.md -H "Accept: application/vnd.github.raw"

# Inway/outway component tests
gh api repos/logius-standaarden/fsc-test-suite/contents/inway.md -H "Accept: application/vnd.github.raw"
gh api repos/logius-standaarden/fsc-test-suite/contents/outway.md -H "Accept: application/vnd.github.raw"
```

### Centrale Checks
```bash
# WCAG check op FSC core documentatie
npx @axe-core/cli https://logius-standaarden.github.io/fsc-core/ --tags wcag2aa

# Markdown lint
npx markdownlint-cli '**/*.md'
```

## Handige Commando's

```bash
# Alle FSC repos
gh api orgs/logius-standaarden/repos --paginate --jq '[.[] | select(.name | startswith("fsc"))] | sort_by(.name) | .[].name'

# Laatste wijzigingen aan fsc-core
gh api repos/logius-standaarden/fsc-core/commits --jq '.[:5] | .[] | "\(.commit.committer.date) \(.commit.message | split("\n")[0])"'

# Open issues/PRs
gh issue list --repo logius-standaarden/fsc-core
gh pr list --repo logius-standaarden/fsc-core

# Core spec inhoud ophalen
gh api repos/logius-standaarden/fsc-core/contents --jq '.[].name'
```

## Gerelateerde Skills

| Skill | Relatie |
|-------|---------|
| `/ls-dk` | FSC als opvolger/aanvulling voor Digikoppeling |
| `/ls-iam` | Authenticatie en autorisatie binnen FSC |
| `/ls-logboek` | Logging van FSC transacties raakt aan Logboek Dataverwerkingen |
| `/ls-pub` | ReSpec tooling voor FSC documentatie |
| `/ls` | Overzicht alle standaarden |

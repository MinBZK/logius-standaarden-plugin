---
name: ls-pub
description: "Gebruik deze skill wanneer de gebruiker vraagt over 'publicatie', 'ReSpec', 'documentatie tooling', 'GitHub Actions workflows', 'build workflow', 'check workflow', 'publish workflow', 'tech radar', 'WCAG check', 'markdownlint', 'Muffet', 'link validatie', 'automatisering', 'HTML generatie', 'PDF generatie'."
model: sonnet
allowed-tools:
  - Bash(gh api *)
  - Bash(gh issue list *)
  - Bash(gh pr list *)
  - Bash(gh search *)
  - Bash(curl -s *)
  - Bash(npx respec *)
  - Bash(npx markdownlint *)
  - Bash(npx @axe-core/cli *)
  - Bash(npx muffet *)
  - Bash(node *)
  - WebFetch(*)
---

# Publicatie & Tooling

De publicatie-standaarden en tooling vormen de infrastructuur voor het bouwen, valideren en publiceren van alle Logius standaarden. Dit omvat ReSpec (documentatie-generator), GitHub Actions workflows, en de centrale publicatie-repo.

## Repositories

| Repository | Beschrijving | Publicatie |
|-----------|-------------|-----------|
| [publicatie](https://github.com/logius-standaarden/publicatie) | Centrale publicatie-repo: alle gepubliceerde standaarden | [Lees online](https://logius-standaarden.github.io/publicatie/) |
| [Publicatie-Preview](https://github.com/logius-standaarden/Publicatie-Preview) | Preview-omgeving voor documenten in ontwikkeling | [Lees online](https://logius-standaarden.github.io/Publicatie-Preview/) |
| [respec](https://github.com/logius-standaarden/respec) | Logius fork van W3C ReSpec documentatie-tool | - |
| [ReSpec-template](https://github.com/logius-standaarden/ReSpec-template) | Basis template voor nieuwe ReSpec documenten | [Lees online](https://logius-standaarden.github.io/ReSpec-template/) |
| [ReSpec-template-Logius](https://github.com/logius-standaarden/ReSpec-template-Logius) | Logius-specifiek ReSpec template met huisstijl | [Lees online](https://logius-standaarden.github.io/ReSpec-template-Logius/) |
| [Automatisering](https://github.com/logius-standaarden/Automatisering) | Herbruikbare GitHub Actions workflows | - |
| [automatisering-test](https://github.com/logius-standaarden/automatisering-test) | Testomgeving voor de automatiseringworkflows | - |
| [tech-radar](https://github.com/logius-standaarden/tech-radar) | Technologie radar voor Logius standaarden | [Lees online](https://logius-standaarden.github.io/tech-radar/) |

## Kernconcepten

- **ReSpec** - W3C tool voor het genereren van technische specificaties als HTML/PDF vanuit Markdown
- **Logius ReSpec fork** - Aangepaste versie met Logius huisstijl en NL-specifieke features
- **GitHub Actions** - CI/CD workflows voor automatisch bouwen, checken en publiceren
- **Publicatie repo** - Centrale repo waar alle definitieve versies van standaarden worden gepubliceerd
- **Preview** - Voorvertoning van documenten in ontwikkeling (per branch/PR)
- **Tech Radar** - Visualisatie van technologiekeuzes en hun status (adopt/trial/assess/hold)

## Centrale Workflows (Automatisering repo)

De `Automatisering` repo bevat herbruikbare GitHub Actions workflows die door alle standaarden-repos gebruikt worden:

### build.yml - Document Generatie
Bouwt ReSpec documenten naar HTML en PDF:
```bash
# Bekijk build workflow
gh api repos/logius-standaarden/Automatisering/contents/.github/workflows/build.yml -H "Accept: application/vnd.github.raw"

# Lokaal een ReSpec document bouwen
npx respec --src index.html --out output.html
```

### check.yml - Kwaliteitschecks
Voert drie checks uit op elk document:
```bash
# 1. WCAG Accessibility (Axe Core, wcag2aa)
npx @axe-core/cli https://logius-standaarden.github.io/<repo>/ --tags wcag2aa

# 2. Markdown linting
npx markdownlint-cli '**/*.md'

# 3. Link validatie (Muffet)
npx muffet https://logius-standaarden.github.io/<repo>/

# Bekijk check workflow
gh api repos/logius-standaarden/Automatisering/contents/.github/workflows/check.yml -H "Accept: application/vnd.github.raw"
```

### publish.yml - Publicatie
Publiceert definitieve versies naar de centrale `publicatie` repo:
```bash
# Bekijk publish workflow
gh api repos/logius-standaarden/Automatisering/contents/.github/workflows/publish.yml -H "Accept: application/vnd.github.raw"
```

### label-updates.yml - PR Labels
Automatische labels op PRs op basis van status:
```bash
# Bekijk label workflow
gh api repos/logius-standaarden/Automatisering/contents/.github/workflows/label-updates.yml -H "Accept: application/vnd.github.raw"
```

## Tests & Validatie

Dit domein IS de test- en validatie-infrastructuur. Gebruik deze commando's om de tooling zelf te onderzoeken:

```bash
# Alle workflows in Automatisering
gh api repos/logius-standaarden/Automatisering/contents/.github/workflows --jq '.[].name'

# ReSpec configuratie voorbeeld
gh api repos/logius-standaarden/ReSpec-template-Logius/contents/js/config.js -H "Accept: application/vnd.github.raw"

# Test de automatisering
gh api repos/logius-standaarden/automatisering-test/contents --jq '.[].name'

# Publicatie repo structuur
gh api repos/logius-standaarden/publicatie/contents --jq '.[].name'
```

## Handige Commando's

```bash
# Laatste wijzigingen aan Automatisering
gh api repos/logius-standaarden/Automatisering/commits --jq '.[:5] | .[] | "\(.commit.committer.date) \(.commit.message | split("\n")[0])"'

# Welke repos gebruiken welke workflows
gh search code "logius-standaarden/Automatisering" --owner logius-standaarden --filename "*.yml"

# Tech radar inhoud
gh api repos/logius-standaarden/tech-radar/contents --jq '.[].name'
```

## Gerelateerde Skills

| Skill | Relatie |
|-------|---------|
| `/ls-bomos` | BOMOS beschrijft het beheerproces dat deze tooling ondersteunt |
| `/ls-api` | API Design Rules repo bevat eigen linter naast centrale checks |
| `/ls-fsc` | FSC heeft eigen test suite naast centrale checks |
| `/ls` | Overzicht alle standaarden |

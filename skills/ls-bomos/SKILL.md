---
name: ls-bomos
description: "Gebruik deze skill wanneer de gebruiker vraagt over 'BOMOS', 'Beheer- en Ontwikkelmodel', 'open standaarden beheer', 'standaardisatie governance', 'beheermodel', 'community management standaarden', 'linked data standaarden', 'open source governance', 'stelselstandaarden'."
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

# BOMOS - Beheer- en Ontwikkelmodel voor Open Standaarden

BOMOS biedt een raamwerk voor het beheer en de ontwikkeling van open standaarden. Het beschrijft processen, rollen en instrumenten die nodig zijn om een standaard succesvol te beheren en door te ontwikkelen.

## Repositories

| Repository | Beschrijving | Publicatie |
|-----------|-------------|-----------|
| [BOMOS-Fundament](https://github.com/logius-standaarden/BOMOS-Fundament) | Kernmodel: activiteiten, rollen, structuur | [Lees online](https://logius-standaarden.github.io/BOMOS-Fundament/) |
| [BOMOS-Verdieping](https://github.com/logius-standaarden/BOMOS-Verdieping) | Verdiepende beschrijving van BOMOS activiteiten | [Lees online](https://logius-standaarden.github.io/BOMOS-Verdieping/) |
| [BOMOS-Aanvullende-Modules](https://github.com/logius-standaarden/BOMOS-Aanvullende-Modules) | Aanvullende modules voor specifieke aspecten | [Lees online](https://logius-standaarden.github.io/BOMOS-Aanvullende-Modules/) |
| [BOMOS-LinkedData](https://github.com/logius-standaarden/BOMOS-LinkedData) | Module: beheer van Linked Data standaarden | [Lees online](https://logius-standaarden.github.io/BOMOS-LinkedData/) |
| [BOMOS-OpenSource](https://github.com/logius-standaarden/BOMOS-OpenSource) | Module: open source governance | [Lees online](https://logius-standaarden.github.io/BOMOS-OpenSource/) |
| [BOMOS-Stelsels](https://github.com/logius-standaarden/BOMOS-Stelsels) | Module: beheer van stelsels van standaarden | [Lees online](https://logius-standaarden.github.io/BOMOS-Stelsels/) |
| [BOMOS-Beheermodel](https://github.com/logius-standaarden/BOMOS-Beheermodel) | Beheermodel voor BOMOS zelf | [Lees online](https://logius-standaarden.github.io/BOMOS-Beheermodel/) |
| [BOMOS-voorbeeld-beheermodel](https://github.com/logius-standaarden/BOMOS-voorbeeld-beheermodel) | Voorbeelddocument voor een beheermodel | [Lees online](https://logius-standaarden.github.io/BOMOS-voorbeeld-beheermodel/) |
| [BOMOS-Community](https://github.com/logius-standaarden/BOMOS-Community) | Community-aspecten van standaardenbeheer | [Lees online](https://logius-standaarden.github.io/BOMOS-Community/) |
| [Logius-Beheermodel](https://github.com/logius-standaarden/Logius-Beheermodel) | Overkoepelend beheermodel van Logius | [Lees online](https://logius-standaarden.github.io/Logius-Beheermodel/) |

## Kernconcepten

- **BOMOS activiteitenmodel** - Vijf hoofdactiviteiten: strategie, tactiek, operationeel, implementatieondersteuning, communicatie
- **Strategielaag** - Visie, governance, financiering
- **Tactieklaag** - Architectuur, specificatiebeheer, wijzigingsbeheer
- **Operationele laag** - Documentatie, helpdesk, tools
- **Implementatieondersteuning** - Pilots, certificering, validatie
- **Communicatielaag** - Promotie, community, events
- **Adoptiefasen** - Ontwikkeling → Introductie → Implementatie → Beheer
- **Linked Data module** - Specifieke richtlijnen voor semantische standaarden (ontologieën, vocabulaires)
- **Open Source module** - Governance voor open source componenten bij standaarden

## Tests & Validatie

BOMOS repos gebruiken de centrale `Automatisering` workflows:

```bash
# WCAG check
npx @axe-core/cli https://logius-standaarden.github.io/BOMOS-Fundament/ --tags wcag2aa

# Markdown lint
npx markdownlint-cli '**/*.md'
```

## Handige Commando's

```bash
# Alle BOMOS repos
gh api orgs/logius-standaarden/repos --paginate --jq '[.[] | select(.name | startswith("BOMOS"))] | sort_by(.name) | .[].name'

# Laatste wijzigingen fundament
gh api repos/logius-standaarden/BOMOS-Fundament/commits --jq '.[:5] | .[] | "\(.commit.committer.date) \(.commit.message | split("\n")[0])"'

# Open issues
gh issue list --repo logius-standaarden/BOMOS-Fundament

# BOMOS Fundament inhoud
gh api repos/logius-standaarden/BOMOS-Fundament/contents --jq '.[].name'

# Zoek naar specifieke BOMOS concepten
gh search code "adoptie" --owner logius-standaarden --filename "*.md"
```

## Gerelateerde Skills

| Skill | Relatie |
|-------|---------|
| `/ls-pub` | Publicatie-tooling wordt gebruikt voor BOMOS documenten |
| `/ls-dk` | Digikoppeling Beheermodel is gebaseerd op BOMOS |
| `/ls-api` | ADR Beheermodel is gebaseerd op BOMOS |
| `/ls` | Overzicht alle standaarden |

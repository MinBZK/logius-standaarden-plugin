---
name: ls-iam
description: "Gebruik deze skill wanneer de gebruiker vraagt over 'OAuth', 'OpenID Connect', 'OIDC', 'authenticatie', 'autorisatie', 'AuthZEN', 'SAML', 'identity management', 'toegangsbeheer', 'OAuth NL profiel', 'OIDC NL GOV', 'authorization decision', 'OIN authenticatie'."
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

# Identity & Access Management (IAM)

De IAM-standaarden van Logius definiëren profielen voor authenticatie en autorisatie bij Nederlandse overheids-APIs. Dit omvat OAuth 2.0, OpenID Connect en AuthZEN profielen specifiek voor de Nederlandse overheid.

## Repositories

| Repository | Beschrijving | Publicatie |
|-----------|-------------|-----------|
| [OAuth-NL-profiel](https://github.com/logius-standaarden/OAuth-NL-profiel) | Nederlands profiel voor OAuth 2.0 | [Lees online](https://logius-standaarden.github.io/OAuth-NL-profiel/) |
| [OIDC-NLGOV](https://github.com/logius-standaarden/OIDC-NLGOV) | OpenID Connect profiel voor NL overheid | [Lees online](https://logius-standaarden.github.io/OIDC-NLGOV/) |
| [OAuth-Inleiding](https://github.com/logius-standaarden/OAuth-Inleiding) | Introductie en achtergrond OAuth | [Lees online](https://logius-standaarden.github.io/OAuth-Inleiding/) |
| [OAuth-Beheermodel](https://github.com/logius-standaarden/OAuth-Beheermodel) | Beheermodel voor OAuth standaarden | [Lees online](https://logius-standaarden.github.io/OAuth-Beheermodel/) |
| [authzen-nlgov](https://github.com/logius-standaarden/authzen-nlgov) | AuthZEN NL GOV profiel (autorisatiebeslissingen) | [Lees online](https://logius-standaarden.github.io/authzen-nlgov/) |
| [authorization-decision-log](https://github.com/logius-standaarden/authorization-decision-log) | Logging van autorisatiebeslissingen | [Lees online](https://logius-standaarden.github.io/authorization-decision-log/) |
| [st-saml-spec](https://github.com/logius-standaarden/st-saml-spec) | SAML specificatie voor NL overheid | [Lees online](https://logius-standaarden.github.io/st-saml-spec/) |
| [OIN-Stelsel](https://github.com/logius-standaarden/OIN-Stelsel) | Organisatie Identificatie Nummer (ook in `/ls-dk`) | [Lees online](https://logius-standaarden.github.io/OIN-Stelsel/) |

## Kernconcepten

- **OAuth 2.0 NL profiel** - Nederlandse aanscherping van OAuth 2.0 met verplichte PKCE, restricties op grant types
- **OIDC NL GOV** - OpenID Connect profiel met eisen voor betrouwbaarheidsniveaus (LoA)
- **AuthZEN** - Standaard voor externalisering van autorisatiebeslissingen (PDP/PEP model)
- **Authorization Decision Log** - Logging van autorisatiebeslissingen voor audit en verantwoording
- **SAML** - Security Assertion Markup Language, nog in gebruik bij DigiD/eHerkenning
- **OIN** - Organisatie Identificatie Nummer voor identificatie van overheidsorganisaties
- **PKIoverheid** - Certificaten voor server- en client-authenticatie
- **Betrouwbaarheidsniveaus** - Laag, substantieel, hoog (eIDAS-conform)
- **PDP/PEP** - Policy Decision Point / Policy Enforcement Point (AuthZEN)
- **Scopes** - OAuth scopes als mechanisme voor toegangsbeheer

## Tests & Validatie

IAM repos gebruiken de centrale `Automatisering` workflows:

```bash
# WCAG check
npx @axe-core/cli https://logius-standaarden.github.io/OAuth-NL-profiel/ --tags wcag2aa

# Markdown lint
npx markdownlint-cli '**/*.md'

# CI/CD workflows bekijken
gh api repos/logius-standaarden/OAuth-NL-profiel/contents/.github/workflows --jq '.[].name'
```

## Handige Commando's

```bash
# Laatste wijzigingen OAuth NL profiel
gh api repos/logius-standaarden/OAuth-NL-profiel/commits --jq '.[:5] | .[] | "\(.commit.committer.date) \(.commit.message | split("\n")[0])"'

# Open issues
gh issue list --repo logius-standaarden/OAuth-NL-profiel
gh issue list --repo logius-standaarden/OIDC-NLGOV

# AuthZEN spec inhoud
gh api repos/logius-standaarden/authzen-nlgov/contents --jq '.[].name'

# Zoek in IAM repos
gh search code "PKCE" --owner logius-standaarden
```

## Gerelateerde Skills

| Skill | Relatie |
|-------|---------|
| `/ls-api` | API Design Rules vereisen OAuth/OIDC voor authenticatie |
| `/ls-dk` | OIN-stelsel gedeeld met Digikoppeling |
| `/ls-logboek` | Authorization Decision Log raakt aan logging dataverwerkingen |
| `/ls` | Overzicht alle standaarden |

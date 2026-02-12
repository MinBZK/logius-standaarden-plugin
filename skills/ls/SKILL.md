---
name: ls
description: "Overzicht van alle Logius standaarden. Gebruik deze skill wanneer de gebruiker vraagt over 'Logius standaarden', 'Nederlandse overheidsstandaarden', 'interoperabiliteit', 'welke standaarden zijn er', 'standaarden overzicht', 'welke standaard moet ik gebruiken', of een vraag heeft die niet duidelijk bij een domein past."
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

**Agent-instructie:** Gebruik deze skill om de gebruiker naar het juiste domein te routeren. Bij een domein-specifieke vraag, verwijs naar de juiste `/ls-*` skill. Bij organisatie-brede vragen, gebruik de gh commando's hieronder.

Logius beheert 88 GitHub repositories onder [github.com/logius-standaarden](https://github.com/logius-standaarden), verdeeld over 9 domeinen:

| Skill | Domein | Repos | Beschrijving |
|-------|--------|-------|-------------|
| `/ls-dk` | Digikoppeling | 17 | Beveiligde gegevensuitwisseling (REST, ebMS2, WUS, GB), OIN |
| `/ls-api` | API Design Rules | 10 | NL GOV API-standaard, Spectral linter, referentie-implementaties |
| `/ls-iam` | Identity & Access Management | 8 | OAuth 2.0 NL, OpenID Connect, AuthZEN, SAML |
| `/ls-fsc` | Federated Service Connectivity | 7 | Federatief netwerk: inway/outway, contracten, service directory |
| `/ls-logboek` | Logboek Dataverwerkingen | 9 | AVG-logging, OpenTelemetry, W3C Trace Context |
| `/ls-notif` | CloudEvents & Notificaties | 4 | NL GOV CloudEvents profiel, pub/sub |
| `/ls-bomos` | BOMOS Governance | 10 | Beheer- en Ontwikkelmodel voor Open Standaarden |
| `/ls-egov` | E-Government Services | 6 | Terugmelding, Digimelding, e-procurement |
| `/ls-pub` | Publicatie & Tooling | 8 | ReSpec, GitHub Actions, WCAG checks, markdown lint |

## Organisatie-brede Commando's

```bash
# Alle repos van logius-standaarden
gh api orgs/logius-standaarden/repos --paginate --jq '.[].name' | sort

# Zoek in alle repos
gh search code "zoekterm" --owner logius-standaarden

# Recent gewijzigde repos
gh api orgs/logius-standaarden/repos --paginate --jq 'sort_by(.pushed_at) | reverse | .[:10] | .[] | "\(.pushed_at) \(.name)"'

# Open issues over alle repos
gh search issues --owner logius-standaarden --state open --sort created
```

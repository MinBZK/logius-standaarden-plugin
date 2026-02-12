---
name: ls-notif
description: "Gebruik deze skill wanneer de gebruiker vraagt over 'CloudEvents', 'notificaties', 'notificatieservices', 'abonnementen', 'event-driven', 'NL GOV CloudEvents', 'pub/sub overheid', 'gebeurtenisnotificatie'."
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

# CloudEvents & Notificaties

De notificatiestandaarden definiëren hoe overheidsorganisaties gebeurtenissen (events) kunnen publiceren en daarop kunnen abonneren. Gebaseerd op de internationale CloudEvents specificatie met een NL GOV profiel.

## Repositories

| Repository | Beschrijving | Publicatie |
|-----------|-------------|-----------|
| [NL-GOV-profile-for-CloudEvents](https://github.com/logius-standaarden/NL-GOV-profile-for-CloudEvents) | Nederlands overheidsprofiel voor CloudEvents | [Lees online](https://logius-standaarden.github.io/NL-GOV-profile-for-CloudEvents/) |
| [CloudEvents-NL-Guidelines](https://github.com/logius-standaarden/CloudEvents-NL-Guidelines) | Richtlijnen voor gebruik van CloudEvents in NL | [Lees online](https://logius-standaarden.github.io/CloudEvents-NL-Guidelines/) |
| [Notificatieservices](https://github.com/logius-standaarden/Notificatieservices) | Specificatie voor notificatieservices | [Lees online](https://logius-standaarden.github.io/Notificatieservices/) |
| [Abonneren](https://github.com/logius-standaarden/Abonneren) | Specificatie voor het abonneren op notificaties | [Lees online](https://logius-standaarden.github.io/Abonneren/) |

## Kernconcepten

- **CloudEvents** - CNCF standaard voor het beschrijven van events in een cloud-omgeving
- **NL GOV profiel** - Nederlandse aanscherping met verplichte attributen (source, subject, etc.)
- **Notificatieservice** - Dienst die events ontvangt van bronnen en doorstuurt naar abonnees
- **Abonnement** - Registratie van een afnemer om specifieke events te ontvangen
- **Event source** - De bron die een gebeurtenis genereert
- **Event subject** - Het onderwerp waarop de event betrekking heeft
- **Pub/Sub** - Publish/Subscribe patroon voor losse koppeling
- **Webhook** - HTTP callback voor het afleveren van events bij abonnees

## Tests & Validatie

Notificatie repos gebruiken de centrale `Automatisering` workflows:

```bash
# WCAG check
npx @axe-core/cli https://logius-standaarden.github.io/NL-GOV-profile-for-CloudEvents/ --tags wcag2aa

# Markdown lint
npx markdownlint-cli '**/*.md'
```

## Handige Commando's

```bash
# Laatste wijzigingen CloudEvents profiel
gh api repos/logius-standaarden/NL-GOV-profile-for-CloudEvents/commits --jq '.[:5] | .[] | "\(.commit.committer.date) \(.commit.message | split("\n")[0])"'

# Open issues
gh issue list --repo logius-standaarden/NL-GOV-profile-for-CloudEvents
gh issue list --repo logius-standaarden/Notificatieservices

# Inhoud bekijken
gh api repos/logius-standaarden/NL-GOV-profile-for-CloudEvents/contents --jq '.[].name'

# Zoek naar CloudEvents documentatie
gh search code "CloudEvents" --owner logius-standaarden
```

## Gerelateerde Skills

| Skill | Relatie |
|-------|---------|
| `/ls-api` | Notificatie-APIs volgen de API Design Rules |
| `/ls-fsc` | FSC kan CloudEvents transporteren |
| `/ls-logboek` | Events kunnen verwerkingen triggeren die gelogd worden |
| `/ls` | Overzicht alle standaarden |

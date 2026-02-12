# Logius Standaarden Plugin - Werkinstructies

## Taal

Alle content in deze repo is in het **Nederlands**: skill descriptions, body tekst, commit messages, en documentatie. Gebruik Nederlands tenzij de gebruiker expliciet in het Engels communiceert.

## Repo structuur

- `.claude-plugin/plugin.json` - Plugin manifest
- `skills/ls/SKILL.md` - Meta-skill (overzicht, routing)
- `skills/ls-<domein>/SKILL.md` - Domein-skills (9 stuks)

## Conventies voor skills

### SKILL.md opbouw
1. YAML frontmatter met `name`, `description`, `model: sonnet`, `allowed-tools`
2. Korte introductie - wat is deze standaard, voor wie
3. Repository tabel - alle repos met GitHub links en publicatie-URLs
4. Technische secties - architectuur, protocollen, configuratie (per domein)
5. Implementatievoorbeelden - werkende code in Python, JavaScript, curl, XML/JSON
6. Foutafhandeling - error responses, status codes, retry patronen
7. Tests & Validatie - beschikbare checks en commando's
8. Handige commando's - `gh api` en `curl` voorbeelden
9. Gerelateerde skills - kruisverwijzingen

Elke skill moet genoeg bevatten zodat een agent standaard-conforme code kan genereren zonder externe bronnen op te halen.

### Description triggers
Descriptions bevatten zowel Nederlandse als technische Engelse triggerwoorden zodat de skill bij relevante vragen wordt geactiveerd.

### Allowed tools
Pas per skill aan. Niet elke skill heeft alle tools nodig. Standaard set:
- `Bash(gh api *)`, `Bash(gh issue list *)`, `Bash(gh pr list *)`, `Bash(gh search *)`
- `Bash(curl -s *)`, `WebFetch(*)`
- `Bash(npx markdownlint *)`, `Bash(npx @axe-core/cli *)` (centrale checks)

Extra tools alleen waar relevant (bijv. `Bash(npx @stoplight/spectral-cli *)` alleen voor `/ls-api`).

## Content strategie

**On-demand ophalen, niet opslaan.** De GitHub repos van logius-standaarden zijn de bron van waarheid. Skills bevatten samenvattingen en structuur; actuele content wordt live opgehaald via `gh api` of `WebFetch`.

## GitHub organisatie

Alle repos staan onder: `https://github.com/logius-standaarden/`
Publicaties op: `https://logius-standaarden.github.io/<repo>/`

## Testen

```bash
claude --plugin-dir /Users/anneschuth/standaarden
```

Verificatievragen:
- "Welke Logius standaarden zijn er?" → moet `/ls` triggeren
- "Wat zijn de API Design Rules?" → moet `/ls-api` triggeren
- "Toon de laatste wijzigingen aan Digikoppeling" → moet `/ls-dk` triggeren

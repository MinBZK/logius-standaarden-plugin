# Logius Standaarden Plugin - Werkinstructies

## Taal

Alle content in deze repo is in het **Nederlands**: skill descriptions, body tekst, commit messages, en documentatie. Gebruik Nederlands tenzij de gebruiker expliciet in het Engels communiceert.

## Repo structuur

- `.claude-plugin/plugin.json` - Plugin manifest
- `skills/ls/SKILL.md` - Meta-skill (overzicht, routing)
- `skills/ls-<domein>/SKILL.md` - Domein-skills (9 stuks)
- `skills/ls-<domein>/reference.md` - Achtergrondinfo per domein (optioneel)

## Conventies voor skills

### SKILL.md opbouw
1. YAML frontmatter met `name`, `description`, `model: sonnet`, `allowed-tools`
2. Agent-instructie (2-3 regels: wanneer wordt deze skill gebruikt, wat moet de agent doen)
3. Repository tabel - alle repos met GitHub links en publicatie-URLs
4. Beslisboom / keuzematrix (waar relevant)
5. Implementatievoorbeelden - realistische code in Python, JavaScript, curl, XML/JSON
6. Foutafhandeling - domein-specifieke error responses, status codes
7. Verwijzing naar reference.md voor achtergrondinfo (indien aanwezig)

Elke skill moet genoeg bevatten zodat een agent standaard-conforme code kan genereren zonder externe bronnen op te halen. Encyclopedische uitleg hoort in `reference.md`, niet in `SKILL.md`.

### reference.md patroon
Achtergrondkennis die niet direct nodig is voor code-generatie wordt verplaatst naar `reference.md` in dezelfde skill-directory. Denk aan:
- Architectuurbeschrijvingen en conceptuele uitleg
- Historische context en levensfasen
- Praktijkvoorbeelden en case studies
- Gedetailleerde protocol-uitleg
- Handige commando's voor repo-exploratie

De `SKILL.md` verwijst onderaan naar `reference.md` met een korte zin.

### Description triggers
Descriptions bevatten zowel Nederlandse als technische Engelse triggerwoorden zodat de skill bij relevante vragen wordt geactiveerd.

### Allowed tools
Pas per skill aan. Niet elke skill heeft alle tools nodig. Standaard set:
- `Bash(gh api *)`, `Bash(gh issue list *)`, `Bash(gh pr list *)`, `Bash(gh search *)`
- `Bash(curl -s *)`, `WebFetch(*)`

Extra tools alleen waar relevant:
- `Bash(npx @stoplight/spectral-cli *)` alleen voor `/ls-api`
- `Bash(npx markdownlint *)`, `Bash(npx @axe-core/cli *)`, `Bash(npx muffet *)` alleen voor `/ls-pub`

### Maximale omvang
SKILL.md bestanden mogen maximaal **500 regels** bevatten. Grotere content hoort in `reference.md`.

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
- "Welke Logius standaarden zijn er?" -> moet `/ls` triggeren
- "Wat zijn de API Design Rules?" -> moet `/ls-api` triggeren
- "Toon de laatste wijzigingen aan Digikoppeling" -> moet `/ls-dk` triggeren
- "Hoe stel ik een beheermodel op?" -> moet `/ls-bomos` triggeren
- "Run een WCAG check" -> moet `/ls-pub` triggeren (niet ls-dk of ls-api!)

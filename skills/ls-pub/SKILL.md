---
name: ls-pub
description: "Gebruik deze skill wanneer de gebruiker vraagt over 'publicatie', 'ReSpec', 'documentatie tooling', 'GitHub Actions workflows', 'build workflow', 'check workflow', 'publish workflow', 'tech radar', 'WCAG check', 'markdownlint', 'lint', 'kwaliteitscheck', 'Muffet', 'link validatie', 'automatisering', 'HTML generatie', 'PDF generatie', 'accessibility check', 'a11y'."
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

**Agent-instructie:** Deze skill is de centrale plek voor publicatie-tooling en kwaliteitschecks van alle Logius standaarden. Gebruik deze skill wanneer de gebruiker een document wil bouwen, valideren (WCAG, markdown lint, link check), publiceren, of een nieuw ReSpec-document wil opzetten. Dit is de ENIGE skill met markdownlint, axe-core en muffet tools.

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

## ReSpec Documentatie-systeem

ReSpec genereert technische specificaties als HTML en PDF vanuit Markdown. Logius gebruikt een eigen fork met aangepaste huisstijl.

### Hoe ReSpec werkt

Het hoofdbestand `index.html` laadt de ReSpec-engine en verwijst via `data-include` naar losse Markdown-bestanden per hoofdstuk:

```html
<section data-include-format="markdown" data-include="ch01.md"></section>
<section data-include-format="markdown" data-include="ch02.md"></section>
```

### ReSpec Configuratie (`js/config.js`)

```javascript
{
  useLogo: true,
  useLabel: true,
  license: "cc0",
  specStatus: "WV",        // WV=Working Version, CV=Consultation, VV=Vastgesteld, DEF=Definitief
  specType: "HR",           // HR=Handreiking, ST=Standaard, PR=Praktijkrichtlijn, IM=Informatiemodel
  pubDomain: "dk",
  shortName: "template",
  publishDate: "2023-01-31",
  publishVersion: "0.0.1",
  title: "Template",
  content: { "ch01": "informative", "ch02": "" },
  alternateFormats: [{ label: "pdf", uri: "template.pdf" }]
}
```

### Directory Structuur

```
.github/workflows/    # CI/CD workflows (verwijzen naar Automatisering repo)
js/config.js          # ReSpec configuratie
media/                # Afbeeldingen, diagrammen
ch01.md, ch02.md      # Hoofdstukken
abstract.md           # Samenvatting
index.html            # ReSpec entry point
```

## GitHub Actions Workflows (Automatisering repo)

Alle standaarden-repos roepen centrale workflows aan uit de `Automatisering` repo.

### build.yml - Document Generatie

1. Branch-detectie: `consultatie/*` branches krijgen automatisch `specStatus: "cv"`
2. HTML generatie: `npx respec --localhost --src index.html --out static/index.html`
3. PDF generatie via Puppeteer/headless Chrome
4. Cache opslag als GitHub Actions cache

### check.yml - Kwaliteitschecks (3 parallelle checks)

1. **WCAG check**: `npx @axe-core/cli http://localhost:8080/index.html --tags wcag2aa`
2. **Markdown lint**: `npx markdownlint-cli sections/`
3. **Link validatie**: Muffet valideert alle hyperlinks

### publish.yml - Publicatie

Publiceert definitieve versies naar de centrale `publicatie` repo.

### Calling Workflow Voorbeeld

```yaml
# .github/workflows/build.yml in een standaarden-repo
name: Build document
on:
  push:
    branches: [main, 'consultatie/**']
  pull_request:
    branches: [main]

jobs:
  build:
    uses: logius-standaarden/Automatisering/.github/workflows/build.yml@main
    with:
      source-files: index.html
```

## Nieuw Document Starten

1. **Fork het template**: Gebruik [ReSpec-template-Logius](https://github.com/logius-standaarden/ReSpec-template-Logius) als basis
2. **Configureer `js/config.js`**: Pas `specStatus`, `specType`, `pubDomain`, `shortName`, `title` aan
3. **Schrijf content**: Maak hoofdstukken als losse Markdown-bestanden
4. **Push naar GitHub**: CI/CD bouwt automatisch HTML en PDF

## Checks Lokaal Draaien

```bash
# ReSpec bouwen naar statische HTML
npx respec --src index.html --out output.html

# WCAG Accessibility check (wcag2aa niveau)
npx @axe-core/cli output.html --tags wcag2aa

# Markdown linting
npx markdownlint-cli 'sections/**/*.md'

# Link validatie (start eerst een lokale server)
npx http-server -p 8080 . &
muffet http://localhost:8080/index.html
```

## Foutafhandeling

| Fout | Oorzaak | Oplossing |
|------|---------|----------|
| `ReSpec error: data-include file not found` | Markdown-bestand ontbreekt | Controleer `data-include` verwijzingen |
| `WCAG violation: Images must have alternate text` | Afbeelding zonder alt-tekst | Voeg alt-tekst toe: `![Beschrijving](media/img.svg)` |
| `markdownlint MD013: Line length` | Regel te lang | Breek af op ~120 karakters |
| `muffet: 404 Not Found` | Dode link | Verwijder of update de link |
| `PDF generation failed` | Puppeteer crash | Controleer of document valid HTML genereert |

### Consultatie Branch Gedrag

Op `consultatie/*` branches wordt `specStatus` automatisch overschreven naar `"cv"`. Na merge naar `main` wordt `specStatus` uit `js/config.js` gebruikt.

## Achtergrondinfo

Zie [reference.md](reference.md) voor gedetailleerde info over tech radar, label-updates workflow, en workflow-configuratie.

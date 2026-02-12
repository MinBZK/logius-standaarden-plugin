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

## ReSpec Documentatie-systeem

ReSpec is een W3C tool voor het genereren van technische specificaties als HTML en PDF vanuit Markdown. Logius gebruikt een eigen fork met aangepaste huisstijl, NL-specifieke features en ondersteuning voor het Logius publicatieproces.

### Hoe ReSpec werkt

Het hoofdbestand van elk document is `index.html`. Dit bestand bevat geen inhoud zelf, maar laadt de ReSpec-engine en verwijst via `data-include` attributen naar losse Markdown-bestanden per hoofdstuk. ReSpec verwerkt deze Markdown naar volledige HTML met inhoudsopgave, kruisverwijzingen, referenties en huisstijl.

Content wordt als volgt in `index.html` opgenomen:

```html
<section data-include-format="markdown" data-include="ch01.md"></section>
<section data-include-format="markdown" data-include="ch02.md"></section>
```

Elke `<section>` met `data-include` laadt het bijbehorende Markdown-bestand en rendert het als een HTML-sectie in het document.

### ReSpec Configuratie

De configuratie staat in `js/config.js` en bepaalt metadata, publicatiestatus en documenttype:

```javascript
{
  useLogo: true,
  useLabel: true,
  license: "cc0",
  specStatus: "WV",        // WV=Working Version, CV=Consultation, REC=Recommendation
  specType: "HR",           // HR=Handreiking, ST=Standaard, etc.
  pubDomain: "dk",
  shortName: "template",
  publishDate: "2023-01-31",
  publishVersion: "0.0.1",
  title: "Template",
  content: {
    "ch01": "informative",
    "ch02": ""
  },
  alternateFormats: [{ label: "pdf", uri: "template.pdf" }]
}
```

Belangrijke configuratieparameters:

| Parameter | Beschrijving | Waarden |
|-----------|-------------|---------|
| `specStatus` | Publicatiestatus van het document | `WV` (Working Version), `CV` (Consultation Version), `VV` (Vastgestelde Versie), `DEF` (Definitief), `REC` (Recommendation) |
| `specType` | Type document | `HR` (Handreiking), `ST` (Standaard), `PR` (Praktijkrichtlijn), `IM` (Informatiemodel), `WA` (Werkafspraak) |
| `pubDomain` | Publicatiedomein | `dk` (Digikoppeling), `api` (API), `logius` (Logius), etc. |
| `content` | Mapping van hoofdstukken naar hun status | `"informative"` of `""` (normatief) |

### Directory Structuur

Een typisch ReSpec-document heeft de volgende structuur:

```
.github/workflows/    # CI/CD workflows (verwijzen naar Automatisering repo)
js/config.js          # ReSpec configuratie
media/                # Afbeeldingen, diagrammen en overige assets
ch01.md               # Hoofdstuk 1
ch02.md               # Hoofdstuk 2
mermaid.md            # Mermaid diagrammen (optioneel)
abstract.md           # Samenvatting / abstract
index.html            # ReSpec entry point (hoofdbestand)
```

De `media/` directory bevat afbeeldingen en diagrammen die vanuit de Markdown-bestanden worden gerefereerd. Mermaid-diagrammen kunnen inline in Markdown worden opgenomen of in een apart `mermaid.md` bestand staan.

## GitHub Actions Workflows (Automatisering repo)

De `Automatisering` repo bevat herbruikbare GitHub Actions workflows die door alle standaarden-repos worden aangeroepen. Individuele repos hoeven alleen een dunne workflow te definiëren die de centrale workflow aanroept.

### build.yml - Document Generatie

Bouwt ReSpec documenten naar statische HTML en PDF. De workflow voert de volgende stappen uit:

1. **Branch-detectie**: Controleert of de branch een `consultatie/*` branch is. Zo ja, wordt de `specStatus` automatisch overschreven naar `"cv"` (Consultation Version), zodat het document de juiste consultatie-opmaak krijgt.
2. **HTML generatie**: Draait ReSpec in headless modus om het document om te zetten naar statische HTML:
   ```bash
   npx respec --localhost --src index.html --out static/index.html
   ```
   De `--localhost` vlag zorgt ervoor dat ReSpec een lokale server start voor het resolven van relatieve paden.
3. **PDF generatie**: Converteert de gegenereerde HTML naar PDF via Puppeteer met headless Chrome. Dit levert een print-vriendelijke versie van het document op.
4. **Cache opslag**: Slaat de gegenereerde bestanden op als GitHub Actions cache, zodat volgende workflow-stappen (check, publish) deze kunnen hergebruiken zonder opnieuw te bouwen.

```bash
# Bekijk de volledige build workflow
gh api repos/logius-standaarden/Automatisering/contents/.github/workflows/build.yml \
  -H "Accept: application/vnd.github.raw"
```

### check.yml - Kwaliteitschecks

Voert drie parallelle kwaliteitschecks uit op elk gebouwd document:

**1. WCAG Accessibility Check**
Controleert het document op toegankelijkheid volgens de WCAG 2.1 AA richtlijnen met behulp van axe-core. De check gebruikt Chrome via browser-driver-manager:
```bash
npx @axe-core/cli http://localhost:8080/index.html --tags wcag2aa
```
Dit detecteert problemen zoals ontbrekende alt-teksten, onvoldoende kleurcontrast, incorrecte heading-hierarchie en ontbrekende ARIA-labels.

**2. Markdown Linting**
Controleert alle Markdown-bestanden op consistente opmaak en stijl met markdownlint-cli. De linting-configuratie wordt opgehaald uit de Automatisering repo zodat alle standaarden dezelfde regels volgen:
```bash
npx markdownlint-cli sections/
```

**3. Link Validatie**
Valideert alle hyperlinks in de gegenereerde HTML- en PDF-bestanden met Muffet, een snelle link checker. Dit vangt dode links, incorrecte ankers en onbereikbare externe URL's af.

```bash
# Bekijk de volledige check workflow
gh api repos/logius-standaarden/Automatisering/contents/.github/workflows/check.yml \
  -H "Accept: application/vnd.github.raw"
```

### publish.yml - Publicatie

Publiceert definitieve versies naar de centrale `publicatie` repo. Deze workflow wordt getriggerd wanneer een document klaar is voor publicatie en kopieert de gegenereerde HTML en PDF naar de juiste locatie in de publicatie-repo, zodat het document beschikbaar wordt op de publieke website.

```bash
# Bekijk de volledige publish workflow
gh api repos/logius-standaarden/Automatisering/contents/.github/workflows/publish.yml \
  -H "Accept: application/vnd.github.raw"
```

### label-updates.yml - Automatische PR-status Labels

Beheert automatisch labels op Pull Requests op basis van hun status. Dit maakt het eenvoudig om in een oogopslag te zien welke PRs aandacht nodig hebben, welke in review zijn, en welke klaar zijn voor merge.

```bash
# Bekijk de label workflow
gh api repos/logius-standaarden/Automatisering/contents/.github/workflows/label-updates.yml \
  -H "Accept: application/vnd.github.raw"
```

## Nieuw Document Starten

Stap-voor-stap handleiding om een nieuw standaarddocument op te zetten:

1. **Fork het template**: Maak een nieuwe repository aan op basis van [ReSpec-template-Logius](https://github.com/logius-standaarden/ReSpec-template-Logius) (gebruik "Use this template" op GitHub).

2. **Configureer `js/config.js`**: Pas de volgende velden aan:
   - `specStatus` - Begint meestal als `"WV"` (Working Version)
   - `specType` - Het type document (`"ST"`, `"HR"`, etc.)
   - `pubDomain` - Het domein waartoe het document behoort
   - `shortName` - Korte naam voor URL-generatie
   - `title` - Volledige titel van het document
   - `publishDate` en `publishVersion` - Publicatiedatum en versienummer
   - `content` - Mapping van hoofdstuknamen naar hun normatieve status

3. **Schrijf content in Markdown**: Maak hoofdstukken aan als losse Markdown-bestanden (`ch01.md`, `ch02.md`, etc.). Verwijs ernaar vanuit `index.html` met `data-include` secties.

4. **Voeg diagrammen toe**: Gebruik Mermaid-syntax in een apart `mermaid.md` bestand of inline in hoofdstukken. Statische afbeeldingen komen in de `media/` directory.

5. **Push naar GitHub**: De CI/CD workflows bouwen automatisch HTML en PDF bij elke push. Controleer de Actions tab voor de build-status.

6. **Preview op GitHub Pages**: Na een succesvolle build is het document beschikbaar als preview via GitHub Pages op `https://logius-standaarden.github.io/<repo-naam>/`.

## Checks Lokaal Draaien

Voor snellere feedback kunnen de kwaliteitschecks ook lokaal worden uitgevoerd:

```bash
# ReSpec bouwen naar statische HTML
npx respec --src index.html --out output.html

# WCAG Accessibility check (wcag2aa niveau)
npx @axe-core/cli output.html --tags wcag2aa

# Markdown linting op alle Markdown-bestanden
npx markdownlint-cli 'sections/**/*.md'

# Link validatie (start eerst een lokale server)
npx http-server -p 8080 . &
muffet http://localhost:8080/index.html
```

Tip: Draai deze checks lokaal voordat je een PR opent om CI-failures te voorkomen.

## Tech Radar

De [Tech Radar](https://logius-standaarden.github.io/tech-radar/) is een visualisatie van technologiekeuzes binnen het Logius standaardenportfolio. Technologieen worden ingedeeld in vier ringen:

| Ring | Betekenis |
|------|-----------|
| **Adopt** | Bewezen technologie, actief aanbevolen voor gebruik |
| **Trial** | Veelbelovend, wordt actief getest in projecten |
| **Assess** | Interessant, wordt onderzocht en geevalueerd |
| **Hold** | Niet aanbevolen voor nieuw gebruik, wordt uitgefaseerd |

De radar wordt beheerd in de [tech-radar](https://github.com/logius-standaarden/tech-radar) repository en gepubliceerd als interactieve webpagina.

```bash
# Tech radar inhoud bekijken
gh api repos/logius-standaarden/tech-radar/contents --jq '.[].name'
```

## Handige Commando's

```bash
# Alle workflows in Automatisering repo bekijken
gh api repos/logius-standaarden/Automatisering/contents/.github/workflows --jq '.[].name'

# ReSpec configuratie van het Logius template ophalen
gh api repos/logius-standaarden/ReSpec-template-Logius/contents/js/config.js \
  -H "Accept: application/vnd.github.raw"

# Test de automatisering repo structuur
gh api repos/logius-standaarden/automatisering-test/contents --jq '.[].name'

# Publicatie repo structuur bekijken
gh api repos/logius-standaarden/publicatie/contents --jq '.[].name'

# Laatste wijzigingen aan Automatisering
gh api repos/logius-standaarden/Automatisering/commits \
  --jq '.[:5] | .[] | "\(.commit.committer.date) \(.commit.message | split("\n")[0])"'

# Welke repos gebruiken de centrale workflows
gh search code "logius-standaarden/Automatisering" --owner logius-standaarden --filename "*.yml"
```

## Gerelateerde Skills

| Skill | Relatie |
|-------|---------|
| `/ls-bomos` | BOMOS beschrijft het beheerproces dat deze tooling ondersteunt |
| `/ls-api` | API Design Rules repo bevat eigen linter naast centrale checks |
| `/ls-fsc` | FSC heeft eigen test suite naast centrale checks |
| `/ls` | Overzicht alle standaarden |

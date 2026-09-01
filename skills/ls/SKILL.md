---
name: ls
description: "Overzicht van Logius standaarden voor de Nederlandse overheid. Routeert naar sub-skills (Digikoppeling, API Design Rules, OAuth, FSC, BOMOS, etc.) of geeft hulp bij interoperabiliteitsvragen."
model: sonnet
allowed-tools:
  - Bash(gh api *)
  - Bash(gh issue list *)
  - Bash(gh pr list *)
  - Bash(gh search *)
  - Bash(curl -s *)
  - WebFetch(*)
metadata:
  created-with-ai: "true"
  created-with-model: claude-opus-4-20250514
  created-date: "2025-02-12"
  status: concept
---

> **CONCEPT — Let op:** Deze skill is geen officieel product van Logius. De beschrijvingen zijn informatieve samenvattingen — niet de officiële standaarden zelf. De definities op [forumstandaardisatie.nl](https://www.forumstandaardisatie.nl/open-standaarden) en [Logius](https://www.logius.nl) zijn altijd leidend. Overheidsorganisaties die generatieve AI inzetten dienen te voldoen aan het [Overheidsbreed standpunt voor de inzet van generatieve AI](https://open.overheid.nl/documenten/bc03ce31-0cf1-4946-9c94-e934a62ebe73/file). Zie [DISCLAIMER.md](../../DISCLAIMER.md) en onze [verantwoording](https://github.com/developer-overheid-nl/skills-marketplace/blob/main/docs/verantwoording.md).

# Logius Standaarden - Overzicht

**Agent-instructie:** Gebruik deze skill om de gebruiker naar het juiste domein te routeren. Bij een domein-specifieke vraag, verwijs naar de juiste `/ls-*` skill. Bij organisatie-brede vragen, gebruik de gh commando's hieronder.

Logius beheert 88 GitHub repositories onder [github.com/logius-standaarden](https://github.com/logius-standaarden). Deze plugin bevat skills voor 77 daarvan, verdeeld over 9 domeinen (de overige repos zijn gearchiveerd, meta/test-repos of duplicaten).

## Versiemodel

Logius-standaarden kennen twee publicatiekanalen (vergelijkbaar met W3C):

- **Vastgestelde versie (DEF)**: officieel goedgekeurd, gepubliceerd op `gitdocumentatie.logius.nl`
- **Werkversie (draft)**: werk-in-uitvoering, gepubliceerd op `logius-standaarden.github.io`

Niet alle standaarden hebben al een vastgestelde versie. De domeinen ADR, Digikoppeling, BOMOS, IAM (OAuth-NL-profiel v1.1.0, OIDC-NLGOV v1.0.1), FSC (fsc-core v1.1.2, fsc-logging v1.0.0) en Notificaties (CloudEvents NL GOV v1.1) hebben vastgestelde publicaties (DEF). De overige domeinen (Logboek, E-Gov) publiceren vooralsnog alleen werkversies. Een standaard kan tegelijkertijd een vastgestelde versie en een werkversie (draft voor de volgende release) hebben. Modules en subcomponenten ontlenen hun status aan de standaard die ernaar verwijst.

## Forum Standaardisatie Status

Niet alle Logius-standaarden staan op de 'pas-toe-of-leg-uit'-lijst van het Forum Standaardisatie:

- **Verplicht**: [API Design Rules](https://www.forumstandaardisatie.nl/open-standaarden/rest-api-design-rules), [Digikoppeling](https://www.forumstandaardisatie.nl/open-standaarden/digikoppeling) (inclusief FSC sinds 5 november 2025), [Authenticatie-standaarden (OIDC+SAML)](https://www.forumstandaardisatie.nl/open-standaarden/authenticatie-standaarden), [OAuth-NL-profiel v1.0](https://www.forumstandaardisatie.nl/open-standaarden/nl-gov-assurance-profile-oauth-20), [CloudEvents v1.0](https://www.forumstandaardisatie.nl/open-standaarden/nl-gov-profile-cloudevents), [NLCIUS](https://www.forumstandaardisatie.nl/open-standaarden/nlcius)
- **Aanbevolen**: Peppol BIS
- **Niet op de lijst**: BOMOS (raamwerk, geen interoperabiliteitsstandaard), Logboek (in ontwikkeling), Digimelding/Terugmelding, e-procurement

> OAuth-NL-profiel v1.1 is vastgesteld door Logius (DEF), maar op het Forum Standaardisatie nog ["in procedure"](https://www.forumstandaardisatie.nl/intakeadvies-nl-gov-assurance-profile-oauth-20-versie-11) (intake goedgekeurd 24-09-2025); v1.0 is de verplichte versie.

## Forum Beslisboom Open Standaarden

Het Forum Standaardisatie heeft een [Beslisboom Open Standaarden](https://www.forumstandaardisatie.nl/beslisboom/beslisboom-open-standaarden): zes vragen over sector, communicatiekanaal en gegevenssoort, met als uitkomst de standaarden die waarschijnlijk relevant zijn. Waar deze skills routeren op domein (je weet al dat je Digikoppeling nodig hebt), routeert de beslisboom op situatie.

Gebruik de beslisboom bij vragen als "welke standaarden gelden voor mijn project" in plaats van "hoe implementeer ik standaard X". De onderliggende data is als JSON:API op te halen:

```bash
# Alle standaarden die de beslisboom kan aanraden
curl -s -H "Accept: application/vnd.api+json" \
  "https://www.forumstandaardisatie.nl/jsonapi/node/decision_tree?include=decisionTreeSteps.questions.answers.standards" \
  | jq -r '.included[] | select(.type=="node--standaarden") | .attributes.title' | sort -u
```

### Dekking en scope

De beslisboom kent 55 standaarden. Deze plugins dekken er 27. De rest valt in drie groepen:

| Groep | Standaarden | Waarom |
|-------|------------|--------|
| **Sectorstandaarden — bewust buiten scope** | Aquo, GWSW, NLCS, SIKB0101, SIKB0102, Stosag, VISI, SETU, XBRL, EML_NL, Erfgoedstandaard, E-Portfolio NL, WDO Datamodel | Eigen sectorcommunities met eigen tooling en documentatie. Deze skills dekken de generieke overheidsstandaarden (Logius, Geonovum, internet.nl); sectorstandaarden dekken zou betekenen dat we een Forum-mirror worden. |
| **Backlog — reële gaten** | ACME, STIX en TAXII, AdES Baseline Profiles, NEN-ISO/IEC 27001, NEN-ISO/IEC 27002 | Passen wel bij de scope maar zijn nog niet uitgewerkt. 27001/27002 worden nu alleen genoemd als grondslag van de BIO, niet als standaard behandeld. |
| **Juridisch en documentformaten — geen eigenaar** | BWB, ECLI, JCDR, ODF, PDF/A, PDF/UA, EPUB, iCalendar, CMIS, ISO 3166-1, WPA2 Enterprise | Geen van de huidige domein-skills claimt dit. ECLI en BWB (wetgeving en jurisprudentie) zijn het meest kansrijk om alsnog op te pakken. |
| **StUF — bewust niet** | StUF | Verplicht en centraal in gemeentelijke interoperabiliteit, maar niet correct te dekken in een skill: de standaard leunt op sectormodellen en implementatiekeuzes die per koppelvlak verschillen. Een half-correcte StUF-skill is schadelijker dan geen. Advies van Eelco Hotting, 2026-08-31. |

> **Let op bij dekkingsanalyses:** naam-matching geeft in twee richtingen een verkeerd beeld. Het onderschat de dekking wanneer inhoud onder een andere noemer staat: HTTPS en HSTS worden behandeld in `/inet-web` en ACME-certificaatautomatisering in `/inet-toolbox`. Het overschat de dekking wanneer een standaard alleen terloops genoemd wordt: NEN-ISO/IEC 27001 en 27002 komen voor als grondslag van de BIO, maar worden nergens als standaard behandeld. Controleer bij twijfel wat de skill daadwerkelijk over de standaard zegt, niet of de naam voorkomt.

## Domeinen

| Skill | Domein | Repos | Beschrijving |
|-------|--------|-------|-------------|
| `/ls-dk` | Digikoppeling | 17 | Beveiligde gegevensuitwisseling (REST, ebMS2, WUS, GB), OIN |
| `/ls-api` | API Design Rules | 9 | NL GOV API-standaard, Spectral linter, referentie-implementaties |
| `/ls-iam` | Identity & Access Management | 8 | OAuth 2.0 NL, OpenID Connect, AuthZEN, SAML |
| `/ls-fsc` | Federated Service Connectivity | 7 | Federatief netwerk: inway/outway, contracten, service directory |
| `/ls-logboek` | Logboek Dataverwerkingen | 8 | AVG-logging, OpenTelemetry, W3C Trace Context |
| `/ls-notif` | CloudEvents & Notificaties | 4 | NL GOV CloudEvents profiel, pub/sub |
| `/ls-bomos` | BOMOS Governance | 10 | Beheer- en Ontwikkelmodel voor Open Standaarden |
| `/ls-egov` | E-Government Services | 6 | Terugmelding, Digimelding, e-procurement |
| `/ls-pub` | Publicatie & Tooling | 9 | ReSpec, GitHub Actions, WCAG checks, markdown lint |

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

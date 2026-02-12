---
name: ls-dk
description: "Gebruik deze skill wanneer de gebruiker vraagt over 'Digikoppeling', 'MSP', 'MSH', 'MSL', 'koppelvlak', 'ebMS2', 'WUS', 'grote berichten', 'OIN', 'Organisatie Identificatie Nummer', 'PKIoverheid', 'beveiligingsstandaarden overheid', 'routering adressering', 'CPA', 'berichtuitwisseling overheid'."
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

# Digikoppeling

Digikoppeling is de Nederlandse standaard voor beveiligde elektronische gegevensuitwisseling tussen overheidsorganisaties. Het definieert de architectuur en koppelvlakspecificaties voor MSP (Meldingmaker), MSH (Meldingafhandelaar) en MSL (Logistiek).

## Repositories

| Repository | Beschrijving | Publicatie |
|-----------|-------------|-----------|
| [Digikoppeling-Architectuur](https://github.com/logius-standaarden/Digikoppeling-Architectuur) | Overkoepelende architectuurbeschrijving | [Lees online](https://logius-standaarden.github.io/Digikoppeling-Architectuur/) |
| [Digikoppeling-Koppelvlakstandaard-REST-API](https://github.com/logius-standaarden/Digikoppeling-Koppelvlakstandaard-REST-API) | REST-API koppelvlakspecificatie | [Lees online](https://logius-standaarden.github.io/Digikoppeling-Koppelvlakstandaard-REST-API/) |
| [Digikoppeling-Koppelvlakstandaard-ebMS2](https://github.com/logius-standaarden/Digikoppeling-Koppelvlakstandaard-ebMS2) | ebMS2 koppelvlakspecificatie (MSH/MSL) | [Lees online](https://logius-standaarden.github.io/Digikoppeling-Koppelvlakstandaard-ebMS2/) |
| [Digikoppeling-Koppelvlakstandaard-WUS](https://github.com/logius-standaarden/Digikoppeling-Koppelvlakstandaard-WUS) | WUS (WSDL/UDDI/SOAP) koppelvlakspecificatie | [Lees online](https://logius-standaarden.github.io/Digikoppeling-Koppelvlakstandaard-WUS/) |
| [Digikoppeling-Koppelvlakstandaard-GB](https://github.com/logius-standaarden/Digikoppeling-Koppelvlakstandaard-GB) | Grote Berichten koppelvlakspecificatie | [Lees online](https://logius-standaarden.github.io/Digikoppeling-Koppelvlakstandaard-GB/) |
| [Digikoppeling-Beveiligingsstandaarden-en-voorschriften](https://github.com/logius-standaarden/Digikoppeling-Beveiligingsstandaarden-en-voorschriften) | Beveiligingsstandaarden en -voorschriften | [Lees online](https://logius-standaarden.github.io/Digikoppeling-Beveiligingsstandaarden-en-voorschriften/) |
| [Digikoppeling-Identificatie-en-Authenticatie](https://github.com/logius-standaarden/Digikoppeling-Identificatie-en-Authenticatie) | Identificatie en authenticatie | [Lees online](https://logius-standaarden.github.io/Digikoppeling-Identificatie-en-Authenticatie/) |
| [Digikoppeling-Gebruik-en-achtergrond-certificaten](https://github.com/logius-standaarden/Digikoppeling-Gebruik-en-achtergrond-certificaten) | Gebruik van PKIoverheid-certificaten | [Lees online](https://logius-standaarden.github.io/Digikoppeling-Gebruik-en-achtergrond-certificaten/) |
| [Digikoppeling-Handreiking-Adressering-en-Routering](https://github.com/logius-standaarden/Digikoppeling-Handreiking-Adressering-en-Routering) | Handreiking voor adressering en routering | [Lees online](https://logius-standaarden.github.io/Digikoppeling-Handreiking-Adressering-en-Routering/) |
| [Digikoppeling-Best-Practices-ebMS2](https://github.com/logius-standaarden/Digikoppeling-Best-Practices-ebMS2) | Best practices voor ebMS2 implementatie | [Lees online](https://logius-standaarden.github.io/Digikoppeling-Best-Practices-ebMS2/) |
| [Digikoppeling-Best-Practices-WUS](https://github.com/logius-standaarden/Digikoppeling-Best-Practices-WUS) | Best practices voor WUS implementatie | [Lees online](https://logius-standaarden.github.io/Digikoppeling-Best-Practices-WUS/) |
| [Digikoppeling-Best-Practices-GB](https://github.com/logius-standaarden/Digikoppeling-Best-Practices-GB) | Best practices voor Grote Berichten | [Lees online](https://logius-standaarden.github.io/Digikoppeling-Best-Practices-GB/) |
| [Digikoppeling-Overzicht-Actuele-Documentatie-en-Compliance](https://github.com/logius-standaarden/Digikoppeling-Overzicht-Actuele-Documentatie-en-Compliance) | Overzicht documentatie en compliance | [Lees online](https://logius-standaarden.github.io/Digikoppeling-Overzicht-Actuele-Documentatie-en-Compliance/) |
| [Digikoppeling-Wat-is-Digikoppeling](https://github.com/logius-standaarden/Digikoppeling-Wat-is-Digikoppeling) | Introductie en uitleg | [Lees online](https://logius-standaarden.github.io/Digikoppeling-Wat-is-Digikoppeling/) |
| [Digikoppeling-Algemeen](https://github.com/logius-standaarden/Digikoppeling-Algemeen) | Algemene informatie en bronbestanden | - |
| [Digikoppeling-Beheermodel](https://github.com/logius-standaarden/Digikoppeling-Beheermodel) | Beheermodel voor Digikoppeling | [Lees online](https://logius-standaarden.github.io/Digikoppeling-Beheermodel/) |
| [OIN-Stelsel](https://github.com/logius-standaarden/OIN-Stelsel) | Organisatie Identificatie Nummer stelsel | [Lees online](https://logius-standaarden.github.io/OIN-Stelsel/) |

## Kernconcepten

- **MSP (Meldingmaker/Service Provider)** - De partij die een bericht stuurt
- **MSH (Meldingafhandelaar/Service Handler)** - De partij die een bericht ontvangt en verwerkt
- **MSL (MSessaging Service Layer)** - De logistieke laag voor berichtverkeer
- **Koppelvlakken** - Vier varianten: REST-API, ebMS2, WUS (SOAP), Grote Berichten
- **OIN** - Organisatie Identificatie Nummer, uniek nummer voor overheidsorganisaties
- **PKIoverheid** - Public Key Infrastructure voor certificaten
- **Beveiligingsniveaus** - Basis, ondertekend, versleuteld
- **CPA** - MSellaboration Protocol Agreement (ebMS2)
- **MSH-adressering** - Routering op basis van OIN en endpoint
- **Grote Berichten** - Protocol voor berichten > 20MB via MSessage Transfer Protocol

## Tests & Validatie

Digikoppeling repos gebruiken de centrale `Automatisering` workflows:

```bash
# WCAG check op gepubliceerde architectuur
npx @axe-core/cli https://logius-standaarden.github.io/Digikoppeling-Architectuur/ --tags wcag2aa

# Markdown lint
npx markdownlint-cli '**/*.md'

# Bekijk de CI/CD configuratie
gh api repos/logius-standaarden/Digikoppeling-Architectuur/contents/.github/workflows --jq '.[].name'
```

## Handige Commando's

```bash
# Laatste wijzigingen architectuur
gh api repos/logius-standaarden/Digikoppeling-Architectuur/commits --jq '.[:5] | .[] | "\(.commit.committer.date) \(.commit.message | split("\n")[0])"'

# Alle Digikoppeling repos
gh api orgs/logius-standaarden/repos --paginate --jq '[.[] | select(.name | startswith("Digikoppeling"))] | sort_by(.name) | .[].name'

# Inhoud van een document
gh api repos/logius-standaarden/Digikoppeling-Architectuur/contents/ch01_Inleiding.md -H "Accept: application/vnd.github.raw"

# Open issues over alle DK repos
for repo in Digikoppeling-Architectuur Digikoppeling-Koppelvlakstandaard-REST-API Digikoppeling-Koppelvlakstandaard-ebMS2; do
  echo "=== $repo ===" && gh issue list --repo logius-standaarden/$repo
done

# OIN stelsel documentatie
gh api repos/logius-standaarden/OIN-Stelsel/contents --jq '.[].name'
```

## Gerelateerde Skills

| Skill | Relatie |
|-------|---------|
| `/ls-api` | REST-API koppelvlak is gebaseerd op API Design Rules |
| `/ls-iam` | Authenticatie via OAuth/OIDC, OIN ook gebruikt voor IAM |
| `/ls-fsc` | FSC als opvolger/aanvulling voor federatieve dienstverlening |
| `/ls-pub` | ReSpec tooling voor alle DK documenten |
| `/ls` | Overzicht alle standaarden |

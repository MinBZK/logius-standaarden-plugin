# Changelog

Alle noemenswaardige wijzigingen aan deze plugin worden hier gedocumenteerd.

Het formaat is gebaseerd op [Keep a Changelog](https://keepachangelog.com/nl/1.1.0/)
en dit project volgt [Semantic Versioning](https://semver.org/lang/nl/).

## [1.0.0] - 2026-02-12

### Toegevoegd

- 10 skills voor 9 domeinen en 88 Logius standaarden repositories
  - `/ls` - Meta-skill: overzicht en routing naar het juiste domein
  - `/ls-api` - API Design Rules (10 repos): NL GOV API-standaard, Spectral linter, referentie-implementaties
  - `/ls-dk` - Digikoppeling (17 repos): REST, ebMS2, WUS, Grote Berichten, OIN
  - `/ls-iam` - Identity & Access Management (8 repos): OAuth 2.0 NL, OpenID Connect, AuthZEN, SAML
  - `/ls-fsc` - Federated Service Connectivity (7 repos): core protocol, inway/outway, service directory
  - `/ls-logboek` - Logboek Dataverwerkingen (9 repos): REST API, juridisch kader, Docker demo
  - `/ls-notif` - CloudEvents & Notificaties (4 repos): NL GOV CloudEvents profiel, pub/sub
  - `/ls-bomos` - BOMOS Governance (10 repos): Beheer- en Ontwikkelmodel voor Open Standaarden
  - `/ls-egov` - E-Government Services (6 repos): Terugmelding, Digimelding, e-procurement
  - `/ls-pub` - Publicatie & Tooling (8 repos): ReSpec, GitHub Actions workflows, tech radar
- Implementatievoorbeelden in Python, JavaScript, curl, XML en YAML
- Foutafhandeling met error codes en retry patronen
- Validatie-integraties (Spectral, markdownlint, axe-core)
- Marketplace metadata voor plugin distributie
- Upstream tracking metadata in plugin.json

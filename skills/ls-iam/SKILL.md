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

De IAM-standaarden van Logius definiëren profielen voor authenticatie en autorisatie bij Nederlandse overheids-APIs. Dit omvat OAuth 2.0, OpenID Connect, AuthZEN en aanvullende profielen specifiek voor de Nederlandse overheid. Deze standaarden waarborgen een consistent en veilig identiteits- en toegangsbeheer binnen het publieke domein.

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

---

## OAuth 2.0 NL Profiel

Het Nederlandse OAuth 2.0 profiel scherpt de basisspecificatie (RFC 6749) aan met strikte beveiligingseisen voor gebruik binnen de overheid. Doel is het voorkomen van veelvoorkomende kwetsbaarheden en het garanderen van interoperabiliteit.

### Verplichte beveiligingseisen

- **PKCE (Proof Key for Code Exchange)** is verplicht voor alle clients, inclusief confidential clients. Dit voorkomt authorization code interception aanvallen. Clients genereren een `code_verifier` en sturen een `code_challenge` mee in het authorization request.
- **Grant types**: uitsluitend `authorization_code` en `client_credentials` zijn toegestaan. Implicit grant en Resource Owner Password Credentials zijn expliciet verboden vanwege bekende beveiligingsrisico's.
- **Tokens als HTTP headers**: access tokens worden uitsluitend als HTTP `Authorization` header (Bearer scheme) meegegeven. Transport via query parameters is niet toegestaan om te voorkomen dat tokens in server logs en browser history terechtkomen.

### Token specificaties

- **Access tokens**: JWT-formaat (RFC 9068) wordt aanbevolen. Dit maakt lokale validatie mogelijk zonder een introspection endpoint aan te roepen, wat de prestaties verbetert.
- **Refresh tokens**: verplicht bij de `authorization_code` flow. Hiermee kunnen clients nieuwe access tokens verkrijgen zonder hernieuwde gebruikersinteractie. Refresh token rotation wordt aanbevolen.
- **Token levensduur**: access tokens hebben een korte levensduur. De exacte duur wordt bepaald door de authorization server, maar korte TTL's (minuten) verdienen de voorkeur.

### Client authenticatie

Het token endpoint vereist sterke client authenticatie:

- **mTLS (Mutual TLS)**: de client presenteert een TLS-certificaat (bij voorkeur PKIoverheid) bij het aanroepen van het token endpoint. Dit biedt sterke cryptografische binding.
- **private_key_jwt**: de client ondertekent een JWT-assertion met zijn private key en stuurt deze mee als client authenticatie. De authorization server valideert met de geregistreerde public key.

### Toegangsbeheer en aanbevelingen

- **Scopes** vormen het primaire mechanisme voor toegangsbeheer. Scopes definiëren de reikwijdte van de toegang die een client namens de resource owner verkrijgt.
- **Pushed Authorization Requests (PAR)** worden aanbevolen (RFC 9126). Hiermee stuurt de client het authorization request eerst naar een dedicated endpoint van de authorization server, in plaats van via de browser. Dit voorkomt manipulatie van request parameters en vergroot de vertrouwelijkheid.

---

## OpenID Connect NL GOV Profiel

Het OIDC NL GOV profiel bouwt voort op OpenID Connect Core 1.0 en het iGov-profiel, en specificeert eisen voor authenticatie binnen de Nederlandse overheid. Het profiel legt de nadruk op betrouwbaarheidsniveaus en gestructureerde identiteitsinformatie.

### Betrouwbaarheidsniveaus (Levels of Assurance)

Het profiel definieert drie betrouwbaarheidsniveaus conform de eIDAS-verordening:

| Niveau | eIDAS Level | Toepassing |
|--------|-------------|------------|
| **Laag** | Low | Zelfregistratie, beperkte toegang |
| **Substantieel** | Substantial | DigiD Midden/Hoog, eHerkenning EH3 |
| **Hoog** | High | eHerkenning EH4, face-to-face verificatie |

De `acr_values` parameter in het authentication request mapt naar deze eIDAS-niveaus. De OpenID Provider geeft het daadwerkelijk bereikte niveau terug in de `acr` claim van het ID Token.

### Verplichte claims in het ID Token

Het ID Token moet minimaal de volgende claims bevatten:

| Claim | Beschrijving |
|-------|-------------|
| `sub` | Unieke identifier van de gebruiker (subject) |
| `iss` | Identifier van de OpenID Provider (issuer) |
| `aud` | Identifier van de Relying Party (audience) |
| `exp` | Verloopdatum van het token (expiration) |
| `iat` | Tijdstip van uitgifte (issued at) |
| `acr` | Betrouwbaarheidsniveau van de authenticatie (authentication context class reference) |

### Userinfo endpoint

Voor aanvullende claims (zoals naam, e-mailadres, BSN) wordt het Userinfo endpoint gebruikt. De Relying Party roept dit endpoint aan met het access token. Welke claims beschikbaar zijn hangt af van de gevraagde scopes en het autorisatiebeleid van de OpenID Provider.

### ID Token validatie

Relying Parties moeten het ID Token volledig valideren:

- Controleer de `iss` claim tegen de verwachte issuer
- Controleer de `aud` claim tegen de eigen client_id
- Controleer de `exp` claim (token mag niet verlopen zijn)
- Valideer de digitale handtekening met de publieke sleutel van de OpenID Provider (via het JWKS endpoint)
- Controleer de `acr` claim tegen het gevraagde betrouwbaarheidsniveau

### Client registratie

Clients moeten vooraf worden geregistreerd bij de OpenID Provider. Bij registratie worden vastgelegd:

- Redirect URI's (exacte match, geen wildcards)
- Ondersteunde response types en grant types
- Client authenticatiemethode (conform OAuth NL profiel)
- Gevraagde scopes en claims
- Publieke sleutels voor token validatie

---

## AuthZEN NL GOV

Het AuthZEN NL GOV profiel specificeert een architectuur voor geëxternaliseerde autorisatie. In plaats van autorisatielogica in applicaties te bouwen, worden autorisatiebeslissingen gedelegeerd aan een centraal beslispunt. Dit bevordert consistentie, herbruikbaarheid en auditeerbaarheid.

### Architectuurcomponenten

Het profiel volgt het XACML-referentiemodel met vier kerncomponenten:

| Component | Rol | Beschrijving |
|-----------|-----|-------------|
| **PDP** | Policy Decision Point | Evalueert autorisatieverzoeken tegen het beleid en neemt een beslissing (permit/deny) |
| **PEP** | Policy Enforcement Point | Onderschept verzoeken en handhaaft de beslissing van het PDP |
| **PAP** | Policy Administration Point | Beheert autorisatiebeleid: aanmaken, wijzigen, verwijderen van policies |
| **PIP** | Policy Information Point | Levert aanvullende attributen en contextinformatie aan het PDP voor besluitvorming |

### Evaluation endpoint

Het PDP biedt een gestandaardiseerd evaluation endpoint:

```
POST /access/v1/evaluation
Content-Type: application/json
```

### Request formaat

Een autorisatieverzoek bestaat uit vier elementen:

- **subject**: de entiteit die toegang vraagt (gebruiker, systeem)
- **action**: de gevraagde actie
- **resource**: het object waarop de actie wordt uitgevoerd
- **context**: aanvullende omstandigheden (optioneel)

Voorbeeld van een autorisatieverzoek:

```json
{
  "subject": {
    "type": "user",
    "id": "alice"
  },
  "action": {
    "name": "approve"
  },
  "resource": {
    "type": "holiday-request",
    "id": "446epbc8y7",
    "properties": {
      "employee": "bob"
    }
  }
}
```

In dit voorbeeld vraagt gebruiker Alice toestemming om een verlofaanvraag van Bob goed te keuren. Het PDP evalueert of Alice de juiste rol en bevoegdheid heeft om deze actie uit te voeren.

### Response formaat

Het PDP retourneert een beslissing:

- **decision** (boolean): `true` voor toestaan, `false` voor weigeren
- **context** (optioneel): aanvullende informatie, zoals de reden van weigering of referenties naar de toegepaste beleidsregels

```json
{
  "decision": true
}
```

Bij een weigering kan het PDP een reden meegeven:

```json
{
  "decision": false,
  "context": {
    "reason": {
      "48": "No signing authority"
    }
  }
}
```

---

## Authorization Decision Log

De Authorization Decision Log standaard definieert een gestructureerd formaat voor het vastleggen van autorisatiebeslissingen. Dit is essentieel voor audit, verantwoording en het kunnen reproduceren van historische beslissingen.

### Verplichte velden

| Veld | Beschrijving |
|------|-------------|
| `timestamp` | Tijdstip van de beslissing (ISO 8601) |
| `trace_id` | Unieke trace identifier conform W3C Trace Context |
| `span_id` | Span identifier binnen de trace |
| `type` | Type van het record, bijvoorbeeld `evaluation` |
| `request` | Het oorspronkelijke autorisatieverzoek (subject, action, resource, context) |
| `response` | De autorisatiebeslissing (decision en eventuele context) |

### Optionele velden

| Veld | Beschrijving |
|------|-------------|
| `policies` | Referenties naar toegepaste beleidsregels, inclusief versienummers |
| `information` | Aanvullende data die het PIP heeft geleverd voor de besluitvorming |
| `configuration` | Configuratie van het PDP ten tijde van de beslissing |

### Voorbeeld log record

```json
{
  "timestamp": "2025-09-07T10:14:18Z",
  "trace_id": "28dbeec32e77635cc19bc3204ec56c41",
  "span_id": "893e1b2ac52d712f",
  "type": "evaluation",
  "request": {
    "subject": { "type": "user", "id": "alice" },
    "action": { "name": "approve" },
    "resource": { "type": "holiday-request", "id": "446epbc8y7" }
  },
  "response": {
    "decision": false,
    "context": {
      "reason": {
        "48": "No signing authority"
      }
    }
  }
}
```

### Integratie en transport

- **FSC Logging**: de Authorization Decision Log integreert met FSC (Federated Service Connectivity) logging via het `transaction_id` veld. Hiermee kunnen autorisatiebeslissingen worden gerelateerd aan de bredere transactieketen.
- **OpenTelemetry Protocol (OTLP)**: OTLP wordt aanbevolen als transportprotocol voor het verzenden van log records naar centrale logging-infrastructuur. Dit sluit aan bij de W3C Trace Context die al in de `trace_id` en `span_id` velden wordt gebruikt.
- **Replay van historische beslissingen**: doordat het volledige request, de response en optioneel de policies en PIP-informatie worden vastgelegd, is het mogelijk om historische beslissingen te reproduceren en te analyseren. Dit is waardevol voor audits en het opsporen van beleidsfouten.

---

## SAML

Security Assertion Markup Language (SAML) is een legacy standaard voor federatieve authenticatie en autorisatie. Hoewel nieuwere implementaties de voorkeur geven aan OAuth 2.0 en OpenID Connect, is SAML nog steeds in gebruik bij:

- **DigiD**: het authenticatiesysteem voor burgers
- **eHerkenning**: het authenticatiesysteem voor bedrijven

De SAML-specificatie voor de Nederlandse overheid beschrijft profielen voor het uitwisselen van authenticatie-assertions tussen Identity Providers en Service Providers. Het profiel specificeert onder andere:

- Verplichte gebruik van ondertekende en versleutelde assertions
- Metadata-eisen voor Identity Providers en Service Providers
- Binding-specificaties (HTTP POST, HTTP Redirect, SOAP)
- Vereisten voor Single Sign-On en Single Logout

Nieuwe implementaties worden aangemoedigd om OpenID Connect te adopteren, maar bestaande SAML-koppelingen met DigiD en eHerkenning blijven ondersteund.

---

## OIN (Organisatie Identificatie Nummer)

Het Organisatie Identificatie Nummer (OIN) is de unieke identifier voor organisaties binnen het Nederlandse overheidsdomein. Het OIN wordt onder andere gebruikt in:

- **PKIoverheid-certificaten**: het OIN is opgenomen in het `subject.serialNumber` veld van PKIoverheid-certificaten die worden gebruikt voor server- en client-authenticatie (mTLS)
- **Digikoppeling**: als organisatie-identifier in berichtenuitwisseling
- **OAuth en OIDC**: als client identifier of als claim voor organisatie-identificatie

Het OIN-stelsel wordt ook beschreven in de Digikoppeling-standaarden (zie `/ls-dk`).

---

## Tests & Validatie

IAM repositories gebruiken de centrale `Automatisering` workflows:

```bash
# WCAG toegankelijkheidscheck op gepubliceerde specificatie
npx @axe-core/cli https://logius-standaarden.github.io/OAuth-NL-profiel/ --tags wcag2aa

# Markdown lint op bronbestanden
npx markdownlint-cli '**/*.md'

# CI/CD workflows bekijken voor een specifieke repository
gh api repos/logius-standaarden/OAuth-NL-profiel/contents/.github/workflows --jq '.[].name'
```

## Handige Commando's

```bash
# Laatste wijzigingen OAuth NL profiel
gh api repos/logius-standaarden/OAuth-NL-profiel/commits --jq '.[:5] | .[] | "\(.commit.committer.date) \(.commit.message | split("\n")[0])"'

# Open issues per repository
gh issue list --repo logius-standaarden/OAuth-NL-profiel
gh issue list --repo logius-standaarden/OIDC-NLGOV
gh issue list --repo logius-standaarden/authzen-nlgov
gh issue list --repo logius-standaarden/authorization-decision-log

# AuthZEN specificatie inhoud bekijken
gh api repos/logius-standaarden/authzen-nlgov/contents --jq '.[].name'

# Zoek naar specifieke termen in IAM repositories
gh search code "PKCE" --owner logius-standaarden
gh search code "acr_values" --owner logius-standaarden
gh search code "PDP" --owner logius-standaarden
```

## Gerelateerde Skills

| Skill | Relatie |
|-------|---------|
| `/ls-api` | API Design Rules vereisen OAuth/OIDC voor authenticatie |
| `/ls-dk` | OIN-stelsel gedeeld met Digikoppeling, mTLS-certificaten |
| `/ls-logboek` | Authorization Decision Log raakt aan logging dataverwerkingen |
| `/ls-fsc` | FSC Logging integratie via transaction_id |
| `/ls` | Overzicht alle standaarden |

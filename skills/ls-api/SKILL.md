---
name: ls-api
description: "Gebruik deze skill wanneer de gebruiker vraagt over 'API Design Rules', 'ADR', 'REST API standaard', 'API richtlijnen', 'NL GOV API', 'Spectral linter', 'API linter', 'OpenAPI validatie', 'API beveiliging', 'transport security', 'API signing', 'API encryption', 'geospatial API', of 'api-linter'."
model: sonnet
allowed-tools:
  - Bash(gh api *)
  - Bash(gh issue list *)
  - Bash(gh pr list *)
  - Bash(gh search *)
  - Bash(curl -s *)
  - Bash(npx @stoplight/spectral-cli *)
  - Bash(npx markdownlint *)
  - Bash(npx @axe-core/cli *)
  - Bash(node *)
  - Bash(uv run *)
  - WebFetch(*)
---

# API Design Rules (NL GOV)

De API Design Rules (ADR) zijn de Nederlandse standaard voor het ontwerpen van RESTful APIs bij de overheid. Ze zijn verplicht onder het "pas-toe-of-leg-uit" regime van het Forum Standaardisatie. De standaard bevat concrete, toetsbare regels voor URI-ontwerp, HTTP-methoden, versiebeheer, beveiliging, foutafhandeling en meer.

## Repositories

| Repository | Beschrijving | Publicatie |
|-----------|-------------|-----------|
| [API-Design-Rules](https://github.com/logius-standaarden/API-Design-Rules) | Hoofdspecificatie met alle ontwerp- en technische regels | [Lees online](https://logius-standaarden.github.io/API-Design-Rules/) |
| [ADR-Beheermodel](https://github.com/logius-standaarden/ADR-Beheermodel) | Beheermodel specifiek voor de ADR standaard | [Lees online](https://logius-standaarden.github.io/ADR-Beheermodel/) |
| [API-Standaarden-Beheermodel](https://github.com/logius-standaarden/API-Standaarden-Beheermodel) | Overkoepelend beheermodel voor alle API-standaarden | [Lees online](https://logius-standaarden.github.io/API-Standaarden-Beheermodel/) |
| [API-mod-transport-security](https://github.com/logius-standaarden/API-mod-transport-security) | Module: Transport Security (TLS, certificaten) | [Lees online](https://logius-standaarden.github.io/API-mod-transport-security/) |
| [API-mod-signing](https://github.com/logius-standaarden/API-mod-signing) | Module: HTTP Message Signing | [Lees online](https://logius-standaarden.github.io/API-mod-signing/) |
| [API-mod-encryption](https://github.com/logius-standaarden/API-mod-encryption) | Module: Encryption (JWE) | [Lees online](https://logius-standaarden.github.io/API-mod-encryption/) |
| [API-mod-geospatial](https://github.com/logius-standaarden/API-mod-geospatial) | Module: Geospatial (GeoJSON, CRS) | [Lees online](https://logius-standaarden.github.io/API-mod-geospatial/) |
| [api-linter-impactanalyse](https://github.com/logius-standaarden/api-linter-impactanalyse) | Python tool: test Spectral regels tegen echte OpenAPI specs uit het API-register | - |
| [Respec-example-API-Designrules](https://github.com/logius-standaarden/Respec-example-API-Designrules) | ReSpec template voorbeeld voor ADR documentatie | - |
| [zaakgericht-werken-api](https://github.com/logius-standaarden/zaakgericht-werken-api) | API-specificatie voor zaakgericht werken | - |

## Technische Regels

### URI-ontwerp

- Gebruik **kebab-case** voor padsegmenten: `/financiele-claims` (niet `/financieleClaims`)
- Gebruik **camelCase** voor query-parameters: `?sortOrder=desc`
- Geen trailing slashes: `/api/v1/users` (niet `/api/v1/users/`)
- Geen bestandsextensies (gebruik Accept-header voor content negotiation)
- Major versie in URI: `/v1/resources`
- Operaties als sub-resources: laatste padsegment mag starten met `_` (bijv. `/_search`)
- Geen gevoelige informatie in URIs (niet beschermd door TLS)

### HTTP-methoden

| Methode | Gebruik | Veilig | Idempotent |
|---------|--------|--------|------------|
| GET | Resource ophalen, nooit wijzigen | Ja | Ja |
| POST | Subresource aanmaken in collectie | Nee | Nee |
| PUT | Resource aanmaken of volledig vervangen | Nee | Ja |
| PATCH | Gedeeltelijke update | Nee | Nee |
| DELETE | Resource verwijderen | Nee | Ja |

### Versiebeheer

- Major versie in URI-pad: `/v1`, `/v2`
- Volledige versie in `API-Version` response header: `1.2.3`
- Semantic versioning (major.minor.patch) in `info.version`
- Deprecation schedule publiceren bij major version changes
- Vaste transitieperiode tussen major versies

### Foutafhandeling

Gebruik `application/problem+json` (RFC 9457) voor foutresponses:

```json
{
  "type": "https://example.com/errors/insufficient-funds",
  "title": "Insufficient Funds",
  "status": 422,
  "detail": "Your current balance is 30, but that costs 50.",
  "instance": "/account/12345/transactions/abc"
}
```

Geen technische details (stack traces, interne hints) in foutmeldingen.

### Naamgeving

- Resources als **zelfstandige naamwoorden** (niet werkwoorden)
- **Meervoud** voor collecties: `/users`, `/orders`
- **Enkelvoud** voor individuele resources: `/users/{id}`
- Definieer interfaces in het **Nederlands** tenzij er een officieel Engels glossary bestaat
- Verberg implementatiedetails (geen framework- of databasenamen)

### OpenAPI Documentatie

- OpenAPI 3.0+ specificatie verplicht
- Publiceer op standaardlocatie: `/openapi.json` en `/openapi.yaml`
- Contactinformatie verplicht (name, email, url)
- CORS ondersteunen voor documentatie-toegang

## Beveiligingsmodules

### Transport Security (TLS)

Alle verbindingen MOETEN TLS gebruiken (wettelijk verplicht). Volg de laatste NCSC-richtlijnen.

Verplichte security headers in alle API-responses:

| Header | Doel |
|--------|------|
| `Cache-Control: no-store` | Voorkom caching van gevoelige data |
| `Content-Security-Policy: frame-ancestors 'none'` | Clickjacking bescherming |
| `Content-Type: application/json` | Specificeer content type |
| `Strict-Transport-Security` | Vereis HTTPS |
| `X-Content-Type-Options: nosniff` | Voorkom MIME sniffing |
| `X-Frame-Options: DENY` | Clickjacking bescherming |
| `Access-Control-Allow-Origin` | CORS beleid |

### Signing Module (JAdES)

Voor end-to-end berichtintegriteit en authenticiteit. Gebruikt JAdES detached signatures:

- **Algoritme**: RSASSA-PSS (PS256), minimaal 256 bits
- **Payload-Signature**: JWS in `Payload-Signature` HTTP header
- **Message-Signature**: JWS in `Message-Signature` HTTP header (inclusief request-target, host, content-type, digest)

OpenAPI representatie:
```yaml
components:
  headers:
    nlgov-adr-payload-sig:
      schema:
        type: string
        format: jws-compact-detached
        pattern: ^[A-Za-z0-9_-]+(?:(\\.\\.)[A-Za-z0-9_-]+){1}$
```

### Encryption Module (JWE)

Voor end-to-end versleuteling van request/response payloads wanneer transport-level encryptie niet voldoende is (bijv. bij niet-vertrouwde intermediairs).

### Geospatial Module

Verplicht bij geospatiale data. Regelt:
1. Encodering van geo-data in request/response payloads (GeoJSON)
2. Filtering van collecties op bounding box
3. Omgang met verschillende coördinaatsystemen (CRS)

## Implementatievoorbeelden

### Python/FastAPI

```python
from fastapi import FastAPI, Response
from pydantic import BaseModel

app = FastAPI(
    openapi_url="/v1/openapi.json",
    title="Example API",
    version="1.0.0",
    contact={
        "name": "API Team",
        "url": "https://example.com/support",
        "email": "support@example.com"
    },
    servers=[{"url": "https://api.example.com/v1"}]
)

@app.get("/v1/items")
def get_item(response: Response):
    response.headers["API-Version"] = "1.0.0"
    return {"id": 1, "name": "Example"}
```

### Express.js

```javascript
const express = require('express');
const app = express();

// API-Version header in alle responses
app.use((req, res, next) => {
  res.header('API-Version', '1.0.0');
  next();
});

// OpenAPI endpoint
app.get('/openapi.json', (req, res) => {
  res.json({
    openapi: '3.0.0',
    info: {
      title: 'Example API',
      version: '1.0.0',
      contact: {
        name: 'API Support',
        url: 'https://example.com/support',
        email: 'support@example.com'
      }
    },
    servers: [{ url: 'https://example.com/api/v1' }]
  });
});
```

### Go/Gin

```go
func setupRouter() *gin.Engine {
    r := gin.Default()
    r.Use(func(c *gin.Context) {
        c.Header("API-Version", "1.0.0")
        c.Next()
    })
    return r
}
```

Er zijn ook referentie-implementaties in **ASP.NET** en **Quarkus** beschikbaar.

## Implementatie Checklist

- [ ] OpenAPI 3.0+ spec op `/openapi.json` en `/openapi.yaml`
- [ ] Contactinformatie in spec
- [ ] Major versie in URI-pad (`/v1`)
- [ ] Volledige versie in `API-Version` response header
- [ ] Semantic versioning in `info.version`
- [ ] Kebab-case paden, camelCase query-parameters
- [ ] Geen trailing slashes
- [ ] Verplichte security headers
- [ ] TLS op alle verbindingen
- [ ] `application/problem+json` voor foutresponses
- [ ] Geen gevoelige data in URIs
- [ ] CORS geconfigureerd

## Tests & Validatie

### Spectral Linter (30+ regels)

```bash
# Lint een OpenAPI spec tegen de ADR regels
gh api repos/logius-standaarden/API-Design-Rules/contents/linter/spectral.yml \
  -H "Accept: application/vnd.github.raw" > /tmp/adr-spectral.yml
npx @stoplight/spectral-cli lint <jouw-spec.yaml> --ruleset /tmp/adr-spectral.yml

# Bekijk beschikbare regels
gh api repos/logius-standaarden/API-Design-Rules/contents/linter/spectral.yml \
  -H "Accept: application/vnd.github.raw" | grep "nlgov:"

# Linter testcases draaien
gh api repos/logius-standaarden/API-Design-Rules/contents/linter/testcases --jq '.[].name'
```

Belangrijke Spectral regels:
- `nlgov:version-in-uri` - Major versie in URI pad
- `nlgov:paths-no-trailing-slash` - Geen trailing slashes
- `nlgov:paths-kebab-case` - Kebab-case padsegmenten
- `nlgov:query-params-camel-case` - camelCase query parameters
- `nlgov:http-methods-only-standard` - Alleen standaard HTTP methoden
- `nlgov:info-contact` - Contactinformatie aanwezig
- `nlgov:api-version-header` - Version header in 2xx/3xx responses
- `nlgov:error-response-problem-json` - Problem+json voor fouten
- `nlgov:semver-version` - Semantic versioning formaat

### Impact Analyse Tool

```bash
gh repo clone logius-standaarden/api-linter-impactanalyse /tmp/api-linter
cd /tmp/api-linter
uv run run-spectral-for-single-rule.py --rule nlgov:paths-no-trailing-slash
```

### Centrale Checks

```bash
# WCAG accessibility check
npx @axe-core/cli https://logius-standaarden.github.io/API-Design-Rules/ --tags wcag2aa

# Markdown lint
npx markdownlint-cli '**/*.md'
```

## Handige Commando's

```bash
# Laatste wijzigingen aan de ADR
gh api repos/logius-standaarden/API-Design-Rules/commits \
  --jq '.[:5] | .[] | "\(.commit.committer.date) \(.commit.message | split("\n")[0])"'

# Open issues/PRs
gh issue list --repo logius-standaarden/API-Design-Rules
gh pr list --repo logius-standaarden/API-Design-Rules

# Zoek naar specifiek onderwerp in de spec
gh search code "pagination" --repo logius-standaarden/API-Design-Rules

# Inhoud van een bestand ophalen
gh api repos/logius-standaarden/API-Design-Rules/contents/README.md \
  -H "Accept: application/vnd.github.raw"
```

## Gerelateerde Skills

| Skill | Relatie |
|-------|---------|
| `/ls-iam` | OAuth/OIDC profielen voor API-authenticatie |
| `/ls-dk` | Digikoppeling REST-API koppelvlak gebruikt ADR |
| `/ls-fsc` | FSC dienstverlening via ADR-conforme APIs |
| `/ls-pub` | ReSpec tooling voor ADR documentatie |
| `/ls` | Overzicht alle standaarden |

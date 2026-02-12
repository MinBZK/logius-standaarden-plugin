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
  - WebFetch(*)
---

# API Design Rules (NL GOV)

**Agent-instructie:** Deze skill helpt bij het implementeren van APIs conform de NL GOV API Design Rules. Gebruik de Spectral linter om OpenAPI specs te valideren. De regels zijn verplicht onder 'pas-toe-of-leg-uit' van het Forum Standaardisatie.

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

Voor end-to-end berichtintegriteit en authenticiteit. Gebruikt JAdES detached signatures met RSASSA-PSS (PS256), minimaal 256 bits. Signatures in `Payload-Signature` en `Message-Signature` HTTP headers. OpenAPI representatie met `format: jws-compact-detached`.

### Encryption Module (JWE)

Voor end-to-end versleuteling van request/response payloads wanneer transport-level encryptie niet voldoende is (bijv. bij niet-vertrouwde intermediairs).

### Geospatial Module

Verplicht bij geospatiale data. Regelt GeoJSON encodering, bounding box filtering, en coördinaatsystemen (CRS).

## Implementatievoorbeeld

### Python/FastAPI

```python
from fastapi import FastAPI, Query, Request, HTTPException
from fastapi.responses import JSONResponse
from pydantic import BaseModel
from datetime import date

app = FastAPI(
    openapi_url="/v1/openapi.json",
    title="Zaakgericht Werken API",
    version="1.2.0",
    contact={"name": "API Team", "url": "https://example.com/support", "email": "api@example.com"},
    servers=[{"url": "https://api.example.com"}],
)

class Zaak(BaseModel):
    id: str
    zaaktype: str
    omschrijving: str
    startdatum: date
    status: str

@app.middleware("http")
async def add_adr_headers(request: Request, call_next):
    response = await call_next(request)
    response.headers["API-Version"] = "1.2.0"
    response.headers["Content-Type"] = "application/json"
    response.headers["Cache-Control"] = "no-store"
    response.headers["Strict-Transport-Security"] = "max-age=31536000"
    response.headers["X-Content-Type-Options"] = "nosniff"
    response.headers["X-Frame-Options"] = "DENY"
    response.headers["Content-Security-Policy"] = "frame-ancestors 'none'"
    return response

@app.get("/v1/zaken", response_model=list[Zaak])
async def list_zaken(
    status: str | None = Query(None, description="Filter op status"),
    startdatum__gte: date | None = Query(None, alias="startdatumGte", description="Vanaf datum"),
    page: int = Query(1, ge=1),
    page_size: int = Query(20, ge=1, le=100, alias="pageSize"),
):
    """Haal zaken op met paginering en filtering (ADR-compliant)."""
    # Paginering met standaard page_size=20, max 100
    offset = (page - 1) * page_size
    return db.query_zaken(status=status, since=startdatum__gte, offset=offset, limit=page_size)

@app.exception_handler(HTTPException)
async def problem_json_handler(request: Request, exc: HTTPException):
    """RFC 9457 problem+json (verplicht per ADR)."""
    return JSONResponse(
        status_code=exc.status_code,
        content={
            "type": f"https://api.example.com/errors/{exc.status_code}",
            "title": exc.detail if isinstance(exc.detail, str) else "Error",
            "status": exc.status_code,
            "instance": str(request.url),
        },
        media_type="application/problem+json",
    )
```

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

## Spectral Linter

De Spectral linter valideert OpenAPI specs tegen 30+ ADR regels.

```bash
# Haal spectral.yml op en lint een OpenAPI spec
gh api repos/logius-standaarden/API-Design-Rules/contents/linter/spectral.yml \
  -H "Accept: application/vnd.github.raw" > /tmp/adr-spectral.yml
npx @stoplight/spectral-cli lint <jouw-spec.yaml> --ruleset /tmp/adr-spectral.yml

# Bekijk beschikbare regels
gh api repos/logius-standaarden/API-Design-Rules/contents/linter/spectral.yml \
  -H "Accept: application/vnd.github.raw" | grep "nlgov:"

# Linter testcases bekijken
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

## Achtergrondinfo

Zie [reference.md](reference.md) voor Express.js/Go voorbeelden, impact analyse tool, en repo-exploratie commando's.

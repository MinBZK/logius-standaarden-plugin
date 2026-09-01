---
name: ls-api
description: "NL GOV API Design Rules (ADR): normatieve regels voor REST APIs: naming, versioning, problem+json, transport security, signing, encryption, geo, Spectral ADR-ruleset."
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

# API Design Rules (NL GOV)

**Agent-instructie:** Deze skill bevat de normatieve regels uit de NL GOV API Design Rules (ADR), verplicht onder ['pas-toe-of-leg-uit'](https://www.forumstandaardisatie.nl/open-standaarden/rest-api-design-rules) van het Forum Standaardisatie. Voor de praktische bouw-flow (design-first met OAS-generator, validatie met don-checker, schema-register lookup, codegen) zie de `don-tools` skill in [developer-overheid-nl/skills-developer-overheid-nl](https://github.com/developer-overheid-nl/skills-developer-overheid-nl).

De API Design Rules (ADR) zijn de Nederlandse standaard voor het ontwerpen van RESTful APIs bij de overheid. Ze zijn verplicht onder het "pas-toe-of-leg-uit" regime van het Forum Standaardisatie. De standaard bevat concrete, toetsbare regels voor URI-ontwerp, HTTP-methoden, versiebeheer, beveiliging, foutafhandeling en meer.

## Standaardcomponenten hergebruiken

De ADR levert herbruikbare headers en foutresponses op `https://static.developer.overheid.nl/adr/components.yaml`. Verwijs ernaar met externe `$ref`s in plaats van ze inline te herdefiniëren — zo blijft je OAS consistent met de standaard en hoef je deze componenten niet zelf bij te houden. Beschikbaar: headers `API-Version` en `Link`; responses `400` (problem+json), `401`, `403`, `404`, `204`, `501`.

```yaml
paths:
  /bieren/{id}:
    get:
      responses:
        "200":
          description: OK
          headers:
            API-Version:
              $ref: 'https://static.developer.overheid.nl/adr/components.yaml#/headers/API-Version'
        "404":
          $ref: 'https://static.developer.overheid.nl/adr/components.yaml#/responses/404'
        "400":
          $ref: 'https://static.developer.overheid.nl/adr/components.yaml#/responses/400'
```

> **Let op:** `components.yaml` heeft `headers:` en `responses:` op rootniveau (geen `components:`-wrapper), dus de `$ref`-paden zijn `#/headers/...` en `#/responses/...`.

## Versiemodel

De ADR kent twee publicatiekanalen (vergelijkbaar met W3C-standaarden):

- **Vastgestelde versie (DEF)**: de officieel goedgekeurde versie, gepubliceerd op `gitdocumentatie.logius.nl`
- **Werkversie (draft)**: de ontwikkeling richting de volgende release, gepubliceerd op `logius-standaarden.github.io`. De werkversie op GitHub Pages is de lopende ontwikkeling richting de volgende release.

Modules hebben geen eigen vaststellingsproces — ze ontlenen hun status aan de standaard die ernaar verwijst. Als de ADR in een vastgestelde versie normatief naar een module verwijst, is die module daarmee ook vastgesteld. ADR v2.2.0 bundelt de modules onder één sectie *Normative modules* met eigen regels: `/core/modules/geospatial`, `/core/modules/signing` en `/core/modules/encryption`. Daarmee zijn ook Signing en Encryption normatief geworden; in v2.1.0 waren dat nog losse werkversies. De inhoud van Transport Security is sinds v2.1.0 ingebed in de hoofdspecificatie (in v2.2.0 sectie 2.11) met eigen regels (`/core/transport/*`); de GitHub-repository is gearchiveerd.

## Repositories

| Repository | Beschrijving | Licentie | Vastgesteld | Draft |
|-----------|-------------|--------|------------|-------|
| [API-Design-Rules](https://github.com/logius-standaarden/API-Design-Rules) | Hoofdspecificatie (ADR) | [CC-BY-4.0](https://creativecommons.org/licenses/by/4.0/legalcode.en) | [v2.2.0](https://gitdocumentatie.logius.nl/publicatie/api/adr/2.2.0/) | [Draft](https://logius-standaarden.github.io/API-Design-Rules/) |
| [ADR-Beheermodel](https://github.com/logius-standaarden/ADR-Beheermodel) | Beheermodel voor de ADR standaard — **gearchiveerd**, vervangen door API-Standaarden-Beheermodel | [CC-BY-4.0](https://creativecommons.org/licenses/by/4.0/legalcode.en) | [v1.0](https://gitdocumentatie.logius.nl/publicatie/api/adr-beheer/1.0/) | - |
| [API-Standaarden-Beheermodel](https://github.com/logius-standaarden/API-Standaarden-Beheermodel) | Overkoepelend beheermodel API-standaarden | [CC-BY-4.0](https://creativecommons.org/licenses/by/4.0/legalcode.en) | - | [Draft](https://logius-standaarden.github.io/API-Standaarden-Beheermodel/) |
| [API-mod-geospatial](https://github.com/logius-standaarden/API-mod-geospatial) | Module: Geospatial (GeoJSON, CRS) — normatief in ADR v2.2.0 (`/core/modules/geospatial`) | [CC-BY-4.0](https://creativecommons.org/licenses/by/4.0/legalcode.en) | [v1.0.3](https://gitdocumentatie.logius.nl/publicatie/api/mod-geo/1.0.3/) | [Draft](https://logius-standaarden.github.io/API-mod-geospatial/) |
| [API-mod-transport-security](https://github.com/logius-standaarden/API-mod-transport-security) | Module: Transport Security — **gearchiveerd**; inhoud ingebed in de hoofdspecificatie sinds ADR v2.1.0 (v2.2.0 sectie 2.11) | [CC-BY-4.0](https://creativecommons.org/licenses/by/4.0/legalcode.en) | - | - |
| [API-mod-signing](https://github.com/logius-standaarden/API-mod-signing) | Module: HTTP Message Signing — normatief in ADR v2.2.0 (`/core/modules/signing`) | [CC-BY-4.0](https://creativecommons.org/licenses/by/4.0/legalcode.en) | - | [Draft](https://logius-standaarden.github.io/API-mod-signing/) |
| [API-mod-encryption](https://github.com/logius-standaarden/API-mod-encryption) | Module: Encryption (JWE) — normatief in ADR v2.2.0 (`/core/modules/encryption`) | [CC-BY-4.0](https://creativecommons.org/licenses/by/4.0/legalcode.en) | - | [Draft](https://logius-standaarden.github.io/API-mod-encryption/) |
| [api-linter-impactanalyse](https://github.com/logius-standaarden/api-linter-impactanalyse) | Python tool: test Spectral regels tegen echte OpenAPI specs uit het API-register | Niet gespecificeerd | - | - |
| [zaakgericht-werken-api](https://github.com/logius-standaarden/zaakgericht-werken-api) | API-specificatie voor zaakgericht werken | [CC-BY-4.0](https://creativecommons.org/licenses/by/4.0/legalcode.en) | - | - |

## Technische Regels

### URI-ontwerp

- Gebruik **kebab-case** voor padsegmenten: `/mijn-documenten` (niet `/mijnDocumenten`)
- Gebruik **camelCase** voor query-parameters: `?sortOrder=desc`
- Geen trailing slashes: `/api/v1/users` (niet `/api/v1/users/`)
- Geen bestandsextensies (gebruik Accept-header voor content negotiation)
- Major versie in URI: `/v1/resources` (Let op: in de OAS moet dit op serverniveau worden gespecificeerd, niet in individuele paden. Dit om uit te sluiten dat één API meerdere major versies in verschillende paden heeft, wat verwarrend is. De linter controleert op aanwezigheid van major versie in server URL, niet in individuele paden.)
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

### Datum en tijd

- Gebruik RFC 3339 formaat voor datum/tijd waarden
- Altijd een timezone specificeren bij `date-time` velden (bijv. `2026-04-11T10:30:00+02:00`)
- Gebruik `format: date` wanneer alleen een datum nodig is (niet `date-time`)
- Specificeer altijd een `format` voor properties die datum of tijd bevatten
- De Spectral linter bevat 4 regels die dit afdwingen (`date-time-ensure-timezone`, `specify-format-for-date-and-time`, `time-without-timezone`, `use-date-instead-of-datetime`)

### Naamgeving

- Resources als **zelfstandige naamwoorden** (niet werkwoorden)
- **Meervoud** voor collecties: `/users`, `/orders`
- **Enkelvoud** voor individuele resources: `/users/{id}`
- Definieer interfaces in het **Nederlands** tenzij er een officieel Engelstalig begrippenkader bestaat
- Geen implementatiedetails in URI's: een API SHOULD geen framework-, platform- of databasenamen blootleggen in resource-paden (ADR)

### OpenAPI Documentatie

- OpenAPI 3.0+ specificatie verplicht
- Publiceer JSON op standaardlocatie: `/openapi.json` (VERPLICHT); YAML (`/openapi.yaml`) is OPTIONEEL
- Contactinformatie (`info.contact` met `name`, `email`, `url`) wordt sterk aanbevolen voor publieke APIs (ADR `/core/doc-openapi-contact`: SHOULD); de Spectral linter dwingt deze velden af als error voor publieke APIs. Verwijs naar het verantwoordelijke **team**, niet naar een individu of algemene helpdesk (`info@…`); gebruik als `url` bij voorkeur een issuetracker i.p.v. een homepage
- CORS ondersteunen voor documentatie-toegang

## Modules

### Transport Security (TLS)

> **Let op:** De Transport Security module werd als aparte module normatief verwezen door ADR v2.0.0. Vanaf ADR v2.1.0 zijn de transport-security-eisen **ingebed in de hoofdspecificatie** (in v2.2.0 sectie 2.11, regels `/core/transport/*`) en is de [repository gearchiveerd](https://github.com/logius-standaarden/API-mod-transport-security).

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

### Geospatial Module (v1.0.3 — vastgesteld)

Normatief onderdeel van ADR v2.2.0 (`/core/modules/geospatial`). Verplicht bij geospatiale data. Regelt GeoJSON encodering, bounding box filtering, en coördinaatsystemen (CRS). Zie de [vastgestelde versie](https://gitdocumentatie.logius.nl/publicatie/api/mod-geo/1.0.3/).

### Signing Module (JAdES) — draft

> **Let op:** Deze module is nog in **concept** (draft) en is nog niet goedgekeurd door het Technisch Overleg. De inhoud kan nog wijzigen.

Voor end-to-end berichtintegriteit en authenticiteit. Gebruikt JAdES detached signatures met RSASSA-PSS (PS256), minimaal 256 bits. Signatures in `Payload-Signature` en `Message-Signature` HTTP headers. OpenAPI representatie met `format: jws-compact-detached`.

### Encryption Module (JWE) — draft

> **Let op:** Deze module is nog in **concept** (draft) en is nog niet goedgekeurd door het Technisch Overleg. De inhoud kan nog wijzigen.

Voor end-to-end versleuteling van request/response payloads wanneer transport-level encryptie niet voldoende is (bijv. bij niet-vertrouwde intermediairs).

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
    contact={"name": "API Team", "url": "https://github.com/example/api/issues", "email": "api@example.com"},
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

- [ ] OpenAPI 3.0+ spec op `/openapi.json` (VERPLICHT); `/openapi.yaml` is optioneel
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

## OAS valideren

OpenAPI-specs worden tegen de ADR-ruleset gevalideerd. Er zijn twee varianten van de Spectral-ruleset in omloop:

- **DON-hosted** (`https://static.developer.overheid.nl/adr/ruleset.yaml`) — 11 regels
- **GitHub** (`logius-standaarden/API-Design-Rules`, `media/linter.yaml`) — 22 regels, met aanvullende checks voor naamgeving, contactinformatie, datum/tijd en foutafhandeling

Voor de praktische uitvoering (CLI-commando's voor de DON Checker en Spectral, CI-integratie, ruleset inspecteren) zie de `don-tools` skill in [developer-overheid-nl/skills-developer-overheid-nl](https://github.com/developer-overheid-nl/skills-developer-overheid-nl).

### Belangrijke regels

Kern (DON-naam / GitHub-naam):
- `include-major-version-in-uri` / `nlgov:include-major-version-in-uri` - Major versie in URI pad
- `paths-no-trailing-slash` / `nlgov:paths-no-trailing-slash` - Geen trailing slashes
- `paths-kebab-case` / `nlgov:paths-kebab-case` - Kebab-case padsegmenten
- `http-methods` / `nlgov:http-methods` - Alleen standaard HTTP methoden
- `missing-version-header` / `nlgov:missing-version-header` - Version header in 2xx/3xx responses
- `use-problem-schema` / `nlgov:use-problem-schema` - Problem+json voor fouten

Alleen in de GitHub-versie (22 regels totaal, 11 extra t.o.v. DON):
- `nlgov:query-keys-camel-case` - camelCase query parameters
- `nlgov:info-contact-fields-exist` - Contactinformatie velden aanwezig
- `info-contact` - Contactobject aanwezig (zonder `nlgov:` prefix)
- `nlgov:semver` - Semantic versioning formaat
- `nlgov:openapi-root-exists` - OpenAPI root object aanwezig
- `oas3-api-servers` - Servers array aanwezig (built-in regel op error gezet)
- `nlgov:problem-schema-members` - Verplichte velden in problem+json schema (RFC 9457)
- `nlgov:date-time-ensure-timezone` - Datum/tijd velden met timezone
- `nlgov:time-without-timezone` - Detecteert `time` format zonder timezone
- `nlgov:specify-format-for-date-and-time` - Verplicht format voor datum/tijd properties
- `nlgov:use-date-instead-of-datetime` - Gebruik `date` i.p.v. `date-time` waar geen tijd nodig is

## Achtergrondinfo

Zie [reference.md](reference.md) voor Express.js/Go voorbeelden, impact analyse tool, en repo-exploratie commando's. Zie [conflicts.md](conflicts.md) voor bekende discrepanties tussen GitHub-tags en gepubliceerde versies.

---
name: ls-logboek
description: "Gebruik deze skill wanneer de gebruiker vraagt over 'Logboek Dataverwerkingen', 'dataverwerkingen logging', 'transparantie dataverwerkingen', 'NEN 7513', 'logging API overheid', 'logboek extensie', 'AVG logging', 'GDPR logging', 'verwerkingenlogging'."
model: sonnet
allowed-tools:
  - Bash(gh api *)
  - Bash(gh issue list *)
  - Bash(gh pr list *)
  - Bash(gh search *)
  - Bash(curl -s *)
  - Bash(npx markdownlint *)
  - Bash(npx @axe-core/cli *)
  - Bash(docker compose *)
  - Bash(make *)
  - WebFetch(*)
---

# Logboek Dataverwerkingen

Het Logboek Dataverwerkingen is de standaard voor het transparant vastleggen van dataverwerkingen door
overheidsorganisaties. Het biedt een gestandaardiseerde manier om verwerkingsactiviteiten te registreren
en raadplegen, in lijn met de AVG/GDPR verantwoordingsplicht. De standaard is gebaseerd op
OpenTelemetry en W3C Trace Context, waardoor verwerkingen over organisatiegrenzen heen traceerbaar zijn.

## Repositories

| Repository                                                                                                                               | Beschrijving                               | Publicatie                                                                                           |
|------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------|------------------------------------------------------------------------------------------------------|
| [logboek-dataverwerkingen](https://github.com/logius-standaarden/logboek-dataverwerkingen)                                               | Hoofdspecificatie met REST API definitie   | [Lees online](https://logius-standaarden.github.io/logboek-dataverwerkingen/)                        |
| [logboek-dataverwerkingen-inleiding](https://github.com/logius-standaarden/logboek-dataverwerkingen-inleiding)                           | Introductie en achtergrond                 | [Lees online](https://logius-standaarden.github.io/logboek-dataverwerkingen-inleiding/)              |
| [logboek-dataverwerkingen-juridisch-beleidskader](https://github.com/logius-standaarden/logboek-dataverwerkingen-juridisch-beleidskader) | Juridisch en beleidskader                  | [Lees online](https://logius-standaarden.github.io/logboek-dataverwerkingen-juridisch-beleidskader/) |
| [logboek-dataverwerkingen-demo](https://github.com/logius-standaarden/logboek-dataverwerkingen-demo)                                     | Docker demo-omgeving met meerdere services | -                                                                                                    |
| [logboek-extensie-lezen](https://github.com/logius-standaarden/logboek-extensie-lezen)                                                   | Extensie: leesoperaties op het logboek     | [Lees online](https://logius-standaarden.github.io/logboek-extensie-lezen/)                          |
| [logboek-extensie-nen7513](https://github.com/logius-standaarden/logboek-extensie-nen7513)                                               | Extensie: NEN 7513 (logging in de zorg)    | [Lees online](https://logius-standaarden.github.io/logboek-extensie-nen7513/)                        |
| [logboek-extensie-object](https://github.com/logius-standaarden/logboek-extensie-object)                                                 | Extensie: objectgegevens bij verwerkingen  | [Lees online](https://logius-standaarden.github.io/logboek-extensie-object/)                         |
| [logboek-extensie-template](https://github.com/logius-standaarden/logboek-extensie-template)                                             | Template voor nieuwe extensies             | [Lees online](https://logius-standaarden.github.io/logboek-extensie-template/)                       |
| [logboek-dataverwerkingen_Juridisch-beleidskader](https://github.com/logius-standaarden/logboek-dataverwerkingen_Juridisch-beleidskader) | Alternatieve juridisch beleidskader repo   | -                                                                                                    |

## Architectuur

De architectuur volgt een gedecentraliseerd model met drie kerncomponenten:

```
                    ┌─────────────┐
                    │  Applicatie  │
                    │ (verwerker)  │
                    └──────┬──────┘
                           │ schrijft logregels
                           v
┌─────────────┐     ┌─────────────┐
│  Register   │<────│  Logboek    │
│  (Art. 30)  │ ref │ (log store) │
└─────────────┘     └─────────────┘
```

**Kernprincipes:**

- Elke verwerkingsverantwoordelijke beheert een **eigen Logboek**.
- Een applicatie logt uitsluitend de **eigen verwerkingsactiviteiten**.
- Het Logboek verwijst naar het **Register van Verwerkingsactiviteiten** (AVG Art. 30) via `dpl.core.processing_activity_id`.
- Er wordt **geen inhoudelijke data gedeeld** tussen organisaties; alleen Trace-metadata (trace_id, span_id) wordt meegegeven om verwerkingen over organisatiegrenzen te correleren.
- Het Register beschrijft het *wat* en *waarom*, het Logboek beschrijft het *wanneer* en *voor wie*.

## Log Record Structuur

Elk log record volgt de OpenTelemetry Span-structuur met verplichte en optionele velden:

| Veld | Type | Verplicht | Beschrijving |
|------|------|-----------|--------------|
| `trace_id` | 16 bytes | Ja | Uniek trace ID over systemen heen (W3C Trace Context) |
| `span_id` | 8 bytes | Ja | Uniek action ID binnen een verwerking |
| `parent_span_id` | 8 bytes | Nee | ID van de aanroepende actie (voor parent-child relaties) |
| `status` | enum | Ja | `Unset`, `Ok`, of `Error` |
| `name` | string | Ja | Mensleesbare actienaam (bijv. "Opvragen persoonsgegevens") |
| `start_time` | uint64 | Ja | Starttijd in milliseconden sinds Unix Epoch |
| `end_time` | uint64 | Ja | Eindtijd in milliseconden sinds Unix Epoch |
| `resource` | object | Nee | Systeem- en applicatie-identificatie |
| `attributes` | object | Ja | Verwerkingsmetadata in de `dpl` namespace |

**Toelichting:**

- `trace_id` en `span_id` worden automatisch gegenereerd conform W3C Trace Context.
- `parent_span_id` maakt het mogelijk om een boom van gerelateerde acties op te bouwen.
- `status` geeft aan of de verwerking succesvol was; bij `Error` is aanvullende foutinformatie aan te raden.
- `resource` identificeert het systeem (naam, versie, omgeving) dat de logregel produceert.
- `attributes` bevat de domeinspecifieke metadata in de `dpl.core` namespace.

## Core Attributes (dpl.core namespace)

De `dpl.core` namespace bevat de verplichte en optionele verwerkingsattributen:

| Attribuut | Type | Verplicht | Beschrijving |
|-----------|------|-----------|--------------|
| `dpl.core.processing_activity_id` | URI | Ja | Verwijzing naar de verwerkingsactiviteit in het Register (AVG Art. 30). Koppelt de logregel aan het doel en de grondslag van de verwerking. |
| `dpl.core.data_subject_id` | string | Ja | Versleuteld of gepseudonimiseerd ID van de betrokkene. Mag nooit een onversleuteld BSN of direct identificerend gegeven bevatten. |
| `dpl.core.data_subject_id_type` | string | Ja | Type identificatie van de betrokkene. Mogelijke waarden: `BSN`, `personeelsnummer`, `URI`, of een ander organisatiespecifiek type. |
| `dpl.core.foreign_operation.processor` | URL | Nee | Link naar de externe applicatie of organisatie die betrokken is bij een cross-organisatie verwerking. Wordt ingevuld wanneer een verwerking een andere partij betreft. |

**Voorbeeld attributes object:**

```json
{
  "dpl.core.processing_activity_id": "https://register.example.org/activiteiten/123",
  "dpl.core.data_subject_id": "sha256:a1b2c3d4e5f6...",
  "dpl.core.data_subject_id_type": "BSN",
  "dpl.core.foreign_operation.processor": "https://api.andere-organisatie.nl/service"
}
```

## W3C Trace Context Integratie

De standaard gebruikt W3C Trace Context voor het doorgeven van trace-informatie over systeemgrenzen heen. Dit maakt het mogelijk om verwerkingen over meerdere organisaties te correleren zonder inhoudelijke data te delen.

**Inkomende requests:**

- Parse de `traceparent` header uit het inkomende HTTP-request.
- Gebruik het ontvangen `trace_id` voor de eigen logregel.
- Sla de ontvangen `span_id` op als `parent_span_id` in het eigen log record.

**Uitgaande requests:**

- Voeg een `traceparent` header toe aan uitgaande HTTP-requests.
- Gebruik het actieve `trace_id` en een nieuw gegenereerd `span_id`.

**Header formaat:**

```
traceparent: <version>-<trace_id>-<span_id>-<trace_flags>
```

Voorbeeld:
```
traceparent: 00-4bf92f3577b34da6a3ce929d0e0e4736-00f067aa0ba902b7-01
```

- `version`: altijd `00` in de huidige specificatie
- `trace_id`: 32 hex karakters (16 bytes), uniek per verwerkingsketen
- `span_id`: 16 hex karakters (8 bytes), uniek per actie
- `trace_flags`: `01` = gesampled, `00` = niet gesampled

Hiermee worden verwerkingen over organisatiegrenzen heen traceerbaar, zonder dat inhoudelijke persoonsgegevens worden uitgewisseld.

## Cross-organisatie Flow

Onderstaand voorbeeld toont hoe trace-informatie over organisatiegrenzen stroomt:

```
Organisatie A: Applicatie vraagt data op
  -> Genereert trace_id (bijv. 4bf92f...) en span_id (bijv. 00f067...)
  -> Logt verwerking in eigen Logboek met dpl.core attributes
  -> Stuurt HTTP-request met traceparent header naar Organisatie B

Organisatie B: Service ontvangt request
  -> Ontvangt trace_id (4bf92f...) uit traceparent header
  -> Maakt nieuw span_id (bijv. a1b2c3...), zet parent_span_id = 00f067...
  -> Logt verwerking in eigen Logboek met zelfde trace_id
  -> Kan doorketen naar Organisatie C met traceparent header

Organisatie C: Backend verwerkt aanvraag
  -> Ontvangt trace_id (4bf92f...) uit traceparent header
  -> Maakt nieuw span_id, zet parent_span_id = a1b2c3...
  -> Logt verwerking in eigen Logboek met zelfde trace_id

Resultaat:
  - Alle logregels delen hetzelfde trace_id
  - Elke organisatie kan alleen het eigen Logboek doorzoeken
  - Via trace_id is de volledige verwerkingsketen reconstrueerbaar
  - Geen persoonsgegevens verlaten de organisatie, alleen trace-metadata
```

## Protocol

De standaard beveelt **OpenTelemetry Protocol (OTLP)** aan als transportprotocol voor het aanleveren van logregels aan het Logboek.

**Vereisten:**

- **TLS** is verplicht ondersteund voor alle communicatie met het Logboek.
- Het Logboek **MOET** elke schrijfactie bevestigen met een bevestigingsbericht (acknowledgement). De applicatie weet hierdoor zeker dat de logregel is opgeslagen.
- OTLP ondersteunt zowel gRPC als HTTP/protobuf als transportlaag.
- Bij gebruik van gRPC wordt bidirectionele streaming ondersteund voor hoge volumes.
- Bij gebruik van HTTP/protobuf worden standaard REST-semantieken gevolgd.

**Transportopties:**

| Protocol | Poort | Beschrijving |
|----------|-------|--------------|
| OTLP/gRPC | 4317 | Primair protocol, geschikt voor hoge volumes |
| OTLP/HTTP | 4318 | Alternatief voor omgevingen zonder gRPC-ondersteuning |

## Extensies

De standaard is modulair uitbreidbaar via extensies. Elke extensie voegt aanvullende velden of functionaliteit toe aan het basismodel.

### Object Extensie

Voegt geo-object informatie toe aan logregels. Bruikbaar wanneer verwerkingen betrekking hebben op specifieke objecten (bijv. kadastrale percelen, voertuigen, gebouwen). Definieert aanvullende attributes in de `dpl.object` namespace.

- Repository: [logboek-extensie-object](https://github.com/logius-standaarden/logboek-extensie-object)

### Lees Extensie

Biedt een gestandaardiseerde API voor het raadplegen van het Logboek. De basisstandaard definieert alleen het schrijven; deze extensie voegt leesoperaties toe. Ondersteunt filteren op trace_id, tijdsbereik en betrokkene.

- Repository: [logboek-extensie-lezen](https://github.com/logius-standaarden/logboek-extensie-lezen)

### NEN 7513 Extensie

Zorg-specifieke uitbreiding voor logging conform NEN 7513. Voegt verplichte attributen toe voor de zorgsector, zoals gebruikersidentificatie, patiëntidentificatie en de specifieke actie op het medisch dossier.

- Repository: [logboek-extensie-nen7513](https://github.com/logius-standaarden/logboek-extensie-nen7513)

### Extensie Template

Startpunt voor het ontwikkelen van nieuwe extensies. Bevat de standaard structuur, naamgevingsconventies en documentatierichtlijnen.

- Repository: [logboek-extensie-template](https://github.com/logius-standaarden/logboek-extensie-template)

## Docker Demo-omgeving

De demo-omgeving simuleert een multi-organisatie opstelling met meerdere microservices:

| Service | Beschrijving |
|---------|--------------|
| `currus` | Applicatieservice die verwerkingen uitvoert en logregels genereert |
| `grafana` | Visualisatie en dashboard voor het inzien van logregels |
| `lamina` | Logboek-implementatie die logregels opslaat en beheert |
| `munera` | Register-service met verwerkingsactiviteiten (Art. 30) |

```bash
# Demo repo klonen en structuur bekijken
gh repo clone logius-standaarden/logboek-dataverwerkingen-demo /tmp/logboek-demo
gh api repos/logius-standaarden/logboek-dataverwerkingen-demo/contents --jq '.[].name'

# Docker compose configuratie ophalen
gh api repos/logius-standaarden/logboek-dataverwerkingen-demo/contents/docker-compose.yml \
  -H "Accept: application/vnd.github.raw"

# Demo starten
cd /tmp/logboek-demo && docker compose up -d

# Of via Makefile
make build && make run
```

## Implementatievoorbeelden

### Python Applicatie met OpenTelemetry SDK

```python
from opentelemetry import trace
from opentelemetry.sdk.trace import TracerProvider
from opentelemetry.sdk.trace.export import BatchSpanProcessor
from opentelemetry.exporter.otlp.proto.grpc.trace_exporter import OTLPSpanExporter
from opentelemetry.sdk.resources import Resource

# Configureer de tracer met applicatie-metadata
resource = Resource.create({
    "service.name": "mijn-applicatie",
    "service.version": "1.0.0",
    "deployment.environment": "production",
})

provider = TracerProvider(resource=resource)
provider.add_span_processor(
    BatchSpanProcessor(OTLPSpanExporter(endpoint="grpc://logboek:4317"))
)
trace.set_tracer_provider(provider)
tracer = trace.get_tracer("mijn-applicatie")

def verwerk_aanvraag(bsn: str, verwerking_id: str):
    """Log een dataverwerking conform Logboek Dataverwerkingen standaard."""
    with tracer.start_as_current_span("verwerk-aanvraag") as span:
        # Verplichte dpl.core attributen
        span.set_attribute("dpl.core.processing_activity_id",
            f"https://register.example.com/verwerkingen/{verwerking_id}")
        span.set_attribute("dpl.core.data_subject_id", bsn)  # versleuteld in productie
        span.set_attribute("dpl.core.data_subject_id_type", "BSN")

        # Verwerking uitvoeren
        resultaat = opvragen_gegevens(bsn)

        if resultaat.error:
            span.set_status(trace.StatusCode.ERROR, resultaat.error)
            span.set_attribute("exception.message", str(resultaat.error))
        else:
            span.set_status(trace.StatusCode.OK)

        return resultaat
```

### Cross-organisatie Verwerking met W3C Trace Context

```python
import requests
from opentelemetry import trace
from opentelemetry.propagate import inject

tracer = trace.get_tracer("organisatie-a")

def vraag_gegevens_op_bij_organisatie_b(bsn: str):
    """Cross-organisatie call met W3C Trace Context propagatie."""
    with tracer.start_as_current_span("opvragen-externe-gegevens") as span:
        # dpl.core attributen voor deze verwerking
        span.set_attribute("dpl.core.processing_activity_id",
            "https://register.example.com/verwerkingen/opvragen-brp")
        span.set_attribute("dpl.core.data_subject_id", bsn)
        span.set_attribute("dpl.core.data_subject_id_type", "BSN")

        # W3C Trace Context headers automatisch injecteren
        headers = {}
        inject(headers)  # voegt traceparent en tracestate headers toe
        # headers bevat nu: {"traceparent": "00-<trace_id>-<span_id>-01"}

        # Request naar Organisatie B
        response = requests.get(
            "https://api.organisatie-b.nl/v1/personen",
            params={"bsn": bsn},
            headers=headers,
            cert=("pkio_cert.pem", "pkio_key.pem"),
        )

        # Referentie naar externe verwerking loggen
        span.set_attribute("dpl.core.foreign_operation.processor",
            "https://api.organisatie-b.nl/v1/personen")

        return response.json()
```

### Ontvangen van Cross-organisatie Requests (FastAPI)

```python
from fastapi import FastAPI, Request
from opentelemetry import trace
from opentelemetry.propagate import extract

app = FastAPI()
tracer = trace.get_tracer("organisatie-b")

@app.get("/v1/personen")
async def get_persoon(bsn: str, request: Request):
    """Ontvang request met W3C Trace Context van andere organisatie."""
    # W3C Trace Context uit inkomende headers extraheren
    context = extract(dict(request.headers))

    with tracer.start_as_current_span("verwerk-extern-verzoek", context=context) as span:
        # Dezelfde trace_id als Organisatie A, nieuwe span_id
        span.set_attribute("dpl.core.processing_activity_id",
            "https://register.organisatie-b.nl/verwerkingen/leveren-brp")
        span.set_attribute("dpl.core.data_subject_id", bsn)
        span.set_attribute("dpl.core.data_subject_id_type", "BSN")

        # Bron van het externe verzoek vastleggen
        span.set_attribute("dpl.core.foreign_operation.processor",
            request.headers.get("origin", "onbekend"))

        persoon = database.get_persoon(bsn)
        return persoon
```

### Meerdere Betrokkenen per Verwerking

```python
def verwerk_batch(bsn_lijst: list[str], verwerking_id: str):
    """Bij meerdere betrokkenen: apart logregel per persoon."""
    with tracer.start_as_current_span("batch-verwerking") as parent_span:
        parent_span.set_attribute("dpl.core.processing_activity_id",
            f"https://register.example.com/verwerkingen/{verwerking_id}")

        for bsn in bsn_lijst:
            # Child span per betrokkene (zelfde trace_id, unieke span_id)
            with tracer.start_as_current_span(f"verwerk-persoon") as child_span:
                child_span.set_attribute("dpl.core.data_subject_id", bsn)
                child_span.set_attribute("dpl.core.data_subject_id_type", "BSN")
                child_span.set_attribute("dpl.core.processing_activity_id",
                    f"https://register.example.com/verwerkingen/{verwerking_id}")
                verwerk_persoon(bsn)
```

### Docker Demo Starten en Gebruiken

```bash
# Demo-omgeving clonen en starten
gh repo clone logius-standaarden/logboek-dataverwerkingen-demo /tmp/logboek-demo
cd /tmp/logboek-demo

# Services starten (currus=logboek, grafana=dashboard, lamina=register, munera=applicatie)
docker compose up -d

# Status controleren
docker compose ps

# Grafana dashboard openen (http://localhost:3000)
# Logregels bekijken via de Grafana Tempo data source

# Test-verwerkingen genereren
curl -X POST http://localhost:8080/api/v1/verwerkingen \
  -H "Content-Type: application/json" \
  -d '{
    "processing_activity_id": "https://register.example.com/verwerkingen/test",
    "data_subject_id": "999990342",
    "data_subject_id_type": "BSN"
  }'

# Demo stoppen
docker compose down
```

## Foutafhandeling

### Status Waarden en Wanneer te Gebruiken

| Status | Wanneer | Voorbeeld |
|--------|---------|-----------|
| **Unset** (standaard) | Verwerking afgerond zonder systeemfout, ook als er geen resultaat is | Persoon niet gevonden in BRP |
| **Ok** | Expliciete succesmarkering (optioneel) | Gegevens succesvol opgehaald |
| **Error** | Systeemfout tijdens verwerking | Database timeout, API onbereikbaar |

**Let op:** Een gebruikersannulering is GEEN error. Markeer als Ok met een expliciete actie.

### Error Logging met Exception Details

```python
with tracer.start_as_current_span("verwerk-gegevens") as span:
    span.set_attribute("dpl.core.processing_activity_id", verwerking_url)
    span.set_attribute("dpl.core.data_subject_id", bsn)
    span.set_attribute("dpl.core.data_subject_id_type", "BSN")
    try:
        result = external_api.get(bsn)
    except TimeoutError as e:
        span.set_status(trace.StatusCode.ERROR, "External API timeout")
        span.set_attribute("exception.type", "TimeoutError")
        span.set_attribute("exception.message", str(e))
        span.set_attribute("exception.stacktrace", traceback.format_exc())
        raise
```

### Verplichte Velden Validatie

Een logregel MOET minimaal bevatten:
- `trace_id` (16 bytes) - uniek per verwerkingsketen
- `span_id` (8 bytes) - uniek per actie
- `name` - beschrijvende naam van de actie
- `start_time` en `end_time` - milliseconden sinds Epoch
- `status` - Unset, Ok, of Error
- `dpl.core.processing_activity_id` - URI naar het register
- `dpl.core.data_subject_id` - (versleuteld) ID van betrokkene
- `dpl.core.data_subject_id_type` - type identifier (BSN, KvK, etc.)

Als een verplicht veld ontbreekt, MOET de software een standaardwaarde invullen om runtime-fouten te voorkomen. Log sampling is NIET toegestaan.

## Tests & Validatie

### Centrale Checks

```bash
# WCAG toegankelijkheidscheck op de publicatie
npx @axe-core/cli https://logius-standaarden.github.io/logboek-dataverwerkingen/ --tags wcag2aa

# Markdown lint op alle documentatie
npx markdownlint-cli '**/*.md'
```

## Handige Commando's

```bash
# Laatste wijzigingen hoofdspec
gh api repos/logius-standaarden/logboek-dataverwerkingen/commits \
  --jq '.[:5] | .[] | "\(.commit.committer.date) \(.commit.message | split("\n")[0])"'

# Alle logboek repos ophalen
gh api orgs/logius-standaarden/repos --paginate \
  --jq '[.[] | select(.name | startswith("logboek"))] | sort_by(.name) | .[].name'

# Open issues bekijken
gh issue list --repo logius-standaarden/logboek-dataverwerkingen

# API specificatie zoeken
gh search code "openapi" --repo logius-standaarden/logboek-dataverwerkingen
```

## Gerelateerde Skills

| Skill     | Relatie                                              |
|-----------|------------------------------------------------------|
| `/ls-iam` | Authorization Decision Log als aanvulling op logboek |
| `/ls-fsc` | FSC transactielogging raakt aan logboek              |
| `/ls-dk`  | Digikoppeling MSL-logging                            |
| `/ls-pub` | ReSpec tooling voor documentatie                     |
| `/ls`     | Overzicht alle standaarden                           |

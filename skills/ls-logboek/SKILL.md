---
name: ls-logboek
description: "Logboek Dataverwerkingen voor AVG/GDPR-transparantie. NEN 7513, OpenTelemetry/OTLP, dpl.core, verwerkingenlogging in overheidssystemen."
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

# Logboek Dataverwerkingen

**Agent-instructie:** Deze skill helpt bij het implementeren van logging van dataverwerkingen conform AVG/GDPR. Gebruik OpenTelemetry SDK met de dpl.core namespace attributen. Trace Context headers propageren automatisch over organisatiegrenzen.

Het Logboek Dataverwerkingen is de standaard voor het transparant vastleggen van dataverwerkingen door
overheidsorganisaties. Het biedt een gestandaardiseerde manier om verwerkingsactiviteiten te registreren
en raadplegen, in lijn met de AVG/GDPR verantwoordingsplicht. De standaard is gebaseerd op
OpenTelemetry en W3C Trace Context, waardoor verwerkingen over organisatiegrenzen heen traceerbaar zijn.

## Versiemodel

Net als andere Logius-standaarden kent Logboek Dataverwerkingen twee publicatiekanalen (vergelijkbaar met W3C):

- **Vastgestelde versie (DEF)**: officieel goedgekeurd, gepubliceerd op `gitdocumentatie.logius.nl`
- **Werkversie (draft)**: werk-in-uitvoering, gepubliceerd op `logius-standaarden.github.io`

De normatieve hoofdspecificatie **logboek-dataverwerkingen** heeft sinds 9 april 2026 een vastgestelde versie v1.0.0. De overige onderdelen (inleiding, juridisch-beleidskader, extensies) hebben nog alleen werkversies. Extensies ontlenen hun status aan de hoofdspecificatie. Logboek Dataverwerkingen is nog niet opgenomen op de lijst van het Forum Standaardisatie.

## Repositories

| Repository | Beschrijving | Licentie | Vastgesteld | Draft |
|-----------|-------------|--------|------------|-------|
| [logboek-dataverwerkingen](https://github.com/logius-standaarden/logboek-dataverwerkingen) | Normatieve hoofdspecificatie (logging-interface) | [CC-BY-4.0](https://creativecommons.org/licenses/by/4.0/legalcode.en) | [v1.0.0](https://gitdocumentatie.logius.nl/publicatie/logboek/dataverwerkingen/1.0.0/) | [Draft](https://logius-standaarden.github.io/logboek-dataverwerkingen/) |
| [logboek-dataverwerkingen-inleiding](https://github.com/logius-standaarden/logboek-dataverwerkingen-inleiding) | Introductie en achtergrond | [CC-BY-4.0](https://creativecommons.org/licenses/by/4.0/legalcode.en) | - | [Draft](https://logius-standaarden.github.io/logboek-dataverwerkingen-inleiding/) |
| [logboek-dataverwerkingen-juridisch-beleidskader](https://github.com/logius-standaarden/logboek-dataverwerkingen-juridisch-beleidskader) | Juridisch en beleidskader | [CC-BY-4.0](https://creativecommons.org/licenses/by/4.0/legalcode.en) | - | [Draft](https://logius-standaarden.github.io/logboek-dataverwerkingen-juridisch-beleidskader/) |
| [logboek-dataverwerkingen-demo](https://github.com/logius-standaarden/logboek-dataverwerkingen-demo) ⚠️ | Docker demo-omgeving met meerdere services (**gearchiveerd** augustus 2026, README meldt dat de demo verouderd is) | [EUPL-1.2](https://eupl.eu/1.2/en) | - | - |
| [logboek-extensie-lezen](https://github.com/logius-standaarden/logboek-extensie-lezen) | Extensie: leesoperaties op het logboek | [CC-BY-4.0](https://creativecommons.org/licenses/by/4.0/legalcode.en) | - | [Draft](https://logius-standaarden.github.io/logboek-extensie-lezen/) |
| [logboek-extensie-nen7513](https://github.com/logius-standaarden/logboek-extensie-nen7513) | Extensie: NEN 7513 (logging in de zorg) | [CC-BY-4.0](https://creativecommons.org/licenses/by/4.0/legalcode.en) | - | [Draft](https://logius-standaarden.github.io/logboek-extensie-nen7513/) |
| [logboek-extensie-object](https://github.com/logius-standaarden/logboek-extensie-object) | Extensie: objectgegevens bij verwerkingen | [CC-BY-4.0](https://creativecommons.org/licenses/by/4.0/legalcode.en) | - | [Draft](https://logius-standaarden.github.io/logboek-extensie-object/) |
| [logboek-extensie-template](https://github.com/logius-standaarden/logboek-extensie-template) | Template voor nieuwe extensies | [CC-BY-4.0](https://creativecommons.org/licenses/by/4.0/legalcode.en) | - | [Draft](https://logius-standaarden.github.io/logboek-extensie-template/) |

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
| `dpl.core.data_subject_id` | string | Ja | Unieke, versleutelde identificerende code van de betrokkene. De specificatie BEVEELT AAN om deze te pseudonimiseren (SHOULD). |
| `dpl.core.data_subject_id_type` | string | Ja | Type identificatie van de betrokkene. Mogelijke waarden: `BSN`, `personeelsnummer`, `URI`, of een ander organisatiespecifiek type. |
| `dpl.core.foreign_operation.processor` | URL | Nee | Link naar de externe applicatie of organisatie die betrokken is bij een cross-organisatie verwerking. Wordt ingevuld wanneer een verwerking een andere partij betreft. |

**Voorbeeld attributes object:**

```json
{
  "dpl.core.processing_activity_id": "https://register.example.org/activiteiten/123",
  "dpl.core.data_subject_id": "a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5d6e7f8a9b0c1d2e3f4a5b6c7d8e9f0a1b2",
  "dpl.core.data_subject_id_type": "BSN",
  "dpl.core.foreign_operation.processor": "https://api.andere-organisatie.nl/service"
}
```

> **Let op:** `dpl.core.data_subject_id` bevat hier een HMAC-SHA256 hash van het BSN. De specificatie BEVEELT AAN (SHOULD) om de identificerende code te pseudonimiseren. In het implementatievoorbeeld hieronder tonen we HMAC-SHA256 als een mogelijke aanpak; de specificatie schrijft geen specifiek algoritme voor.

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

De standaard schrijft geen specifiek transportprotocol voor, maar beveelt **OpenTelemetry Protocol (OTLP)** aan als transportprotocol voor het aanleveren van logregels aan het Logboek.

**Vereisten:**

- Het Logboek **MOET** TLS kunnen afdwingen (capability-eis aan de software). De spec verduidelijkt expliciet dat hiermee niet wordt bedoeld dat TLS verplicht is — of TLS-connecties daadwerkelijk worden toegepast is een organisatie-keuze.
- Het Logboek **MOET** elke schrijfactie bevestigen met een bevestigingsbericht (acknowledgement). De applicatie weet hierdoor zeker dat de logregel is opgeslagen.
- OTLP ondersteunt zowel gRPC als HTTP/protobuf als transportlaag (standaard OTLP-poorten: 4317 voor gRPC, 4318 voor HTTP — niet voorgeschreven door de Logboek-standaard zelf).

## Extensies

De standaard is modulair uitbreidbaar via extensies. Elke extensie voegt aanvullende velden of functionaliteit toe aan het basismodel.

### Object Extensie

Voegt objectgegevens toe aan logregels. Bruikbaar wanneer verwerkingen betrekking hebben op specifieke objecten (bijv. kadastrale percelen, voertuigen, gebouwen, maar ook andere domeinobjecten). Definieert aanvullende attributes in de `dpl.objects` namespace.

- Repository: [logboek-extensie-object](https://github.com/logius-standaarden/logboek-extensie-object)

### Lees Extensie

Biedt een gestandaardiseerde API voor het raadplegen van het Logboek. De basisstandaard definieert alleen het schrijven; deze extensie voegt leesoperaties toe. Ondersteunt filteren op trace_id, tijdsbereik en betrokkene.

- Repository: [logboek-extensie-lezen](https://github.com/logius-standaarden/logboek-extensie-lezen)

### NEN 7513 Extensie

Zorg-specifieke uitbreiding voor logging conform NEN 7513. Voegt verplichte attributen toe voor de zorgsector, zoals gebruikersidentificatie, patiëntidentificatie en de specifieke actie op het medisch dossier.

- Repository: [logboek-extensie-nen7513](https://github.com/logius-standaarden/logboek-extensie-nen7513)

## Implementatievoorbeelden

### Python Applicatie met OpenTelemetry SDK

```python
import hashlib, hmac, os
from opentelemetry import trace
from opentelemetry.sdk.trace import TracerProvider
from opentelemetry.sdk.trace.export import BatchSpanProcessor
from opentelemetry.exporter.otlp.proto.grpc.trace_exporter import OTLPSpanExporter
from opentelemetry.sdk.resources import Resource

# --- BSN pseudonimisering (AANBEVOLEN door de specificatie, niet verplicht) ---
# De spec schrijft geen specifiek algoritme voor; HMAC-SHA256 is een veelgebruikte aanpak.
ORG_SALT = os.environ["DPL_ORG_SALT"]  # organisatie-breed geheim, beheerd als secret

def pseudonimiseer_bsn(bsn: str) -> str:
    """HMAC-SHA256 pseudonimisering van BSN. Eenrichtings, deterministisch per organisatie."""
    return hmac.new(ORG_SALT.encode(), bsn.encode(), hashlib.sha256).hexdigest()

# Configureer de tracer met applicatie-metadata
resource = Resource.create({
    "service.name": "mijn-applicatie",
    "service.version": "1.0.0",
    "deployment.environment": "production",
})

provider = TracerProvider(resource=resource)
provider.add_span_processor(
    BatchSpanProcessor(OTLPSpanExporter(endpoint="http://logboek:4317", insecure=True))
)
trace.set_tracer_provider(provider)
tracer = trace.get_tracer("mijn-applicatie")

def verwerk_aanvraag(bsn: str, verwerking_id: str):
    """Log een dataverwerking conform Logboek Dataverwerkingen standaard."""
    with tracer.start_as_current_span("verwerk-aanvraag") as span:
        # Verplichte dpl.core attributen
        span.set_attribute("dpl.core.processing_activity_id",
            f"https://register.example.com/verwerkingen/{verwerking_id}")
        span.set_attribute("dpl.core.data_subject_id", pseudonimiseer_bsn(bsn))  # pseudonimisering aanbevolen
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

def vraag_gegevens_op_bij_organisatie_b(bsn: str):  # pseudonimiseer_bsn() gedefinieerd in eerste codeblok
    """Cross-organisatie call met W3C Trace Context propagatie."""
    with tracer.start_as_current_span("opvragen-externe-gegevens") as span:
        # dpl.core attributen voor deze verwerking
        span.set_attribute("dpl.core.processing_activity_id",
            "https://register.example.com/verwerkingen/opvragen-brp")
        span.set_attribute("dpl.core.data_subject_id", pseudonimiseer_bsn(bsn))
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
# pseudonimiseer_bsn() gedefinieerd in eerste codeblok hierboven

@app.get("/v1/personen")
async def get_persoon(bsn: str, request: Request):
    """Ontvang request met W3C Trace Context van andere organisatie."""
    # W3C Trace Context uit inkomende headers extraheren
    context = extract(dict(request.headers))

    with tracer.start_as_current_span("verwerk-extern-verzoek", context=context) as span:
        # Dezelfde trace_id als Organisatie A, nieuwe span_id
        span.set_attribute("dpl.core.processing_activity_id",
            "https://register.organisatie-b.nl/verwerkingen/leveren-brp")
        span.set_attribute("dpl.core.data_subject_id", pseudonimiseer_bsn(bsn))
        span.set_attribute("dpl.core.data_subject_id_type", "BSN")

        # Bron van het externe verzoek vastleggen
        span.set_attribute("dpl.core.foreign_operation.processor",
            request.headers.get("origin", "onbekend"))

        persoon = database.get_persoon(bsn)
        return persoon
```

### Meerdere Betrokkenen per Verwerking

```python
# pseudonimiseer_bsn() gedefinieerd in eerste codeblok hierboven
def verwerk_batch(bsn_lijst: list[str], verwerking_id: str):
    """Bij meerdere betrokkenen: apart logregel per persoon."""
    with tracer.start_as_current_span("batch-verwerking") as parent_span:
        parent_span.set_attribute("dpl.core.processing_activity_id",
            f"https://register.example.com/verwerkingen/{verwerking_id}")

        for bsn in bsn_lijst:
            # Child span per betrokkene (zelfde trace_id, unieke span_id)
            with tracer.start_as_current_span(f"verwerk-persoon") as child_span:
                child_span.set_attribute("dpl.core.data_subject_id", pseudonimiseer_bsn(bsn))
                child_span.set_attribute("dpl.core.data_subject_id_type", "BSN")
                child_span.set_attribute("dpl.core.processing_activity_id",
                    f"https://register.example.com/verwerkingen/{verwerking_id}")
                verwerk_persoon(bsn)
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
import traceback
# pseudonimiseer_bsn() en tracer gedefinieerd in eerste codeblok hierboven

with tracer.start_as_current_span("verwerk-gegevens") as span:
    span.set_attribute("dpl.core.processing_activity_id", verwerking_url)
    span.set_attribute("dpl.core.data_subject_id", pseudonimiseer_bsn(bsn))
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

## Achtergrondinfo

Zie [reference.md](reference.md) voor de architectuurbeschrijving, W3C Trace Context details, Docker demo-omgeving, en extensie-details.
Zie [conflicts.md](conflicts.md) voor bronconflicten en gemaakte keuzes.

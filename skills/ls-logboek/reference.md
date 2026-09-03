# Logboek Dataverwerkingen - Achtergrondinfo

Dit document bevat achtergrondinfo en detailbeschrijvingen voor de Logboek Dataverwerkingen standaard.

## Architectuur - Kernprincipes

De architectuur volgt een gedecentraliseerd model. Belangrijke principes:

- Elke verwerkingsverantwoordelijke beheert een **eigen Logboek**.
- Een applicatie logt uitsluitend de **eigen verwerkingsactiviteiten**.
- Het Logboek verwijst naar het **Register van Verwerkingsactiviteiten** (AVG Art. 30) via `dpl.core.processing_activity_id`.
- Er wordt **geen inhoudelijke data gedeeld** tussen organisaties; alleen Trace-metadata (trace_id, span_id) wordt meegegeven om verwerkingen over organisatiegrenzen te correleren.
- Het Register beschrijft het *wat* en *waarom*, het Logboek beschrijft het *wanneer* en *voor wie*.

## W3C Trace Context Integratie - Details

De standaard gebruikt W3C Trace Context voor het doorgeven van trace-informatie over systeemgrenzen heen. Dit maakt het mogelijk om verwerkingen over meerdere organisaties te correleren zonder inhoudelijke data te delen.

### Inkomende requests

- Parse de `traceparent` header uit het inkomende HTTP-request.
- Gebruik het ontvangen `trace_id` voor de eigen logregel.
- Sla de ontvangen `span_id` op als `parent_span_id` in het eigen log record.

### Uitgaande requests

- Voeg een `traceparent` header toe aan uitgaande HTTP-requests.
- Gebruik het actieve `trace_id` en een nieuw gegenereerd `span_id`.

### Header formaat

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

## Docker Demo-omgeving

De demo-omgeving simuleert een multi-organisatie opstelling met meerdere microservices:

| Service | Beschrijving |
|---------|--------------|
| `munera` | Fictieve "Mijn Gemeente"-omgeving waar burgers gemeentezaken kunnen regelen |
| `currus` | API voor het beheren van parkeervergunningen |
| `lamina` | API voor het beschikbaar stellen van registratiegegevens van voertuigen |
| `grafana` | Visualisatie en dashboard voor het inzien van logregels |

### Demo starten

> **Let op:** de demo-repository is in augustus 2026 gearchiveerd en Logius heeft een waarschuwing in de README gezet dat de demo verouderd is. De code is nog te bekijken als referentie, maar sluit niet meer aan op de huidige v1.0.0-specificatie. Gebruik de normatieve specificatie als bron, niet de demo.

```bash
# Demo repo klonen en structuur bekijken
gh repo clone logius-standaarden/logboek-dataverwerkingen-demo /tmp/logboek-demo
gh api repos/logius-standaarden/logboek-dataverwerkingen-demo/contents --jq '.[].name'

# Docker compose configuratie ophalen
gh api repos/logius-standaarden/logboek-dataverwerkingen-demo/contents/docker-compose.yaml \
  -H "Accept: application/vnd.github.raw"

# Demo starten
cd /tmp/logboek-demo && docker compose up -d

# Of via Makefile
make build && make start
```

### Demo gebruiken

```bash
# Demo-omgeving clonen en starten
gh repo clone logius-standaarden/logboek-dataverwerkingen-demo /tmp/logboek-demo
cd /tmp/logboek-demo

# Services starten (munera=Mijn Gemeente app, currus=parkeervergunningen, lamina=voertuigregistratie, grafana=dashboard)
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

## Extensie Template Details

De extensie template biedt een startpunt voor het ontwikkelen van nieuwe extensies. Het bevat:

- Standaard structuur voor extensie-documentatie
- Naamgevingsconventies voor nieuwe namespaces
- Documentatierichtlijnen en voorbeelden
- Template voor OpenAPI specificaties

Repository: [logboek-extensie-template](https://github.com/logius-standaarden/logboek-extensie-template)

## Praktijkvoorbeeld: MijnOverheid Zakelijk (MOZa)

[MijnOverheid Zakelijk](https://github.com/MinBZK/MijnOverheidZakelijk) (MinBZK) is een van de eerste overheidsprojecten die Logboek Dataverwerkingen in productie implementeert. Het team heeft een open-source Java/Kotlin library ontwikkeld die het eenvoudig maakt om LDV toe te voegen aan Jakarta-applicaties.

### Library: moza-logboekdataverwerking

- **Repository:** [MinBZK/moza-logboekdataverwerking](https://github.com/MinBZK/moza-logboekdataverwerking)
- **Taal:** Kotlin (bruikbaar vanuit Java en Kotlin)
- **Opslag:** ClickHouse database
- **Kennisbank:** [Jakarta Service Method Interceptor](https://developer.overheid.nl/kennisbank/data/standaarden/logboek-dataverwerkingen/implementaties/jakarta)

De library biedt een declaratieve aanpak via een `@Logboek` annotatie:

```java
@GET
@Path("/{identificatieType}/{identificatieNummer}")
@Logboek(name = "behandelen-aanvraag", processingActivityId = "1234")
public Response behandelAanvraag() {
    // LogboekContext wordt automatisch geïnjecteerd
    logboekContext.setDataSubjectId("000000000");
    logboekContext.setDataSubjectType("KVK");
    logboekContext.setStatus(StatusCode.OK);
    return Response.ok(result).build();
}
```

De vier kerncomponenten:

| Component | Rol |
|-----------|-----|
| `@Logboek` annotatie | Decorateert service-methoden met verwerkingsnaam en activiteit-ID |
| `LogboekInterceptor` | Jakarta interceptor die spans aanmaakt, HTTP trace-headers verwerkt |
| `LogboekContext` | Request-scoped klasse voor dynamische attributen (subject ID, status) |
| `ProcessingHandler` | Initialiseert OpenTelemetry en beheert de `Tracer` |

### Configuratie

```yaml
logboekdataverwerking:
    enabled: true
    service-name: profiel-service
    clickhouse:
        endpoint: http://localhost:8123
        username: user
        password: password
        database: db_name
        table: table_name
```

### Geleerde lessen

Het MOZa-team documenteerde hun ervaringen in twee Architecture Decision Records:

1. **Federatieve performance** ([ADR-0007](https://github.com/MinBZK/MijnOverheidZakelijk/blob/main/Docs/structurizr/decisions/0007-logboek-dataverwer.md)): Bij cross-organisatie traces moet recursief bij alle LDV-services worden gecontroleerd op child-spans. Dit schaalt niet zonder een trust registry of overkoepelend trace-ID. Na overleg met het LDV-team op het Fieldlab 2025 zijn oplossingen gevonden.

2. **Implementatiebesluit** ([ADR-0010](https://github.com/MinBZK/MijnOverheidZakelijk/blob/main/Docs/structurizr/decisions/0010-ldv-implementatie.md)): MOZa implementeert LDV in de profielservice en toont de verwerkingshistorie aan ingelogde gebruikers in het portaal. De inzichten-API (lees-extensie) wordt in de toekomst gekoppeld.

### Vergelijking met Python-aanpak

De SKILL.md voorbeelden gebruiken de OpenTelemetry Python SDK met handmatige `span.set_attribute()` aanroepen. De MOZa-library abstraheert dit via Jakarta CDI:

| Aspect | Python (SKILL.md) | Java/Kotlin (MOZa) |
|--------|--------------------|---------------------|
| Span aanmaken | `tracer.start_as_current_span()` | `@Logboek` annotatie + interceptor |
| Attributen zetten | `span.set_attribute("dpl.core.…", …)` | `logboekContext.setDataSubjectId(…)` |
| Trace propagatie | `inject(headers)` / `extract(headers)` | Automatisch via interceptor |
| Opslag | Configureerbaar (OTLP exporter) | ClickHouse |

## Gerelateerde Skills

| Skill     | Relatie                                              |
|-----------|------------------------------------------------------|
| `/ls-iam` | Authorization Decision Log als aanvulling op logboek |
| `/ls-fsc` | FSC transactielogging raakt aan logboek              |
| `/ls-dk`  | Digikoppeling MSL-logging                            |
| `/ls-pub` | ReSpec tooling voor documentatie                     |
| `/ls`     | Overzicht alle standaarden                           |

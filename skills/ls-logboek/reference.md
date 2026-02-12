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
| `currus` | Applicatieservice die verwerkingen uitvoert en logregels genereert |
| `grafana` | Visualisatie en dashboard voor het inzien van logregels |
| `lamina` | Logboek-implementatie die logregels opslaat en beheert |
| `munera` | Register-service met verwerkingsactiviteiten (Art. 30) |

### Demo starten

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

### Demo gebruiken

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

## Gerelateerde Skills

| Skill     | Relatie                                              |
|-----------|------------------------------------------------------|
| `/ls-iam` | Authorization Decision Log als aanvulling op logboek |
| `/ls-fsc` | FSC transactielogging raakt aan logboek              |
| `/ls-dk`  | Digikoppeling MSL-logging                            |
| `/ls-pub` | ReSpec tooling voor documentatie                     |
| `/ls`     | Overzicht alle standaarden                           |

---
name: ls-notif
description: "Gebruik deze skill wanneer de gebruiker vraagt over 'CloudEvents', 'notificaties', 'notificatieservices', 'abonnementen', 'event-driven', 'NL GOV CloudEvents', 'pub/sub overheid', 'gebeurtenisnotificatie'."
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

# CloudEvents & Notificaties

De notificatiestandaarden definieren hoe overheidsorganisaties gebeurtenissen (events) kunnen publiceren en daarop kunnen abonneren. Gebaseerd op de internationale CNCF CloudEvents specificatie (v1.0) met een NL GOV profiel dat specifieke eisen stelt aan naamgeving, identificatie en het gebruik van context-attributen binnen de Nederlandse overheid.

## Repositories

| Repository | Beschrijving | Publicatie |
|-----------|-------------|-----------|
| [NL-GOV-profile-for-CloudEvents](https://github.com/logius-standaarden/NL-GOV-profile-for-CloudEvents) | Nederlands overheidsprofiel voor CloudEvents | [Lees online](https://logius-standaarden.github.io/NL-GOV-profile-for-CloudEvents/) |
| [CloudEvents-NL-Guidelines](https://github.com/logius-standaarden/CloudEvents-NL-Guidelines) | Richtlijnen voor gebruik van CloudEvents in NL | [Lees online](https://logius-standaarden.github.io/CloudEvents-NL-Guidelines/) |
| [Notificatieservices](https://github.com/logius-standaarden/Notificatieservices) | Specificatie voor notificatieservices | [Lees online](https://logius-standaarden.github.io/Notificatieservices/) |
| [Abonneren](https://github.com/logius-standaarden/Abonneren) | Specificatie voor het abonneren op notificaties | [Lees online](https://logius-standaarden.github.io/Abonneren/) |

## CloudEvents NL GOV Profiel

Het NL GOV profiel scherpt de CNCF CloudEvents specificatie aan voor gebruik binnen de Nederlandse overheid. Hieronder de volledige set verplichte en optionele attributen.

### Verplichte attributen

| Attribuut | Type | Beschrijving |
|-----------|------|-------------|
| `id` | String | Unieke event-identifier. Bij voorkeur een persistent (domeinspecifiek) ID, bijvoorbeeld `doc2021033441`. Wanneer geen persistent ID beschikbaar is, gebruik een UUID (v4). De combinatie `source` + `id` moet globaal uniek zijn. |
| `source` | URI-reference | Context waarin het event is ontstaan. Het NL GOV profiel schrijft URN-notatie voor met de `nld` namespace. Formaat: `urn:nld:oin:<OIN>:systeem:<systeemnaam>`. Voorbeeld: `urn:nld:oin:00000001823288444000:systeem:BRP-component`. |
| `specversion` | String | De versie van de CloudEvents specificatie. Moet `"1.0"` zijn. |
| `type` | String | Het type event in Reverse Domain Name Notation. Voorbeeld: `nl.brp.persoon-verhuisd`. Versioning gebeurt met een `v` prefix: `nl.brp.verhuizing.v2`. Types worden geregistreerd in een centraal register. |

### Optionele attributen

| Attribuut | Type | Beschrijving |
|-----------|------|-------------|
| `datacontenttype` | String | Media type van de event data payload. JSON (`application/json`) wordt aanbevolen als standaardformaat. |
| `dataschema` | URI | Verwijzing naar het schema waaraan de event data voldoet. Maakt validatie door ontvangende partijen mogelijk. |
| `subject` | String | Het onderwerp waarop het event betrekking heeft, bijvoorbeeld een BSN (`999990342`) of zaak-ID. Hiermee kunnen afnemers filteren zonder de payload te openen. |
| `time` | Timestamp | Tijdstip in RFC 3339 formaat. Let op: dit is het tijdstip van logging/registratie van het event, niet noodzakelijk het moment van de werkelijke gebeurtenis. Voorbeeld: `2024-01-15T10:30:00Z`. |
| `dataref` | URI | Verwijzing naar een externe locatie waar de volledige event payload beschikbaar is. Implementeert het Claim Check Pattern voor grote payloads die niet in het event zelf passen. |
| `sequence` | String | Geeft de relatieve volgorde van events aan. Nuttig wanneer de volgorde van verwerking van belang is voor de afnemer. |

### Type Systeem

CloudEvents definieert de volgende datatypes voor attributen:

| Type | Beschrijving |
|------|-------------|
| Boolean | `true` of `false` |
| Integer | 32-bit signed integer |
| String | Unicode tekenreeks |
| Binary | Base64-gecodeerde binaire data |
| URI | Absolute URI conform RFC 3986 |
| URI-reference | URI of relatieve referentie conform RFC 3986 |
| Timestamp | Datum en tijd conform RFC 3339 (bijv. `2024-01-15T10:30:00Z`) |

## Notificatieservices API

De Notificatieservices API is gespecificeerd als OpenAPI 3.x en biedt een centraal endpoint voor het publiceren van events.

### Event publiceren

```
POST /api/v1/notifications
Authorization: Bearer <JWT>
Content-Type: application/json

{
  "id": "f3dce042-cd6e-4977-844d-05be8dce7cea",
  "source": "urn:nld:oin:00000001823288444000:systeem:BRP-component",
  "specversion": "1.0",
  "type": "nl.brp.persoon-verhuisd",
  "subject": "999990342",
  "time": "2024-01-15T10:30:00Z",
  "datacontenttype": "application/json",
  "data": {
    "oud_adres": "Keizersgracht 100, Amsterdam",
    "nieuw_adres": "Herengracht 200, Amsterdam"
  }
}
```

### Authenticatie en autorisatie

De API maakt gebruik van JWT Bearer Tokens voor authenticatie. Publicerende systemen hebben een token nodig met de scope `notifications.distribute`. Tokens worden uitgegeven door een vertrouwde identity provider binnen het overheidsdomein.

## Abonneren

Het Abonneren-component beschrijft hoe afnemers zich kunnen registreren om specifieke events te ontvangen. Twee hoofdmodellen worden onderscheiden:

### Push-model (aanbevolen)

Event-driven aflevering via webhooks. De notificatieservice stuurt events actief naar een door de afnemer opgegeven endpoint. Dit is het voorkeursmodel vanwege lage latency en efficienter resourcegebruik.

### Pull-model

Polling-gebaseerde aflevering. De afnemer bevraagt periodiek de notificatieservice op nieuwe events. Geschikt voor situaties waar de afnemer geen inkomend endpoint kan aanbieden (bijv. vanwege firewallrestricties).

### Aanbevolen technologieen

| Technologie | Type | Toelichting |
|-------------|------|-------------|
| AMQP (OASIS) | Message broker | Volwassen standaard voor betrouwbare berichtaflevering |
| CNCF CloudEvents Subscription API | Subscription management | Standaard API voor het beheren van abonnementen op CloudEvents |
| Apache Kafka | Event streaming | Geschikt voor hoge volumes en event replay |
| NATS JetStream | Lightweight messaging | Lichtgewicht alternatief met persistence |

### Niet aanbevolen technologieen

| Technologie | Reden |
|-------------|-------|
| WebSub | Verouderd, beperkte ondersteuning en community |
| MQTT | Primair gericht op IoT-toepassingen, niet op overheidsnotificaties |
| GraphQL Subscriptions | Te sterk gekoppeld aan specifieke API-implementaties |

## Security & Privacy

Bij het werken met notificaties gelden strikte eisen op het gebied van beveiliging en privacy:

- **Geen gevoelige data in context-attributen**: Context-attributen (zoals `source`, `type`, `subject`) zijn introspectable en worden gelogd door tussenliggende systemen. Plaats nooit persoonsgegevens of gevoelige informatie in deze velden. Gebruik hiervoor de versleutelde `data`-payload.
- **Versleuteling van event data**: Wanneer de payload gevoelige gegevens bevat, moet deze versleuteld worden (end-to-end encryptie). De context-attributen blijven leesbaar voor routering.
- **TLS verplicht**: Alle communicatie tussen bronnen, notificatieservices en afnemers moet plaatsvinden over TLS (minimaal versie 1.2).
- **Claim Check Pattern**: Bij grote of gevoelige payloads kan het `dataref`-attribuut verwijzen naar een beveiligd endpoint. De afnemer haalt de payload separaat op met eigen autorisatie.

## Kernconcepten

- **CloudEvents** - CNCF standaard voor het beschrijven van events in een cloud-omgeving
- **NL GOV profiel** - Nederlandse aanscherping met verplichte attributen en URN-naamgeving
- **Notificatieservice** - Dienst die events ontvangt van bronnen en doorstuurt naar abonnees
- **Abonnement** - Registratie van een afnemer om specifieke events te ontvangen
- **Event source** - De bron die een gebeurtenis genereert (geidentificeerd via OIN in URN-notatie)
- **Event subject** - Het onderwerp waarop de event betrekking heeft (bijv. BSN, zaak-ID)
- **Pub/Sub** - Publish/Subscribe patroon voor losse koppeling tussen bron en afnemer
- **Webhook** - HTTP callback voor het afleveren van events bij abonnees (push-model)
- **Claim Check Pattern** - Patroon waarbij de payload extern wordt opgeslagen en het event alleen een referentie bevat

## Implementatievoorbeelden

### Event Publiceren met CloudEvents SDK (Python)

```python
from cloudevents.http import CloudEvent
from cloudevents.conversion import to_json
import requests, uuid
from datetime import datetime, timezone

# CloudEvent aanmaken conform NL GOV profiel
event = CloudEvent({
    "id": str(uuid.uuid4()),
    "source": "urn:nld:oin:00000001823288444000:systeem:BRP-component",
    "type": "nl.brp.persoon-verhuisd",
    "specversion": "1.0",
    "subject": "999990342",  # BSN van betrokkene
    "time": datetime.now(timezone.utc).isoformat(),
    "datacontenttype": "application/json",
}, data={
    "oud_adres": {"straat": "Keizersgracht", "huisnummer": "100", "plaats": "Amsterdam"},
    "nieuw_adres": {"straat": "Herengracht", "huisnummer": "200", "plaats": "Amsterdam"},
})

# Publiceer naar notificatieservice
response = requests.post(
    "https://notificaties.example.com/api/v1/notifications",
    headers={
        "Authorization": "Bearer eyJ...",
        "Content-Type": "application/cloudevents+json",
    },
    data=to_json(event),
)
print(f"Status: {response.status_code}")  # 200 OK
```

### Webhook Ontvanger (FastAPI)

```python
from fastapi import FastAPI, Request, HTTPException
from cloudevents.http import from_http

app = FastAPI(title="CloudEvents Webhook Ontvanger")

@app.post("/webhooks/cloudevents")
async def receive_event(request: Request):
    """Ontvang en verwerk CloudEvents conform NL GOV profiel."""
    body = await request.body()
    headers = dict(request.headers)

    try:
        event = from_http(headers, body)
    except Exception:
        raise HTTPException(400, "Ongeldig CloudEvent formaat")

    # Verplichte attributen valideren (NL GOV profiel)
    assert event["specversion"] == "1.0"
    assert event["source"].startswith("urn:nld:")  # URN met nld namespace
    assert "." in event["type"]  # Reverse Domain Name Notation

    # Event verwerken op basis van type
    match event["type"]:
        case "nl.brp.persoon-verhuisd":
            await verwerk_verhuizing(event)
        case "nl.kvk.inschrijving-gewijzigd":
            await verwerk_kvk_wijziging(event)
        case _:
            print(f"Onbekend event type: {event['type']}")

    return {"status": "accepted", "event_id": event["id"]}

async def verwerk_verhuizing(event):
    """Verwerk een BRP verhuizing notificatie."""
    subject = event["subject"]  # BSN
    data = event.data
    print(f"Persoon {subject} verhuisd naar {data['nieuw_adres']}")
```

### Event Publiceren met curl

```bash
# CloudEvent publiceren in structured content mode
curl -X POST https://notificaties.example.com/api/v1/notifications \
  -H "Authorization: Bearer $JWT_TOKEN" \
  -H "Content-Type: application/cloudevents+json" \
  -d '{
    "specversion": "1.0",
    "id": "f3dce042-cd6e-4977-844d-05be8dce7cea",
    "source": "urn:nld:oin:00000001823288444000:systeem:BRP-component",
    "type": "nl.brp.persoon-verhuisd",
    "subject": "999990342",
    "time": "2024-01-15T10:30:00Z",
    "datacontenttype": "application/json",
    "data": {"nieuw_adres": "Herengracht 200, Amsterdam"}
  }'

# Binary content mode (attributen als HTTP headers)
curl -X POST https://notificaties.example.com/api/v1/notifications \
  -H "Authorization: Bearer $JWT_TOKEN" \
  -H "Content-Type: application/json" \
  -H "ce-specversion: 1.0" \
  -H "ce-id: f3dce042-cd6e-4977-844d-05be8dce7cea" \
  -H "ce-source: urn:nld:oin:00000001823288444000:systeem:BRP-component" \
  -H "ce-type: nl.brp.persoon-verhuisd" \
  -H "ce-subject: 999990342" \
  -H "ce-time: 2024-01-15T10:30:00Z" \
  -d '{"nieuw_adres": "Herengracht 200, Amsterdam"}'
```

### Express.js Webhook Ontvanger

```javascript
const express = require('express');
const { HTTP } = require('cloudevents');

const app = express();
app.use(express.json());

app.post('/webhooks/cloudevents', (req, res) => {
  try {
    const event = HTTP.toEvent({ headers: req.headers, body: req.body });

    // NL GOV profiel validatie
    if (!event.source.startsWith('urn:nld:')) {
      return res.status(400).json({ error: 'Source moet URN nld namespace gebruiken' });
    }

    console.log(`Event ontvangen: ${event.type} van ${event.source}`);
    console.log(`Subject: ${event.subject}, Data:`, event.data);

    // Verwerk event
    switch (event.type) {
      case 'nl.brp.persoon-verhuisd':
        handleVerhuizing(event);
        break;
      default:
        console.log(`Onbekend type: ${event.type}`);
    }

    res.status(200).json({ status: 'accepted', event_id: event.id });
  } catch (err) {
    res.status(400).json({ error: 'Ongeldig CloudEvent' });
  }
});

app.listen(8080, () => console.log('Webhook listener op poort 8080'));
```

### Claim Check Pattern voor Grote Payloads

```python
# Wanneer event data te groot is voor het bericht zelf,
# gebruik het dataref attribuut om naar een extern endpoint te verwijzen.

event = CloudEvent({
    "id": str(uuid.uuid4()),
    "source": "urn:nld:oin:00000001823288444000:systeem:DMS",
    "type": "nl.dms.document-beschikbaar",
    "specversion": "1.0",
    "subject": "doc-2024-001",
    "dataref": "https://dms.example.com/api/v1/documents/doc-2024-001",  # claim check
    # Geen 'data' veld - payload wordt separaat opgehaald door ontvanger
})

# Ontvanger haalt payload op via dataref met eigen autorisatie
document = requests.get(
    event["dataref"],
    headers={"Authorization": "Bearer receiver_token"},
    cert=("pkio_cert.pem", "pkio_key.pem"),
)
```

## Foutafhandeling

### Notificatieservice Error Responses

| Status | Beschrijving | Actie |
|--------|-------------|-------|
| 200 | Event succesvol gepubliceerd | - |
| 400 | Ongeldig CloudEvent (verplicht attribuut ontbreekt) | Corrigeer event en probeer opnieuw |
| 401 | JWT token ongeldig of verlopen | Vernieuw token |
| 403 | Scope `notifications.distribute` ontbreekt | Vraag juiste scope aan |
| 429 | Rate limit bereikt | Wacht en probeer opnieuw (exponential backoff) |
| 500 | Interne fout notificatieservice | Probeer opnieuw na 30 seconden |

### Webhook Delivery Retry Patroon

Bij het afleveren van events aan webhook endpoints geldt een retry-strategie:

```python
import time

def deliver_event(webhook_url: str, event: dict, max_retries: int = 5):
    """Lever event af aan webhook met exponential backoff."""
    for attempt in range(max_retries):
        try:
            response = requests.post(webhook_url, json=event, timeout=10)
            if response.status_code == 200:
                return True
            if response.status_code >= 500:
                # Server error - retry met backoff
                wait = min(2 ** attempt * 5, 300)  # max 5 minuten
                time.sleep(wait)
                continue
            if response.status_code >= 400:
                # Client error - niet opnieuw proberen
                log.error(f"Webhook afgewezen: {response.status_code}")
                return False
        except requests.Timeout:
            wait = min(2 ** attempt * 5, 300)
            time.sleep(wait)
    return False  # Alle pogingen mislukt
```

## Tests & Validatie

Notificatie repos gebruiken de centrale `Automatisering` workflows:

```bash
# WCAG check
npx @axe-core/cli https://logius-standaarden.github.io/NL-GOV-profile-for-CloudEvents/ --tags wcag2aa

# Markdown lint
npx markdownlint-cli '**/*.md'
```

## Handige Commando's

```bash
# Laatste wijzigingen CloudEvents profiel
gh api repos/logius-standaarden/NL-GOV-profile-for-CloudEvents/commits --jq '.[:5] | .[] | "\(.commit.committer.date) \(.commit.message | split("\n")[0])"'

# Open issues
gh issue list --repo logius-standaarden/NL-GOV-profile-for-CloudEvents
gh issue list --repo logius-standaarden/Notificatieservices
gh issue list --repo logius-standaarden/Abonneren

# Inhoud bekijken
gh api repos/logius-standaarden/NL-GOV-profile-for-CloudEvents/contents --jq '.[].name'
gh api repos/logius-standaarden/Notificatieservices/contents --jq '.[].name'

# Zoek naar CloudEvents documentatie
gh search code "CloudEvents" --owner logius-standaarden

# Bekijk OpenAPI specificatie Notificatieservices
gh api repos/logius-standaarden/Notificatieservices/contents/openapi.yaml --jq '.download_url'
```

## Gerelateerde Skills

| Skill | Relatie |
|-------|---------|
| `/ls-api` | Notificatie-APIs volgen de API Design Rules |
| `/ls-fsc` | FSC kan CloudEvents transporteren |
| `/ls-logboek` | Events kunnen verwerkingen triggeren die gelogd worden |
| `/ls` | Overzicht alle standaarden |

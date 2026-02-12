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

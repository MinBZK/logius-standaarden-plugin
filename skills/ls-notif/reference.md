# CloudEvents & Notificaties - Achtergrondinfo

Dit document bevat achtergrondkennis over CloudEvents, notificatieservices en het NL GOV profiel. Deze informatie ondersteunt de implementatievoorbeelden in SKILL.md.

## Type Systeem

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

## Aanbevolen technologieen

| Technologie | Type | Toelichting |
|-------------|------|-------------|
| AMQP (OASIS) | Message broker | Volwassen standaard voor betrouwbare berichtaflevering |
| CNCF CloudEvents Subscription API | Subscription management | Standaard API voor het beheren van abonnementen op CloudEvents |
| Apache Kafka | Event streaming | Geschikt voor hoge volumes en event replay |
| NATS JetStream | Lightweight messaging | Lichtgewicht alternatief met persistence |

## Niet aanbevolen technologieen

| Technologie | Reden |
|-------------|-------|
| WebSub | Verouderd, beperkte ondersteuning en community |
| MQTT | Primair gericht op IoT-toepassingen, niet op overheidsnotificaties |
| GraphQL Subscriptions | Te sterk gekoppeld aan specifieke API-implementaties |

## Authenticatie en autorisatie

De Notificatieservices API maakt gebruik van JWT Bearer Tokens voor authenticatie. Publicerende systemen hebben een token nodig met de scope `notifications.distribute`. Tokens worden uitgegeven door een vertrouwde identity provider binnen het overheidsdomein.

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

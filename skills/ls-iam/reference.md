# Identity & Access Management - Achtergrondinfo

Deze referentiedocumentatie bevat aanvullende informatie over SAML legacy implementaties, het OIN-stelsel, en uitgebreide protocoldocumentatie voor OAuth en OpenID Connect. Voor praktische implementatie-informatie, zie [SKILL.md](SKILL.md).

---

## SAML

Security Assertion Markup Language (SAML) is een legacy standaard voor federatieve authenticatie en autorisatie. Hoewel nieuwere implementaties de voorkeur geven aan OAuth 2.0 en OpenID Connect, is SAML nog steeds in gebruik bij:

- **DigiD**: het authenticatiesysteem voor burgers
- **eHerkenning**: het authenticatiesysteem voor bedrijven

De SAML-specificatie voor de Nederlandse overheid beschrijft profielen voor het uitwisselen van authenticatie-assertions tussen Identity Providers en Service Providers. Het profiel specificeert onder andere:

- Verplichte gebruik van ondertekende en versleutelde assertions
- Metadata-eisen voor Identity Providers en Service Providers
- Binding-specificaties (HTTP POST, HTTP Redirect, SOAP)
- Vereisten voor Single Sign-On en Single Logout

Nieuwe implementaties worden aangemoedigd om OpenID Connect te adopteren, maar bestaande SAML-koppelingen met DigiD en eHerkenning blijven ondersteund.

### SAML Implementatiedetails

**Metadata**: Elke SAML-deelnemer (Identity Provider en Service Provider) publiceert een metadata-document dat de volgende informatie bevat:
- Entity ID (unieke identifier)
- Endpoints voor Single Sign-On, Single Logout
- Publieke sleutels voor handtekeningverificatie en encryptie
- Ondersteunde bindings en protocollen
- Organisatie-informatie

**Assertions**: SAML assertions bevatten authenticatie- en autorisatie-informatie:
- Subject (de geauthenticeerde gebruiker)
- Authentication context (hoe en wanneer authenticatie plaatsvond)
- Attributes (gebruikersattributen zoals BSN, naam, e-mailadres)
- Validity period (geldigheid van de assertion)

**Bindings**: Het profiel ondersteunt de volgende bindings:
- HTTP POST binding (aanbevolen voor productie-omgevingen)
- HTTP Redirect binding (voor eenvoudige integraties)
- SOAP binding (voor backchannel communicatie)

**Security Requirements**:
- Assertions MOETEN worden ondertekend door de Identity Provider
- Assertions KUNNEN worden versleuteld voor de Service Provider
- TLS MOET worden gebruikt voor alle communicatie
- Metadata MOET worden ondertekend en periodiek worden gevalideerd

---

## OIN (Organisatie Identificatie Nummer)

Het Organisatie Identificatie Nummer (OIN) is de unieke identifier voor organisaties binnen het Nederlandse overheidsdomein. Het OIN wordt onder andere gebruikt in:

- **PKIoverheid-certificaten**: het OIN is opgenomen in het `subject.serialNumber` veld van PKIoverheid-certificaten die worden gebruikt voor server- en client-authenticatie (mTLS)
- **Digikoppeling**: als organisatie-identifier in berichtenuitwisseling
- **OAuth en OIDC**: als client identifier of als claim voor organisatie-identificatie

Het OIN-stelsel wordt ook beschreven in de Digikoppeling-standaarden (zie `/ls-dk`).

### OIN Structuur

Een OIN bestaat uit 20 cijfers met de volgende structuur:
- De eerste 8 cijfers identificeren de organisatie
- De resterende cijfers zijn een volgnummer

**OIN-toekenning**: OIN's worden toegekend door Logius. Organisaties kunnen een OIN aanvragen via het OIN-register. Het OIN is niet hetzelfde als een KvK-nummer, maar er bestaat wel een mapping tussen beide.

**OIN in certificaten**: In PKIoverheid-certificaten wordt het OIN opgenomen in het `subject.serialNumber` veld. Dit maakt het mogelijk om tijdens mTLS-authenticatie de organisatie-identiteit cryptografisch te verifiëren.

**OIN in OAuth/OIDC**: Bij OAuth en OpenID Connect kan het OIN worden gebruikt als:
- Client identifier (bij registratie van de client)
- Custom claim in access tokens of ID tokens (voor autorisatiebeslissingen)
- Organisatie-identifier in scopes (bijvoorbeeld `oin:00000001234567890123:read`)

---

## OAuth 2.0 - Uitgebreide Protocoldocumentatie

### Toegangsbeheer en Scopes

**Scopes** vormen het primaire mechanisme voor toegangsbeheer. Scopes definiëren de reikwijdte van de toegang die een client namens de resource owner verkrijgt.

**Scope-naamgeving**: Scopes volgen een hiërarchische structuur:
- Basis scope: `resource.action` (bijvoorbeeld `api.read`, `api.write`)
- Gedetailleerde scope: `resource.subresource.action` (bijvoorbeeld `users.profile.read`)
- Organisatie-scope: `oin:12345678:resource.action`

**Scope-validatie**: De authorization server MOET controleren of:
- De gevraagde scopes geldig zijn
- De client geautoriseerd is om deze scopes te vragen
- De resource owner akkoord gaat met de gevraagde scopes

### Pushed Authorization Requests (PAR)

**Pushed Authorization Requests (PAR)** worden aanbevolen (RFC 9126). Hiermee stuurt de client het authorization request eerst naar een dedicated endpoint van de authorization server, in plaats van via de browser. Dit voorkomt manipulatie van request parameters en vergroot de vertrouwelijkheid.

**PAR Workflow**:
1. Client stuurt authorization parameters naar het PAR endpoint via een backend-to-backend call
2. Authorization server retourneert een request URI
3. Client gebruikt de request URI in het authorization request via de browser
4. Authorization server valideert de request URI en toont de consent screen

**Voordelen van PAR**:
- Authorization parameters worden niet zichtbaar in browser history
- Parameters kunnen niet worden gemanipuleerd door malware
- Grotere requests mogelijk (geen URL-lengtelimieten)
- Verbeterde vertrouwelijkheid van client credentials

---

## OpenID Connect - Uitgebreide Protocoldocumentatie

### Userinfo Endpoint

Voor aanvullende claims (zoals naam, e-mailadres, BSN) wordt het Userinfo endpoint gebruikt. De Relying Party roept dit endpoint aan met het access token. Welke claims beschikbaar zijn hangt af van de gevraagde scopes en het autorisatiebeleid van de OpenID Provider.

**Userinfo Request**:
```http
GET /userinfo HTTP/1.1
Host: auth.example.com
Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9...
```

**Userinfo Response**:
```json
{
  "sub": "248289761001",
  "name": "Jane Doe",
  "given_name": "Jane",
  "family_name": "Doe",
  "email": "jane.doe@example.com",
  "email_verified": true,
  "bsn": "123456789"
}
```

**Privacy en minimale gegevensuitwisseling**: De Relying Party MOET alleen de claims vragen die strikt noodzakelijk zijn voor de dienstverlening. De OpenID Provider MOET gebruikers informeren over welke claims worden gedeeld en met welke Relying Parties.

### Client Registratie

Clients moeten vooraf worden geregistreerd bij de OpenID Provider. Bij registratie worden vastgelegd:

- **Redirect URI's**: exacte match, geen wildcards toegestaan voor beveiliging
- **Ondersteunde response types en grant types**: bijvoorbeeld `code` voor authorization code flow
- **Client authenticatiemethode**: conform OAuth NL profiel (mTLS of private_key_jwt)
- **Gevraagde scopes en claims**: welke informatie de client mag opvragen
- **Publieke sleutels**: voor token validatie en client authenticatie
- **Client metadata**: naam, logo, privacy policy URL, terms of service URL

**Dynamische registratie**: OpenID Connect ondersteunt dynamische client registratie (RFC 7591), maar voor overheidstoepassingen wordt handmatige registratie aanbevolen om strikte controle te behouden.

**Client credentials**: Bij confidential clients worden credentials uitgegeven:
- Bij mTLS: een PKIoverheid-certificaat
- Bij private_key_jwt: registratie van de publieke sleutel via JWKS URI of direct

---

## Authorization Decision Log - Uitgebreide Documentatie

### Integratie en Transport

**FSC Logging**: de Authorization Decision Log integreert met FSC (Federated Service Connectivity) logging via het `transaction_id` veld. Hiermee kunnen autorisatiebeslissingen worden gerelateerd aan de bredere transactieketen.

**FSC Transactieketen**: Wanneer een API-aanroep meerdere services doorloopt, wordt de `transaction_id` doorgegeven. Elke service logt zijn autorisatiebeslissingen met deze ID, waardoor de volledige transactieketen kan worden gereconstrueerd voor audit doeleinden.

**OpenTelemetry Protocol (OTLP)**: OTLP wordt aanbevolen als transportprotocol voor het verzenden van log records naar centrale logging-infrastructuur. Dit sluit aan bij de W3C Trace Context die al in de `trace_id` en `span_id` velden wordt gebruikt.

**OTLP Integratie**:
- Log records worden verzonden als OTLP log events
- Trace context wordt automatisch doorgegeven via OTLP headers
- Centrale logging-infrastructuur kan logs correleren met distributed traces
- Ondersteuning voor batch verzending voor performantie

**Replay van historische beslissingen**: doordat het volledige request, de response en optioneel de policies en PIP-informatie worden vastgelegd, is het mogelijk om historische beslissingen te reproduceren en te analyseren. Dit is waardevol voor audits en het opsporen van beleidsfouten.

**Replay Scenario's**:
- **Audit onderzoek**: reproduceer een specifieke autorisatiebeslissing om te verifiëren of deze correct was
- **Policy debugging**: test nieuwe beleidsversies tegen historische requests
- **Compliance rapportage**: genereer rapporten over wie wanneer toegang heeft gekregen tot welke resources
- **Incident response**: analyseer autorisatiepatronen rondom een security incident

### Retention en Privacy

**Bewaartermijn**: Log records MOETEN minimaal 12 maanden worden bewaard voor audit doeleinden. Langere bewaartermijnen kunnen nodig zijn afhankelijk van regelgeving (bijvoorbeeld AVG, Archiefwet).

**Privacy overwegingen**:
- Persoonlijke identificeerbare informatie (PII) in log records MOET worden beschermd
- Toegang tot logs MOET worden beperkt tot geautoriseerd personeel
- Log records MOGEN pseudonimisering of encryptie gebruiken voor PII
- Bij het delen van logs voor analyse MOETEN PII worden geanonimiseerd

---

## Tests & Validatie

IAM repositories gebruiken de centrale `Automatisering` workflows voor kwaliteitscontrole en validatie.

### Toegankelijkheidsvalidatie

```bash
# WCAG toegankelijkheidscheck op gepubliceerde specificatie
npx @axe-core/cli https://logius-standaarden.github.io/OAuth-NL-profiel/ --tags wcag2aa
```

De specificaties MOETEN voldoen aan WCAG 2.1 niveau AA voor toegankelijkheid. Dit omvat:
- Correcte heading structuur
- Alt-teksten voor afbeeldingen
- Voldoende kleurcontrast
- Toetsenbordnavigatie

### Markdown Linting

```bash
# Markdown lint op bronbestanden
npx markdownlint-cli '**/*.md'
```

Markdown bestanden MOETEN voldoen aan de markdownlint regels voor consistente opmaak.

### CI/CD Workflows

```bash
# CI/CD workflows bekijken voor een specifieke repository
gh api repos/logius-standaarden/OAuth-NL-profiel/contents/.github/workflows --jq '.[].name'
```

Alle IAM repositories gebruiken GitHub Actions voor:
- Automatische publicatie naar GitHub Pages
- Markdown linting bij elke commit
- Toegankelijkheidsvalidatie bij releases

---

## Handige Commando's

### Repository Status

```bash
# Laatste wijzigingen OAuth NL profiel
gh api repos/logius-standaarden/OAuth-NL-profiel/commits --jq '.[:5] | .[] | "\(.commit.committer.date) \(.commit.message | split("\n")[0])"'

# Laatste wijzigingen OIDC NL GOV
gh api repos/logius-standaarden/OIDC-NLGOV/commits --jq '.[:5] | .[] | "\(.commit.committer.date) \(.commit.message | split("\n")[0])"'

# Laatste wijzigingen AuthZEN NL GOV
gh api repos/logius-standaarden/authzen-nlgov/commits --jq '.[:5] | .[] | "\(.commit.committer.date) \(.commit.message | split("\n")[0])"'
```

### Issue Tracking

```bash
# Open issues per repository
gh issue list --repo logius-standaarden/OAuth-NL-profiel
gh issue list --repo logius-standaarden/OIDC-NLGOV
gh issue list --repo logius-standaarden/authzen-nlgov
gh issue list --repo logius-standaarden/authorization-decision-log
gh issue list --repo logius-standaarden/st-saml-spec
gh issue list --repo logius-standaarden/OIN-Stelsel

# Issues met specifieke labels
gh issue list --repo logius-standaarden/OAuth-NL-profiel --label "enhancement"
gh issue list --repo logius-standaarden/OIDC-NLGOV --label "bug"
```

### Content Verkenning

```bash
# AuthZEN specificatie inhoud bekijken
gh api repos/logius-standaarden/authzen-nlgov/contents --jq '.[].name'

# Authorization Decision Log specificatie inhoud
gh api repos/logius-standaarden/authorization-decision-log/contents --jq '.[].name'

# SAML specificatie inhoud
gh api repos/logius-standaarden/st-saml-spec/contents --jq '.[].name'
```

### Zoeken naar Specifieke Termen

```bash
# Zoek naar PKCE in IAM repositories
gh search code "PKCE" --owner logius-standaarden

# Zoek naar acr_values (betrouwbaarheidsniveaus)
gh search code "acr_values" --owner logius-standaarden

# Zoek naar PDP (Policy Decision Point)
gh search code "PDP" --owner logius-standaarden

# Zoek naar private_key_jwt
gh search code "private_key_jwt" --owner logius-standaarden

# Zoek naar mTLS
gh search code "mTLS" --owner logius-standaarden
```

### Pull Requests

```bash
# Open pull requests per repository
gh pr list --repo logius-standaarden/OAuth-NL-profiel
gh pr list --repo logius-standaarden/OIDC-NLGOV
gh pr list --repo logius-standaarden/authzen-nlgov

# Pull requests met specifieke status
gh pr list --repo logius-standaarden/OAuth-NL-profiel --state merged
gh pr list --repo logius-standaarden/OIDC-NLGOV --state closed
```

---

## Gerelateerde Skills

| Skill | Relatie |
|-------|---------|
| `/ls-api` | API Design Rules vereisen OAuth/OIDC voor authenticatie |
| `/ls-dk` | OIN-stelsel gedeeld met Digikoppeling, mTLS-certificaten |
| `/ls-logboek` | Authorization Decision Log raakt aan logging dataverwerkingen |
| `/ls-fsc` | FSC Logging integratie via transaction_id |
| `/ls` | Overzicht alle standaarden |

---

## Aanvullende Bronnen

- **OAuth 2.0 RFC 6749**: De basis OAuth 2.0 specificatie
- **RFC 7636 (PKCE)**: Proof Key for Code Exchange
- **RFC 9068**: JSON Web Token (JWT) Profile for OAuth 2.0 Access Tokens
- **RFC 9126**: OAuth 2.0 Pushed Authorization Requests
- **OpenID Connect Core 1.0**: De basis OIDC specificatie
- **iGov Profile**: International Government Assurance Profile for OpenID Connect
- **eIDAS Verordening**: EU-verordening voor elektronische identificatie en vertrouwensdiensten
- **XACML**: eXtensible Access Control Markup Language (referentiemodel voor AuthZEN)
- **W3C Trace Context**: Standaard voor distributed tracing (gebruikt in Authorization Decision Log)

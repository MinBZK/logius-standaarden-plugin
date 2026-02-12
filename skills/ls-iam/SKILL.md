---
name: ls-iam
description: "Gebruik deze skill wanneer de gebruiker vraagt over 'OAuth', 'OpenID Connect', 'OIDC', 'authenticatie', 'autorisatie', 'AuthZEN', 'SAML', 'identity management', 'toegangsbeheer', 'OAuth NL profiel', 'OIDC NL GOV', 'authorization decision', 'OIN authenticatie'."
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

# Identity & Access Management (IAM)

De IAM-standaarden van Logius definiëren profielen voor authenticatie en autorisatie bij Nederlandse overheids-APIs. Dit omvat OAuth 2.0, OpenID Connect, AuthZEN en aanvullende profielen specifiek voor de Nederlandse overheid. Deze standaarden waarborgen een consistent en veilig identiteits- en toegangsbeheer binnen het publieke domein.

## Repositories

| Repository | Beschrijving | Publicatie |
|-----------|-------------|-----------|
| [OAuth-NL-profiel](https://github.com/logius-standaarden/OAuth-NL-profiel) | Nederlands profiel voor OAuth 2.0 | [Lees online](https://logius-standaarden.github.io/OAuth-NL-profiel/) |
| [OIDC-NLGOV](https://github.com/logius-standaarden/OIDC-NLGOV) | OpenID Connect profiel voor NL overheid | [Lees online](https://logius-standaarden.github.io/OIDC-NLGOV/) |
| [OAuth-Inleiding](https://github.com/logius-standaarden/OAuth-Inleiding) | Introductie en achtergrond OAuth | [Lees online](https://logius-standaarden.github.io/OAuth-Inleiding/) |
| [OAuth-Beheermodel](https://github.com/logius-standaarden/OAuth-Beheermodel) | Beheermodel voor OAuth standaarden | [Lees online](https://logius-standaarden.github.io/OAuth-Beheermodel/) |
| [authzen-nlgov](https://github.com/logius-standaarden/authzen-nlgov) | AuthZEN NL GOV profiel (autorisatiebeslissingen) | [Lees online](https://logius-standaarden.github.io/authzen-nlgov/) |
| [authorization-decision-log](https://github.com/logius-standaarden/authorization-decision-log) | Logging van autorisatiebeslissingen | [Lees online](https://logius-standaarden.github.io/authorization-decision-log/) |
| [st-saml-spec](https://github.com/logius-standaarden/st-saml-spec) | SAML specificatie voor NL overheid | [Lees online](https://logius-standaarden.github.io/st-saml-spec/) |
| [OIN-Stelsel](https://github.com/logius-standaarden/OIN-Stelsel) | Organisatie Identificatie Nummer (ook in `/ls-dk`) | [Lees online](https://logius-standaarden.github.io/OIN-Stelsel/) |

---

## OAuth 2.0 NL Profiel

Het Nederlandse OAuth 2.0 profiel scherpt de basisspecificatie (RFC 6749) aan met strikte beveiligingseisen voor gebruik binnen de overheid. Doel is het voorkomen van veelvoorkomende kwetsbaarheden en het garanderen van interoperabiliteit.

### Verplichte beveiligingseisen

- **PKCE (Proof Key for Code Exchange)** is verplicht voor alle clients, inclusief confidential clients. Dit voorkomt authorization code interception aanvallen. Clients genereren een `code_verifier` en sturen een `code_challenge` mee in het authorization request.
- **Grant types**: uitsluitend `authorization_code` en `client_credentials` zijn toegestaan. Implicit grant en Resource Owner Password Credentials zijn expliciet verboden vanwege bekende beveiligingsrisico's.
- **Tokens als HTTP headers**: access tokens worden uitsluitend als HTTP `Authorization` header (Bearer scheme) meegegeven. Transport via query parameters is niet toegestaan om te voorkomen dat tokens in server logs en browser history terechtkomen.

### Token specificaties

- **Access tokens**: JWT-formaat (RFC 9068) wordt aanbevolen. Dit maakt lokale validatie mogelijk zonder een introspection endpoint aan te roepen, wat de prestaties verbetert.
- **Refresh tokens**: verplicht bij de `authorization_code` flow. Hiermee kunnen clients nieuwe access tokens verkrijgen zonder hernieuwde gebruikersinteractie. Refresh token rotation wordt aanbevolen.
- **Token levensduur**: access tokens hebben een korte levensduur. De exacte duur wordt bepaald door de authorization server, maar korte TTL's (minuten) verdienen de voorkeur.

### Client authenticatie

Het token endpoint vereist sterke client authenticatie:

- **mTLS (Mutual TLS)**: de client presenteert een TLS-certificaat (bij voorkeur PKIoverheid) bij het aanroepen van het token endpoint. Dit biedt sterke cryptografische binding.
- **private_key_jwt**: de client ondertekent een JWT-assertion met zijn private key en stuurt deze mee als client authenticatie. De authorization server valideert met de geregistreerde public key.

### Toegangsbeheer en aanbevelingen

- **Scopes** vormen het primaire mechanisme voor toegangsbeheer. Scopes definiëren de reikwijdte van de toegang die een client namens de resource owner verkrijgt.
- **Pushed Authorization Requests (PAR)** worden aanbevolen (RFC 9126). Hiermee stuurt de client het authorization request eerst naar een dedicated endpoint van de authorization server, in plaats van via de browser. Dit voorkomt manipulatie van request parameters en vergroot de vertrouwelijkheid.

---

## OpenID Connect NL GOV Profiel

Het OIDC NL GOV profiel bouwt voort op OpenID Connect Core 1.0 en het iGov-profiel, en specificeert eisen voor authenticatie binnen de Nederlandse overheid. Het profiel legt de nadruk op betrouwbaarheidsniveaus en gestructureerde identiteitsinformatie.

### Betrouwbaarheidsniveaus (Levels of Assurance)

Het profiel definieert drie betrouwbaarheidsniveaus conform de eIDAS-verordening:

| Niveau | eIDAS Level | Toepassing |
|--------|-------------|------------|
| **Laag** | Low | Zelfregistratie, beperkte toegang |
| **Substantieel** | Substantial | DigiD Midden/Hoog, eHerkenning EH3 |
| **Hoog** | High | eHerkenning EH4, face-to-face verificatie |

De `acr_values` parameter in het authentication request mapt naar deze eIDAS-niveaus. De OpenID Provider geeft het daadwerkelijk bereikte niveau terug in de `acr` claim van het ID Token.

### Verplichte claims in het ID Token

Het ID Token moet minimaal de volgende claims bevatten:

| Claim | Beschrijving |
|-------|-------------|
| `sub` | Unieke identifier van de gebruiker (subject) |
| `iss` | Identifier van de OpenID Provider (issuer) |
| `aud` | Identifier van de Relying Party (audience) |
| `exp` | Verloopdatum van het token (expiration) |
| `iat` | Tijdstip van uitgifte (issued at) |
| `acr` | Betrouwbaarheidsniveau van de authenticatie (authentication context class reference) |

### Userinfo endpoint

Voor aanvullende claims (zoals naam, e-mailadres, BSN) wordt het Userinfo endpoint gebruikt. De Relying Party roept dit endpoint aan met het access token. Welke claims beschikbaar zijn hangt af van de gevraagde scopes en het autorisatiebeleid van de OpenID Provider.

### ID Token validatie

Relying Parties moeten het ID Token volledig valideren:

- Controleer de `iss` claim tegen de verwachte issuer
- Controleer de `aud` claim tegen de eigen client_id
- Controleer de `exp` claim (token mag niet verlopen zijn)
- Valideer de digitale handtekening met de publieke sleutel van de OpenID Provider (via het JWKS endpoint)
- Controleer de `acr` claim tegen het gevraagde betrouwbaarheidsniveau

### Client registratie

Clients moeten vooraf worden geregistreerd bij de OpenID Provider. Bij registratie worden vastgelegd:

- Redirect URI's (exacte match, geen wildcards)
- Ondersteunde response types en grant types
- Client authenticatiemethode (conform OAuth NL profiel)
- Gevraagde scopes en claims
- Publieke sleutels voor token validatie

---

## AuthZEN NL GOV

Het AuthZEN NL GOV profiel specificeert een architectuur voor geëxternaliseerde autorisatie. In plaats van autorisatielogica in applicaties te bouwen, worden autorisatiebeslissingen gedelegeerd aan een centraal beslispunt. Dit bevordert consistentie, herbruikbaarheid en auditeerbaarheid.

### Architectuurcomponenten

Het profiel volgt het XACML-referentiemodel met vier kerncomponenten:

| Component | Rol | Beschrijving |
|-----------|-----|-------------|
| **PDP** | Policy Decision Point | Evalueert autorisatieverzoeken tegen het beleid en neemt een beslissing (permit/deny) |
| **PEP** | Policy Enforcement Point | Onderschept verzoeken en handhaaft de beslissing van het PDP |
| **PAP** | Policy Administration Point | Beheert autorisatiebeleid: aanmaken, wijzigen, verwijderen van policies |
| **PIP** | Policy Information Point | Levert aanvullende attributen en contextinformatie aan het PDP voor besluitvorming |

### Evaluation endpoint

Het PDP biedt een gestandaardiseerd evaluation endpoint:

```
POST /access/v1/evaluation
Content-Type: application/json
```

### Request formaat

Een autorisatieverzoek bestaat uit vier elementen:

- **subject**: de entiteit die toegang vraagt (gebruiker, systeem)
- **action**: de gevraagde actie
- **resource**: het object waarop de actie wordt uitgevoerd
- **context**: aanvullende omstandigheden (optioneel)

Voorbeeld van een autorisatieverzoek:

```json
{
  "subject": {
    "type": "user",
    "id": "alice"
  },
  "action": {
    "name": "approve"
  },
  "resource": {
    "type": "holiday-request",
    "id": "446epbc8y7",
    "properties": {
      "employee": "bob"
    }
  }
}
```

In dit voorbeeld vraagt gebruiker Alice toestemming om een verlofaanvraag van Bob goed te keuren. Het PDP evalueert of Alice de juiste rol en bevoegdheid heeft om deze actie uit te voeren.

### Response formaat

Het PDP retourneert een beslissing:

- **decision** (boolean): `true` voor toestaan, `false` voor weigeren
- **context** (optioneel): aanvullende informatie, zoals de reden van weigering of referenties naar de toegepaste beleidsregels

```json
{
  "decision": true
}
```

Bij een weigering kan het PDP een reden meegeven:

```json
{
  "decision": false,
  "context": {
    "reason": {
      "48": "No signing authority"
    }
  }
}
```

---

## Authorization Decision Log

De Authorization Decision Log standaard definieert een gestructureerd formaat voor het vastleggen van autorisatiebeslissingen. Dit is essentieel voor audit, verantwoording en het kunnen reproduceren van historische beslissingen.

### Verplichte velden

| Veld | Beschrijving |
|------|-------------|
| `timestamp` | Tijdstip van de beslissing (ISO 8601) |
| `trace_id` | Unieke trace identifier conform W3C Trace Context |
| `span_id` | Span identifier binnen de trace |
| `type` | Type van het record, bijvoorbeeld `evaluation` |
| `request` | Het oorspronkelijke autorisatieverzoek (subject, action, resource, context) |
| `response` | De autorisatiebeslissing (decision en eventuele context) |

### Optionele velden

| Veld | Beschrijving |
|------|-------------|
| `policies` | Referenties naar toegepaste beleidsregels, inclusief versienummers |
| `information` | Aanvullende data die het PIP heeft geleverd voor de besluitvorming |
| `configuration` | Configuratie van het PDP ten tijde van de beslissing |

### Voorbeeld log record

```json
{
  "timestamp": "2025-09-07T10:14:18Z",
  "trace_id": "28dbeec32e77635cc19bc3204ec56c41",
  "span_id": "893e1b2ac52d712f",
  "type": "evaluation",
  "request": {
    "subject": { "type": "user", "id": "alice" },
    "action": { "name": "approve" },
    "resource": { "type": "holiday-request", "id": "446epbc8y7" }
  },
  "response": {
    "decision": false,
    "context": {
      "reason": {
        "48": "No signing authority"
      }
    }
  }
}
```

### Integratie en transport

- **FSC Logging**: de Authorization Decision Log integreert met FSC (Federated Service Connectivity) logging via het `transaction_id` veld. Hiermee kunnen autorisatiebeslissingen worden gerelateerd aan de bredere transactieketen.
- **OpenTelemetry Protocol (OTLP)**: OTLP wordt aanbevolen als transportprotocol voor het verzenden van log records naar centrale logging-infrastructuur. Dit sluit aan bij de W3C Trace Context die al in de `trace_id` en `span_id` velden wordt gebruikt.
- **Replay van historische beslissingen**: doordat het volledige request, de response en optioneel de policies en PIP-informatie worden vastgelegd, is het mogelijk om historische beslissingen te reproduceren en te analyseren. Dit is waardevol voor audits en het opsporen van beleidsfouten.

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

---

## OIN (Organisatie Identificatie Nummer)

Het Organisatie Identificatie Nummer (OIN) is de unieke identifier voor organisaties binnen het Nederlandse overheidsdomein. Het OIN wordt onder andere gebruikt in:

- **PKIoverheid-certificaten**: het OIN is opgenomen in het `subject.serialNumber` veld van PKIoverheid-certificaten die worden gebruikt voor server- en client-authenticatie (mTLS)
- **Digikoppeling**: als organisatie-identifier in berichtenuitwisseling
- **OAuth en OIDC**: als client identifier of als claim voor organisatie-identificatie

Het OIN-stelsel wordt ook beschreven in de Digikoppeling-standaarden (zie `/ls-dk`).

---

## Implementatievoorbeelden

### OAuth 2.0 Authorization Code Flow met PKCE (Python)

```python
# Complete example using requests library
import hashlib, base64, secrets, requests

# 1. PKCE code_verifier en code_challenge genereren
code_verifier = secrets.token_urlsafe(64)  # min 43, max 128 chars
code_challenge = base64.urlsafe_b64encode(
    hashlib.sha256(code_verifier.encode()).digest()
).rstrip(b'=').decode()

# 2. Authorization request (redirect user to this URL)
auth_url = "https://auth.example.com/authorize"
params = {
    "client_id": "my-client-id",
    "response_type": "code",
    "scope": "openid profile",
    "redirect_uri": "https://myapp.example.com/callback",
    "state": secrets.token_urlsafe(32),  # min 128 bits entropy
    "code_challenge": code_challenge,
    "code_challenge_method": "S256",
    "acr_values": "http://eidas.europa.eu/LoA/substantial",  # eIDAS level
}
# redirect_url = f"{auth_url}?{'&'.join(f'{k}={v}' for k, v in params.items())}"

# 3. Token request (after receiving authorization code)
token_response = requests.post("https://auth.example.com/token", data={
    "grant_type": "authorization_code",
    "code": "received_auth_code",
    "redirect_uri": "https://myapp.example.com/callback",
    "client_id": "my-client-id",
    "code_verifier": code_verifier,  # PKCE proof
})
tokens = token_response.json()
# tokens = {"access_token": "eyJ...", "token_type": "Bearer", "id_token": "eyJ...", "expires_in": 3600}
```

### Client Credentials Flow met private_key_jwt (Python)

```python
import jwt, time, uuid, requests

# JWT client assertion aanmaken (private_key_jwt authenticatie)
private_key = open("client_private_key.pem").read()

now = int(time.time())
client_assertion = jwt.encode({
    "iss": "my-client-id",
    "sub": "my-client-id",
    "aud": "https://auth.example.com/token",
    "jti": str(uuid.uuid4()),  # uniek per request
    "iat": now,
    "exp": now + 300,  # max 5 minuten geldig
}, private_key, algorithm="PS256", headers={"kid": "my-key-id"})

# Token request met client assertion
response = requests.post("https://auth.example.com/token", data={
    "grant_type": "client_credentials",
    "scope": "api.read api.write",
    "client_assertion_type": "urn:ietf:params:oauth:client-assertion-type:jwt-bearer",
    "client_assertion": client_assertion,
})
access_token = response.json()["access_token"]
```

### JWT Access Token Validatie (Python)

```python
import jwt, requests

# JWKS ophalen van de authorization server
jwks_url = "https://auth.example.com/.well-known/jwks.json"
jwks = requests.get(jwks_url).json()

# Access token valideren
try:
    payload = jwt.decode(
        access_token,
        jwt.PyJWKClient(jwks_url).get_signing_key_from_jwt(access_token),
        algorithms=["RS256", "PS256"],
        audience="https://api.example.com",  # verwachte audience
        issuer="https://auth.example.com",   # verwachte issuer
        options={
            "verify_exp": True,
            "verify_iat": True,
            "require": ["iss", "sub", "aud", "exp", "jti", "client_id"],
        }
    )
    # Verplichte claims per NL GOV profiel
    assert "client_id" in payload
    assert "jti" in payload  # uniek, niet herbruikbaar
except jwt.ExpiredSignatureError:
    # Token verlopen - nieuwe token ophalen
    pass
except jwt.InvalidTokenError as e:
    # Token ongeldig - toegang weigeren
    pass
```

### OIDC Discovery Endpoint Ophalen (curl)

```bash
# Discovery document ophalen
curl -s https://auth.example.com/.well-known/openid-configuration | jq '{
  issuer,
  authorization_endpoint,
  token_endpoint,
  jwks_uri,
  scopes_supported,
  response_types_supported,
  grant_types_supported,
  acr_values_supported,
  subject_types_supported,
  token_endpoint_auth_methods_supported
}'
```

### ID Token Validatie Checklist

Bij het valideren van een OIDC ID Token MOETEN de volgende stappen worden doorlopen:

1. **Signature** - Verifieer de JWS handtekening met de publieke sleutel van de provider (via `jwks_uri`)
2. **iss** - MOET exact overeenkomen met de `issuer` uit het discovery document
3. **aud** - MOET de `client_id` van de relying party bevatten
4. **nonce** - MOET exact overeenkomen met de nonce uit het authorization request
5. **exp** - MOET in de toekomst liggen (token niet verlopen)
6. **iat** - MAG niet te ver in het verleden liggen (max 5 minuten aanbevolen)
7. **acr** - MOET minimaal het gevraagde betrouwbaarheidsniveau bevatten
8. **jti** - MOET uniek zijn (bewaar 12+ maanden om hergebruik te detecteren)

### FastAPI Resource Server met Token Validatie

```python
from fastapi import FastAPI, Depends, HTTPException, Security
from fastapi.security import HTTPBearer, HTTPAuthorizationCredentials
import jwt, requests

app = FastAPI(title="NL GOV Protected Resource", version="1.0.0")
security = HTTPBearer()

JWKS_URL = "https://auth.example.com/.well-known/jwks.json"
ISSUER = "https://auth.example.com"
AUDIENCE = "https://api.example.com"

jwks_client = jwt.PyJWKClient(JWKS_URL)

async def validate_token(credentials: HTTPAuthorizationCredentials = Security(security)):
    """Valideer OAuth 2.0 NL GOV access token."""
    try:
        signing_key = jwks_client.get_signing_key_from_jwt(credentials.credentials)
        payload = jwt.decode(
            credentials.credentials,
            signing_key,
            algorithms=["RS256", "PS256"],
            audience=AUDIENCE,
            issuer=ISSUER,
        )
        return payload
    except jwt.InvalidTokenError:
        raise HTTPException(status_code=401, detail="Invalid or expired token")

@app.get("/v1/resources")
async def get_resources(token: dict = Depends(validate_token)):
    """Beschermd endpoint - vereist geldig NL GOV OAuth token."""
    return {
        "client_id": token.get("client_id"),
        "sub": token.get("sub"),
        "scope": token.get("scope"),
        "data": [{"id": 1, "name": "Resource"}]
    }
```

## Foutafhandeling

### OAuth 2.0 Error Responses

Het NL GOV profiel volgt RFC 6749 voor error responses:

```json
{
  "error": "invalid_grant",
  "error_description": "The authorization code has expired"
}
```

| Error Code | HTTP Status | Beschrijving |
|-----------|-------------|-------------|
| `invalid_request` | 400 | Verplichte parameter ontbreekt of ongeldig |
| `invalid_client` | 401 | Client authenticatie mislukt (verkeerde client_assertion) |
| `invalid_grant` | 400 | Authorization code verlopen of al gebruikt |
| `unauthorized_client` | 400 | Client niet geautoriseerd voor dit grant type |
| `unsupported_grant_type` | 400 | Grant type niet ondersteund |
| `invalid_scope` | 400 | Scope ongeldig of niet beschikbaar |

### Token Introspection Response

```bash
# Token introspection (voor resource servers die tokens niet zelf kunnen valideren)
curl -X POST https://auth.example.com/introspect \
  -H "Authorization: Bearer $RESOURCE_SERVER_TOKEN" \
  -d "token=$ACCESS_TOKEN" | jq

# Response bij geldig token:
# {"active": true, "scope": "api.read", "client_id": "...", "sub": "...", "exp": 1234567890}
# Response bij ongeldig token:
# {"active": false}
```

---

## Tests & Validatie

IAM repositories gebruiken de centrale `Automatisering` workflows:

```bash
# WCAG toegankelijkheidscheck op gepubliceerde specificatie
npx @axe-core/cli https://logius-standaarden.github.io/OAuth-NL-profiel/ --tags wcag2aa

# Markdown lint op bronbestanden
npx markdownlint-cli '**/*.md'

# CI/CD workflows bekijken voor een specifieke repository
gh api repos/logius-standaarden/OAuth-NL-profiel/contents/.github/workflows --jq '.[].name'
```

## Handige Commando's

```bash
# Laatste wijzigingen OAuth NL profiel
gh api repos/logius-standaarden/OAuth-NL-profiel/commits --jq '.[:5] | .[] | "\(.commit.committer.date) \(.commit.message | split("\n")[0])"'

# Open issues per repository
gh issue list --repo logius-standaarden/OAuth-NL-profiel
gh issue list --repo logius-standaarden/OIDC-NLGOV
gh issue list --repo logius-standaarden/authzen-nlgov
gh issue list --repo logius-standaarden/authorization-decision-log

# AuthZEN specificatie inhoud bekijken
gh api repos/logius-standaarden/authzen-nlgov/contents --jq '.[].name'

# Zoek naar specifieke termen in IAM repositories
gh search code "PKCE" --owner logius-standaarden
gh search code "acr_values" --owner logius-standaarden
gh search code "PDP" --owner logius-standaarden
```

## Gerelateerde Skills

| Skill | Relatie |
|-------|---------|
| `/ls-api` | API Design Rules vereisen OAuth/OIDC voor authenticatie |
| `/ls-dk` | OIN-stelsel gedeeld met Digikoppeling, mTLS-certificaten |
| `/ls-logboek` | Authorization Decision Log raakt aan logging dataverwerkingen |
| `/ls-fsc` | FSC Logging integratie via transaction_id |
| `/ls` | Overzicht alle standaarden |

---
name: ls-fsc
description: "Gebruik deze skill wanneer de gebruiker vraagt over 'FSC', 'Federated Service Connectivity', 'federatieve dienstverlening', 'inway', 'outway', 'service directory', 'regulated area', 'external contract', 'fsc-core'."
model: sonnet
allowed-tools:
  - Bash(gh api *)
  - Bash(gh issue list *)
  - Bash(gh pr list *)
  - Bash(gh search *)
  - Bash(curl -s *)
  - Bash(npx markdownlint *)
  - Bash(npx @axe-core/cli *)
  - Bash(node *)
  - WebFetch(*)
---

# Federated Service Connectivity (FSC)

FSC is de standaard voor federatieve dienstverlening binnen de Nederlandse overheid. Het definieert hoe organisaties op een gestandaardiseerde manier services kunnen aanbieden en afnemen via een federatief netwerk. FSC maakt gebruik van mTLS-beveiliging, OAuth 2.0-tokens en een contractmodel met cryptografische handtekeningen om veilige, verifieerbare communicatie tussen organisaties te garanderen.

## Repositories

| Repository | Beschrijving | Publicatie |
|-----------|-------------|-----------|
| [fsc-core](https://github.com/logius-standaarden/fsc-core) | Core specificatie: architectuur, protocol, componenten | [Lees online](https://logius-standaarden.github.io/fsc-core/) |
| [fsc-logging](https://github.com/logius-standaarden/fsc-logging) | Logging specificatie voor FSC transacties | [Lees online](https://logius-standaarden.github.io/fsc-logging/) |
| [fsc-properties](https://github.com/logius-standaarden/fsc-properties) | Metadata-properties voor FSC services | [Lees online](https://logius-standaarden.github.io/fsc-properties/) |
| [fsc-regulated-area](https://github.com/logius-standaarden/fsc-regulated-area) | Regulated Area specificatie (governance zones) | [Lees online](https://logius-standaarden.github.io/fsc-regulated-area/) |
| [fsc-external-contract](https://github.com/logius-standaarden/fsc-external-contract) | External Contract specificatie (afspraken tussen partijen) | [Lees online](https://logius-standaarden.github.io/fsc-external-contract/) |
| [fsc-extensie-template](https://github.com/logius-standaarden/fsc-extensie-template) | Template voor FSC extensies | [Lees online](https://logius-standaarden.github.io/fsc-extensie-template/) |
| [fsc-test-suite](https://github.com/logius-standaarden/fsc-test-suite) | Integratietests en componenttests | - |

## Architectuur

FSC kent een componentmodel waarin organisaties (Peers) via proxies en managers met elkaar communiceren. Hieronder de kerncomponenten:

### Peer

Een **Peer** is een actor (organisatie) die deelneemt aan het FSC-netwerk. Een Peer kan services aanbieden (provider) en/of afnemen (consumer). Elke Peer wordt uniek geidentificeerd door een **PeerID**, afgeleid uit het subject van het X.509-certificaat dat de Peer gebruikt voor mTLS-verbindingen.

### Group

Een **Group** is een logisch systeem van een Peer, bestaande uit een combinatie van Inways, Outways en een Manager. Een Group vertegenwoordigt de technische infrastructuur waarmee een Peer deelneemt aan het netwerk. Een Peer kan meerdere Groups hebben, bijvoorbeeld voor verschillende omgevingen (acceptatie, productie).

### Inway

De **Inway** is een reverse proxy die inkomende verbindingen van andere Peers afhandelt. De Inway:

- Ontvangt HTTP-verzoeken van Outways van andere organisaties
- Valideert het `Fsc-Authorization` header (JWT access token)
- Controleert of het token geldig is en de juiste claims bevat
- Stuurt het verzoek door naar de achterliggende service bij succesvolle validatie
- Retourneert de response van de service naar de aanvragende Outway

### Outway

De **Outway** is een forward proxy die uitgaande verbindingen naar services van andere Peers afhandelt. De Outway:

- Ontvangt verzoeken van interne applicaties van de organisatie
- Verkrijgt een access token bij de Manager van de aanbieder via OAuth 2.0 Client Credentials
- Voegt het token toe als `Fsc-Authorization` header aan het uitgaande verzoek
- Stuurt het verzoek via mTLS naar de Inway van de aanbieder
- Retourneert de response naar de interne applicatie

### Manager

De **Manager** is de centrale API-component die contracten beheert en als authorization server fungeert. De Manager:

- Beheert het aanmaken, ondertekenen, accepteren en intrekken van Contracts
- Fungeert als OAuth 2.0 Authorization Server voor het uitgeven van access tokens
- Publiceert services in de Directory via ServicePublicationGrants
- Biedt een API voor het opvragen van Peers en Services
- Handelt de `POST /token` endpoint af voor tokenuitgifte

### Directory

De **Directory** is een speciale Manager die functioneert als centraal register voor service- en peer-discovery. Peers publiceren hun services in de Directory via een **ServicePublicationGrant** in een contract. Andere Peers kunnen de Directory raadplegen om beschikbare services en hun aanbieders te vinden.

### Contract

Een **Contract** is een formele, cryptografisch ondertekende overeenkomst tussen twee of meer Peers. Een Contract bevat:

- Een of meer **Grants** (rechten/toestemmingen)
- Cryptografische **handtekeningen** van alle betrokken partijen
- Een **geldigheidsperiode** (validFrom, validUntil)
- **Certificate thumbprints** van de ondertekenaars

## Trust Model

FSC gebruikt een op certificaten gebaseerd vertrouwensmodel:

### mTLS met X.509-certificaten

Alle verbindingen tussen FSC-componenten worden beveiligd met mutual TLS (mTLS). Beide partijen (client en server) presenteren een X.509-certificaat en valideren het certificaat van de tegenpartij.

### Trust Anchors

Een **Trust Anchor** is een root- of intermediate-certificaat dat wordt vertrouwd binnen het netwerk. Alle Peers moeten certificaten gebruiken die zijn uitgegeven onder een gedeelde Trust Anchor. Dit vormt de basis van het vertrouwensmodel.

### PeerID

Het **PeerID** wordt afgeleid uit het subject van het X.509-certificaat van een Peer. Dit is de unieke identifier waarmee een organisatie wordt herkend binnen het FSC-netwerk. Het PeerID wordt gebruikt in contracten, tokens en API-aanroepen.

### Certificate Thumbprints

Contracten bevatten **Certificate Thumbprints** (SHA-256 hashes van de DER-encoded certificaten) van de ondertekenaars. Hiermee kan worden geverifieerd welk specifiek certificaat is gebruikt voor de ondertekening van een contract. De thumbprint wordt ook opgenomen in access tokens via de `cnf` (confirmation) claim.

## Service Connectivity Flow

Het proces waarmee een consumer-organisatie een service van een provider-organisatie afneemt verloopt in de volgende stappen:

### Stap 1: Contract aanmaken

De consumer maakt een Contract aan met daarin een **ServiceConnectionGrant**. Dit grant specificeert welke service de consumer wil afnemen en bij welke provider.

### Stap 2: Contract ondertekenen door consumer

De consumer ondertekent het contract cryptografisch met zijn eigen certificaat. De handtekening bevat de certificate thumbprint (SHA-256).

### Stap 3: Contract verzenden naar provider

Het ondertekende contract wordt via de Manager API (`POST /contracts`) naar de Manager van de provider gestuurd.

### Stap 4: Contract ondertekenen door provider

De provider beoordeelt het contract en ondertekent het bij akkoord (`PUT /contracts/{hash}/accept`). Het contract is nu geldig en beide partijen hebben een ondertekend exemplaar.

### Stap 5: Access token verkrijgen

De consumer (via de Outway) vraagt een access token aan bij de Manager van de provider via het **OAuth 2.0 Client Credentials** grant:

```
POST /token HTTP/1.1
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials
&scope=<GrantHash>
&client_id=<PeerID>
```

- `grant_type`: altijd `client_credentials`
- `scope`: de hash van het specifieke Grant dat de consumer wil gebruiken
- `client_id`: het PeerID van de consumer (afgeleid uit het certificaat)

De Manager valideert de aanvraag, controleert of er een geldig contract bestaat met het opgegeven grant, en geeft een JWT access token terug.

### Stap 6: Verzoek via Outway naar Inway

De Outway van de consumer stuurt het HTTP-verzoek naar de Inway van de provider. Het access token wordt meegegeven in de `Fsc-Authorization` header:

```
GET /service/endpoint HTTP/1.1
Host: inway.provider.example.nl
Fsc-Authorization: Bearer <JWT-token>
```

### Stap 7: Validatie en doorsturen door Inway

De Inway van de provider:

1. Extraheert het JWT uit de `Fsc-Authorization` header
2. Valideert de handtekening van het token
3. Controleert de claims (`aud`, `svc`, `gth`, `cnf`)
4. Verifieert dat het certificaat van de verbinding overeenkomt met de `cnf` thumbprint
5. Stuurt het verzoek door naar de achterliggende service
6. Retourneert de response aan de Outway

## Contract & Grant Types

### Contract JSON-structuur

Een contract heeft de volgende globale structuur:

```json
{
  "version": "1",
  "validFrom": "2025-01-01T00:00:00Z",
  "validUntil": "2026-01-01T00:00:00Z",
  "grants": [
    {
      "type": "ServiceConnectionGrant",
      "servicePeerID": "<PeerID-provider>",
      "serviceName": "mijn-service",
      "consumerPeerID": "<PeerID-consumer>"
    }
  ],
  "signatures": [
    {
      "peerID": "<PeerID-consumer>",
      "certificateThumbprint": "<SHA-256 hash>",
      "signature": "<base64-encoded signature>"
    },
    {
      "peerID": "<PeerID-provider>",
      "certificateThumbprint": "<SHA-256 hash>",
      "signature": "<base64-encoded signature>"
    }
  ]
}
```

### Grant Types

FSC kent vier typen grants:

#### 1. ServicePublicationGrant

Publiceert een service in de Directory. Hiermee maakt een Peer zijn service vindbaar voor andere Peers.

- **Gebruik**: Een provider registreert zijn service in de Directory
- **Partijen**: Provider (eigenaar service) en Directory
- **Effect**: De service verschijnt in de Directory en is vindbaar via `GET /services`

#### 2. DelegatedServicePublicationGrant

Publiceert een service in de Directory namens een andere Peer. Dit is relevant wanneer een organisatie het beheer van servicepublicatie delegeert aan een derde partij.

- **Gebruik**: Een gemachtigde partij publiceert een service namens de eigenaar
- **Partijen**: Gemachtigde, eigenaar van de service, en Directory
- **Effect**: De service verschijnt in de Directory met verwijzing naar de eigenaar

#### 3. ServiceConnectionGrant

Geeft een consumer toegang tot een specifieke service van een provider. Dit is het meest gebruikte grant type voor reguliere service-afname.

- **Gebruik**: Een consumer wil een service van een provider afnemen
- **Partijen**: Consumer en Provider
- **Effect**: De consumer kan access tokens aanvragen voor deze service

#### 4. DelegatedServiceConnectionGrant

Geeft een gemachtigde partij toegang tot een service namens een andere consumer. Dit maakt het mogelijk dat een intermediair namens een organisatie services afneemt.

- **Gebruik**: Een intermediair neemt een service af namens een opdrachtgever
- **Partijen**: Gemachtigde, opdrachtgever (consumer) en Provider
- **Effect**: De gemachtigde kan access tokens aanvragen namens de opdrachtgever

## Access Token (JWT)

Het access token is een JSON Web Token (JWT) dat wordt uitgegeven door de Manager van de provider. Het token bevat de volgende claims:

| Claim | Beschrijving |
|-------|-------------|
| `sub` | Subject: het PeerID van de consumer die de service afneemt |
| `iss` | Issuer: het PeerID van de provider die het token uitgeeft |
| `svc` | Service: de naam van de service waarvoor het token geldig is |
| `aud` | Audience: het PeerID van de provider (ontvanger van het token) |
| `gth` | Grant Hash: de hash van het specifieke grant in het contract |
| `gid` | Grant ID: identifier van het grant binnen het contract |
| `cnf` | Confirmation: object met `x5t#S256` (SHA-256 thumbprint van het client-certificaat) |
| `exp` | Expiration: verloopdatum van het token |
| `iat` | Issued At: tijdstip van uitgifte |

De `cnf` claim bindt het token aan het specifieke certificaat van de consumer, waardoor token-diefstal wordt voorkomen (certificate-bound tokens).

```json
{
  "sub": "PeerID-consumer",
  "iss": "PeerID-provider",
  "svc": "mijn-service",
  "aud": "PeerID-provider",
  "gth": "sha256-hash-van-grant",
  "gid": "grant-identifier",
  "cnf": {
    "x5t#S256": "sha256-thumbprint-van-client-certificaat"
  },
  "exp": 1735689600,
  "iat": 1735686000
}
```

## Manager API Endpoints

De Manager biedt de volgende REST API endpoints:

### Peer-beheer

| Methode | Endpoint | Beschrijving |
|---------|----------|-------------|
| `PUT` | `/announce` | Registreer of update een Peer in het netwerk. Bevat informatie over de Peer zoals naam, Inway-adres en beschikbare services. |
| `GET` | `/peer` | Haal informatie op over de eigen Peer (self-discovery). |
| `GET` | `/peers` | Lijst van alle bekende Peers in het netwerk. |

### Contract-beheer

| Methode | Endpoint | Beschrijving |
|---------|----------|-------------|
| `POST` | `/contracts` | Dien een nieuw (ondertekend) contract in bij de tegenpartij. |
| `GET` | `/contracts` | Haal alle contracten op (met optionele filters). |
| `PUT` | `/contracts/{hash}/accept` | Accepteer een contract door het te ondertekenen. |
| `PUT` | `/contracts/{hash}/reject` | Wijs een contract af. |
| `PUT` | `/contracts/{hash}/revoke` | Trek een eerder geaccepteerd contract in. |

### Token-uitgifte

| Methode | Endpoint | Beschrijving |
|---------|----------|-------------|
| `POST` | `/token` | Vraag een access token aan via OAuth 2.0 Client Credentials. Vereist `grant_type`, `scope` (GrantHash) en `client_id` (PeerID). |

### Service-discovery

| Methode | Endpoint | Beschrijving |
|---------|----------|-------------|
| `GET` | `/services` | Lijst van alle gepubliceerde services (beschikbaar op de Directory). |

## Netwerkconfiguratie

### Poorten

FSC definieert twee poorten voor verschillende typen verkeer:

| Poort | Gebruik | Beschrijving |
|-------|---------|-------------|
| **443** | Dataverkeer | Standaardpoort voor service-aanroepen tussen Outway en Inway. Draagt de eigenlijke API-verzoeken en responses. |
| **8443** | Managementverkeer | Poort voor communicatie tussen Managers onderling en tussen Outways en Managers. Gebruikt voor contractbeheer, tokenuitgifte en peer-discovery. |

### HTTP-vereisten

- **HTTP/1.1** is verplicht als minimale versie voor alle FSC-verbindingen
- **HTTP/2** is optioneel en mag worden ondersteund als aanvulling
- Alle verbindingen gebruiken **TLS 1.2** of hoger
- **mTLS** is verplicht: zowel client als server presenteren een certificaat

## Logging Extensie

De FSC Logging extensie (`fsc-logging`) beschrijft hoe transactielogs worden vastgelegd voor audit- en verantwoordingsdoeleinden.

### Transactie-ID

Elk verzoek krijgt een uniek **transaction_id** mee via de `Fsc-Transaction-Id` HTTP header. Dit ID wordt door alle componenten in de keten doorgegeven, zodat een volledig pad van consumer naar provider en terug traceerbaar is.

```
GET /service/endpoint HTTP/1.1
Fsc-Authorization: Bearer <JWT-token>
Fsc-Transaction-Id: 550e8400-e29b-41d4-a716-446655440000
```

### Logvelden

Elk transactielog bevat minimaal de volgende velden:

| Veld | Beschrijving |
|------|-------------|
| `transaction_id` | Uniek ID van de transactie (UUID), corresponderend met de `Fsc-Transaction-Id` header |
| `direction` | Richting van het verzoek: `in` (ontvangen) of `out` (verzonden) |
| `grant_hash` | Hash van het grant dat is gebruikt voor autorisatie |
| `source` | PeerID van de verzendende partij |
| `destination` | PeerID van de ontvangende partij |
| `timestamp` | Tijdstip van het logmoment |
| `service` | Naam van de aangeroepen service |

### Logging per component

- **Outway**: logt uitgaande verzoeken met `direction: out`, het gebruikte grant en de bestemming
- **Inway**: logt inkomende verzoeken met `direction: in`, het gevalideerde grant en de bron
- Beide componenten loggen dezelfde `transaction_id`, waardoor correlatie mogelijk is

## Implementatievoorbeelden

### Outway Proxy (Python/FastAPI)

```python
from fastapi import FastAPI, Request, Response
import requests, jwt

app = FastAPI(title="FSC Outway Proxy")

MANAGER_URL = "https://manager.example.com:8443"
MTLS_CERT = ("pkio_cert.pem", "pkio_key.pem")
CA_BUNDLE = "pkio_ca_chain.pem"

# Token cache per grant_hash
token_cache: dict[str, dict] = {}

def get_access_token(grant_hash: str, peer_id: str) -> str:
    """Haal access token op van de Manager (OAuth 2.0 Client Credentials)."""
    cached = token_cache.get(grant_hash)
    if cached and cached["exp"] > time.time():
        return cached["token"]

    response = requests.post(
        f"{MANAGER_URL}/token",
        data={
            "grant_type": "client_credentials",
            "scope": grant_hash,
            "client_id": peer_id,
        },
        cert=MTLS_CERT,
        verify=CA_BUNDLE,
    )
    response.raise_for_status()
    token_data = response.json()
    token_cache[grant_hash] = {
        "token": token_data["access_token"],
        "exp": jwt.decode(token_data["access_token"], options={"verify_signature": False})["exp"]
    }
    return token_data["access_token"]

@app.api_route("/{service_name}/{path:path}", methods=["GET", "POST", "PUT", "PATCH", "DELETE"])
async def proxy_request(service_name: str, path: str, request: Request):
    """Proxy verzoeken naar een service via FSC Inway."""
    grant_hash = resolve_grant_hash(service_name)  # uit lokale contracten
    inway_url = resolve_inway_url(service_name)     # uit directory

    access_token = get_access_token(grant_hash, "mijn-oin")

    # Forward request naar Inway met Fsc-Authorization header
    response = requests.request(
        method=request.method,
        url=f"{inway_url}/{service_name}/{path}",
        headers={
            "Fsc-Authorization": f"Bearer {access_token}",
            "Fsc-Transaction-Id": str(uuid.uuid4()),  # UUID V7 per request
            **{k: v for k, v in request.headers.items()
               if k.lower() not in ("host", "fsc-authorization")},
        },
        data=await request.body(),
        cert=MTLS_CERT,
        verify=CA_BUNDLE,
    )

    return Response(
        content=response.content,
        status_code=response.status_code,
        headers=dict(response.headers),
    )
```

### Inway Token Validatie (Python)

```python
import jwt
from cryptography.x509 import load_pem_x509_certificate
from cryptography.hazmat.primitives.hashes import SHA256

def validate_fsc_token(token: str, client_cert_pem: bytes, group_id: str) -> dict:
    """Valideer een FSC access token op de Inway."""
    # 1. Decode token (signature verificatie met Manager's publieke sleutel)
    payload = jwt.decode(token, manager_public_key, algorithms=["RS256", "PS256"])

    # 2. Controleer group_id
    if payload["gid"] != group_id:
        raise ValueError(f"Verkeerde group_id: {payload['gid']}")

    # 3. Controleer certificate binding (RFC 8705)
    cert = load_pem_x509_certificate(client_cert_pem)
    cert_thumbprint = base64url_encode(cert.fingerprint(SHA256()))
    if payload["cnf"]["x5t#S256"] != cert_thumbprint:
        raise ValueError("Certificate thumbprint komt niet overeen met token")

    # 4. Controleer service bestaat
    if payload["svc"] not in registered_services:
        raise ValueError(f"Service niet gevonden: {payload['svc']}")

    # 5. Controleer expiratie (standaard JWT)
    # jwt.decode doet dit automatisch met verify_exp=True

    return payload
```

### Contract Aanmaken en Ondertekenen (curl)

```bash
# Stap 1: Contract indienen bij provider
curl -X POST https://provider-manager.example.com:8443/contracts \
  --cert pkio_cert.pem --key pkio_key.pem --cacert pkio_ca_chain.pem \
  -H "Content-Type: application/json" \
  -d '{
    "content": {
      "fsc_version": "1.0.0",
      "iv": "'$(uuidgen)'",
      "group_id": "nl-overheid-productie",
      "validity": {
        "not_before": '$(date +%s)',
        "not_after": '$(date -v+1y +%s)'
      },
      "grants": [{
        "type": "ServiceConnectionGrant",
        "service_name": "brp-bevraging",
        "outway": {
          "public_key_thumbprint": "sha256:abc123..."
        }
      }],
      "hash_algorithm": "HASH_ALGORITHM_SHA3_512",
      "created_at": '$(date +%s)'
    },
    "signatures": [{
      "peer_id": "00000001823288444000",
      "certificate_thumbprint": "sha256:def456...",
      "signature": "base64_signature_here",
      "acceptance": "ACCEPTED"
    }]
  }'

# Stap 2: Contract accepteren door provider
CONTRACT_HASH="returned_contract_hash"
curl -X PUT "https://provider-manager.example.com:8443/contracts/${CONTRACT_HASH}/accept" \
  --cert pkio_cert.pem --key pkio_key.pem --cacert pkio_ca_chain.pem \
  -H "Content-Type: application/json" \
  -d '{
    "signature": {
      "peer_id": "00000001234567890000",
      "certificate_thumbprint": "sha256:ghi789...",
      "signature": "base64_provider_signature",
      "acceptance": "ACCEPTED"
    }
  }'
```

### Service Discovery via Directory (curl)

```bash
# Beschikbare services ophalen
curl -s https://directory.example.com:8443/services \
  --cert pkio_cert.pem --key pkio_key.pem --cacert pkio_ca_chain.pem | jq

# Peers in de groep ophalen
curl -s https://directory.example.com:8443/peers \
  --cert pkio_cert.pem --key pkio_key.pem --cacert pkio_ca_chain.pem | jq

# Specifieke service details
curl -s "https://directory.example.com:8443/services?name=brp-bevraging" \
  --cert pkio_cert.pem --key pkio_key.pem --cacert pkio_ca_chain.pem | jq
```

## Foutafhandeling

### Inway Error Codes

| HTTP Status | Fsc-Error-Code | Beschrijving | Actie |
|------------|---------------|-------------|-------|
| 401 | `ACCESS_TOKEN_MISSING` | Geen Fsc-Authorization header | Voeg token toe via Outway |
| 401 | `ACCESS_TOKEN_INVALID` | Token handtekening ongeldig | Nieuw token ophalen |
| 401 | `ACCESS_TOKEN_EXPIRED` | Token verlopen | Nieuw token ophalen |
| 403 | `WRONG_GROUP_ID_IN_TOKEN` | Group ID in token matcht niet | Controleer groepsconfiguratie |
| 404 | `SERVICE_NOT_FOUND` | Service niet geregistreerd | Controleer servicenaam in contract |
| 500 | `TRANSACTION_LOG_WRITE_ERROR` | Transactielog schrijven mislukt | Probeer later opnieuw |
| 502 | `SERVICE_UNREACHABLE` | Achterliggende service onbereikbaar | Probeer later opnieuw |

### Error Response Formaat

```json
{
  "code": "ACCESS_TOKEN_EXPIRED",
  "domain": "inway",
  "details": "Token expired at 2024-01-15T10:30:00Z"
}
```

De `Fsc-Error-Code` header wordt altijd meegegeven bij foutresponses, naast de HTTP status code.

### Outway Error Code

| HTTP Status | Fsc-Error-Code | Beschrijving |
|------------|---------------|-------------|
| 405 | `METHOD_UNSUPPORTED` | CONNECT methode niet ondersteund |

### Retry Strategie

```python
import time

def call_service_via_fsc(service_name: str, path: str, max_retries: int = 3):
    """Roep een FSC service aan met retry bij tijdelijke fouten."""
    for attempt in range(max_retries):
        response = outway.proxy_request(service_name, path)

        error_code = response.headers.get("Fsc-Error-Code")
        if not error_code:
            return response  # Succes of applicatie-error

        if error_code in ("ACCESS_TOKEN_EXPIRED", "ACCESS_TOKEN_INVALID"):
            # Token vernieuwen en opnieuw proberen
            token_cache.pop(service_name, None)
            continue

        if error_code in ("SERVICE_UNREACHABLE", "TRANSACTION_LOG_WRITE_ERROR"):
            # Tijdelijke fout - exponential backoff
            time.sleep(min(2 ** attempt * 5, 60))
            continue

        # Permanente fouten niet opnieuw proberen
        break

    return response
```

## Tests & Validatie

### FSC Test Suite

Dedicated test suite met integratie- en componenttests:

```bash
# Bekijk test suite structuur
gh api repos/logius-standaarden/fsc-test-suite/contents --jq '.[].name'

# Integratie test specificatie
gh api repos/logius-standaarden/fsc-test-suite/contents/integration.md -H "Accept: application/vnd.github.raw"

# Manager service tests
gh api repos/logius-standaarden/fsc-test-suite/contents/manager.md -H "Accept: application/vnd.github.raw"

# Inway/outway component tests
gh api repos/logius-standaarden/fsc-test-suite/contents/inway.md -H "Accept: application/vnd.github.raw"
gh api repos/logius-standaarden/fsc-test-suite/contents/outway.md -H "Accept: application/vnd.github.raw"
```

### Centrale Checks

```bash
# WCAG check op FSC core documentatie
npx @axe-core/cli https://logius-standaarden.github.io/fsc-core/ --tags wcag2aa

# Markdown lint
npx markdownlint-cli '**/*.md'
```

## Handige Commando's

```bash
# Alle FSC repos
gh api orgs/logius-standaarden/repos --paginate --jq '[.[] | select(.name | startswith("fsc"))] | sort_by(.name) | .[].name'

# Laatste wijzigingen aan fsc-core
gh api repos/logius-standaarden/fsc-core/commits --jq '.[:5] | .[] | "\(.commit.committer.date) \(.commit.message | split("\n")[0])"'

# Open issues/PRs
gh issue list --repo logius-standaarden/fsc-core
gh pr list --repo logius-standaarden/fsc-core

# Core spec inhoud ophalen
gh api repos/logius-standaarden/fsc-core/contents --jq '.[].name'

# Contract endpoint testen (voorbeeld)
curl -s --cert client.pem --key client-key.pem --cacert ca.pem \
  https://manager.provider.example.nl:8443/contracts | jq .

# Services opvragen bij Directory
curl -s --cert client.pem --key client-key.pem --cacert ca.pem \
  https://directory.example.nl:8443/services | jq .
```

## Gerelateerde Skills

| Skill | Relatie |
|-------|---------|
| `/ls-dk` | FSC als opvolger/aanvulling voor Digikoppeling |
| `/ls-iam` | Authenticatie en autorisatie binnen FSC (mTLS, OAuth 2.0, certificaten) |
| `/ls-logboek` | Logging van FSC transacties raakt aan Logboek Dataverwerkingen |
| `/ls-pub` | ReSpec tooling voor FSC documentatie |
| `/ls` | Overzicht alle standaarden |

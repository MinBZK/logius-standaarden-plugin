---
name: ls-dk
description: "Gebruik deze skill wanneer de gebruiker vraagt over Digikoppeling, koppelvlakstandaarden, ebMS2, WUS, SOAP, REST-API koppelvlak, grote berichten, OIN, Organisatie Identificatie Nummer, PKIoverheid, beveiligingsstandaarden overheid, routering en adressering, CPA, berichtuitwisseling tussen overheidsorganisaties."
model: sonnet
allowed-tools:
  - Bash(gh api *)
  - Bash(gh issue list *)
  - Bash(gh pr list *)
  - Bash(gh search *)
  - Bash(curl -s *)
  - WebFetch(*)
---

# Digikoppeling

**Agent-instructie:** Deze skill helpt bij het implementeren van Digikoppeling koppelvlakken. Gebruik de beslisboom om het juiste profiel te kiezen. De implementatievoorbeelden bevatten werkende code voor alle vier koppelvlakstandaarden.

Digikoppeling is de Nederlandse standaard voor beveiligde elektronische gegevensuitwisseling tussen overheidsorganisaties. Het definieert de architectuur en koppelvlakspecificaties waarmee overheden op een gestandaardiseerde, veilige en betrouwbare manier berichten en gegevens uitwisselen. Digikoppeling is opgenomen op de "pas toe of leg uit"-lijst van het Forum Standaardisatie.

## Repositories

| Repository | Beschrijving | Publicatie |
|-----------|-------------|-----------|
| [Digikoppeling-Architectuur](https://github.com/logius-standaarden/Digikoppeling-Architectuur) | Overkoepelende architectuurbeschrijving | [Lees online](https://logius-standaarden.github.io/Digikoppeling-Architectuur/) |
| [Digikoppeling-Koppelvlakstandaard-REST-API](https://github.com/logius-standaarden/Digikoppeling-Koppelvlakstandaard-REST-API) | REST-API koppelvlakspecificatie | [Lees online](https://logius-standaarden.github.io/Digikoppeling-Koppelvlakstandaard-REST-API/) |
| [Digikoppeling-Koppelvlakstandaard-ebMS2](https://github.com/logius-standaarden/Digikoppeling-Koppelvlakstandaard-ebMS2) | ebMS2 koppelvlakspecificatie | [Lees online](https://logius-standaarden.github.io/Digikoppeling-Koppelvlakstandaard-ebMS2/) |
| [Digikoppeling-Koppelvlakstandaard-WUS](https://github.com/logius-standaarden/Digikoppeling-Koppelvlakstandaard-WUS) | WUS (WSDL/UDDI/SOAP) koppelvlakspecificatie | [Lees online](https://logius-standaarden.github.io/Digikoppeling-Koppelvlakstandaard-WUS/) |
| [Digikoppeling-Koppelvlakstandaard-GB](https://github.com/logius-standaarden/Digikoppeling-Koppelvlakstandaard-GB) | Grote Berichten koppelvlakspecificatie | [Lees online](https://logius-standaarden.github.io/Digikoppeling-Koppelvlakstandaard-GB/) |
| [Digikoppeling-Beveiligingsstandaarden-en-voorschriften](https://github.com/logius-standaarden/Digikoppeling-Beveiligingsstandaarden-en-voorschriften) | Beveiligingsstandaarden en -voorschriften | [Lees online](https://logius-standaarden.github.io/Digikoppeling-Beveiligingsstandaarden-en-voorschriften/) |
| [Digikoppeling-Identificatie-en-Authenticatie](https://github.com/logius-standaarden/Digikoppeling-Identificatie-en-Authenticatie) | Identificatie en authenticatie | [Lees online](https://logius-standaarden.github.io/Digikoppeling-Identificatie-en-Authenticatie/) |
| [Digikoppeling-Gebruik-en-achtergrond-certificaten](https://github.com/logius-standaarden/Digikoppeling-Gebruik-en-achtergrond-certificaten) | Gebruik van PKIoverheid-certificaten | [Lees online](https://logius-standaarden.github.io/Digikoppeling-Gebruik-en-achtergrond-certificaten/) |
| [Digikoppeling-Handreiking-Adressering-en-Routering](https://github.com/logius-standaarden/Digikoppeling-Handreiking-Adressering-en-Routering) | Handreiking voor adressering en routering | [Lees online](https://logius-standaarden.github.io/Digikoppeling-Handreiking-Adressering-en-Routering/) |
| [Digikoppeling-Best-Practices-ebMS2](https://github.com/logius-standaarden/Digikoppeling-Best-Practices-ebMS2) | Best practices voor ebMS2 implementatie | [Lees online](https://logius-standaarden.github.io/Digikoppeling-Best-Practices-ebMS2/) |
| [Digikoppeling-Best-Practices-WUS](https://github.com/logius-standaarden/Digikoppeling-Best-Practices-WUS) | Best practices voor WUS implementatie | [Lees online](https://logius-standaarden.github.io/Digikoppeling-Best-Practices-WUS/) |
| [Digikoppeling-Best-Practices-GB](https://github.com/logius-standaarden/Digikoppeling-Best-Practices-GB) | Best practices voor Grote Berichten | [Lees online](https://logius-standaarden.github.io/Digikoppeling-Best-Practices-GB/) |
| [Digikoppeling-Overzicht-Actuele-Documentatie-en-Compliance](https://github.com/logius-standaarden/Digikoppeling-Overzicht-Actuele-Documentatie-en-Compliance) | Overzicht documentatie en compliance | [Lees online](https://logius-standaarden.github.io/Digikoppeling-Overzicht-Actuele-Documentatie-en-Compliance/) |
| [Digikoppeling-Wat-is-Digikoppeling](https://github.com/logius-standaarden/Digikoppeling-Wat-is-Digikoppeling) | Introductie en uitleg | [Lees online](https://logius-standaarden.github.io/Digikoppeling-Wat-is-Digikoppeling/) |
| [Digikoppeling-Algemeen](https://github.com/logius-standaarden/Digikoppeling-Algemeen) | Algemene informatie en bronbestanden | - |
| [Digikoppeling-Beheermodel](https://github.com/logius-standaarden/Digikoppeling-Beheermodel) | Beheermodel voor Digikoppeling | [Lees online](https://logius-standaarden.github.io/Digikoppeling-Beheermodel/) |
| [OIN-Stelsel](https://github.com/logius-standaarden/OIN-Stelsel) | Organisatie Identificatie Nummer stelsel | [Lees online](https://logius-standaarden.github.io/OIN-Stelsel/) |

## Profielkeuze

De keuze voor het juiste Digikoppeling-profiel hangt af van de functionele en niet-functionele eisen van de berichtuitwisseling. Gebruik de volgende beslisboom:

```
Berichtgrootte > 20 MB?
  JA  --> Grote Berichten (aanvullend op WUS of ebMS2)
  NEE --> Ga verder

Is snelheid/eenvoud belangrijk en past een synchrone request/response?
  JA  --> Is een REST-API beschikbaar/gewenst?
            JA  --> REST-API profiel (met FSC sinds 1/1/2025)
            NEE --> WUS profiel (2W-be)
  NEE --> Ga verder

Is betrouwbare (reliable) aflevering vereist?
  JA  --> ebMS2 reliable profiel (osb-rm)
  NEE --> ebMS2 best-effort profiel (osb-be)

Gaat het bericht via een niet-vertrouwde intermediair?
  JA  --> Kies het signed (-S) of signed+encrypted (-SE/-E) variant van het gekozen profiel
  NEE --> Basisprofiel volstaat
```

## Profielkenmerken Matrix

| Profiel | Synchroon | Asynchroon | Best-effort | Reliable | Signed | Encrypted |
|---------|:---------:|:----------:|:-----------:|:--------:|:------:|:---------:|
| **REST-API** | Ja | Nee | Ja | Nee | Nee* | Nee* |
| **2W-be** | Ja | Nee | Ja | Nee | Nee | Nee |
| **2W-be-S** | Ja | Nee | Ja | Nee | Ja | Nee |
| **2W-be-SE** | Ja | Nee | Ja | Nee | Ja | Ja |
| **osb-be** | Nee | Ja | Ja | Nee | Nee | Nee |
| **osb-rm** | Nee | Ja | Nee | Ja | Nee | Nee |
| **osb-be-S** | Nee | Ja | Ja | Nee | Ja | Nee |
| **osb-rm-S** | Nee | Ja | Nee | Ja | Ja | Nee |
| **osb-be-E** | Nee | Ja | Ja | Nee | Nee | Ja |
| **osb-rm-E** | Nee | Ja | Nee | Ja | Nee | Ja |
| **Grote Berichten** | n.v.t. | n.v.t. | Ja | Nee | Nee | Nee** |

\* REST-API maakt gebruik van TLS voor transportbeveiliging. Signing en encryptie op berichtniveau zijn niet gestandaardiseerd binnen het REST-API-profiel, maar kunnen op applicatieniveau worden toegepast (bijv. via JWS/JWE).

\** Grote Berichten gebruikt TLS voor transport. Payload-encryptie kan aanvullend worden toegepast maar is niet voorgeschreven in het profiel.

## Implementatievoorbeelden

### REST-API Koppelvlak met OIN (Python/FastAPI)

```python
from fastapi import FastAPI, Request, Response, HTTPException
from fastapi.responses import JSONResponse

app = FastAPI(
    title="Digikoppeling REST-API Voorbeeld",
    version="1.0.0",
    contact={"name": "API Support", "email": "support@example.com"},
    servers=[{"url": "https://api.example.com/dk/v1"}]
)

@app.middleware("http")
async def add_digikoppeling_headers(request: Request, call_next):
    """Voeg verplichte Digikoppeling headers toe aan responses."""
    response = await call_next(request)
    response.headers["API-Version"] = "1.0.0"
    response.headers["Strict-Transport-Security"] = "max-age=31536000"
    response.headers["X-Content-Type-Options"] = "nosniff"
    response.headers["X-Frame-Options"] = "DENY"
    response.headers["Content-Security-Policy"] = "frame-ancestors 'none'"
    response.headers["Cache-Control"] = "no-store"
    return response

@app.get("/v1/resources/{resource_id}")
async def get_resource(resource_id: str, request: Request):
    """Beveiligd endpoint - mTLS met PKIoverheid certificaat vereist."""
    # OIN uit het client certificaat (via reverse proxy/API gateway)
    client_oin = request.headers.get("X-Client-OIN")
    if not client_oin:
        raise HTTPException(status_code=403, detail="OIN niet gevonden in certificaat")
    return {"id": resource_id, "requested_by_oin": client_oin}

@app.exception_handler(HTTPException)
async def problem_json_handler(request: Request, exc: HTTPException):
    """RFC 9457 problem+json foutafhandeling (verplicht per API Design Rules)."""
    return JSONResponse(
        status_code=exc.status_code,
        content={
            "type": f"https://api.example.com/errors/{exc.status_code}",
            "title": exc.detail,
            "status": exc.status_code,
            "instance": str(request.url),
        },
        media_type="application/problem+json"
    )
```

### WUS/SOAP Bevraging (Python met zeep)

```python
from zeep import Client
from zeep.wsse.username import UsernameToken
from zeep.transports import Transport
from requests import Session

# mTLS sessie met PKIoverheid certificaten
session = Session()
session.cert = ("pkio_client_cert.pem", "pkio_client_key.pem")
session.verify = "pkio_ca_bundle.pem"  # PKIoverheid CA chain

transport = Transport(session=session)

# WSDL-gebaseerde client (service contract)
client = Client(
    "https://service.example.com/dk/wus?wsdl",
    transport=transport
)

# Synchrone bevraging (profiel 2W-be)
response = client.service.GeefPersoon(
    BSN="999990342",
    Organisatie={"OIN": "00000001823288444000"}
)
print(f"Naam: {response.Naam}")
```

### WUS SOAP Envelope Structuur

```xml
<?xml version="1.0" encoding="UTF-8"?>
<soapenv:Envelope
    xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:wsa="http://www.w3.org/2005/08/addressing"
    xmlns:wsse="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd">
  <soapenv:Header>
    <!-- WS-Addressing (verplicht voor routing) -->
    <wsa:MessageID>550e8400-e29b-41d4-a716-446655440000@urn:osb:dgd</wsa:MessageID>
    <wsa:To>https://service.example.com/dk/wus</wsa:To>
    <wsa:Action>http://example.com/GeefPersoon</wsa:Action>
    <!-- WS-Security (profiel 2W-be-S: signed) -->
    <wsse:Security>
      <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
        <!-- SHA-256 digest, PKIoverheid certificaat -->
      </ds:Signature>
    </wsse:Security>
  </soapenv:Header>
  <soapenv:Body>
    <GeefPersoonRequest>
      <BSN>999990342</BSN>
    </GeefPersoonRequest>
  </soapenv:Body>
</soapenv:Envelope>
```

### ebMS2 Bericht Structuur

```xml
<?xml version="1.0" encoding="UTF-8"?>
<soapenv:Envelope
    xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:eb="http://www.oasis-open.org/committees/ebxml-msg/schema/msg-header-2_0.xsd">
  <soapenv:Header>
    <eb:MessageHeader soapenv:mustUnderstand="1" eb:version="2.0">
      <eb:From>
        <eb:PartyId eb:type="urn:osb:oin">00000001823288444000</eb:PartyId>
      </eb:From>
      <eb:To>
        <eb:PartyId eb:type="urn:osb:oin">00000001234567890000</eb:PartyId>
      </eb:To>
      <eb:CPAId>cpa_oin1_oin2_service</eb:CPAId>
      <eb:ConversationId>conv-2024-001</eb:ConversationId>
      <eb:Service>urn:example:melding</eb:Service>
      <eb:Action>Indienen</eb:Action>
      <eb:MessageData>
        <eb:MessageId>msg-550e8400@urn:osb:dgd</eb:MessageId>
        <eb:Timestamp>2024-01-15T10:30:00Z</eb:Timestamp>
      </eb:MessageData>
    </eb:MessageHeader>
    <!-- Profiel osb-rm: Reliable Messaging -->
    <eb:AckRequested soapenv:mustUnderstand="1" eb:version="2.0"
        eb:signed="false" soapenv:actor="urn:oasis:names:tc:ebxml-msg:actor:toPartyMSH"/>
  </soapenv:Header>
  <soapenv:Body>
    <eb:Manifest eb:version="2.0">
      <eb:Reference xlink:href="cid:payload@example.com" xlink:role="payload"/>
    </eb:Manifest>
  </soapenv:Body>
</soapenv:Envelope>
```

### Grote Berichten - PULL Variant (curl)

```bash
# Stap 1: Metadata verzenden via Digikoppeling (REST/WUS/ebMS2)
# Het metadatabericht bevat de download-URL, bestandsgrootte en checksum

# Stap 2: Ontvanger downloadt bestand via HTTPS GET met BYTE-RANGE
# Eerste request: geheel bestand
curl -X GET https://sender.example.com/gb/files/document-123 \
  --cert pkio_client_cert.pem --key pkio_client_key.pem \
  --cacert pkio_ca_bundle.pem \
  -H "Accept: application/octet-stream" \
  -o document.pdf

# Bij onderbreking: hervatten met Range header
curl -X GET https://sender.example.com/gb/files/document-123 \
  --cert pkio_client_cert.pem --key pkio_client_key.pem \
  --cacert pkio_ca_bundle.pem \
  -H "Range: bytes=1048576-" \
  -H "If-Match: \"etag-value-from-previous-response\"" \
  -C - -o document.pdf
# Response: 206 Partial Content met Content-Range header
```

### Grote Berichten - PUSH Variant (curl)

```bash
# Verzender uploadt bestand naar ontvanger
curl -X PUT https://receiver.example.com/gb/upload/document-123 \
  --cert pkio_client_cert.pem --key pkio_client_key.pem \
  --cacert pkio_ca_bundle.pem \
  -H "Content-Type: application/octet-stream" \
  -H "Content-Length: 52428800" \
  --data-binary @large_document.pdf

# Response: 200 OK (volledig ontvangen) of 206 Partial Content
```

### Nginx mTLS Configuratie voor Digikoppeling

```nginx
server {
    listen 443 ssl;
    server_name api.example.com;

    # PKIoverheid server certificaat
    ssl_certificate     /etc/ssl/pkio/server_cert.pem;
    ssl_certificate_key /etc/ssl/pkio/server_key.pem;

    # mTLS: PKIoverheid client certificaat vereist
    ssl_client_certificate /etc/ssl/pkio/pkio_ca_chain.pem;
    ssl_verify_client on;
    ssl_verify_depth 4;

    # TLS configuratie (NCSC 2025 richtlijnen)
    ssl_protocols TLSv1.2 TLSv1.3;  # TLS001/TLS004
    ssl_prefer_server_ciphers on;
    ssl_ciphers 'ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384';

    # OIN uit client certificaat doorgeven aan applicatie
    proxy_set_header X-Client-OIN $ssl_client_s_dn_serial;
    proxy_set_header X-Client-CN  $ssl_client_s_dn_cn;

    location /dk/v1/ {
        proxy_pass http://localhost:8080;
    }
}
```

## Foutafhandeling

### HTTP Status Codes per Koppelvlak

| Status | REST-API | WUS | ebMS2 |
|--------|---------|-----|-------|
| 200 | OK - resource opgehaald | N/A (SOAP 200) | N/A |
| 206 | Partial Content (Grote Berichten) | N/A | N/A |
| 400 | Ongeldig verzoek | SOAP Fault | eb:Error |
| 401 | Niet geauthenticeerd | SOAP Fault | eb:Error |
| 403 | OIN niet geautoriseerd | SOAP Fault | eb:Error |
| 404 | Resource niet gevonden | SOAP Fault | N/A |
| 405 | Methode niet toegestaan | N/A | N/A |
| 500 | Interne fout | SOAP Fault | eb:Error |
| 503 | Service niet beschikbaar | SOAP Fault | eb:Error |

### REST-API Error Response (RFC 9457)

```json
{
  "type": "https://api.example.com/errors/oin-unauthorized",
  "title": "OIN niet geautoriseerd",
  "status": 403,
  "detail": "OIN 00000001823288444000 heeft geen toegang tot deze service",
  "instance": "/dk/v1/resources/123"
}
```

### ebMS2 Error Bericht

```xml
<eb:ErrorList>
  <eb:Error eb:errorCode="SecurityFailure" eb:severity="Error">
    <eb:Description>OIN verificatie mislukt</eb:Description>
  </eb:Error>
</eb:ErrorList>
```

### Retry Strategie (ebMS2 Reliable Messaging)

Bij het profiel `osb-rm` gelden de volgende retry-regels:
- **Acknowledgement timeout**: Wacht maximaal de afgesproken tijd op een bevestiging
- **Retry interval**: Exponential backoff (bijv. 30s, 60s, 120s, 240s)
- **Max retries**: Configureerbaar in de CPA (typisch 3-5 pogingen)
- **Duplicate elimination**: Ontvanger herkent duplicaten op basis van MessageId
- **At-most-once delivery**: Berichten worden nooit dubbel afgeleverd

## Achtergrondinfo

Zie [reference.md](reference.md) voor de architectuur, beveiligingsstandaarden, en gedetailleerde protocol-beschrijvingen.

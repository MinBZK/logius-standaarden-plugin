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
  - Bash(npx markdownlint *)
  - Bash(npx @axe-core/cli *)
  - WebFetch(*)
---

# Digikoppeling

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

## Architectuur

De Digikoppeling-architectuur is gebaseerd op een drielagenmodel dat inhoud, logistiek en transport van elkaar scheidt. Dit ontkoppelingsprincipe zorgt ervoor dat wijzigingen in de ene laag geen impact hebben op de andere lagen.

### Drielagenmodel

1. **Inhoud (Content/Payload)**: De daadwerkelijke bedrijfsgegevens die uitgewisseld worden. Dit is de verantwoordelijkheid van de applicaties en valt buiten de scope van Digikoppeling. Digikoppeling is payload-agnostisch: het maakt niet uit of het om XML, JSON, PDF of een ander formaat gaat.

2. **Logistiek (Envelope/Wrapper)**: De laag die zorgt voor de adressering, routering, beveiliging en betrouwbaarheid van het berichtenverkeer. Hier worden metadata toegevoegd zoals afzender-OIN, ontvanger-OIN, Message-ID, tijdstempel en beveiligingskenmerken. Dit is de kernlaag van Digikoppeling.

3. **Transport (Netwerk)**: De fysieke transportlaag die zorgt voor de daadwerkelijke verzending over het netwerk. Digikoppeling schrijft het gebruik van HTTPS (TLS) voor als transportprotocol. Communicatie verloopt typisch over Diginetwerk of het publieke internet.

### Componenten in de Keten

- **Applicatie**: De bedrijfsapplicatie die gegevens produceert of consumeert. Communiceert met de Digikoppeling-adapter via een interne interface.
- **API Gateway**: Optionele component voor het ontsluiten van REST-API's. Biedt functionaliteit als rate limiting, caching en API-management.
- **Message Broker / ESB**: Optionele intermediaire component voor berichtroutering, transformatie en orchestratie. Kan meerdere Digikoppeling-adapters aansturen.
- **Digikoppeling Adapter**: De kerncomponent die het Digikoppeling-protocol implementeert. Vertaalt interne berichten naar het juiste koppelvlakformaat (ebMS2, WUS, REST-API) en vice versa.
- **PKIoverheid Certificaten**: TLS-certificaten en optioneel signing/encryptie-certificaten uitgegeven onder de PKIoverheid-hierarchie. Worden gebruikt voor authenticatie, integriteit en vertrouwelijkheid.
- **Service Contract**: De formele beschrijving van de dienst. Bij WUS is dit een WSDL, bij ebMS2 een CPA (Collaboration Protocol Agreement), bij REST-API een OpenAPI-specificatie.

### Ontkoppelingsprincipe

Het ontkoppelingsprincipe houdt in dat de drie lagen onafhankelijk van elkaar kunnen worden aangepast. Een organisatie kan bijvoorbeeld overstappen van ebMS2 naar REST-API (logistieke laag) zonder dat de bedrijfsapplicatie (inhoudelijke laag) of het netwerk (transportlaag) gewijzigd hoeft te worden. Dit maakt Digikoppeling toekomstbestendig en flexibel.

## Vier Koppelvlakstandaarden

Digikoppeling definieert vier koppelvlakstandaarden die elk een specifiek communicatiepatroon ondersteunen.

### REST-API

De REST-API koppelvlakstandaard is de nieuwste toevoeging aan Digikoppeling en biedt een moderne, lichtgewicht manier om gegevens uit te wisselen.

- **Communicatiepatroon**: Synchroon (request/response)
- **Basis**: Gebaseerd op de API Design Rules (ADR) van het Kennisplatform API's, zoals vastgesteld door het Forum Standaardisatie
- **Service Contract**: OpenAPI Specification (OAS) 3.x
- **Berichtformaat**: JSON (primair) of XML
- **HTTP-methoden**: GET (ophalen), POST (aanmaken), PUT (vervangen), PATCH (wijzigen), DELETE (verwijderen)
- **Adressering**: RESTful URL-structuur met OIN in het endpoint of header
- **Beveiliging**: TLS (transport), optioneel OAuth 2.0 voor autorisatie, PKIoverheid-certificaten voor authenticatie
- **FSC-integratie**: Sinds 1 januari 2025 is het gebruik van Federated Service Connectivity (FSC) verplicht bij Digikoppeling REST-API. FSC verzorgt de federatieve autorisatie en logging van API-aanroepen tussen organisaties.

REST-API is geschikt voor synchrone bevragingen, CRUD-operaties en scenario's waar snelheid en eenvoud belangrijk zijn.

### WUS (SOAP)

WUS staat voor WSDL, UDDI en SOAP en biedt een gestandaardiseerde manier voor synchrone berichtuitwisseling op basis van webservices.

- **Communicatiepatroon**: Synchroon (request/response)
- **Service Contract**: WSDL (Web Services Description Language)
- **Berichtformaat**: SOAP XML (met SOAP envelope, header en body)
- **Profielen**:
  - **2W-be** (Messaging, Best-effort): Basis synchroon berichtverkeer zonder extra beveiliging op berichtniveau
  - **2W-be-S** (Messaging, Best-effort, Signed): Bericht wordt digitaal ondertekend met WS-Security en XML Digital Signature (XMLDSIG). Garandeert integriteit en onweerlegbaarheid.
  - **2W-be-SE** (Messaging, Best-effort, Signed & Encrypted): Bericht wordt ondertekend en versleuteld met WS-Security en XML Encryption. Garandeert integriteit, onweerlegbaarheid en vertrouwelijkheid.
- **Standaarden**: WS-Security 1.1, WS-Addressing, SOAP 1.2, MTOM voor binaire bijlagen
- **Adressering**: WS-Addressing headers met OIN-gebaseerde endpoint-URL's

WUS is geschikt voor synchrone transacties waarbij interoperabiliteit met bestaande SOAP-webservices vereist is.

### ebMS2

ebMS2 (Electronic Business Messaging Service version 2) biedt asynchrone, betrouwbare berichtuitwisseling op basis van de OASIS ebXML Messaging standaard.

- **Communicatiepatroon**: Asynchroon (Message-based, Message Queuing)
- **Service Contract**: CPA (Collaboration Protocol Agreement) - een XML-document dat door beide partijen wordt overeengekomen en de technische afspraken beschrijft
- **Berichtformaat**: SOAP 1.1 met ebMS2 headers, payload als MIME-attachment
- **Profielen**:
  - **osb-be** (Best-effort): Asynchroon berichtverkeer zonder betrouwbaarheidsgaranties
  - **osb-rm** (Reliable Messaging): Asynchroon berichtverkeer met MessageOrder, at-most-once delivery, duplicate-eliminatie en Message Acknowledgements
  - **osb-be-S** (Best-effort, Signed): Asynchroon met ondertekening op berichtniveau
  - **osb-rm-S** (Reliable, Signed): Betrouwbaar asynchroon met ondertekening
  - **osb-be-E** (Best-effort, Encrypted): Asynchroon met versleuteling op berichtniveau
  - **osb-rm-E** (Reliable, Encrypted): Betrouwbaar asynchroon met versleuteling
- **Betrouwbaarheidsmechanismen** (rm-profielen):
  - **At-most-once delivery**: Garandeert dat een bericht maximaal eenmaal wordt afgeleverd
  - **Duplicate-eliminatie**: Herkent en verwijdert duplicaten op basis van MessageId
  - **Message Acknowledgements**: Ontvangstbevestigingen (Message Status Response)
  - **Message Ordering**: Optionele volgordelijkheid van berichten via ConversationId en SequenceNumber
  - **Retry-mechanisme**: Automatische herhaalpogingen bij falen, configureerbaar in de CPA

ebMS2 is geschikt voor scenario's waar betrouwbare, asynchrone aflevering essentieel is, zoals meldingen, mutaties en batchverwerking.

### Grote Berichten

De koppelvlakstandaard Grote Berichten biedt een oplossing voor het uitwisselen van berichten groter dan 20 MB. Het gebruikt het claim-check patroon: het grote bestand wordt buiten de reguliere berichtenuitwisseling om overgedragen, terwijl een referentie (claim-check) via het normale kanaal wordt verstuurd.

- **Drempelwaarde**: Berichten groter dan 20 MB moeten via Grote Berichten worden uitgewisseld
- **Patroon**: Claim-check (ook wel Attachment Deferral genoemd)
- **Protocol**: HTTP 1.1 met ondersteuning voor BYTE-RANGE (RFC 7233) voor hervatbare downloads

#### PULL-variant (ontvanger haalt op bij verzender)

1. Verzender plaatst het grote bestand op een beveiligde HTTP-server
2. Verzender stuurt een regulier Digikoppeling-bericht (via WUS of ebMS2) met daarin de URL naar het bestand
3. Ontvanger ontvangt het bericht met de URL (de claim-check)
4. Ontvanger downloadt het bestand via HTTPS van de opgegeven URL
5. Bij onderbreking kan de download hervat worden dankzij BYTE-RANGE ondersteuning

#### PUSH-variant (verzender uploadt naar ontvanger)

1. Verzender stuurt een regulier Digikoppeling-bericht met metadata over het grote bestand
2. Ontvanger stelt een upload-endpoint beschikbaar
3. Verzender uploadt het bestand via HTTPS naar het endpoint van de ontvanger
4. Bij onderbreking kan de upload hervat worden dankzij BYTE-RANGE ondersteuning

#### Kenmerken

- **Hervatbaarheid**: HTTP 1.1 BYTE-RANGE ondersteuning maakt het mogelijk om onderbroken transfers te hervatten zonder opnieuw te beginnen
- **Beveiliging**: TLS voor transport, PKIoverheid-certificaten voor authenticatie, optioneel payload-encryptie
- **Combinatie**: Grote Berichten is altijd een aanvulling op WUS of ebMS2 -- het reguliere bericht dat de claim-check bevat wordt via een van deze twee protocollen verstuurd
- **Geen maximum**: Er is geen bovengrens gedefinieerd voor de bestandsgrootte

## Beveiliging

Digikoppeling hanteert een meerlaags beveiligingsmodel dat zowel op transport- als op berichtniveau werkt.

### TLS-vereisten

TLS (Transport Layer Security) is verplicht voor alle Digikoppeling-communicatie. De volgende concrete voorschriften gelden:

| Regel | Voorschrift |
|-------|-------------|
| **TLS001** | TLS 1.2 is minimaal vereist. Eerdere versies (SSL 3.0, TLS 1.0, TLS 1.1) zijn niet toegestaan. |
| **TLS002** | TLS 1.3 wordt aanbevolen wanneer beide partijen dit ondersteunen. |
| **TLS003** | Mutual TLS (mTLS) is verplicht: zowel client als server moeten zich authenticeren met een PKIoverheid-certificaat. |
| **TLS004** | Cipher suites moeten voldoen aan de NCSC-richtlijnen voor TLS. Zwakke cipher suites zijn niet toegestaan. |
| **TLS005** | Certificaten moeten geldig zijn en de volledige keten tot aan de PKIoverheid-root moet verifieerbaar zijn. Revocation checking (CRL/OCSP) is verplicht. |
| **TLS006** | Het OIN van de organisatie moet opgenomen zijn in het Subject.SerialNumber-veld van het PKIoverheid-certificaat. |

### PKIoverheid Certificaten

Alle Digikoppeling-deelnemers moeten PKIoverheid-certificaten gebruiken voor authenticatie. Er zijn twee typen relevant:

- **TLS/SSL-certificaat**: Voor de transportbeveiliging (HTTPS). Verplicht voor alle koppelvlakken.
- **Ondertekening-/versleutelingscertificaat**: Voor berichtniveau-beveiliging (WS-Security, XMLDSIG). Vereist bij de signed en encrypted profielen.

### OIN-identificatie

Het Organisatie Identificatie Nummer (OIN) is de unieke identifier voor overheidsorganisaties binnen Digikoppeling:

- Elke deelnemende organisatie heeft een uniek OIN
- Het OIN wordt opgenomen in het PKIoverheid-certificaat (Subject.SerialNumber)
- Het OIN wordt gebruikt voor adressering en routering van berichten
- Het OIN-stelsel wordt beheerd door Logius
- Sub-OIN's zijn mogelijk voor organisatieonderdelen of specifieke systemen

### Berichtniveau-beveiliging

Naast transportbeveiliging (TLS) biedt Digikoppeling optioneel beveiliging op berichtniveau:

- **Ondertekening (Signing)**: Digitale handtekening op het bericht met behulp van XML Digital Signature (XMLDSIG). Algoritme: minimaal SHA-256 (SHA-1 is niet meer toegestaan). Garandeert integriteit en onweerlegbaarheid, ook wanneer het bericht via intermediairs (Message Broker, Message Gateway) wordt gerouteerd.
- **Versleuteling (Encryption)**: Versleuteling van de berichtinhoud met behulp van XML Encryption. Symmetrische versleuteling met AES-128 of AES-256 voor de payload, asymmetrische versleuteling (RSA) voor de sessiesleutel. Garandeert vertrouwelijkheid, ook bij opslag of doorgave via intermediairs.

Berichtniveau-beveiliging is essentieel wanneer er niet-vertrouwde intermediairs in de keten zitten, omdat TLS alleen de point-to-point verbinding beveiligt.

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

### Aanvullende overwegingen

- **Message Broker of Gateway in de keten**: Gebruik signed en/of encrypted profielen om end-to-end beveiliging te garanderen onafhankelijk van de intermediair.
- **Wettelijke onweerlegbaarheid vereist**: Gebruik een signed profiel zodat de afzender niet kan ontkennen het bericht te hebben verstuurd.
- **Gevoelige gegevens**: Gebruik een encrypted profiel om de vertrouwelijkheid van de payload te waarborgen, ook als het bericht wordt opgeslagen of gelogd.
- **Migratiepad**: Organisaties die van ebMS2/WUS naar REST-API willen migreren kunnen beide koppelvlakken parallel aanbieden gedurende een overgangsperiode.

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

## Tests & Validatie

Digikoppeling repos gebruiken de centrale `Automatisering` workflows:

```bash
# WCAG check op gepubliceerde architectuur
npx @axe-core/cli https://logius-standaarden.github.io/Digikoppeling-Architectuur/ --tags wcag2aa

# Markdown lint
npx markdownlint-cli '**/*.md'

# Bekijk de CI/CD configuratie
gh api repos/logius-standaarden/Digikoppeling-Architectuur/contents/.github/workflows --jq '.[].name'
```

## Handige Commando's

```bash
# Laatste wijzigingen architectuur
gh api repos/logius-standaarden/Digikoppeling-Architectuur/commits --jq '.[:5] | .[] | "\(.commit.committer.date) \(.commit.message | split("\n")[0])"'

# Alle Digikoppeling repos
gh api orgs/logius-standaarden/repos --paginate --jq '[.[] | select(.name | startswith("Digikoppeling"))] | sort_by(.name) | .[].name'

# Inhoud van een document
gh api repos/logius-standaarden/Digikoppeling-Architectuur/contents/ch01_Inleiding.md -H "Accept: application/vnd.github.raw"

# Open issues over alle DK repos
for repo in Digikoppeling-Architectuur Digikoppeling-Koppelvlakstandaard-REST-API Digikoppeling-Koppelvlakstandaard-ebMS2; do
  echo "=== $repo ===" && gh issue list --repo logius-standaarden/$repo
done

# OIN stelsel documentatie
gh api repos/logius-standaarden/OIN-Stelsel/contents --jq '.[].name'
```

## Gerelateerde Skills

| Skill | Relatie |
|-------|---------|
| `/ls-api` | REST-API koppelvlak is gebaseerd op API Design Rules |
| `/ls-iam` | Authenticatie via OAuth/OIDC, OIN ook gebruikt voor IAM |
| `/ls-fsc` | FSC als opvolger/aanvulling voor federatieve dienstverlening |
| `/ls-pub` | ReSpec tooling voor alle DK documenten |
| `/ls` | Overzicht alle standaarden |

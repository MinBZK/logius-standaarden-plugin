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

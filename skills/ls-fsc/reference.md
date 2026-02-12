# Federated Service Connectivity - Achtergrondinfo

Dit document bevat achtergrondinfo over FSC-componenten, trust model, tests en handige commando's. Voor implementatiedocumentatie, zie [SKILL.md](SKILL.md).

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

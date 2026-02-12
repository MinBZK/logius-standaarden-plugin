---
name: ls-egov
description: "Gebruik deze skill wanneer de gebruiker vraagt over 'Terugmelding', 'Terugmelden', 'Digimelding', 'basisfactuur', 'basisorder', 'e-procurement', 'inkoopstandaarden overheid', 'factuurstandaard', 'PEPPOLBIS', 'elektronisch bestellen'."
model: sonnet
allowed-tools:
  - Bash(gh api *)
  - Bash(gh issue list *)
  - Bash(gh pr list *)
  - Bash(gh search *)
  - Bash(curl -s *)
  - WebFetch(*)
---

# E-Government Services

**Agent-instructie:** Deze skill helpt bij het implementeren van terugmeldingen op basisregistraties en e-facturatie voor de Rijksoverheid. Gebruik de Terugmelden-API (OpenAPI 3.1.0) voor meldingen. Basisfactuur Rijk volgt Peppol BIS 3 / NLCIUS.

De E-Government standaarden omvatten diensten voor terugmelding op basisregistraties, Digimelding, en e-procurement (elektronisch bestellen en factureren) voor de Nederlandse overheid. Deze standaarden worden beheerd door Logius en vormen een essentieel onderdeel van de digitale overheidsdienstverlening.

## Repositories

| Repository | Beschrijving | Publicatie |
|-----------|-------------|-----------|
| [Terugmelding](https://github.com/logius-standaarden/Terugmelding) | Standaard voor terugmelden op basisregistraties | [Lees online](https://logius-standaarden.github.io/Terugmelding/) |
| [Terugmelden-API](https://github.com/logius-standaarden/Terugmelden-API) | REST API specificatie (OpenAPI 3.1.0) voor terugmelden | [Lees online](https://logius-standaarden.github.io/Terugmelden-API/) |
| [Digimelding-Koppelvlakspecificatie](https://github.com/logius-standaarden/Digimelding-Koppelvlakspecificatie) | Koppelvlakspecificatie voor Digimelding | [Lees online](https://logius-standaarden.github.io/Digimelding-Koppelvlakspecificatie/) |
| [basisfactuur-rijk](https://github.com/logius-standaarden/basisfactuur-rijk) | Standaard basisfactuur voor de Rijksoverheid | [Lees online](https://logius-standaarden.github.io/basisfactuur-rijk/) |
| [basisorder-rijk](https://github.com/logius-standaarden/basisorder-rijk) | Standaard basisorder voor de Rijksoverheid | [Lees online](https://logius-standaarden.github.io/basisorder-rijk/) |
| [e-procurement](https://github.com/logius-standaarden/e-procurement) | Overkoepelende e-procurement standaarden | [Lees online](https://logius-standaarden.github.io/e-procurement/) |

---

## Terugmelden-API

De Terugmelden-API is een REST API gespecificeerd in **OpenAPI 3.1.0**. Deze API is gebaseerd op de Digimelding-functionaliteit en biedt een moderne interface voor het indienen en opvragen van terugmeldingen.

Kenmerken van de API:

- RESTful architectuur conform de API Design Rules (ADR)
- JSON-gebaseerde request/response berichten
- OpenAPI 3.1.0 specificatie voor machineleesbare documentatie
- Ondersteuning voor het aanmaken, opvragen en bijwerken van terugmeldingen

---

## Basisfactuur Rijk: Verplichte Elementen

De Basisfactuur Rijk definieert de minimum data-eisen waaraan facturen aan de Rijksoverheid moeten voldoen. Facturen die de verplichte velden missen worden **automatisch afgewezen**. De volgende elementen zijn verplicht:

### 1. Factuuridentificatie en -datering
- Uniek factuurnummer
- Factuurdatum
- Factuurtypeaanduiding (factuur, creditnota, etc.)
- Valutacode

### 2. Leverancieridentificatie
- Naam en adresgegevens leverancier
- KvK-nummer of vergelijkbaar identificatienummer
- BTW-identificatienummer
- Bankgegevens (IBAN en eventueel BIC)

### 3. Ontvanger (overheidsorganisatie)
- Naam van de ontvangende organisatie
- OIN (Overheids Identificatie Nummer) of vergelijkbaar
- Adresgegevens

### 4. Regelitems
- Omschrijving van het geleverde product of de dienst
- Hoeveelheid en eenheid
- Prijs per eenheid
- Totaalbedrag per regel
- BTW-percentage en BTW-bedrag per regel

### 5. Order- en referentie-informatie
- Ordernummer (inkoopordernummer van de overheidsorganisatie)
- Contractreferentie (indien van toepassing)
- Projectreferentie (indien van toepassing)

### 6. Betalingsvoorwaarden
- Betalingstermijn
- Betalingscondities
- Bankgegevens voor betaling (IBAN)

### 7. Bijlagen
- Maximaal **9 bijlagen** toegestaan
- Totale grootte van alle bijlagen mag niet groter zijn dan **20 MB**
- Ondersteunde formaten conform Peppol-specificaties (PDF, afbeeldingen, etc.)

---

## Samenhang Standaarden

```
E-Procurement (overkoepelend)
├── Basisfactuur Rijk
│   ├── Peppol BIS Billing 3.0
│   ├── NLCIUS (NL-specifieke invulling EN 16931)
│   └── UBL-OHNL
└── Basisorder Rijk
    ├── Peppol BIS Order 3.0
    └── UBL 2.1 Order
```

---

## Implementatievoorbeelden

### Terugmelding Indienen via REST API (curl)

```bash
# Terugmelding aanmaken (root-annotatie)
curl -X POST https://digimelding.example.com/api/v1/terugmeldingen \
  -H "Authorization: Bearer $JWT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "registratie": "BRP",
    "bronhouder_oin": "00000001234567890000",
    "gegeven": {
      "attribuut": "verblijfplaats",
      "huidige_waarde": "Keizersgracht 100, Amsterdam",
      "verwachte_waarde": "Herengracht 200, Amsterdam"
    },
    "betrokkene": {
      "type": "BSN",
      "id": "999990342"
    },
    "toelichting": "Persoon is verhuisd per 1 januari 2024",
    "contactgegevens": {
      "naam": "Gemeente Amsterdam",
      "email": "terugmelding@amsterdam.nl",
      "organisatie_oin": "00000001823288444000"
    }
  }'

# Response: 201 Created
# {
#   "id": "550e8400-e29b-41d4-a716-446655440000",
#   "status": "ontvangen",
#   "registratie": "BRP",
#   "created_at": "2024-01-15T10:30:00Z"
# }
```

### Terugmelding Status Opvragen (curl)

```bash
# Status van een terugmelding ophalen
curl -X GET https://digimelding.example.com/api/v1/terugmeldingen/550e8400-e29b-41d4-a716-446655440000 \
  -H "Authorization: Bearer $JWT_TOKEN" | jq

# Alle leaf-annotaties (reacties) bij een terugmelding
curl -X GET https://digimelding.example.com/api/v1/terugmeldingen/550e8400-e29b-41d4-a716-446655440000/annotaties \
  -H "Authorization: Bearer $JWT_TOKEN" | jq
```

### Terugmelding Verwerken in Python

```python
import requests

class DigimeldingClient:
    """Client voor de Terugmelden-API (OpenAPI 3.1.0)."""

    def __init__(self, base_url: str, token: str):
        self.base_url = base_url
        self.session = requests.Session()
        self.session.headers.update({
            "Authorization": f"Bearer {token}",
            "Content-Type": "application/json",
        })

    def maak_terugmelding(self, registratie: str, attribuut: str,
                          huidige_waarde: str, verwachte_waarde: str,
                          betrokkene_id: str, toelichting: str) -> dict:
        """Dien een nieuwe terugmelding in (root-annotatie)."""
        response = self.session.post(f"{self.base_url}/api/v1/terugmeldingen", json={
            "registratie": registratie,
            "gegeven": {
                "attribuut": attribuut,
                "huidige_waarde": huidige_waarde,
                "verwachte_waarde": verwachte_waarde,
            },
            "betrokkene": {"type": "BSN", "id": betrokkene_id},
            "toelichting": toelichting,
        })
        response.raise_for_status()
        return response.json()

    def get_status(self, terugmelding_id: str) -> dict:
        """Haal de huidige status van een terugmelding op."""
        response = self.session.get(
            f"{self.base_url}/api/v1/terugmeldingen/{terugmelding_id}")
        response.raise_for_status()
        return response.json()

    def voeg_aanvulling_toe(self, terugmelding_id: str, tekst: str) -> dict:
        """Voeg een aanvulling toe als leaf-annotatie."""
        response = self.session.post(
            f"{self.base_url}/api/v1/terugmeldingen/{terugmelding_id}/annotaties",
            json={"type": "aanvulling", "tekst": tekst})
        response.raise_for_status()
        return response.json()

# Gebruik
client = DigimeldingClient("https://digimelding.example.com", token="eyJ...")
tm = client.maak_terugmelding(
    registratie="BAG",
    attribuut="huisnummer",
    huidige_waarde="100",
    verwachte_waarde="100a",
    betrokkene_id="999990342",
    toelichting="Huisnummer is gesplitst na verbouwing"
)
print(f"Terugmelding ingediend: {tm['id']}")
```

### UBL 2.1 Basisfactuur XML (Minimum Viable)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Invoice xmlns="urn:oasis:names:specification:ubl:schema:xsd:Invoice-2"
         xmlns:cac="urn:oasis:names:specification:ubl:schema:xsd:CommonAggregateComponents-2"
         xmlns:cbc="urn:oasis:names:specification:ubl:schema:xsd:CommonBasicComponents-2">

  <!-- 1. Factuuridentificatie -->
  <cbc:UBLVersionID>2.1</cbc:UBLVersionID>
  <cbc:ID>INV-2024-001</cbc:ID>
  <cbc:IssueDate>2024-01-15</cbc:IssueDate>
  <cbc:InvoiceTypeCode>380</cbc:InvoiceTypeCode>  <!-- 380=factuur, 381=creditnota -->
  <cbc:DocumentCurrencyCode>EUR</cbc:DocumentCurrencyCode>

  <!-- Ordernummer (verplicht voor Rijksoverheid) -->
  <cac:OrderReference>
    <cbc:ID>PO-2024-12345</cbc:ID>
  </cac:OrderReference>

  <!-- 2. Leverancier -->
  <cac:AccountingSupplierParty>
    <cac:Party>
      <cac:PartyIdentification>
        <cbc:ID schemeID="0106">12345678</cbc:ID>  <!-- KvK-nummer -->
      </cac:PartyIdentification>
      <cac:PartyName>
        <cbc:Name>Voorbeeld Leverancier B.V.</cbc:Name>
      </cac:PartyName>
      <cac:PostalAddress>
        <cbc:StreetName>Voorbeeldstraat 1</cbc:StreetName>
        <cbc:CityName>Den Haag</cbc:CityName>
        <cbc:PostalZone>2511 AA</cbc:PostalZone>
        <cac:Country><cbc:IdentificationCode>NL</cbc:IdentificationCode></cac:Country>
      </cac:PostalAddress>
      <cac:PartyTaxScheme>
        <cbc:CompanyID>NL123456789B01</cbc:CompanyID>
        <cac:TaxScheme><cbc:ID>VAT</cbc:ID></cac:TaxScheme>
      </cac:PartyTaxScheme>
    </cac:Party>
  </cac:AccountingSupplierParty>

  <!-- 3. Ontvanger (overheidsorganisatie) -->
  <cac:AccountingCustomerParty>
    <cac:Party>
      <cac:PartyIdentification>
        <cbc:ID schemeID="0190">00000001823288444000</cbc:ID>  <!-- OIN -->
      </cac:PartyIdentification>
      <cac:PartyName>
        <cbc:Name>Ministerie van Binnenlandse Zaken</cbc:Name>
      </cac:PartyName>
    </cac:Party>
  </cac:AccountingCustomerParty>

  <!-- 6. Betalingsvoorwaarden -->
  <cac:PaymentMeans>
    <cbc:PaymentMeansCode>30</cbc:PaymentMeansCode>  <!-- Bankoverschrijving -->
    <cac:PayeeFinancialAccount>
      <cbc:ID>NL91ABNA0417164300</cbc:ID>  <!-- IBAN -->
    </cac:PayeeFinancialAccount>
  </cac:PaymentMeans>
  <cac:PaymentTerms>
    <cbc:Note>Betaling binnen 30 dagen</cbc:Note>
  </cac:PaymentTerms>

  <!-- BTW totaal -->
  <cac:TaxTotal>
    <cbc:TaxAmount currencyID="EUR">210.00</cbc:TaxAmount>
    <cac:TaxSubtotal>
      <cbc:TaxableAmount currencyID="EUR">1000.00</cbc:TaxableAmount>
      <cbc:TaxAmount currencyID="EUR">210.00</cbc:TaxAmount>
      <cac:TaxCategory>
        <cbc:ID>S</cbc:ID>
        <cbc:Percent>21.0</cbc:Percent>
        <cac:TaxScheme><cbc:ID>VAT</cbc:ID></cac:TaxScheme>
      </cac:TaxCategory>
    </cac:TaxSubtotal>
  </cac:TaxTotal>

  <!-- Factuurtotaal -->
  <cac:LegalMonetaryTotal>
    <cbc:LineExtensionAmount currencyID="EUR">1000.00</cbc:LineExtensionAmount>
    <cbc:TaxExclusiveAmount currencyID="EUR">1000.00</cbc:TaxExclusiveAmount>
    <cbc:TaxInclusiveAmount currencyID="EUR">1210.00</cbc:TaxInclusiveAmount>
    <cbc:PayableAmount currencyID="EUR">1210.00</cbc:PayableAmount>
  </cac:LegalMonetaryTotal>

  <!-- 4. Regelitems -->
  <cac:InvoiceLine>
    <cbc:ID>1</cbc:ID>
    <cbc:InvoicedQuantity unitCode="HUR">40</cbc:InvoicedQuantity>
    <cbc:LineExtensionAmount currencyID="EUR">1000.00</cbc:LineExtensionAmount>
    <cac:Item>
      <cbc:Name>Consultancy diensten - januari 2024</cbc:Name>
      <cac:ClassifiedTaxCategory>
        <cbc:ID>S</cbc:ID>
        <cbc:Percent>21.0</cbc:Percent>
        <cac:TaxScheme><cbc:ID>VAT</cbc:ID></cac:TaxScheme>
      </cac:ClassifiedTaxCategory>
    </cac:Item>
    <cac:Price>
      <cbc:PriceAmount currencyID="EUR">25.00</cbc:PriceAmount>
    </cac:Price>
  </cac:InvoiceLine>
</Invoice>
```

## Foutafhandeling

### Terugmelden-API Error Responses

De API gebruikt `application/problem+json` conform RFC 9457:

```json
{
  "type": "https://digimelding.example.com/errors/registratie-onbekend",
  "title": "Onbekende basisregistratie",
  "status": 400,
  "detail": "Registratie 'XYZ' is niet beschikbaar voor terugmelding",
  "instance": "/api/v1/terugmeldingen"
}
```

| Status | Beschrijving |
|--------|-------------|
| 201 | Terugmelding succesvol aangemaakt |
| 400 | Verplicht veld ontbreekt of ongeldige waarde |
| 401 | Authenticatie vereist |
| 403 | OIN niet geautoriseerd voor deze registratie |
| 404 | Terugmelding niet gevonden |
| 422 | Validatiefout (bijv. ongeldig BSN-formaat) |

### Basisfactuur Afwijzing Redenen

Facturen worden automatisch afgewezen wanneer:
- **Ordernummer ontbreekt of onjuist** - Meest voorkomende reden. Controleer formaat, spaties en voorvoegsels
- **Verplicht veld ontbreekt** - KvK-nummer, BTW-ID, IBAN, of factuurdatum
- **BTW-berekening inconsistent** - Regeltotalen kloppen niet met factuurtotaal
- **Bijlagen te groot** - Maximaal 9 bijlagen, totaal max 20 MB
- **Ongeldig UBL-formaat** - XML voldoet niet aan NLCIUS-validatieschema

Bij afwijzing ontvangt de leverancier een e-mail met de specifieke ontbrekende velden.

## Achtergrondinfo

Zie [reference.md](reference.md) voor kernbegrippen, annotatie-framework, best practices leveranciers, en e-procurement details.

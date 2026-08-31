---
name: ls-egov
description: "E-procurement bij de overheid: Peppol BIS, NLCIUS, UBL 2.1, basisfactuur, basisorder, Schematron-validatie. Ook Digimelding/Terugmelden voor basisregistraties."
model: sonnet
allowed-tools:
  - Bash(gh api *)
  - Bash(gh issue list *)
  - Bash(gh pr list *)
  - Bash(gh search *)
  - Bash(curl -s *)
  - WebFetch(*)
metadata:
  created-with-ai: "true"
  created-with-model: claude-opus-4-20250514
  created-date: "2025-02-12"
  status: concept
---

> **CONCEPT — Let op:** Deze skill is geen officieel product van Logius. De beschrijvingen zijn informatieve samenvattingen — niet de officiële standaarden zelf. De definities op [forumstandaardisatie.nl](https://www.forumstandaardisatie.nl/open-standaarden) en [Logius](https://www.logius.nl) zijn altijd leidend. Overheidsorganisaties die generatieve AI inzetten dienen te voldoen aan het [Overheidsbreed standpunt voor de inzet van generatieve AI](https://open.overheid.nl/documenten/bc03ce31-0cf1-4946-9c94-e934a62ebe73/file). Zie [DISCLAIMER.md](../../DISCLAIMER.md) en onze [verantwoording](https://github.com/developer-overheid-nl/skills-marketplace/blob/main/docs/verantwoording.md).

# E-Government Services

**Agent-instructie:** Deze skill helpt bij het implementeren van terugmeldingen op basisregistraties en e-facturatie voor de Rijksoverheid. Gebruik de Digimelding-Koppelvlakspecificatie voor meldingen. Basisfactuur Rijk volgt Peppol BIS 3 / NLCIUS.

De E-Government standaarden omvatten diensten voor terugmelding op basisregistraties, Digimelding, en e-procurement (elektronisch bestellen en factureren) voor de Nederlandse overheid. Deze standaarden worden beheerd door Logius en vormen een essentieel onderdeel van de digitale overheidsdienstverlening.

## Versiemodel

Net als andere Logius-standaarden kennen deze standaarden twee publicatiekanalen (vergelijkbaar met W3C):

- **Vastgestelde versie (DEF)**: officieel goedgekeurd, gepubliceerd op `gitdocumentatie.logius.nl`
- **Werkversie (draft)**: werk-in-uitvoering, gepubliceerd op `logius-standaarden.github.io`

De E-Government standaarden hebben momenteel **alleen werkversies**. Er zijn nog geen vastgestelde versies gepubliceerd.

## Repositories

| Repository | Beschrijving | Licentie | Vastgesteld | Draft |
|-----------|-------------|--------|------------|-------|
| [Terugmelding](https://github.com/logius-standaarden/Terugmelding) | Standaard voor terugmelden op basisregistraties | [CC0-1.0](https://creativecommons.org/publicdomain/zero/1.0/legalcode.en) | - | [Draft](https://logius-standaarden.github.io/Terugmelding/) |
| [Terugmelden-API](https://github.com/logius-standaarden/Terugmelden-API) | Werkrepository voor een toekomstige REST API koppelvlakspecificatie voor terugmelden (nog geen specificatie beschikbaar) | Niet gespecificeerd | - | - |
| [Digimelding-Koppelvlakspecificatie](https://github.com/logius-standaarden/Digimelding-Koppelvlakspecificatie) | Koppelvlakspecificatie voor Digimelding | [CC-BY-4.0](https://creativecommons.org/licenses/by/4.0/legalcode.en) | - | [Draft](https://logius-standaarden.github.io/Digimelding-Koppelvlakspecificatie/) |
| [basisfactuur-rijk](https://github.com/logius-standaarden/basisfactuur-rijk) ⚠️ | Handreiking basisfactuur voor de Rijksoverheid (**gearchiveerd** augustus 2026) | [CC0-1.0](https://creativecommons.org/publicdomain/zero/1.0/legalcode.en) | - | [Draft](https://logius-standaarden.github.io/basisfactuur-rijk/) |
| [basisorder-rijk](https://github.com/logius-standaarden/basisorder-rijk) ⚠️ | Handreiking basisorder voor de Rijksoverheid (**gearchiveerd** augustus 2026) | [CC0-1.0](https://creativecommons.org/publicdomain/zero/1.0/legalcode.en) | - | [Draft](https://logius-standaarden.github.io/basisorder-rijk/) |
| [e-procurement](https://github.com/logius-standaarden/e-procurement) ⚠️ | Overkoepelende e-procurement standaarden (**gearchiveerd** augustus 2026) | [CC-BY-4.0](https://creativecommons.org/licenses/by/4.0/legalcode.en) | - | [Draft](https://logius-standaarden.github.io/e-procurement/) |

---

## Terugmelden-API (in ontwikkeling)

De Terugmelden-API is een **beoogde** REST API voor het indienen en opvragen van terugmeldingen. De repository bevat nog geen uitgewerkte specificatie; de API is in conceptfase. De bestaande koppelvlakspecificatie voor terugmelden is de Digimelding-Koppelvlakspecificatie (SOAP/WUS).

> **Let op:** De onderstaande implementatievoorbeelden voor de REST API zijn **conceptueel/illustratief** en tonen hoe een toekomstige REST API eruit zou kunnen zien. Ze zijn niet gebaseerd op een gepubliceerde OpenAPI-specificatie.

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

**Forum Standaardisatie status:** NLCIUS is **verplicht** (['pas-toe-of-leg-uit'](https://www.forumstandaardisatie.nl/open-standaarden/nlcius)). Peppol BIS is [**aanbevolen**](https://www.forumstandaardisatie.nl/open-standaarden/peppol-bis), niet verplicht. De overige e-government standaarden (Digimelding, Terugmelding, e-procurement) staan niet op de lijst.

---

## Implementatievoorbeelden

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

### Digimelding: Bestaande Koppelvlakspecificatie (SOAP/WUS)

De huidige operationele interface voor terugmelden is SOAP/WUS, beschreven in de [Digimelding-Koppelvlakspecificatie](https://github.com/logius-standaarden/Digimelding-Koppelvlakspecificatie). Zie `/ls-dk` voor WUS-implementatiedetails en WSDL-configuratie.

```bash
# Digimelding koppelvlakspecificatie bekijken
gh api repos/logius-standaarden/Digimelding-Koppelvlakspecificatie/contents --jq '.[].name'
```

### Basisfactuur Validatie (Schematron)

Het repo `basisfactuur-rijk` bevat een Schematron bestand met validatieregels en voorbeeld-XML's:

```bash
# Bekijk de Schematron validatieregels voor de Basisfactuur Rijk
gh api repos/logius-standaarden/basisfactuur-rijk/contents/technische-documentatie/basisfactuur-rijk.sch \
  -H "Accept: application/vnd.github.raw"

# Bekijk een voorbeeldfactuur
gh api repos/logius-standaarden/basisfactuur-rijk/contents/technische-documentatie/NLCIUS%20voorbeeldbericht%20Basisfactuur%20Rijk%20v01.xml \
  -H "Accept: application/vnd.github.raw"
```

---

### Toekomstig: Terugmelden-API (conceptueel)

> **Let op:** De onderstaande REST API voorbeelden zijn **conceptueel/illustratief**. Er bestaat een [werkrepository Terugmelden-API](https://github.com/logius-standaarden/Terugmelden-API) maar deze bevat nog geen OpenAPI-specificatie. De bestaande operationele interface is SOAP/WUS via Digimelding (zie hierboven). Gebruik deze voorbeelden **niet** als basis voor productie-code.

```bash
# Conceptueel: terugmelding aanmaken via toekomstige REST API
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
    "betrokkene": {"type": "BSN", "id": "999990342"},
    "toelichting": "Persoon is verhuisd per 1 januari 2024"
  }'
```

## Achtergrondinfo

Zie [reference.md](reference.md) voor kernbegrippen, annotatie-framework, best practices leveranciers, en e-procurement details.
Zie [conflicts.md](conflicts.md) voor bronconflicten en gemaakte keuzes.

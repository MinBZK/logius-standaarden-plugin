# API Design Rules - Achtergrondinfo

Deze pagina bevat aanvullende voorbeelden en tools die niet in de hoofdskill passen.

## Implementatievoorbeelden

### Express.js

```javascript
const express = require('express');
const app = express();

// API-Version header in alle responses
app.use((req, res, next) => {
  res.header('API-Version', '1.0.0');
  next();
});

// OpenAPI endpoint
app.get('/openapi.json', (req, res) => {
  res.json({
    openapi: '3.0.0',
    info: {
      title: 'Example API',
      version: '1.0.0',
      contact: {
        name: 'API Support',
        url: 'https://example.com/support',
        email: 'support@example.com'
      }
    },
    servers: [{ url: 'https://example.com/api/v1' }]
  });
});
```

### Go/Gin

```go
func setupRouter() *gin.Engine {
    r := gin.Default()
    r.Use(func(c *gin.Context) {
        c.Header("API-Version", "1.0.0")
        c.Next()
    })
    return r
}
```

Er zijn ook referentie-implementaties in **ASP.NET** en **Quarkus** beschikbaar.

## Impact Analyse Tool

De impact analyse tool test Spectral regels tegen echte OpenAPI specs uit het API-register:

```bash
gh repo clone logius-standaarden/api-linter-impactanalyse /tmp/api-linter
cd /tmp/api-linter
uv run run-spectral-for-single-rule.py --rule nlgov:paths-no-trailing-slash
```

## Repository Exploratie Commando's

```bash
# Laatste wijzigingen aan de ADR
gh api repos/logius-standaarden/API-Design-Rules/commits \
  --jq '.[:5] | .[] | "\(.commit.committer.date) \(.commit.message | split("\n")[0])"'

# Open issues/PRs
gh issue list --repo logius-standaarden/API-Design-Rules
gh pr list --repo logius-standaarden/API-Design-Rules

# Zoek naar specifiek onderwerp in de spec
gh search code "pagination" --repo logius-standaarden/API-Design-Rules

# Inhoud van een bestand ophalen
gh api repos/logius-standaarden/API-Design-Rules/contents/README.md \
  -H "Accept: application/vnd.github.raw"
```

## Gerelateerde Skills

| Skill | Relatie |
|-------|---------|
| `/ls-iam` | OAuth/OIDC profielen voor API-authenticatie |
| `/ls-dk` | Digikoppeling REST-API koppelvlak gebruikt ADR |
| `/ls-fsc` | FSC dienstverlening via ADR-conforme APIs |
| `/ls-pub` | ReSpec tooling voor ADR documentatie |
| `/ls` | Overzicht alle standaarden |

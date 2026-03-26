# Feature Documentation: req-NNN

## Overview

<!-- One paragraph describing what this feature does and who it is for. This is the documentation a developer or user reads to understand the feature. -->

---

## API Changes

<!-- Document every new or modified API endpoint. Use OpenAPI-style descriptions or the project's agreed documentation format.
     If no API changes, write "No API changes." -->

### `METHOD /path/to/endpoint`

**Description:** <!-- what this endpoint does -->

**Request:**
```json
{
  "field": "type — description"
}
```

**Response:**
```json
{
  "field": "type — description"
}
```

**Error codes:**
| Code | Meaning |
|------|---------|
| 400 | <!-- validation error --> |
| 404 | <!-- not found --> |

---

## Configuration Changes

<!-- Any new environment variables, feature flags, or config file changes required.
     If none, write "No configuration changes." -->

| Variable | Type | Default | Description |
|----------|------|---------|-------------|
| `VAR_NAME` | string | — | <!-- purpose --> |

---

## Database Changes

<!-- Schema migrations, new tables, index changes, etc.
     If none, write "No database changes." -->

---

## Component / Module Changes

<!-- For frontend or library changes: describe new or modified components, their props/interface, and usage. -->

---

## Usage Examples

<!-- Concrete examples showing how to use the feature. Code snippets preferred. -->

```
<!-- example -->
```

---

## Migration Notes

<!-- Anything a developer or operator needs to do when deploying this feature (run migrations, update config, clear cache, etc.).
     If none, write "No migration steps required." -->

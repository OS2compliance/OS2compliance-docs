---
title: Organisations
layout: default
parent: API
nav_order: 5
---

# Organisations

Base path: `/api/v1/organisations`

Organisation units are **read only** through the API. They are identified by a
UUID (the fkOrg UUID).

## Endpoints

| Method | Path | Description |
|---|---|---|
| `GET` | `/api/v1/organisations` | List all organisation units (paged) |
| `GET` | `/api/v1/organisations/{uuid}` | Fetch a single organisation unit |

### List

`GET /api/v1/organisations` — query parameters `page` (default `0`), `pageSize`
(default `100`, max `500`). Returns a [page](api#pagination) of `Organisation`
objects. `200 OK` · `401 Unauthorized`.

### Fetch

`GET /api/v1/organisations/{uuid}` — `200 OK` · `404 Not Found` ·
`401 Unauthorized`.

---

## The `Organisation` object

| Field | Type | Notes |
|---|---|---|
| `uuid` | string | The fkOrg UUID of the organisation unit |
| `name` | string | Name of the organisation unit |
| `parentUuid` | string | UUID of the parent unit (empty for the root) |
| `active` | boolean | Whether the unit is currently active |

### Example — fetch response

```json
{
  "uuid": "f2dad6d3-79a3-426b-986f-ac568d759983",
  "name": "IT-afdelingen",
  "parentUuid": "a1b2c3d4-0000-0000-0000-000000000000",
  "active": true
}
```

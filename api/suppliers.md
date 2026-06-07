---
title: Suppliers
layout: default
parent: API
nav_order: 4
---

# Suppliers

Base path: `/api/v1/suppliers`

A supplier represents a vendor or other external party, with contact and
address information. Suppliers can be referenced from [assets](assets).

## Endpoints

| Method | Path | Description |
|---|---|---|
| `GET` | `/api/v1/suppliers` | List all suppliers (paged) |
| `GET` | `/api/v1/suppliers/{id}` | Fetch a single supplier |
| `POST` | `/api/v1/suppliers` | Create a new supplier |
| `PUT` | `/api/v1/suppliers/{id}` | Update an existing supplier |
| `DELETE` | `/api/v1/suppliers/{id}` | Delete a supplier |

### List

`GET /api/v1/suppliers` — query parameters `page` (default `0`), `pageSize`
(default `100`, max `500`). Returns a [page](api#pagination) of `Supplier`
objects. `200 OK` · `401 Unauthorized`.

### Fetch

`GET /api/v1/suppliers/{id}` — `200 OK` · `404 Not Found` · `401 Unauthorized`.

### Create

`POST /api/v1/suppliers` — body is a `SupplierCreate` object. Returns the
created supplier. `201 Created` · `401 Unauthorized`.

### Update

`PUT /api/v1/suppliers/{id}` — read-modify-write; the `version` must match the
current version. Send the complete entity back.

> **NOTICE!** `properties` are used by multiple parties — never remove unknown
> properties, and prefix your own properties so they are unique. See
> [custom properties](api#custom-properties).

`204 No Content` · `404 Not Found` · `401 Unauthorized` · `409 Conflict`
(version mismatch).

### Delete

`DELETE /api/v1/suppliers/{id}` — `204 No Content` · `404 Not Found` ·
`401 Unauthorized`.

---

## The `Supplier` object

| Field | Type | Notes |
|---|---|---|
| `id` | integer | ID in OS2compliance. Read-only |
| `version` | integer | Optimistic-locking version. **Required** on update |
| `createdAt` | date-time | Read-only |
| `createdBy` | string | Read-only |
| `updatedAt` | date-time | Read-only |
| `updatedBy` | string | Read-only |
| `name` | string | **Required.** Name of the supplier |
| `responsibleUser` | [User](users#the-user-object) | On write, send `{ "uuid": "..." }` |
| `status` | enum | **Required.** `READY`, `IN_PROGRESS` |
| `cvr` | string | CVR number |
| `zip` | string | Postal code |
| `city` | string | |
| `address` | string | |
| `contact` | string | Contact person |
| `phone` | string | |
| `email` | string | |
| `country` | string | |
| `description` | string | HTML description |
| `properties` | array of `{ key, value }` | [Custom properties](api#custom-properties) |

Only `name` and `status` are required on create.

### Example — fetch response

```json
{
  "id": 1,
  "version": 1,
  "createdAt": "2023-09-27T11:20:17+02:00",
  "createdBy": "Bruger Brugersen",
  "name": "Digital Identity ApS",
  "status": "IN_PROGRESS",
  "cvr": "36074051",
  "zip": "8260",
  "city": "Viby J",
  "address": "Gunnar Clausens Vej 68",
  "contact": "Brian Storm Graversen",
  "phone": "12345678",
  "email": "kontakt@digital-identity.dk",
  "country": "Danmark",
  "description": "<h2>En eksempel overskrift</h2>",
  "properties": [ { "key": "mysystem_id", "value": "1234ABC" } ]
}
```

### Example — create request

```json
{
  "name": "Digital Identity ApS",
  "status": "IN_PROGRESS",
  "responsibleUser": { "uuid": "f2dad6d3-79a3-426b-986f-ac568d759983" },
  "cvr": "36074051",
  "zip": "8260",
  "city": "Viby J",
  "address": "Gunnar Clausens Vej 68",
  "email": "kontakt@digital-identity.dk",
  "country": "Danmark"
}
```

---
title: Assets
layout: default
parent: API
nav_order: 2
---

# Assets

Base path: `/api/v1/assets`

An asset represents an IT system or other registered asset in OS2compliance,
including its owners, supplier(s), supervision/data-processing details and any
custom properties.

## Endpoints

| Method | Path | Description |
|---|---|---|
| `GET` | `/api/v1/assets` | List all assets (paged) |
| `GET` | `/api/v1/assets/{id}` | Fetch a single asset |
| `POST` | `/api/v1/assets` | Create a new asset |
| `PUT` | `/api/v1/assets/{id}` | Update an existing asset |
| `DELETE` | `/api/v1/assets/{id}` | Delete an asset |

---

## List all assets

`GET /api/v1/assets`

Returns a [paged](api#pagination) list of non-deleted assets.

**Query parameters:** `page` (default `0`), `pageSize` (default `100`, max `500`).

**Responses:** `200 OK` — a page of `Asset` objects · `401 Unauthorized`.

## Fetch an asset

`GET /api/v1/assets/{id}`

**Responses:** `200 OK` — an `Asset` · `404 Not Found` · `401 Unauthorized`.

## Create an asset

`POST /api/v1/assets`

Body: an `AssetCreate` object. Returns the created `Asset` (including its
generated `id` and `version`).

**Responses:** `201 Created` — the created `Asset` · `401 Unauthorized`.

## Update an asset

`PUT /api/v1/assets/{id}`

Updates an asset. Follow the read-modify-write pattern: `GET` the asset first,
change the fields you need, and `PUT` the **complete** entity back. The
`version` you send must match the current version or the request fails with
`409 Conflict`.

> **NOTICE!** `properties` are used by multiple parties — never remove unknown
> properties, and prefix your own properties so they are unique. See
> [custom properties](api#custom-properties).

Body: an `AssetUpdate` object.

**Responses:** `204 No Content` · `404 Not Found` · `401 Unauthorized` ·
`409 Conflict` (version mismatch) · `400 Bad Request` (e.g. unknown asset type
or supervision model).

## Delete an asset

`DELETE /api/v1/assets/{id}`

**Responses:** `204 No Content` · `404 Not Found` · `401 Unauthorized`.

---

## The `Asset` object

| Field | Type | Notes |
|---|---|---|
| `id` | integer | Internal ID in OS2compliance. Read-only |
| `version` | integer | Optimistic-locking version. Must match on update |
| `createdAt` | date-time | Read-only |
| `createdBy` | string | Read-only |
| `updatedAt` | date-time | Read-only |
| `updatedBy` | string | Read-only |
| `name` | string | **Required.** Name of the asset |
| `systemOwners` | array of [User](users#the-user-object) | System owners |
| `description` | string | Asset description |
| `assetType` | object `{ identifier, name }` | Type of the asset (e.g. IT-system) |
| `dataProcessingAgreementStatus` | enum | `YES`, `NO`, `ON_GOING`, `NOT_RELEVANT` |
| `dataProcessingAgreementDate` | date | |
| `dataProcessingAgreementLink` | string | |
| `supervisoryModel` | object `{ identifier }` | Supervision model |
| `nextInspection` | enum | `DATE`, `MONTH`, `QUARTER`, `HALF_YEAR`, `YEAR`, `EVERY_2_YEARS`, `EVERY_3_YEARS`, `DBS` |
| `nextInspectionDate` | date | |
| `assetStatus` | enum | `READY`, `ON_GOING`, `NOT_STARTED` |
| `criticality` | enum | `CRITICAL`, `NON_CRITICAL` |
| `assetCategory` | enum | `GREEN`, `YELLOW`, `WHITE`, `RED` |
| `sociallyCritical` | boolean | |
| `emergencyPlanLink` | string | |
| `reEstablishmentPlanLink` | string | |
| `contractLink` | string | |
| `contractDate` | date | |
| `contractTermination` | date | |
| `terminationNotice` | string | e.g. `"3 uger"` |
| `archive` | enum | `UNDECIDED`, `B`, `K`, `BK`, `KD`, `KB`, `UNKNOWN`, `PRESERVEDATACANDISCARDDOCUMENTS` |
| `supplier` | [Supplier](suppliers) (shallow `{ id, name }`) | Main supplier |
| `subSuppliers` | array of supplier `{ id, name }` | Sub-suppliers |
| `responsibleUsers` | array of [User](users#the-user-object) | Responsible users |
| `properties` | array of `{ key, value }` | [Custom properties](api#custom-properties) |
| `productLinks` | array of string | |
| `departments` | array of [Organisation](organisations#the-organisation-object) | |

### Write payloads

When **creating** (`AssetCreate`) or **updating** (`AssetUpdate`) an asset, a
few fields differ from the read representation:

- `version` is **required** on update (and must match the current version);
  it is not sent on create.
- References to other resources are sent as lightweight objects, not full
  entities:
  - Users (`systemOwners`, `responsibleUsers`) are sent as `{ "uuid": "..." }`.
  - Suppliers (`supplier`, `subSuppliers`) are sent as `{ "id": ... }`.
  - `assetType` and `supervisoryModel` are referenced by their `identifier`.

Only `name` is strictly required; all other fields are optional.

### Example — fetch response

```json
{
  "id": 1,
  "version": 1,
  "createdAt": "2023-09-27T11:20:17+02:00",
  "createdBy": "Bruger Brugersen",
  "name": "OS2compliance",
  "description": "A long description of the asset",
  "assetType": { "identifier": "asset-type-it-system-83AF9E", "name": "IT-system" },
  "dataProcessingAgreementStatus": "ON_GOING",
  "assetStatus": "READY",
  "criticality": "CRITICAL",
  "sociallyCritical": true,
  "archive": "UNDECIDED",
  "supplier": { "id": 1, "name": "Digital Identity ApS" },
  "responsibleUsers": [
    { "uuid": "f2dad6d3-79a3-426b-986f-ac568d759983", "userId": "kbp", "name": "Kaspar Bach Pedersen", "email": "eksempel@email.com" }
  ],
  "properties": [ { "key": "mysystem_id", "value": "1234ABC" } ],
  "productLinks": [ "https://www.os2.eu/os2compliance" ]
}
```

### Example — create request

```json
{
  "name": "OS2compliance",
  "description": "A long description of the asset",
  "assetType": { "identifier": "asset-type-it-system-83AF9E" },
  "dataProcessingAgreementStatus": "ON_GOING",
  "assetStatus": "READY",
  "supplier": { "id": 1 },
  "responsibleUsers": [ { "uuid": "f2dad6d3-79a3-426b-986f-ac568d759983" } ],
  "properties": [ { "key": "mysystem_id", "value": "1234ABC" } ]
}
```

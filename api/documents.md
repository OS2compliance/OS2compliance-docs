---
title: Documents
layout: default
parent: API
nav_order: 3
---

# Documents

Base paths: `/api/v1/documents` and `/api/v2/documents`

A document represents a procedure, guide, contract, report or similar artefact
registered in OS2compliance.

## v1 vs. v2 — which should I use?

There are two versions of the documents resource. They are identical except for
how the **document type** is expressed:

| | v1 (`/api/v1/documents`) | v2 (`/api/v2/documents`) |
|---|---|---|
| Document type field | `documentType` — a fixed **enum** | `documentTypeIdentifier` — a **choice-value identifier** (string) |
| Allowed values | `PROCEDURE`, `GUIDE`, `WORKFLOW`, `CONTRACT`, `DATA_PROCESSING_AGREEMENT`, `SUPERVISORY_REPORT`, `MANAGEMENT_REPORT`, `RISK_ASSESSMENT_REPORT`, `CONTROL`, `OTHER` | Any document-type identifier configured in OS2compliance, e.g. `document-type-procedure-123456` |
| Custom document types | Not supported (fixed list only) | Supported |

**Use v2 for new integrations** — it supports custom document types configured
in the installation. v1 remains for backwards compatibility and maps its fixed
enum onto the corresponding built-in choice values.

## Endpoints

The two versions expose the same set of operations; substitute `v1` or `v2` in
the path.

| Method | Path | Description |
|---|---|---|
| `GET` | `/api/{v1,v2}/documents` | List all documents (paged) |
| `GET` | `/api/{v1,v2}/documents/{id}` | Fetch a single document |
| `POST` | `/api/{v1,v2}/documents` | Create a new document |
| `PUT` | `/api/{v1,v2}/documents/{id}` | Update an existing document |
| `DELETE` | `/api/{v1,v2}/documents/{id}` | Delete a document |

### List

`GET /api/v2/documents` — query parameters `page` (default `0`), `pageSize`
(default `100`, max `500`). Returns a [page](api#pagination) of `Document`
objects. `200 OK` · `401 Unauthorized`.

### Fetch

`GET /api/v2/documents/{id}` — `200 OK` · `404 Not Found` · `401 Unauthorized`.

### Create

`POST /api/v2/documents` — body is a `Document` create payload. Returns the
created document. `201 Created` · `400 Bad Request` (invalid document type
identifier) · `401 Unauthorized`.

### Update

`PUT /api/v2/documents/{id}` — read-modify-write; the `version` must match the
current version. Send the complete entity back. `204 No Content` ·
`404 Not Found` · `401 Unauthorized` · `409 Conflict` (version mismatch) ·
`400 Bad Request` (invalid document type identifier).

### Delete

`DELETE /api/v2/documents/{id}` — `204 No Content` · `404 Not Found` ·
`401 Unauthorized`.

---

## The `Document` object (v2)

| Field | Type | Notes |
|---|---|---|
| `version` | integer | **Required** on update; must match current version |
| `name` | string | **Required** |
| `responsibleUser` | [User](users#the-user-object) | On write, send `{ "uuid": "..." }` |
| `status` | enum | **Required.** `NOT_STARTED`, `IN_PROGRESS`, `READY` |
| `documentTypeIdentifier` | string | **Required.** A document-type choice-value identifier, e.g. `document-type-procedure-123456` |
| `description` | string | |
| `link` | string | Link to the document, e.g. `http://someurl.com/filename.doc` |
| `documentVersion` | string | The document's own version label, e.g. `1.2b` |
| `revisionInterval` | enum | `NONE`, `HALF_YEARLY`, `YEARLY`, `EVERY_SECOND_YEAR`, `EVERY_THIRD_YEAR` |
| `nextRevision` | date | |
| `includeInYearWheel` | boolean | Update only — include the revision in the year wheel |

### The `Document` object (v1)

Identical to v2 except that the document type is the fixed enum field
`documentType` (values listed in the table above) instead of
`documentTypeIdentifier`.

### Example — create request (v2)

```json
{
  "name": "Databehandleraftale-procedure",
  "responsibleUser": { "uuid": "f2dad6d3-79a3-426b-986f-ac568d759983" },
  "status": "IN_PROGRESS",
  "documentTypeIdentifier": "document-type-procedure-123456",
  "description": "En beskrivelse af dokumentet",
  "link": "http://someurl.com/filename.doc",
  "documentVersion": "1.2b",
  "revisionInterval": "EVERY_SECOND_YEAR"
}
```

### Example — create request (v1)

```json
{
  "name": "Databehandleraftale-procedure",
  "responsibleUser": { "uuid": "f2dad6d3-79a3-426b-986f-ac568d759983" },
  "status": "IN_PROGRESS",
  "documentType": "GUIDE",
  "description": "En beskrivelse af dokumentet",
  "link": "http://someurl.com/filename.doc",
  "documentVersion": "1.2b",
  "revisionInterval": "EVERY_SECOND_YEAR"
}
```

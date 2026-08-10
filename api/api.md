---
title: API
layout: default
nav_order: 6
has_children: true
---

# OS2compliance API

OS2compliance exposes a REST API that lets external systems read and maintain
assets, documents, suppliers, users and organisation units. The API is the
recommended way to integrate with OS2compliance — for example to keep an asset
register in sync with an external CMDB, or to create documents from another
system.

This documentation is generated from, and kept in sync with, the same source
code that powers the interactive Swagger UI. It is intended as a more readable,
printable reference for integrators.

> **The most up-to-date documentation lives in your own environment.** Every
> OS2compliance installation serves an interactive Swagger UI that always
> matches the exact version running in that environment:
>
> ```
> https://<kommune>.os2compliance.dk/swagger-ui/index.html
> ```
>
> Replace `<kommune>` with your own installation. The raw OpenAPI 3
> specification is available at `https://<kommune>.os2compliance.dk/v3/api-docs`.
>
> Use the Swagger UI when you want to try requests directly and be certain you
> are looking at the newest version; use this reference when you want a stable,
> readable overview to read or print.

## Base URL

All endpoints live under the `/api` path on your OS2compliance installation:

```
https://<your-os2compliance-host>/api
```

Replace `<your-os2compliance-host>` with the hostname of your installation
(for example a test or production environment). All paths shown in this
documentation are relative to the host, e.g. `GET /api/v1/assets`.

## Resources

| Resource | Base path | Description |
|---|---|---|
| [Assets](assets) | `/api/v1/assets` | IT systems and other assets, including suppliers, owners and properties |
| [Documents](documents) | `/api/v1/documents`, `/api/v2/documents` | Documents such as procedures, contracts and reports |
| [Suppliers](suppliers) | `/api/v1/suppliers` | Suppliers and their contact information |
| [Organisations](organisations) | `/api/v1/organisations` | Organisation units (read only) |
| [Users](users) | `/api/v1/users` | Users (read only) |

## Authentication

Every request must include a valid API key in the `ApiKey` HTTP header.
See [Authentication](authentication) for details.

## Content type

All request and response bodies are JSON. Send `Content-Type: application/json`
on requests with a body, and expect `application/json` in responses.
Fields that are `null` are omitted from responses.

## Versioning

Resources are versioned in the path (`/api/v1/...`, `/api/v2/...`). When a
breaking change to a resource is needed, a new version of that resource is
introduced and the previous version is kept for backwards compatibility. Only
the **Documents** resource currently has more than one version — see
[Documents](documents) for the difference between `v1` and `v2`.

In addition to the path version, every individual resource carries its own
**optimistic-locking version** field (see below).

## Pagination

List endpoints (`GET` on a collection) are paged. Two query parameters control
paging:

| Parameter | Type | Default | Notes |
|---|---|---|---|
| `page` | integer | `0` | Zero-based page number — the first page is `0` |
| `pageSize` | integer | `100` | Number of items per page, **maximum 500** |

The response is a **page wrapper** with the following shape:

```json
{
  "totalPages": 3,
  "page": 0,
  "totalCount": 250,
  "count": 100,
  "content": [ ]
}
```

| Field | Type | Description |
|---|---|---|
| `totalPages` | integer | Total number of pages available |
| `page` | integer | The current page number (zero-based) |
| `totalCount` | integer | Total number of items across all pages |
| `count` | integer | Number of items in `content` on this page |
| `content` | array | The items on this page |

To read all items, request `page=0`, then `page=1`, and so on until `page`
reaches `totalPages - 1`.

## Optimistic locking (the `version` field)

Updatable resources carry a `version` field. To avoid two clients overwriting
each other, updates follow a read-modify-write pattern:

1. `GET` the resource to obtain the current representation, including its
   `version`.
2. Change the fields you need, keeping the `version` you just received.
3. `PUT` the **complete** entity back.

If the `version` you send does not match the current version stored in
OS2compliance, the update is rejected with **`409 Conflict`**. Re-fetch the
resource and try again.

> **Send the complete entity on update.** Update (`PUT`) replaces the resource.
> Fetch it first, modify the fields you need and send everything back —
> omitting fields will clear them. This is especially important for the
> `properties` collection (see below).

## Custom properties

Assets and suppliers support a free-form `properties` collection — a set of
key/value pairs that any integrating system can use to store its own
identifiers or flags on the resource:

```json
"properties": [
  { "key": "mysystem_id", "value": "1234ABC" }
]
```

Because **multiple systems share this collection**, follow two rules:

- **Prefix your keys** with your system name (e.g. `mysystem_id`) to avoid
  clashing with other integrations.
- On update, **never drop properties you do not recognise** — they may belong
  to another integration. Fetch the resource first and send the full set back.

## Errors

When a request fails, the API returns the appropriate HTTP status code and, for
most endpoints, a JSON error body:

```json
{
  "timestamp": "2023-09-27T11:20:17+02:00",
  "status": 404,
  "error": "Asset not found",
  "path": "/api/v1/assets/1"
}
```

| Field | Type | Description |
|---|---|---|
| `timestamp` | string (date-time) | When the error occurred |
| `status` | integer | HTTP status code |
| `error` | string | Human-readable error message |
| `path` | string | The request path that produced the error |

Common status codes used across the API:

| Status | Meaning |
|---|---|
| `200 OK` | The request succeeded (read / list) |
| `201 Created` | A new resource was created |
| `204 No Content` | The request succeeded with no response body (update / delete) |
| `400 Bad Request` | The request was malformed or referenced something that does not exist (e.g. an unknown user or supplier) |
| `401 Unauthorized` | The `ApiKey` header was missing or invalid |
| `404 Not Found` | The requested resource does not exist |
| `409 Conflict` | The `version` you sent does not match the current version (see optimistic locking) |

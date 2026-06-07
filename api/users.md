---
title: Users
layout: default
parent: API
nav_order: 6
---

# Users

Base path: `/api/v1/users`

Users are **read only** through the API. A user is identified by a UUID (the
fkOrg UUID) and also has a human-readable `userId`.

## Endpoints

| Method | Path | Description |
|---|---|---|
| `GET` | `/api/v1/users` | List all users (paged) |
| `GET` | `/api/v1/users/{uuid}` | Fetch a single user by UUID |
| `GET` | `/api/v1/users/find?userId=...` | Find a user by their `userId` |

### List

`GET /api/v1/users` — query parameters `page` (default `0`), `pageSize`
(default `100`, max `500`). Returns a [page](api#pagination) of `User` objects.
`200 OK` · `401 Unauthorized`.

### Fetch by UUID

`GET /api/v1/users/{uuid}` — `200 OK` · `404 Not Found` · `401 Unauthorized`.

### Find by userId

`GET /api/v1/users/find?userId={userId}`

Looks up a single user by their `userId` (e.g. `kbp`).

**Query parameters:** `userId` (required).

`200 OK` · `404 Not Found` · `401 Unauthorized`.

---

## The `User` object

| Field | Type | Notes |
|---|---|---|
| `uuid` | string | The fkOrg UUID of the user |
| `userId` | string | The user's login id, e.g. `kbp` |
| `name` | string | Full name |
| `email` | string | Email address |

When **referencing** a user from another resource (an asset's owner, a
document's responsible user, etc.), only the `uuid` is sent:

```json
{ "uuid": "f2dad6d3-79a3-426b-986f-ac568d759983" }
```

### Example — fetch response

```json
{
  "uuid": "f2dad6d3-79a3-426b-986f-ac568d759983",
  "userId": "kbp",
  "name": "Kaspar Bach Pedersen",
  "email": "eksempel@email.com"
}
```

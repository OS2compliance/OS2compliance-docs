---
title: Authentication
layout: default
parent: API
nav_order: 1
---

# Authentication

The OS2compliance API uses a simple **API key** scheme. Every request to a path
under `/api/` must carry a valid key in a custom HTTP header named `ApiKey`.

```
ApiKey: <your-api-key>
```

> **Why a custom header and not `Authorization`?** OS2compliance authenticates
> interactive users with SAML, and the SAML filter does not play well with the
> standard `Authorization` header. The API therefore uses the dedicated `ApiKey`
> header instead.

## Example request

```bash
curl https://<your-os2compliance-host>/api/v1/assets \
  -H "ApiKey: 3f9a1c7e-2b44-4e90-9c1a-7d2f5b8e0a16"
```

## Obtaining an API key

API keys are issued **per integrating system** (an "API client"). Each API
client has a name, an application identifier and a key. Keys are provisioned by
an OS2compliance administrator / Digital Identity — there is no self-service UI
for creating them.

Contact your OS2compliance administrator (or Digital Identity) to have an API
client and key created for your integration. Treat the key as a secret: store
it securely and do not commit it to source control or share it in plain text.

## Permissions

A request authenticated with a valid key acts as a system user with
authenticated access. The key is not scoped to individual resources — any valid
key may call any of the documented endpoints.

## Failure responses

| Situation | Response |
|---|---|
| `ApiKey` header missing | `401 Unauthorized` — `Invalid ApiKey header` |
| `ApiKey` header present but not recognised | `401 Unauthorized` — `Invalid ApiKey header` |

Invalid attempts are logged on the server. If you receive a `401` even though
you believe the key is correct, verify that the header name is exactly `ApiKey`
(case-insensitive) and that the key has not been rotated.

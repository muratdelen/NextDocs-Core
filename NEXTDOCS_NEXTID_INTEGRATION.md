# NextDocs NextID Integration Contract

This document defines the first safe contract for connecting NextDocs to NextID without breaking the existing Paperless-ngx login behavior.

## Client

Recommended NextID client id:

```text
nextdocs
```

Recommended display name:

```text
NextDocs
```

## Local development redirects

```text
http://localhost:8094/*
http://nextdocs.harran.edu.tr/*
```

Use stricter redirect URIs in production after the final public URL is stable.

## Required OIDC claims

NextDocs should initially consume only common claims:

| Claim | Usage |
|---|---|
| sub | Stable user identifier |
| preferred_username | Username |
| email | User email |
| given_name | First name |
| family_name | Last name |
| name | Display name fallback |
| groups | Group based access control |
| realm_access.roles | Role based access control |

## Initial roles

```text
NEXTDOCS_ADMIN
NEXTDOCS_MANAGER
NEXTDOCS_EDITOR
NEXTDOCS_VIEWER
NEXTDOCS_ARCHIVIST
NEXTDOCS_AUDITOR
```

## Safe rollout model

1. Keep Paperless-ngx local login enabled.
2. Add NextID SSO as an optional path.
3. Test login with one admin user and one normal user.
4. Test logout and session timeout.
5. Only after successful tests, make SSO the preferred path.
6. Keep an emergency local admin login path.

## User mapping

Recommended mapping:

| NextID | NextDocs |
|---|---|
| sub | external_id |
| preferred_username | username |
| email | email |
| given_name | first_name |
| family_name | last_name |
| groups | groups |
| realm_access.roles | permissions source |

## Access strategy

First stage:

- Read roles from token claims.
- Map roles to internal permissions.
- Do not require NextRole service yet.

Second stage:

- Ask NextRole for policy decisions.
- Cache policy decisions for a short period.
- Send allow/deny decisions to NextLog.

## Logout

NextDocs logout should clear:

- NextDocs session
- OAuth proxy session if used
- NextID session when global logout is requested

## Production notes

- HTTPS must be enabled before production.
- Cookies must be secure and same-site aware.
- Admin fallback must be protected and documented.
- Redirect URI wildcards should be reduced after the final domain is fixed.

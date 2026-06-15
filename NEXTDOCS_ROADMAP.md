# NextDocs Roadmap

This file starts the NextDocs planning work without changing runtime code.

## Goal

NextDocs is the document management module for the NextLife platform. It is based on Paperless-ngx and will be extended carefully with NextID, NextRole, NextLog, NextSearch and NextFlow integration.

## Safety Rules

- Do not break the existing Paperless-ngx behavior.
- Keep upstream license and notice files intact.
- Prefer additive files first.
- Keep NextID integration optional until it is tested.
- Do not force SSO before a fallback admin login path exists.

## Phases

### Phase 0 - Stabilize the base

- Verify GHCR image publication.
- Document required environment variables.
- Keep the default branch workflow unchanged.
- Add health-check and startup notes.

### Phase 1 - NextID integration

- Create a `nextdocs` client in NextID.
- Support local and domain redirect URIs.
- Map username, email, first name and last name from OIDC claims.
- Keep the existing Paperless login path until SSO is stable.

### Phase 2 - Document lifecycle

Document states:

- draft
- uploaded
- classified
- pending approval
- approved
- archived
- rejected

Core metadata:

- institution
- unit
- person
- process
- document type
- security level
- retention period

### Phase 3 - Role model

Initial roles:

- NEXTDOCS_ADMIN
- NEXTDOCS_MANAGER
- NEXTDOCS_EDITOR
- NEXTDOCS_VIEWER
- NEXTDOCS_ARCHIVIST
- NEXTDOCS_AUDITOR

### Phase 4 - Audit events

Events to send to NextLog:

- document uploaded
- document viewed
- document downloaded
- document updated
- document deleted
- document archived
- metadata changed
- unauthorized access attempted
- OCR completed
- OCR failed

### Phase 5 - Search

- Index OCR text and metadata into NextSearch.
- Apply permission filters before showing search results.

### Phase 6 - Workflow

- Send a document to approval.
- Attach a document to a task or flow.
- Archive approved documents automatically.

## First implementation steps

1. Add this roadmap.
2. Add a NextID integration contract.
3. Add a NextLog audit event contract.
4. Add NextLife-specific files under a separate additive path.
5. Add example environment files later.

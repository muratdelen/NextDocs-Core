# NextDocs Audit Event Contract

This document defines the first NextLog audit contract for NextDocs.

## Event envelope

```json
{
  "eventId": "uuid",
  "module": "nextdocs",
  "action": "document.uploaded",
  "result": "success",
  "actorUserId": "nextid-sub",
  "actorUsername": "username",
  "documentId": "document-id",
  "timestamp": "2026-06-15T00:00:00Z",
  "ipAddress": "127.0.0.1",
  "userAgent": "browser-or-client",
  "details": {}
}
```

## Required fields

| Field | Description |
|---|---|
| eventId | Unique event id |
| module | Always `nextdocs` |
| action | Event action name |
| result | `success`, `failure`, `denied`, `error` |
| actorUserId | NextID `sub` value when available |
| actorUsername | NextID username when available |
| documentId | Affected document id when available |
| timestamp | Event time in UTC |
| ipAddress | Request source IP |
| userAgent | Client user agent |
| details | Extra structured data |

## Actions

```text
document.uploaded
document.viewed
document.downloaded
document.updated
document.deleted
document.archived
document.metadata_changed
document.ocr_completed
document.ocr_failed
document.access_denied
document.shared
document.unshared
document.retention_changed
```

## Result values

```text
success
failure
denied
error
```

## Sensitive data rule

Do not send full document content to NextLog. Send only identifiers, metadata keys, state changes and error summaries.

Allowed:

```json
{
  "oldStatus": "uploaded",
  "newStatus": "archived",
  "documentType": "official-letter"
}
```

Not allowed:

```json
{
  "fullText": "complete OCR text"
}
```

## Next implementation step

Add a small NextDocs audit adapter that can POST this envelope to:

```text
NEXTLOG_AUDIT_URL
```

The adapter must be optional. If NextLog is down, NextDocs must continue running and log the delivery failure locally.

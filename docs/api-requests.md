# API Requests

This doc lists sample API requests used by the publisher connectors.

## Publish request (example)

POST /publish

Payload:

{
  "platform": "linkedin",
  "content": "...",
  "attachments": [ ... ],
  "schedule_at": "2026-06-10T15:00:00Z"
}

Responses:

- 200 OK: { "status": "published", "id": "..." }
- 4xx/5xx: { "status": "failed", "error": "..." }

Include retry semantics and idempotency keys for safe retries.

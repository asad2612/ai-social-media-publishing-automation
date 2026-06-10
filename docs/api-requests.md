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


Include retry semantics and idempotency keys for safe retries.

# API Requests

## 1. Initialize Upload Session
POST https://api.linkedin.com/rest/images?action=initializeUpload

Headers:
- Linkedin-Version: 202601
- X-Restli-Protocol-Version: 2.0.0
- Content-Type: application/json

Body:
{
  "initializeUploadRequest": {
    "owner": "urn:li:organization:107909136"
  }
}

---

## 2. Upload Media
PUT {{uploadUrl}}

Binary body: image file

---

## 3. Publish Post
POST https://api.linkedin.com/rest/posts

Body includes:
- author
- commentary
- media ID
- visibility

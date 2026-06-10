# API Requests

This document describes the external API calls used in the **AI‑Powered Social Media Publishing & Content Operations Workflow**.  
The workflow integrates with **LinkedIn REST API** and uses **Google Sheets** for tracking publishing status.

---

## 1. LinkedIn Media Upload Flow

### 1.1 Initialize Upload Session

**Endpoint:**

POST https://api.linkedin.com/rest/images?action=initializeUpload

**Headers:**
- Linkedin-Version: 202601
- X-Restli-Protocol-Version: 2.0.0
- Content-Type: application/json
- Authorization: Bearer <access_token>

**Body:**
```json
{
  "initializeUploadRequest": {
    "owner": "urn:li:organization:107909136"
  }
}
```

Purpose:
• Creates an upload session for an image.
• Returns:
	◦ uploadUrl — where the binary file must be uploaded.
	◦ image — the media URN used later in the post.

---

### 1.2 Upload Media

**Endpoint:**
PUT {{uploadUrl}}

**Headers:**
• Content-Type: image/png (or appropriate MIME type)
• Authorization: Bearer <access_token>

**Body:**
Binary image data (sent as file/binary).

**Success Response:**
• statusCode: 201 or 2xx
• Empty or minimal metadata.

---

## 2. LinkedIn Post Publishing

### 2.1 Publish Post

**Endpoint:**
POST https://api.linkedin.com/rest/posts

**Headers:**
• Linkedin-Version: 202601
• X-Restli-Protocol-Version: 2.0.0
• Content-Type: application/json
• Authorization: Bearer <access_token>

**Body Example:**
```json
{
  "author": "urn:li:organization:107909136",
  "commentary": "Develop a comprehensive AI roadmap aligned with your business objectives.",
  "visibility": "PUBLIC",
  "distribution": {
    "feedDistribution": "MAIN_FEED",
    "targetEntities": [],
    "thirdPartyDistributionChannels": []
  },
  "content": {
    "media": {
      "altText": "AI readiness assessment visual",
      "id": "urn:li:image:C4D00AAA..."
    }
  },
  "lifecycleState": "PUBLISHED",
  "isReshareDisabledByAuthor": false
}
```

**Success Response:**
```json
{
  "id": "urn:li:share:1234567890"
}
```

---

## 3. Status Validation & Error Handling

### 3.1 Success Criteria
A publish operation is considered successful when: statusCode == 201

When a publish operation succeeds (statusCode == 201), the workflow writes these values back to the corresponding Google Sheets row:

- `status`: "posted"
- `posted_at`: ISO8601 timestamp (e.g., "2026-04-25T10:00:00Z")
- `post_linkedin`: LinkedIn post URN (e.g., "urn:li:share:1234567890")
- `notes`: a short log string, e.g. "Created | 201 | <post_urn>"

Include idempotency keys or checks to avoid double-posting when re-running the workflow.

### 3.2 Failure Handling
If statusCode != 201 or an error occurs:
- `status`: "failed"
- `posted_at`: ISO8601 timestamp of attempt
- `post_linkedin`: empty or partial URN (if available)
- `notes`: full JSON error body

This ensures a complete audit trail for debugging and retries.

---

## 4. Authentication & Idempotency

### 4.1 Authentication
All LinkedIn API calls use OAuth2: `Authorization: Bearer <access_token>`

The workflow assumes:
- Valid organization permissions (the access token must be for a user/admin with permission to post on the organization page)
- Correct scopes for media upload and post publishing (ensure the token includes required LinkedIn scopes)

### 4.2 Idempotency & Retries
LinkedIn does not require explicit idempotency keys for these endpoints, but the workflow is designed to be safe to re-run:

- Each row is matched by `post_id`.
- Re-running updates the same row instead of duplicating entries.

Retries can be configured in n8n for:
- 5xx errors
- Network failures

Use exponential backoff and a maximum retry count to avoid hitting rate limits; for persistent failures write the full error to `notes` and mark `status` = `failed` so the item can be inspected manually.

---

## 5. Optional: Generic Publish API (Not Used in Workflow)

This is a conceptual abstraction for multi-platform publishing.

**Endpoint:**
POST /publish

**Body:**
```json
{
  "platform": "linkedin",
  "content": "Your post caption here...",
  "attachments": [
    {
      "type": "image",
      "url": "https://example.com/image.png"
    }
  ],
  "schedule_at": "2026-06-10T15:00:00Z",
  "idempotency_key": "post-001-2026-06-10T15:00:00Z"
}
```

**Purpose:**
- Unified publishing interface
- Safe retries using `idempotency_key`

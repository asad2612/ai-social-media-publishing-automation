# Data Structure

Example canonical content object:

- id: string (uuid)
- source: string ("editor", "csv", "api")
- author: { name, id }
- brief: string
- generated: {
  - text: string
  - variations: [ string ]
  - metadata: { tone, length, hashtags }
}
- schedule: { publish_at: ISO8601, timezone }
- platform: ["linkedin", "twitter", "facebook"]
- status: "pending" | "scheduled" | "published" | "failed"
- attempts: number
- response: object (platform API response)

Store this shape in a JSON column or document store for simplicity.

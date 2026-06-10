# Prompt Engineering Notes

This workflow is intentionally **deterministic** and does **not** rely on LLM prompting for any core logic.  
All publishing decisions, captions, and media handling are driven by structured data stored in Google Sheets and processed through n8n.

However, a few workflow components still use *lightweight prompt‑like string construction* to ensure clarity, consistency, and human‑readable outputs.

---

## 1. Where Prompt‑Like Logic Is Used

Although no LLMs are involved, the workflow uses templated strings in three areas:

### **1.1 Caption Formatting**
Captions are written and stored directly in Google Sheets.  
n8n simply injects them into the LinkedIn API request:
{{ $node[”[redacted] Calendar”].json[“linkedin_caption”] }}


This ensures:
- No formatting errors  
- No accidental whitespace or broken characters  
- Captions remain fully human‑controlled  

---

### **1.2 Dynamic Alt‑Text Generation**
Alt‑text is generated using simple variable interpolation:
{{ $node[’[redacted] Calendar’].json[‘post_concept’] || ‘Social media image’ }}


This guarantees:
- Every post has alt‑text  
- Fallback text is always present  
- Accessibility standards are respected  

---

### **1.3 Error Logging & Status Notes**
The workflow writes structured log messages back to Google Sheets:
“Created | {{statusCode}} | {{post_urn}}”


or for failures:
{{ JSON.stringify($node[“Publish LinkedIn Post”].json.body) }}


This ensures:
- Logs are machine‑readable  
- Errors can be debugged quickly  
- No ambiguity in what happened during publishing  

---

## 2. Why Prompt Engineering Is Minimal

This workflow is designed to be:
- **Predictable**  
- **Repeatable**  
- **Auditable**  
- **Safe to re-run**  

Because of this, all content is:
- Pre‑written  
- Pre‑approved  
- Stored in Sheets  
- Passed through n8n without modification  

No generative AI is used for:
- Caption creation  
- Content rewriting  
- Image description  
- Decision-making  

This keeps the workflow stable and avoids unexpected output variations.

---

## 3. Templating Model Used in n8n

n8n uses a **Mustache‑style templating syntax** for referencing:
- Node outputs  
- JSON fields  
- Variables  
- Timestamps  

Examples:
{{ $json.post_id }}
{{ $now.toISO() }}
{{ $node[“Publish LinkedIn Post”].json.statusCode }}


This allows:
- Clean variable injection  
- Consistent formatting  
- Zero ambiguity in API payloads  

---

## 4. Safety & Consistency Considerations

Even without LLMs, prompt‑like strings must be handled carefully:

- **Alt‑text must always exist** → fallback text prevents API errors  
- **Logs must be structured** → ensures debugging is easy  
- **Captions must not break JSON** → Sheets content is sanitized  
- **No dynamic rewriting** → prevents accidental content drift  

This ensures the workflow behaves the same way every time it runs.

---

## Summary

While this workflow does not use LLMs, it still applies **lightweight prompt engineering principles** to ensure:

- Clean captions  
- Consistent alt‑text  
- Reliable logs  
- Predictable API payloads  

All content remains fully deterministic, human‑controlled, and audit‑friendly.






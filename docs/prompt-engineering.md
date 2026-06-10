# Prompt Engineering

Guidelines for building robust prompts for content generation:

- Use explicit instructions for format, tone, and length
- Provide examples and constraints (e.g., character limits)
- Ask the model to output JSON when programmatic parsing is required
- Include safety checks and post-processing rules (no PII, no copyrighted content)

Sample prompt:

"Write a LinkedIn post for a B2B SaaS founder announcing a new feature. Keep it under 1300 characters, professional tone, include a call-to-action, and suggest 3 hashtags in a JSON object: {\"text\": ..., \"hashtags\": [...] }"

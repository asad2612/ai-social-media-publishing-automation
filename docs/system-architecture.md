# System Architecture

Components:

- Orchestrator: runs the workflow (serverless function, cron, or service)
- AI Engine: model calls for content generation and rewriting
- Formatter: converts generated content into platform-specific formats
- Publisher: connectors for LinkedIn, Twitter/X, Facebook, etc.
- Storage: content tracker (database, spreadsheet) and logs
- Monitoring: alerts, dashboards, and retries

Deployment: can be deployed as microservices or a serverless pipeline. Use queues (e.g., SQS) for reliability and idempotency.

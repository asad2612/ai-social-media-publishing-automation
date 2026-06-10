
# System Architecture

## Components
- n8n workflow engine
- Google Sheets (content calendar)
- LinkedIn REST API
- Media asset storage (URLs)
- Logging & status tracking

## Data Flow
1. Scheduler → Sheets  
2. Sheets → Filter  
3. Filter → Media Retrieval  
4. Media → LinkedIn Upload Session  
5. Upload Session → Media Upload  
6. Media Upload → Post Publish  
7. Post Publish → Validation  
8. Validation → Sheets Update  

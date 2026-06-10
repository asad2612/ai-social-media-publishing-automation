# Workflow Overview

This workflow automates the publishing of scheduled LinkedIn posts using n8n, Google Sheets, and the LinkedIn REST API.

---

## 1. Weekly Scheduler
Triggers every Monday at 10:00 AM.

---

## 2. Fetch Content Calendar
Reads rows from Google Sheets → "Automation Ready" tab.

---

## 3. Filter Ready‑to‑Publish Posts
Custom JS:

- post_date ≤ today  
- status = "ready"

Only valid posts continue.

---

## 4. Retrieve Media Asset
Downloads the image file from the URL stored in the sheet.

---

## 5. Create LinkedIn Upload Session
Initializes media upload:
POST https://api.linkedin.com/rest/images?action=initializeUpload (api.linkedin.com in Bing)

Returns:
- uploadUrl  
- image URN

---

## 6. Upload Media to LinkedIn
Binary upload via PUT request.

---

## 7. Publish LinkedIn Post
Publishes caption + media:
POST https://api.linkedin.com/rest/posts

---

## 8. Validate Publishing Success
Checks if statusCode = 201.

---

## 9. Update Content Tracker
Writes:
- posted / failed  
- timestamp  
- LinkedIn post URN  
- error logs

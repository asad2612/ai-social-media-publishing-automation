
# AI‑Powered Social Media Publishing & Content Operations Workflow

An end‑to‑end automation system that retrieves scheduled content from Google Sheets, validates publishing readiness, processes media assets, publishes posts to LinkedIn via API, and updates the content tracker with success/failure logs.

This workflow eliminates repetitive manual publishing work and creates a scalable, reliable content operations engine.

---

## Problem

Marketing teams often manage content calendars manually across:
- Google Sheets
- Media folders
- LinkedIn publishing tools
- Status trackers

This creates:
- Repetitive manual work
- Missed publishing deadlines
- Inconsistent posting cadence
- No centralized publishing history

---

## Solution

This automation system handles the entire publishing lifecycle:

1. **Weekly Scheduler**  
	Triggers the workflow every week at a defined time.

2. **Fetch Content Calendar**  
	Reads scheduled posts from Google Sheets.

3. **Filter Ready‑to‑Publish Posts**  
	Custom JS logic checks:
	- Post date ≤ today  
	- Status = “ready”

4. **Retrieve Media Asset**  
	Downloads the image from the URL stored in the sheet.

5. **Create LinkedIn Upload Session**  
	Initializes a media upload via LinkedIn REST API.

6. **Upload Media to LinkedIn**  
	Sends the binary file to LinkedIn’s upload URL.

7. **Publish LinkedIn Post**  
	Posts the caption + media to the organization page.

8. **Validate Publishing Success**  
	Checks if LinkedIn returned `201 Created`.

9. **Update Content Tracker**  
	Writes:
	- Status: posted / failed  
	- Timestamp  
	- LinkedIn post URN  
	- Notes / error logs

---

## Tech Stack

- **n8n** (workflow automation)
- **Google Sheets API**
- **LinkedIn REST API**
- **HTTP Requests**
- **JavaScript (custom logic)**
- **Media asset processing**

---

## Business Impact

- Reduces manual publishing workload  
- Ensures consistent posting cadence  
- Centralizes publishing logs  
- Enables scalable content operations  
- Reduces human error in scheduling and posting  

---

## Repository Contents

- `/workflow` — full JSON export of the n8n workflow  
- `/docs` — architecture, data model, API details  
- `/sample-output` — success & failure examples  
- `/assets` — screenshots and workflow diagram  

---

## Author  
**Asad Ali**  
AI Automation & GTM Systems  

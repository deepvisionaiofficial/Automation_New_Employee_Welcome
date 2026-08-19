# New Employee Onboarding Notification System

An **n8n workflow automation** that detects new employee entries in a Google Sheet and automatically:

- Sends a personalized welcome email to the new employee
- Copies and shares an onboarding document with them
- Notifies the employee's manager and team lead
- Posts a team announcement on Slack (optional)

Built as part of an internship/project task under **DeepVision.ai**.

---

## Workflow Preview

![n8n Workflow Screenshot](assets/n8n-workflow-screenshot.png)

---

## How It Works

```
Google Sheets (new row added)
        │
        ▼
  Format Employee Data
        │
   ┌────┴─────┐
   ▼          ▼
Copy Doc   Notify Manager
   │          & Lead (Email)
   ▼          │
Share Doc      ▼
   │        Slack Announcement
   ▼        (optional)
Send Welcome
Email
```

1. **Trigger:** A Google Sheets trigger node polls a sheet and fires when a new employee row is added.
2. **Format:** Employee fields (Name, Email, Department, Manager Email, Lead Email, Start Date) are mapped into clean variables.
3. **Onboarding Doc:** A master onboarding document template (Google Drive) is copied and shared directly with the new employee.
4. **Welcome Email:** The employee receives a welcome email with a link to their onboarding document.
5. **Manager/Lead Notification:** The employee's manager and team lead are emailed with the new hire's details.
6. **Slack Announcement (optional):** A message is posted to a team channel announcing the new hire.

---

## Tech Stack

| Tool | Purpose |
|---|---|
| [n8n](https://n8n.io) | Workflow automation engine |
| Google Sheets | Employee data source / trigger |
| Google Drive | Onboarding document storage & sharing |
| Gmail | Sending welcome & notification emails |
| Slack (optional) | Team announcements |

---

## Repository Structure

```
employee-onboarding-automation/
├── README.md                      # This file
├── GUIDE.md                       # Step-by-step setup guide
├── DOCUMENTATION.md               # Technical documentation of each node
├── new_employee_onboarding.json   # Importable n8n workflow
├── onboarding-doc-template.md     # Sample onboarding document content
└── assets/
    └── n8n-workflow-screenshot.png
```

---

## Quick Start

1. Import `new_employee_onboarding.json` into your n8n instance
2. Set up a Google Sheet with the required columns (see [GUIDE.md](GUIDE.md))
3. Connect Google Sheets, Google Drive, and Gmail credentials
4. Activate the workflow

Full setup instructions: **[GUIDE.md](GUIDE.md)**
Technical/node-level details: **[DOCUMENTATION.md](DOCUMENTATION.md)**

---

## Project Credit

Built by **Abdul Wahab Shakeel**
Project assigned under **DeepVision.ai**

---

## License

This project is for educational/internship purposes.

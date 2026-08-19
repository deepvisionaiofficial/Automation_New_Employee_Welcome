# Technical Documentation

Detailed breakdown of every node in `new_employee_onboarding.json`.

## 1. New Employee Row Added (Google Sheets Trigger)

- **Type:** `n8n-nodes-base.googleSheetsTrigger`
- **Trigger event:** `rowAdded`
- **Poll interval:** every minute
- **Input:** Google Sheet with columns `Name, Email, Department, ManagerEmail, LeadEmail, StartDate`
- **Output:** raw row data as JSON

## 2. Format Employee Data

- **Type:** `n8n-nodes-base.set`
- **Purpose:** Maps raw sheet columns into clean, consistently named variables used by every downstream node:
  - `employeeName`
  - `employeeEmail`
  - `department`
  - `managerEmail`
  - `leadEmail`
  - `startDate`

## 3. Copy Onboarding Doc Template

- **Type:** `n8n-nodes-base.googleDrive`
- **Operation:** `copy`
- **Input:** `fileId` of the master onboarding doc template
- **Output name:** `Onboarding Doc - {{employeeName}}`
- **Purpose:** Creates a unique copy of the onboarding doc for each new employee.

## 4. Share Doc With Employee

- **Type:** `n8n-nodes-base.googleDrive`
- **Operation:** `share`
- **Permission:** `reader` access granted to the employee's email address
- **Purpose:** Gives the new employee direct access to their onboarding doc copy.

## 5. Send Welcome Email to Employee

- **Type:** `n8n-nodes-base.gmail`
- **Recipient:** `employeeEmail`
- **Content:** HTML welcome message referencing department and start date, informing them their onboarding doc has been shared.

## 6. Notify Manager and Lead

- **Type:** `n8n-nodes-base.gmail`
- **Recipients:** `managerEmail`, `leadEmail`
- **Content:** HTML notification with the new hire's name, department, start date, and email — prompting the manager/lead to prepare access and a first-day plan.

## 7. Slack Team Announcement (Optional)

- **Type:** `n8n-nodes-base.slack`
- **Channel:** `#hr-updates` (configurable)
- **Content:** Short announcement message introducing the new hire to the team.
- **Note:** This node can be safely deleted if Slack isn't used.

---

## Data Flow Diagram

```
Trigger → Format Data ──┬──> Copy Doc → Share Doc → Welcome Email
                         └──> Notify Manager/Lead → Slack Announcement
```

## Credentials Required

| Service | Used By | Scopes Needed |
|---|---|---|
| Google Sheets | Trigger node | Read access to target sheet |
| Google Drive | Copy/Share nodes | Read + write + share permissions |
| Gmail | Both email nodes | Send email |
| Slack | Announcement node | `chat:write` |

## Extending This Workflow

Ideas for future iterations:
- Add a Slack DM to the new employee alongside the manager's channel post
- Auto-create IT/system access tickets (e.g. Jira, Asana) when a new row appears
- Add a follow-up reminder email a few days before the start date
- Log every onboarding event to a separate "Audit" sheet for HR records

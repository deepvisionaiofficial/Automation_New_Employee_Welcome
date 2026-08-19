# Setup Guide

This guide walks you through setting up the New Employee Onboarding Notification workflow from scratch in n8n.

## 1. Create an n8n Account

Sign up at [n8n.io](https://n8n.io) (cloud) or self-host via Docker. Once logged in, you'll land on the workflows dashboard.

## 2. Create the Google Sheet

Create a new Google Sheet named **Employee Onboarding Tracker**, with the tab named `Sheet1`, and the following column headers in row 1:

| Name | Email | Department | ManagerEmail | LeadEmail | StartDate |
|---|---|---|---|---|---|

Add a test row to try the workflow later, e.g.:

```
Ali Khan | ali.khan@gmail.com | IT | manager@company.com | lead@company.com | 20/08/2026
```

Copy the **Sheet ID** from the URL (the long string between `/d/` and `/edit`).

## 3. Create the Onboarding Document

Create a Google Doc named **Employee Onboarding Guide** and paste in the content from [`onboarding-doc-template.md`](onboarding-doc-template.md).

Copy the **File ID** from the doc's URL (same pattern as the Sheet ID).

## 4. Import the Workflow into n8n

1. Open a **blank/new workflow** in n8n
2. Copy the full contents of `new_employee_onboarding.json`
3. Click on the empty canvas and paste (`Ctrl+V` / `Cmd+V`)

If your n8n version has an explicit import option, it's usually under the **three-dot menu (⋮)** on the canvas, or **Workflows list → Add workflow dropdown → Import from File**.

## 5. Connect Credentials

Each node with a red warning triangle needs a connected account:

- **Google Sheets Trigger** → Create New Credential → sign in with Google → select your sheet in the "Document" field, and `Sheet1` in the "Sheet" field
- **Copy Onboarding Doc Template** (Google Drive) → same Google account → select your onboarding doc in the "File" field
- **Share Doc With Employee** (Google Drive) → reuse the same Drive credential
- **Send Welcome Email to Employee** (Gmail) → Create New Credential → sign in with Gmail
- **Notify Manager and Lead** (Gmail) → reuse the same Gmail credential
- **Slack Team Announcement** (optional) → connect Slack credential, or delete this node if not needed

> Use the **same Google account** for Sheets, Drive, and Gmail to avoid permission issues.

## 6. Test the Workflow

1. Add a new row to your Google Sheet
2. In n8n, click **Execute Workflow** to run it manually
3. Confirm each node shows a green checkmark
4. Check your inbox for the welcome email and manager notification

## 7. Activate

Toggle the **Active** switch (top-right of the workflow editor) to turn on automatic execution — the workflow will now trigger automatically whenever a new row is added (checked roughly every minute).

---

## Troubleshooting

| Issue | Fix |
|---|---|
| Node shows red warning | Credential not connected — open the node and connect/select an account |
| No email received | Check Gmail credential scope and spam folder |
| Sheet not found in dropdown | Make sure the Sheet ID is correct and the Google account has access |
| Doc not sharing | Ensure the Drive file ID is correct and belongs to the connected account |

# n8n Automation Portfolio

Hi, I'm Yaroslav Dudka, an aspiring automation developer based in Kyiv, Ukraine.

This repository contains three personal projects I built in n8n to practise webhooks, data validation, API integrations, and conditional logic. I enjoy connecting tools to make everyday tasks easier to manage.

| Project | What it does | Main tools |
| --- | --- | --- |
| [Lead Processing](lead-processing.json) | Validates incoming leads, checks for duplicates, saves them, and sends a notification | Webhooks, Google Sheets, Telegram |
| [Mini Brain AI](mini-brain-ai.json) | Helps manage notes and calendar events through Telegram | Gemini, Google Calendar, n8n Data Tables |
| [Gmail Outreach CRM](gmail-outreach-crm.json) | Logs sent emails and keeps contact dates up to date | Gmail, Google Sheets, JavaScript |

These are sanitized portfolio exports. Credentials and private account/resource references have been removed or replaced with `YOUR_...` placeholders. Each workflow is inactive and needs setup before it can run. Node labels and most user-facing messages are in Ukrainian; date formatting uses `Europe/Kyiv`.

## 1. Lead Processing

**File:** [lead-processing.json](lead-processing.json)

Receives enquiries through a POST webhook, validates required fields, classifies lead priority by budget, checks for an existing email address, saves new leads to Google Sheets, and sends a Telegram notification. Webhook responses distinguish invalid, duplicate, and accepted enquiries.

**Tools:** n8n, Webhooks, Google Sheets, Telegram.

**Setup:**

- Select your Google Sheets, Telegram, and webhook Header Auth credentials in the relevant nodes. Keep webhook authentication enabled.
- Replace `YOUR_WEBHOOK_PATH` with your own webhook path and `YOUR_TELEGRAM_CHAT_ID` with the notification recipient's chat ID.
- In both Google Sheets nodes, select the same spreadsheet and its leads tab in place of `YOUR_GOOGLE_SHEET_ID` and `YOUR_LEADS_TAB_ID`.
- The leads tab needs these column headers: `lead_id`, `created_at`, `name`, `email`, `service`, `budget_usd`, `source`, `lead_priority`, `status`.

Example test request body with fictitious data:

```json
{
  "name": "Demo Customer",
  "email": "demo@example.com",
  "service": "Air conditioning maintenance",
  "budget_usd": 500,
  "source": "portfolio-demo"
}
```

## 2. Mini Brain AI

**File:** [mini-brain-ai.json](mini-brain-ai.json)

A private Telegram assistant that uses Gemini to interpret requests. It supports saving, listing, and searching notes; creating and listing Google Calendar events; asking for missing dates or times; and storing the state of an unfinished request in an n8n Data Table. A separate model chain handles general chat.

**Tools:** n8n, Telegram, Google Gemini, Google Calendar, n8n Data Tables.

**Setup:**

- Select your Telegram, Gemini, and Google Calendar credentials in all relevant nodes.
- In the `Перевірка власника` (owner check) node, replace `YOUR_TELEGRAM_USER_ID` inside `Number('YOUR_TELEGRAM_USER_ID')` with your numeric Telegram user ID. Leave the private-chat check in place. Until configured, this check rejects access.
- Select your calendar in both calendar nodes in place of `YOUR_GOOGLE_CALENDAR_ID`.
- Create a notes Data Table with `content` (string), `category` (string), and `telegramUserId` (number). Select it wherever `YOUR_NOTES_TABLE_ID` appears.
- Create a memory Data Table with `telegramUserId` (number), `originalMessage` (string), and `intent` (string). Select it wherever `YOUR_MEMORY_TABLE_ID` appears.
- The model selection is preserved from the source export. Select a Gemini model available to your account if needed.

## 3. Gmail Outreach CRM

**File:** [gmail-outreach-crm.json](gmail-outreach-crm.json)

Runs every 15 minutes, reads sent Gmail messages since the saved synchronization checkpoint, extracts recipient addresses and domains, and updates a Google Sheets contact log. Contacts are matched by email; first/last contact dates are updated while existing company names and statuses are retained. A separate sheet tab stores the synchronization checkpoint.

This workflow tracks messages that have already been sent. It does not send outreach emails.

**Tools:** n8n, Gmail, Google Sheets, JavaScript.

**Setup:**

- Select Gmail and Google Sheets credentials in the relevant nodes.
- Select your spreadsheet in place of `YOUR_GOOGLE_SHEET_ID` in every Sheets node.
- Select the contacts tab wherever `YOUR_CONTACTS_TAB_ID` appears. It needs these headers: `Company`, `Email`, `Domain`, `First Contact`, `Last Contact`, `Status`.
- Select the checkpoint tab wherever `YOUR_SYNC_STATE_TAB_ID` appears. It needs `Key` and `LastProcessedAt` columns.
- Add an initial checkpoint row with `Key` set to `outreach` and `LastProcessedAt` set to the ISO date/time from which you want to start processing, for example `2026-01-01T00:00:00.000Z`.

## Importing a workflow

1. Create a new workflow in n8n.
2. Open the workflow menu and choose **Import from File**.
3. Select one of the JSON files in this repository.
4. Complete the setup for that workflow. Use the resource pickers to select your own spreadsheets, tabs, calendar, and Data Tables instead of the placeholder values.
5. Test with your own test accounts and fictitious data, then activate when configured.

Only enter real secrets through n8n credentials. Do not put passwords, API keys, or bot tokens in this repository.

## Contact

[Yaroslav Dudka on LinkedIn](https://www.linkedin.com/in/yaroslav-dudka-702b72447/)

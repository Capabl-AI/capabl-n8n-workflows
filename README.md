## n8n Workflows

Short description:
- **Purpose**: This repository stores exported n8n workflows used by Capabl for lead capture, LinkedIn automation, and support automation.
- **How to use**: Import the JSON files into your n8n instance, configure credentials, and test triggers.


Per-workflow summary:
- **Business Card Reader**: `Business Card Reader/capabl-business-card-reader.json`
  - Purpose: Accept images (Telegram), extract business card fields via an AI model, and append parsed results to Google Sheets.
  - Key nodes: Telegram trigger, image downloader, AI extraction node, code node for JSON sanitization, Google Sheets append.
  - Typical credentials: Telegram, Google Sheets, (Groq/OpenAI) model API.

- **LinkedIn - Blog Post Creation and Notification**: `Linkedin/Blog Post Creation and Notification/linkedin-blog-automation.json`
  - Purpose: Scrape blog pages, generate LinkedIn post drafts via AI, create media folders, and record drafts to Google Sheets for approval.
  - Key nodes: HTTP/HTML extraction, AI agent node, Google Drive (folders), Google Sheets, optional Telegram approvals.
  - Typical credentials: Google Sheets, Google Drive, Telegram, model API.

- **LinkedIn - Upload**: `Linkedin/Linkedin Upload/linkedin-upload.json`
  - Purpose: For posts flagged as ready with media, upload videos to LinkedIn and update publish status in Google Sheets.
  - Key nodes: Google Sheets filter, LinkedIn upload node, sheet update.
  - Typical credentials: Google Sheets, LinkedIn (upload API), Google Drive.

- **Support Automation - Mail**: `Support Automation/Mail/support-automation-mail.json`
  - Purpose: Inbound support email handling — classify, extract structured data (transfer/refund), update sheets, and send transactional emails.
  - Key nodes: Email trigger (Gmail), classification (AI), information-extractor/code nodes, Google Sheets, outgoing mail nodes.
  - Typical credentials: Gmail OAuth2, Google Sheets, model API.

- **Support Automation - WhatsApp (chat flows)**: `Support Automation/WhatsApp/support-automation-chat.json`
  - Purpose: WhatsApp-triggered support chat flows for seat transfers, refunds, confirmations, admin notifications and sheet updates.
  - Key nodes: WhatsApp trigger, AI classifier, information extractor, Google Sheets append/update, Telegram admin notifications, outbound WhatsApp messages.
  - Typical credentials: WhatsApp API, Google Sheets, Gmail (for email notifications), Telegram, model API.

Common credentials to configure in n8n:
- **Gmail OAuth2**: used by support mail/chat flows (often named `Support Gmail`).
- **Google Sheets OAuth2**: used by multiple workflows (sheet names vary: `Support Google Account`, `Pratik Kothari Google Sheets`, etc.).
- **Google Drive / Drive folders**: for media storage used by LinkedIn workflows.
- **Model API (Groq/OpenAI/etc.)**: used by AI nodes for classification and extraction.
- **Telegram**: used for approvals/notifications.
- **WhatsApp**: used by support chat flows.
- **LinkedIn**: used by upload workflow.

How to import and test (quick steps):
1. Open your n8n editor UI and choose "Import" → "Paste JSON" or "Import from file" and select the workflow JSON.
2. Configure the credentials referenced in the workflow nodes (Gmail, Google Sheets, Drive, WhatsApp, Telegram, LinkedIn, model API).
3. Test triggers:
   - For trigger-based flows (Gmail/WhatsApp/Telegram) send a test message to the linked account or run the trigger node's "Execute Node" in n8n.
   - For time/schedule flows, set a temporary manual trigger or run nodes manually for testing.
4. Observe executions in the n8n Execution log and verify that Google Sheets rows are appended/updated and outbound notifications are delivered.


Troubleshooting:
- If a node fails due to credentials, re-create or re-connect the credential in n8n rather than editing the JSON directly.
- For parsing errors, inspect the execution output of intermediate nodes (AI output, code node) to locate mismatch between expected and actual model outputs.
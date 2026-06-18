# automation-intake-router

An n8n workflow that handles client intake end-to-end from a single form submission — no manual steps.

When a prospect fills out the intake form, the workflow saves their details to Google Sheets, sends them a confirmation email with a summary of what they submitted, and sends the agency an internal notification with the full enquiry. All three happen in sequence, automatically.

---

## What it does

1. **Form trigger** — n8n's native form collects Name, Email, Business Type, and a description of what the client wants automated
2. **Google Sheets** — appends a new row with the submission data and a timestamp
3. **Client confirmation** — sends a personalised HTML email to the client summarising their submission
4. **Agency notification** — sends an internal HTML email to the agency with the full enquiry details

---

## Stack

- [n8n](https://n8n.io) (self-hosted)
- Google Sheets (via OAuth2)
- Gmail (via OAuth2)

---

## Setup

### Prerequisites

- A self-hosted n8n instance
- A Google account with access to Google Sheets and Gmail
- OAuth2 credentials configured in n8n for both Google Sheets and Gmail

### Steps

1. Import `workflow.json` into your n8n instance via **Workflows → Import from file**
2. Connect your Google Sheets OAuth2 credential to the **Append row in sheet** node
3. Connect your Gmail OAuth2 credential to both email nodes (**Send a message** and **Send a message1**)
4. In the **Send a message1** node, replace `your-email@example.com` with the agency's actual notification email
5. Point the Google Sheets node at your own spreadsheet — update the document URL in the node settings
6. Set up the spreadsheet with these column headers in row 1: `Name`, `Email`, `Business Type`, `What You Want Automated`, `Submitted At`
7. Activate the workflow — n8n will generate a live form URL

---

## Notes

- The workflow runs sequentially: Sheets append → client email → agency email. If the Sheets step fails, the emails will not send.
- Credentials are not included in the exported JSON. You will need to reconnect them after import — this is expected behaviour for n8n credential exports.
- The internal notification email pulls data from the Sheets node output, so the agency always receives exactly what was saved to the spreadsheet.

---

## Status

Working. Built and tested locally on a self-hosted n8n instance.

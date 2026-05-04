# AI Sales Development Team (n8n + Apollo + AI Agents)

A production-style AI Sales Development workflow that automates the early SDR process: lead intake, company research, enrichment, AI qualification, routing, outreach draft generation, and CRM logging.

This project is built as a portfolio-ready n8n automation showing how AI agents, Apollo enrichment, web research, Slack approvals, Gmail drafts, and Google Sheets can work together in one end-to-end sales workflow.

## What It Does

When a new lead submits a form, the workflow:

1. Receives lead data from a form/CRM webhook
2. Uses an AI research agent to identify the official company website and domain
3. Escalates to a human in Slack if the company match confidence is low
4. Enriches the company profile using the Apollo API
5. Passes enriched data into an AI qualification agent
6. Scores the lead and classifies it as high, moderate, or low quality
7. Routes leads based on quality
8. Generates a personalized outreach draft for high-priority leads
9. Notifies the sales team in Slack
10. Logs enriched lead data and outreach status into Google Sheets

## Why This Matters

Sales teams often lose time on repetitive lead research, enrichment, qualification, and first-draft outreach. This workflow reduces that manual work while keeping human review in the loop when data confidence is low.

## Architecture

```text
GHL/Form Webhook
→ AI Lead Research Agent
→ Confidence Check
→ Human Approval in Slack if needed
→ Apollo Company Enrichment
→ AI Lead Qualification Agent
→ Lead Quality Routing
→ Outreach Draft Agent
→ Gmail Draft + Slack Alert
→ CRM / Google Sheets Log
```

## Tech Stack

- **n8n** — workflow orchestration
- **Apollo API** — company enrichment
- **OpenRouter / LLMs** — AI research, qualification, and outreach generation
- **Tavily Search API** — web research tool for AI agents
- **Slack** — human-in-the-loop approval and sales alerts
- **Gmail** — outreach draft creation
- **Google Sheets** — CRM-style logging
- **GoHighLevel / Form Webhook** — lead intake source

## Key Features

- AI lead research agent
- Apollo company enrichment
- Human-in-the-loop validation for low-confidence company matches
- AI lead scoring and qualification
- Structured JSON outputs for reliable downstream automation
- Lead routing by quality level
- Personalized email draft generation
- Slack alerts for sales team visibility
- CRM-style logging in Google Sheets

## Workflow File

The sanitized workflow export is located here:

```text
workflows/ai-sales-development-team.sanitized.json
```

## Required Setup

Before importing into n8n, configure credentials for:

- Apollo API
- OpenRouter or your preferred LLM provider
- Tavily Search API or another web search API
- Slack
- Gmail
- Google Sheets
- Your CRM/form webhook source

Then update placeholder values such as:

```text
YOUR_APOLLO_API_KEY
YOUR_TAVILY_API_KEY
YOUR_GOOGLE_SHEET_ID
YOUR_N8N_WEBHOOK_URL
YOUR_CREDENTIAL_ID
YOUR_CREDENTIAL_NAME
```

## Notes on Security

This repository uses a sanitized workflow export. API keys, credentials, webhook URLs, pinned test data, and instance metadata have been removed or replaced with placeholders.

Do not commit live credentials, personal data, client data, webhook URLs, or internal CRM IDs.

## Use Cases

- B2B lead qualification
- Agency sales development workflows
- AI-assisted outbound sales
- Lead enrichment and routing
- Sales team triage
- CRM automation
- High-priority lead alerting

## Future Improvements

- Add CRM writeback to HubSpot or GoHighLevel
- Add retry handling for enrichment failures
- Add duplicate lead detection
- Add enrichment cost tracking
- Add reporting dashboard for lead quality and conversion rate
- Add approval-based outbound sending instead of draft-only mode

## Author

Built as a portfolio automation by Eljon Mateo, AI Automation Specialist.

# AI Sales Development Team
### n8n · Apollo · AI Agents · React Dashboard

A production-style AI Sales Development system that automates the early SDR process — lead intake, company research, enrichment, AI qualification, routing, outreach draft generation, CRM logging, and live pipeline monitoring via a custom React dashboard.

Built as a portfolio-ready project showing how AI agents, Apollo enrichment, web research, Slack approvals, Gmail drafts, Google Sheets, and a real-time frontend can work together in one end-to-end sales workflow.

---

## What It Does

When a new lead submits a form, the workflow:

1. Receives lead data from a form/CRM webhook
2. Uses an AI research agent to identify the official company website and domain
3. Escalates to a human in Slack if the company match confidence is low
4. Enriches the company profile using the Apollo API
5. Passes enriched data into an AI qualification agent
6. Scores the lead 1–10 and classifies it as High, Moderate, or Low quality
7. Routes leads to three dedicated Slack channels based on quality tier
8. Generates a personalized outreach email draft for high-priority leads
9. Saves the draft to Gmail, notifies the sales team in Slack
10. Logs enriched lead data and outreach status into Google Sheets
11. Surfaces all pipeline activity in a live React monitoring dashboard

---

## Dashboard

The pipeline writes every lead outcome to a Google Sheets CRM. A separate React dashboard reads from that same sheet and makes the pipeline's decisions visible in real time.

### What the Dashboard Shows

| Section | Details |
|---|---|
| **Metric cards** | Total leads · High / Moderate / Low counts · Draft rate % |
| **Score distribution** | Bar chart — lead counts by score bucket (1–3, 4–6, 7–10) |
| **Quality breakdown** | Donut chart — High / Moderate / Low split by percentage |
| **Draft conversion** | Bar chart by industry — drafted vs not drafted |
| **Pipeline routing** | 3-lane visualization mapped to Slack channels with live counts |
| **Lead table** | Filterable by quality tier · Sortable by score · AI reasoning per row |

### Dashboard Tech Stack

- **React + Vite** — frontend framework
- **Recharts** — all charts
- **Tailwind CSS** — styling
- **Google Sheets API v4** — live data source (read-only)

### Dashboard Features

- **Live / Mock toggle** — switch between live Google Sheets data and mock data for demos
- **CSV export** — downloads the currently filtered view as a `.csv` file
- **Search bar** — real-time company name filtering
- **Lead detail drawer** — click any row to see the full lead profile, AI reasoning, and draft status
- **Score trend chart** — average lead score over time (requires multi-day data)
- **Auto-refresh** — re-fetches live data every 60 seconds when enabled
- **Graceful fallback** — if the Google Sheets fetch fails, the dashboard falls back to mock data silently

### Running the Dashboard

```bash
cd dashboard
npm install
cp .env.example .env
# Add your Google Sheets API key to .env
npm run dev
```

Required `.env` variable:

```
VITE_GOOGLE_SHEETS_API_KEY=your_key_here
```

> The Google Sheets API key should be restricted to HTTP referrers (your localhost or deployed domain) and scoped to the Sheets API only. The spreadsheet must be set to **Anyone with the link → Viewer** access.

---

## Architecture

```
GHL/Form Webhook
  → AI Lead Research Agent (Tavily web search)
  → Confidence Check
    → [Low confidence] Human Approval in Slack
  → Apollo Company Enrichment
  → AI Lead Qualification Agent (score 1–10, structured JSON)
  → Lead Quality Routing (Switch node)
    → [High 7–10]    Outreach Draft Agent → Gmail Draft + Slack Alert → CRM Log
    → [Moderate 4–6] Slack Alert → CRM Log
    → [Low 1–3]      Slack Alert → CRM Log
  → Google Sheets CRM
  → React Dashboard (reads from Google Sheets)
```

---

## Tech Stack

| Layer | Tools |
|---|---|
| Workflow orchestration | n8n |
| Lead intake | GoHighLevel / form webhook |
| Company enrichment | Apollo API |
| Web research | Tavily Search API |
| AI agents | OpenRouter (Grok 4.1 Fast) |
| Human-in-the-loop | Slack |
| Outreach | Gmail (draft creation) |
| CRM logging | Google Sheets |
| Dashboard frontend | React + Vite + Recharts + Tailwind |
| Dashboard data source | Google Sheets API v4 |

---

## Key Features

**Pipeline**
- AI lead research agent with web search
- Apollo company enrichment (industry, headcount, LinkedIn, description)
- Human-in-the-loop Slack approval for low-confidence company matches
- AI lead scoring and qualification across 5 dimensions
- Structured JSON outputs on every agent — reliable downstream automation
- Lead routing by quality tier into 3 dedicated Slack channels
- Personalized cold email draft generation (80–120 words, subject + body)
- CRM-style logging in Google Sheets

**Dashboard**
- Live data from the same Google Sheets CRM the pipeline writes to
- No separate database, no export step, no manual refresh
- Mock/Live toggle for demos and presentations
- CSV export of any filtered view
- Lead detail drawer with full AI reasoning per lead
- Color-coded quality badges consistent across all charts and table

---

## Project Structure

```
/
├── workflows/
│   └── ai-sales-development-team.sanitized.json   # n8n workflow export
├── dashboard/
│   ├── src/
│   │   ├── components/                             # Metric cards, charts, table, drawer
│   │   ├── data/                                   # Mock data + Google Sheets fetch logic
│   │   └── App.jsx                                 # Main dashboard layout
│   ├── .env.example
│   └── package.json
└── README.md
```

---

## Required Setup

### n8n Workflow

Before importing into n8n, configure credentials for:

- Apollo API
- OpenRouter (or your preferred LLM provider)
- Tavily Search API
- Slack
- Gmail
- Google Sheets
- Your CRM / form webhook source

Then replace placeholder values in the workflow:

```
YOUR_APOLLO_API_KEY
YOUR_TAVILY_API_KEY
YOUR_GOOGLE_SHEET_ID
YOUR_N8N_WEBHOOK_URL
YOUR_CREDENTIAL_ID
YOUR_CREDENTIAL_NAME
```

### Dashboard

1. Make the Google Sheet public: **Share → Anyone with the link → Viewer**
2. Create a Google Cloud project, enable the Sheets API, and generate an API key
3. Restrict the key to HTTP referrers and the Sheets API only
4. Add the key to `dashboard/.env` as `VITE_GOOGLE_SHEETS_API_KEY`
5. Update the spreadsheet ID in `dashboard/src/data/sheets.js`

---

## Notes on Security

This repository uses a sanitized workflow export. API keys, credentials, webhook URLs, pinned test data, and instance metadata have been removed or replaced with placeholders.

**Do not commit:**
- Live API keys or credentials
- Personal or client data
- Webhook URLs
- Internal CRM IDs
- `.env` files (already in `.gitignore`)

---

## Use Cases

- B2B lead qualification and routing
- Agency sales development workflows
- AI-assisted outbound sales
- Lead enrichment pipelines
- Sales team triage and prioritization
- CRM automation
- Pipeline health monitoring via dashboard

---

## Future Improvements

- [ ] CRM writeback to HubSpot or GoHighLevel
- [ ] Retry handling for Apollo enrichment failures
- [ ] Duplicate lead detection before enrichment
- [ ] Enrichment cost tracking per lead
- [ ] Dashboard: deploy to Vercel with OAuth for team access
- [ ] Dashboard: email / Slack digest of weekly pipeline summary
- [ ] Approval-based outbound sending instead of draft-only mode

---

## Author

Built by **Eljon Mateo** — AI Automation Specialist

- Portfolio: [eljonmateo.dev](https://eljonmateo.dev)
- GitHub: [github.com/Ejjmateo](https://github.com/Ejjmateo)
- Email: mateoeljon@gmail.com

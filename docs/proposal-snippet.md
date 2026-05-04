# Proposal Snippet

I recently built an AI Sales Development workflow that automates the early SDR process end-to-end. Leads enter through a CRM/form webhook, an AI agent verifies the company domain, Apollo enriches the company data, another AI agent scores and qualifies the lead, and the system routes it based on quality.

High-quality leads trigger a personalized outreach draft and Slack alert, while moderate and low-quality leads are routed into nurturing or lower-priority follow-up. I also added human-in-the-loop validation when the AI is not confident about the company match, so the workflow stays reliable instead of blindly enriching the wrong company.

Tech stack: n8n, Apollo API, OpenRouter/LLMs, Tavily Search, Slack, Gmail, Google Sheets, webhooks.

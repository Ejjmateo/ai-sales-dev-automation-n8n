# Technical Write-Up: AI Sales Development Team

## Overview

The AI Sales Development Team is an n8n-based automation system designed to reduce the manual work involved in early-stage sales development. It automates lead research, company enrichment, lead qualification, routing, and first-draft outreach preparation.

The system is designed around a real SDR workflow: leads come in from a form or CRM, the company is researched and enriched, AI evaluates the lead quality, and the right next action is triggered based on score and fit.

## Problem

Manual sales development work often involves repetitive steps:

- Identifying the correct company domain
- Looking up company information
- Enriching firmographic data
- Deciding whether the lead is worth pursuing
- Drafting an outreach email
- Alerting the sales team
- Logging everything into a CRM

This creates delays and inconsistent handling, especially when lead volume increases.

## Solution

This workflow turns the SDR process into a structured automation pipeline.

The system receives new lead data from a form or CRM webhook, uses an AI research agent to confirm the company domain, enriches the company profile using Apollo, qualifies the lead using AI, and routes the lead based on quality.

High-quality leads trigger personalized outreach draft generation and Slack alerts. Moderate and low-quality leads are routed to separate channels for nurturing or lower-priority follow-up.

## Workflow Breakdown

### 1. Lead Intake

The workflow starts with a webhook that receives lead data from a form or CRM source such as GoHighLevel.

Typical input fields include:

- Company name
- Website
- Contact name
- Email
- Form source

### 2. AI Lead Research

An AI research agent identifies the official company domain based on the submitted company name, website, and email. It can use a web search tool when additional verification is needed.

The agent returns structured output:

```json
{
  "company_name": "Example Company",
  "company_domain": "example.com",
  "company_website": "https://example.com",
  "confidence": "high"
}
```

### 3. Human-in-the-Loop Validation

If the AI is not confident about the company match, the workflow sends the details to Slack for human approval before continuing.

This prevents incorrect enrichment and keeps the system reliable when inputs are incomplete or ambiguous.

### 4. Apollo Enrichment

Once the company domain is confirmed, the workflow calls Apollo's company enrichment API.

Apollo returns firmographic data such as:

- Company name
- Industry
- Short description
- Domain
- Estimated employee count
- Address
- LinkedIn URL

This enriched data becomes the input for the qualification agent.

### 5. AI Lead Qualification

The qualification agent evaluates the lead based on:

- Company size
- Industry relevance
- Decision-making authority
- Growth potential
- Fit for AI automation or workflow automation services

It returns a structured score and recommendation:

```json
{
  "lead_score": 8,
  "lead_quality": "High",
  "reasoning": "The company is a good fit for workflow automation based on size and industry.",
  "recommended_action": "Generate personalized outreach and notify sales team."
}
```

### 6. Lead Routing

A switch node routes the lead based on quality:

- High-quality leads go to personalized outreach generation
- Moderate leads go to nurturing
- Low-quality leads go to low-priority follow-up

This helps sales teams focus on the right opportunities first.

### 7. Outreach Draft Generation

For high-priority leads, an AI outreach agent creates a short, personalized email draft based on enriched company data and qualification reasoning.

The workflow creates a Gmail draft instead of sending automatically, keeping a human approval step before outbound communication.

### 8. Slack Alerts and CRM Logging

The workflow notifies the sales team in Slack and logs the enriched lead data into Google Sheets for tracking.

## Reliability Considerations

The workflow includes several production-minded design choices:

- Structured JSON output parsers for AI responses
- Human approval when company match confidence is low
- Routing based on lead score and quality
- Draft creation instead of auto-send for safer outbound execution
- Clean data mapping before CRM logging

## Business Impact

This system reduces manual SDR work by automating repetitive research, enrichment, scoring, and outreach preparation.

It helps teams:

- Respond to leads faster
- Prioritize high-value opportunities
- Reduce research time
- Keep CRM data structured
- Improve consistency in lead qualification
- Maintain human oversight for uncertain cases

## Suggested Demo Talking Points

When presenting this workflow, emphasize:

1. It starts from a real lead intake flow.
2. AI does not operate blindly; confidence checks and human review are included.
3. Apollo enriches lead data automatically.
4. AI qualification uses structured scoring.
5. The workflow routes leads by priority.
6. High-quality leads generate outreach drafts and Slack alerts.

## Tech Stack

- n8n
- Apollo API
- OpenRouter / LLMs
- Tavily Search API
- Slack
- Gmail
- Google Sheets
- GoHighLevel or webhook-based lead source

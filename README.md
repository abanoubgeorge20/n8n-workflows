# AI Marketing Agents — n8n Workflows

A multi-agent marketing system built with **n8n** and **Google Gemini**. A central orchestrator turns one campaign brief into strategy, market research, content, SEO, paid ads, social publishing, lead nurturing and weekly performance learning, with each agent also usable on its own through a form.

![n8n](https://img.shields.io/badge/n8n-workflows-EA4B71?logo=n8n&logoColor=white)
![Gemini](https://img.shields.io/badge/AI-Google%20Gemini-4285F4?logo=googlegemini&logoColor=white)
![Workflows](https://img.shields.io/badge/workflows-11-blue)

![Marketing Command Center (Orchestrator)](screenshots/00-orchestrator.png)

## How it fits together

```mermaid
flowchart TD
    F([Campaign brief form]) --> O[Marketing Command Center<br/>Orchestrator]
    O --> SP[Strategic Planning]
    O --> MI[Market Intelligence]
    O --> C[Content Creation]
    C --> S[SEO]
    S --> P[Paid Media]
    P --> SM[Social Media]
    SM --> L4[L4 Lead Nurturing]
    L4 --> CRM[(HubSpot / WhatsApp / Gmail)]
    L5[L5 Performance Agent<br/>weekly] -->|learnings| DB[(marketing_learnings)]
    DB --> SP
    E{{Error Alerts}} -.watches.-> O
```

## Workflows

### `marketing-system/`

| # | Workflow | What it does | Trigger | Main tools |
|---|----------|--------------|---------|------------|
| 00 | [Orchestrator](marketing-system/00-orchestrator.json) | A Gemini "Marketing Director" writes one campaign idea, briefs every agent and runs them in order | Form | Gemini, Execute Workflow |
| 01 | [Strategic Planning Agent](marketing-system/01-strategic-planning-agent.json) | Builds a full strategy (positioning, channel mix & budget, campaigns, calendar, KPIs) and exports it as an HTML deck | Form / Orchestrator | Gemini agent, Meta & TikTok Ads data |
| 02 | [Market Intelligence Agent](marketing-system/02-market-intelligence-agent.json) | Collects competitor and trend data from TikTok, Facebook and RSS, then summarizes insights | Form / Orchestrator | Apify, RSS, Gemini |
| 03 | [Content Creation Agent](marketing-system/03-content-creation-agent.json) | Writes blog articles, social posts, ad copy and landing-page copy using live search trends | Form / Orchestrator | Gemini + Google Search |
| 04 | [SEO Agent](marketing-system/04-seo-agent.json) | Crawls pages, runs technical checks (robots, sitemap, Core Web Vitals) and suggests keywords and fixes | Form / Orchestrator | PageSpeed Insights, Google Ads Keyword Planner, Gemini |
| 05 | [Paid Media Agent](marketing-system/05-paid-media-agent.json) | Sets up campaigns on Meta, TikTok, Snapchat and Google Ads, then checks performance daily | Form / Orchestrator / Daily | Meta Graph API, Ads APIs, Gemini |
| 06 | [Social Media Agent](marketing-system/06-social-media-agent.json) | Publishes posts with email approval and handles community replies every 2 hours | Form / Orchestrator / Schedule | Meta Graph API, Gmail, Gemini |
| 07 | [L4 Lead Nurturing Agent](marketing-system/07-lead-nurturing-agent.json) | Scores leads 0–100, sends personalized WhatsApp/email outreach, drip campaigns, and hands hot leads to sales | Called by agents / Webhook | Gemini, HubSpot, WhatsApp, Gmail |
| 08 | [L5 Performance Agent](marketing-system/08-performance-agent.json) | Weekly report: ROI, ROAS, CPA, A/B test significance, and learnings fed back to planning | Every Monday 09:00 | GA4, Google Ads, Meta, TikTok, Gemini |
| 09 | [Error Alerts](marketing-system/09-error-alerts.json) | Emails an alert whenever any agent fails | Error trigger | Gmail |

### `standalone/`

| Workflow | What it does | Trigger |
|----------|--------------|---------|
| [AI News Email Alerts](standalone/ai-news-email-alerts.json) | Reads AI news RSS feeds, removes duplicates, summarizes with Gemini and emails a digest | Every 3 hours |

### `archive/`
Earlier versions and test workflows, kept for reference.

## Screenshots

<details>
<summary><b>01 · Strategic Planning Agent</b></summary>

![Strategic Planning Agent](screenshots/01-strategic-planning-agent.png)
</details>

<details>
<summary><b>02 · Market Intelligence Agent</b></summary>

![Market Intelligence Agent](screenshots/02-market-intelligence-agent.png)
</details>

<details>
<summary><b>03 · Content Creation Agent</b></summary>

![Content Creation Agent](screenshots/03-content-creation-agent.png)
</details>

<details>
<summary><b>04 · SEO Agent</b></summary>

![SEO Agent](screenshots/04-seo-agent.png)
</details>

<details>
<summary><b>05 · Paid Media Agent</b></summary>

![Paid Media Agent](screenshots/05-paid-media-agent.png)
</details>

<details>
<summary><b>06 · Social Media Agent</b></summary>

![Social Media Agent](screenshots/06-social-media-agent.png)
</details>

<details>
<summary><b>07 · L4 Lead Nurturing Agent</b></summary>

![L4 Lead Nurturing Agent](screenshots/07-lead-nurturing-agent.png)
</details>

<details>
<summary><b>08 · L5 Performance Agent</b></summary>

![L5 Performance Agent](screenshots/08-performance-agent.png)
</details>

## How to import

1. In n8n, open **Workflows → Import from File** and pick a `.json` file.
2. Reconnect the credentials each workflow needs (Gemini, Gmail, Meta Graph API, HubSpot, etc.). **Credentials are not included in this repo.**
3. Fill in the setup values described in the sticky notes inside each workflow (account IDs, page IDs, emails).
4. For the full system, import every file in `marketing-system/`, then open the Orchestrator and re-select each sub-workflow in its **Execute Workflow** nodes, because workflow IDs change on import.
5. Replace `your-email@example.com` with your own address where it appears.

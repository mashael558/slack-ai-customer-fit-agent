# 🤖 Slack AI Customer Fit Agent

An AI-powered Slack agent that automatically analyzes new community members, gathers lightweight public research, evaluates potential customer fit using Google Gemini, stores the analysis, and posts the results to a Slack channel.

The project combines **Node.js, Slack Bolt, Google Gemini, LangChain, Express, Axios, and PostgreSQL** to automate an initial customer-fit assessment whenever relevant Slack membership events occur.

---

##  Features

- **Automatic member detection**
  - Handles Slack `team_join` events.
  - Handles `member_joined_channel` events.
  - Processes channel-join events only when `channel_type === 'C'`.

-  **Slack member profiling**
  - Retrieves member information using Slack's `users.info` API.
  - Collects available information such as:
    - Name
    - Username
    - Email
    - Job title
    - Timezone
    - Profile information

- 🔎 **Lightweight public research**
  - Uses a member's company email domain to request the company's homepage.
  - Extracts the website `<title>` as basic company information.
  - Searches GitHub users using the member's real name.
  - Uses publicly available GitHub information such as public repository count.

-  **AI-powered customer-fit analysis**
  - Uses Google Gemini through LangChain.
  - Generates:
    - A fit score from 0–100
    - Key insights
    - Engagement recommendations
  - Considers factors such as job title, company, technical background, and potential budget authority.

-  **Analysis persistence**
  - Saves member analyses and research data to the project's database layer.
  - Tracks whether an analysis has been successfully sent to Slack.

- 📢 **Slack reporting**
  - Posts a structured customer-fit report to a configured Slack channel.
  - Uses Slack Block Kit for readable formatting.

-  **Health endpoint**
  - Provides a `/health` endpoint for service monitoring.

-  **Development test endpoint**
  - Provides a development-only endpoint for manually testing member analysis.

---

##  Architecture

```text
                    ┌─────────────────────┐
                    │       Slack         │
                    │                     │
                    │  New Member / Join  │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │   Slack AI Agent    │
                    │  Node.js + Bolt     │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │   Member Profile    │
                    │    Slack API        │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │ Lightweight Public  │
                    │      Research       │
                    └──────────┬──────────┘
                               │
                    ┌──────────┴──────────┐
                    │                     │
                    ▼                     ▼
             ┌─────────────┐       ┌─────────────┐
             │   Company   │       │   GitHub    │
             │   Website   │       │    Search   │
             └──────┬──────┘       └──────┬──────┘
                    │                     │
                    └──────────┬──────────┘
                               ▼
                    ┌─────────────────────┐
                    │    Google Gemini    │
                    │   Fit Assessment    │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │     PostgreSQL      │
                    │  Store Analysis     │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │       Slack         │
                    │ Fit Score + Insights│
                    │ + Recommendations   │
                    └─────────────────────┘

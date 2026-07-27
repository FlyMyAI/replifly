# replifly 🚀

[![part of Build with FlyMy.AI](https://img.shields.io/badge/part%20of-Build%20with%20FlyMy.AI-b6ff3b?style=flat-square&labelColor=0b0d10)](https://github.com/FlyMyAI/build-with-flymyai)

**We killed Replit.** One sentence - *"deploy my code to prod"* - and Claude + the **FlyMy.AI cloud** stand up the whole thing: compute, database, monitoring, and an agent that phones you when it crashes. On **your** accounts, billed to **you**. No subscription, no lock-in.

## How it got built: one prompt in your terminal

<img src="docs/one-prompt.gif" alt="connect the FlyMy.AI MCP, type one prompt, the cloud provisions and hosts it" width="820">

Connect the MCP once, point it at your repo, and the whole stack is provisioned through it - app, database, error tracking and a watcher agent, all on your accounts. (Recreated from [BUILD_LOG.md](BUILD_LOG.md): the real NocoDB deploy, Fly.io + Neon + Sentry, every step through the MCP.)

```mermaid
flowchart LR
    U(["🧑‍💻 you<br/><b>deploy my code to prod</b>"]) --> C

    subgraph BRAIN["🧠 Claude + FlyMy.AI MCP"]
        C["reads your repo<br/>detects stack · port · needs a DB?"]
    end

    C -->|one MCP call| CLOUD

    subgraph CLOUD["☁️ FlyMy.AI cloud — provisions and runs it all, itself"]
        direction TB
        F["🚀 <b>Fly.io</b><br/>compute + scale"]
        N["🐘 <b>Neon</b><br/>Postgres, auto-wired"]
        S["🛡️ <b>Sentry</b><br/>errors + dashboards"]
        W["🤖 <b>on-call agent</b><br/>watches 24/7"]
    end

    CLOUD --> LIVE(["🌐 <b>live URL</b><br/>your accounts · billed to you"])
    W -.->|crash at 3am| PAGE(["📞 <b>calls your phone</b><br/>prod is down, wake up"])

    classDef you fill:#0b7285,stroke:#0b7285,color:#fff;
    classDef brain fill:#5f3dc4,stroke:#5f3dc4,color:#fff;
    classDef cloud fill:#1864ab,stroke:#1864ab,color:#fff;
    classDef out fill:#2b8a3e,stroke:#2b8a3e,color:#fff;
    classDef pager fill:#c92a2a,stroke:#c92a2a,color:#fff;
    class U you;
    class C brain;
    class F,N,S,W cloud;
    class LIVE out;
    class PAGE pager;
```

## vs a $25/mo Replit subscription

|  | Replit | replifly |
|---|---|---|
| Deploy from one prompt | ✅ (Replit Agent) | ✅ (Claude, any model) |
| Where it runs | Replit's cloud, rented | **your Fly.io / Neon**, billed to you |
| Monitoring + error tracking | basic | **Sentry + dashboards, wired in** |
| Wakes you when it crashes | ❌ | ✅ **an agent calls your phone** |
| Lock-in | your app lives on Replit | **you own the stack, portable** |
| Cost | $25/mo + metered Agent + deploy fees | pay-per-use on your own accounts |

## 💸 The economics (real 2026 prices)

Two costs to compare: **writing** the code, and **running** it in prod.

### Writing it — Replit meters every generation; Claude is flat

| | Replit Agent | Claude (Code) |
|---|---|---|
| Model | metered "effort-based": ~$0.25 a simple change, **"multiple dollars"** for complex tasks | flat subscription: Pro **$20/mo**, Max **$100-200/mo** |
| A full CRUD app | **$30-50+ to build once** | included — code all day, every project |
| Included credits | $25/mo (Core) - **gone in 3-4 days** of active building | Max 20x ≈ **240-480 hrs of coding/week** |
| Overage | **uncapped** - reported **$1,000 in a week**, "$20 from one prompt" | none - interactive coding isn't per-generation billed |

### Running it — you own it, on free/cheap tiers

| | Replit | replifly (your accounts) |
|---|---|---|
| Always-on compute | **Reserved VM $20-160/mo** (locked to Replit) | **Fly.io ~$2-5/mo** |
| Postgres | bundled, metered | **Neon free tier $0** (0.5 GB, 100 CU-hrs) |
| Error tracking | add-on | **Sentry free tier $0** (5k errors/mo) |
| **Monthly to run a small prod app** | **~$25-45/mo** + Agent credits | **~$2-5/mo** (free tiers) → ~$10-30 at scale |

**Bottom line: running your app is 4-10x cheaper on your own Fly + Neon + Sentry (~$2-5/mo) than Replit's Reserved-VM hosting (~$25-45/mo)** - and the code that builds it is a flat Claude subscription instead of a metered credit bucket a single app can burn in one sitting. Your stack, your accounts, portable.

> Prices from [replit.com/pricing](https://replit.com/pricing) · [Replit effort-based billing](https://blog.replit.com/effort-based-pricing) · [claude.com/pricing](https://claude.com/pricing) · [fly.io](https://fly.io/docs/about/pricing/) · [neon.com](https://neon.com/pricing) · [sentry.io](https://sentry.io/pricing/) (2026). Replit's exact overage depends on usage; the $1,000/week figure is a reported real case ([The Register](https://www.theregister.com/2025/09/18/replit_agent3_pricing/)).

## Build it yourself

1. Connect the FlyMy.AI cloud to your agent - one line:
   ```bash
   claude mcp add --transport http flymyai https://mcp-agents.flymy.ai/mcp
   ```
2. In your repo, say it:
   ```
   deploy this repo to prod: Fly.io + a Postgres DB, wire Sentry, and an
   on-call agent that calls me if it goes down.
   ```
3. Claude reads your code, auto-detects the stack, provisions everything through the cloud, and hands you a live URL. See [BUILD_PROMPT.md](BUILD_PROMPT.md) for the exact prompt and [`skill/`](skill/) for the reusable "ship-to-production" skill.

## Receipts

Real end-to-end run (deploying [NocoDB](https://github.com/nocodb/nocodb), an open-source Airtable/CRM), every step through the FlyMy.AI MCP - what worked, what we had to fix, real load numbers - is in [BUILD_LOG.md](BUILD_LOG.md).

## Part of Build with FlyMy.AI

replifly is one demo in **[Build with FlyMy.AI](https://github.com/FlyMyAI/build-with-flymyai)** - a series where each app rebuilds a venture-funded product from a single prompt, with Claude as the builder and the FlyMy.AI agentic cloud as the backend, and publishes the real bill. The umbrella repo holds the shared playbook, the agent rules and the other demos:

- [WhisperFly](https://github.com/FlyMyAI/whisperfly) - dictation straight into Notion, ~$0.03 a note
- **replifly** (you are here) - "deploy my code to prod" on your own accounts
- [higfly](https://github.com/FlyMyAI/higfly) - cinematic AI video, ~$0.20-0.50 a clip

Want to build your own kill? Start with the [playbook](https://github.com/FlyMyAI/build-with-flymyai/blob/main/PLAYBOOK.md).

---

> **Not affiliated with, endorsed by, or sponsored by Replit, Inc.** "Replit" is a trademark of Replit, Inc., used here only to describe the category. replifly is an independent open-source project.

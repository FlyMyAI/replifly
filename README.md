# replifly 🚀

**We killed Replit.** One sentence - *"deploy my code to prod"* - and Claude + the **FlyMy.AI cloud** stand up the whole thing: compute, database, monitoring, and an agent that phones you when it crashes. On **your** accounts, billed to **you**. No subscription, no lock-in.

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
| Cost | $25/mo + deploy fees | pay-per-use on your own accounts |

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

---

> **Not affiliated with, endorsed by, or sponsored by Replit, Inc.** "Replit" is a trademark of Replit, Inc., used here only to describe the category. replifly is an independent open-source project.

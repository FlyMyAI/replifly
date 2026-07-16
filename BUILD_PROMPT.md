# The prompt that builds this

Prereq: your coding agent (Claude Code / claude.ai / Codex) with the FlyMy.AI MCP connected:
```bash
claude mcp add --transport http flymyai https://mcp-agents.flymy.ai/mcp
```

## Deploy your repo to prod (paste in your repo)

```text
Deploy this repo to production through the FlyMy.AI cloud. Do it end to end via the MCP:
1. Detect the stack from my code (Dockerfile / port / does it need a Postgres?). Don't ask what you can detect.
2. If it needs a DB, provision a Neon Postgres and wire the connection string in.
3. Deploy the app to Fly.io - compute, public URL, TLS. Scale to 2 machines.
4. Wire Sentry for error tracking + a dashboard.
5. Stand up an on-call agent that health-checks prod every minute and CALLS my phone
   (+1 ...) if it goes down.
Ask me only for choices you can't infer: where alerts go (Telegram/email/SMS) and my
phone number. Then show me the live URL and the real billed cost of the run.
```

## Reproduce our exact test

We ran the above deploying NocoDB (open-source Airtable/CRM). See [BUILD_LOG.md](BUILD_LOG.md) for every MCP call, what worked, what we fixed, and the load numbers.

# Gatekeeper — "Ship AI safely."

## What is Gatekeeper?

A drop-in reverse proxy + compliance layer that sits between any app and OpenAI/Anthropic.
Change one environment variable. Done. Your AI is now enterprise-safe.

## The Problem

Every company building with AI faces the same blocker:
- Enterprise customers demand SOC-2 / HIPAA compliance before signing
- AI calls leak PII (names, emails, SSNs, API keys) into LLM providers
- Jailbreaks and prompt injections are undetected
- No audit trail of what AI is doing in production
- Zero runtime visibility into agent behavior

## The Solution

Gatekeeper intercepts every LLM API call:
1. Strips PII before it leaves your infrastructure
2. Blocks jailbreaks and prompt injection attempts
3. Enforces policy packs (SOC-2, HIPAA) automatically
4. Logs everything to a tamper-proof audit trail
5. Alerts on anomalies in real time

## Stack

- **Backend:** Python + FastAPI
- **Proxy engine:** httpx async reverse proxy
- **PII detection:** presidio (Microsoft) + custom regex
- **Jailbreak detection:** LLM-as-judge (Claude Haiku, cheap + fast)
- **Database:** Postgres (Neon)
- **Frontend:** Next.js + Tailwind + shadcn/ui
- **Auth:** NextAuth
- **Payments:** Stripe
- **Deploy:** Railway (backend) + Vercel (frontend)
- **CLI:** Node.js, published to npm as `gatekeeper-ai`

## Pricing

- **Free:** CLI scanner, 1 app, 10K requests/mo
- **Pro:** $200/mo, unlimited apps, full policy packs, audit logs
- **Enterprise:** Custom, SLA, dedicated support

## Repo Structure

```
gatekeeper/
├── api/          ← FastAPI proxy backend
├── web/          ← Next.js dashboard + landing page
├── cli/          ← npx gatekeeper-ai CLI
└── docs/         ← Architecture + API docs
```

## Demo Target

**Week 3 demo URL:** demo.gatekeeper.dev
- Paste any prompt → see it scanned live
- Shows PII redactions, blocked jailbreaks, policy violations
- One-click "Connect your app" flow

## Why Workato Can't Copy This

1. Live threat feed from AIDepShield (real IOC data, unique moat)
2. Policy-as-code in YAML (version controlled, engineers love it)
3. SOC-2 + HIPAA packs (months of legal work, already done)
4. Workato-native connector (built specifically for their platform)
5. Paying customers by Week 2 (social proof + momentum)

# Gatekeeper — Context for Interns

## Why We're Building This

The LiteLLM supply chain attack (March 2026) hit 97M downloads via GitHub Actions.
Socket.dev and Snyk missed it. AIDepShield caught it.

That attack proved one thing: **AI infrastructure has zero security tooling.**

Every company building with AI faces the same problem:
- Enterprise customers demand SOC-2 / HIPAA before signing contracts
- Nobody audits what the AI is doing with sensitive data
- One bad prompt can leak a customer's SSN to OpenAI's servers
- Jailbreaks and prompt injections are completely undetected

Gatekeeper is the fix. One line of code. Enterprise-safe AI.

---

## The Pitch

> "Change one environment variable. Your AI is now SOC-2 compliant, HIPAA ready, PII-safe, and fully audited."

---

## The Business Model

- **Free:** CLI scanner, proves the problem exists
- **Pro:** $200/mo, solves the problem
- **Enterprise:** Custom, SLA, dedicated support

**Day 1 customer:** Series-A SaaS startups losing enterprise deals to security questionnaires.

---

## Our Secret Weapon

We own AIDepShield — a live IOC feed of compromised AI packages.
No competitor has this data. It plugs directly into Gatekeeper's threat detection.
This is the moat Workato can't replicate.

---

## The 30-Day Goal

Build a demo that makes Workato's CTO say:
**"We can't build this ourselves. How much?"**

That's it. Everything else is secondary.

---

## Tech Decisions — Don't Debate These

- Backend: FastAPI (not Django, not Express)
- Frontend: Next.js + Tailwind + shadcn/ui (not Vue, not SvelteKit)
- Database: Neon Postgres (already have account)
- Deploy: Railway (backend) + Vercel (frontend)
- PII detection: Microsoft Presidio (battle-tested, not custom)
- Auth: NextAuth (not Clerk, not Auth0 — keep costs zero)
- Payments: Stripe Checkout (not custom — we don't touch card data)

---

## What "Good" Looks Like

**A good day's work:**
- Shipped something that works end-to-end
- Tested it yourself
- Can demo it in 60 seconds

**A bad day's work:**
- Spent 8 hours on architecture diagrams
- Debating which database to use
- Perfecting code that doesn't run yet

**Ship first. Polish later. Demo by Day 21.**

---

## How to Use AI Tools

**Use o3 when:**
- Designing system architecture
- Stuck on a hard problem >30 min
- Reviewing a critical decision

**Use Claude Opus when:**
- Writing code (Cursor / Claude Code)
- Debugging complex issues
- Writing documentation

**Don't use AI when:**
- You already know the answer
- It's faster to just write it

---

## Questions?

Ping on Telegram. Don't be blocked for more than 30 minutes without asking for help.

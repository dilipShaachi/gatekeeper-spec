# Gatekeeper — 30-Day Sprint Plan

## North Star
**Week 3 demo to Workato.** By Day 21, demo.gatekeeper.dev must make their CTO say "we can't build this ourselves."

---

## Daily Standup Format (5 min, async via Telegram)
Each intern posts every morning:
```
✅ Done yesterday:
🔨 Working on today:
🚧 Blocked on:
```

---

## Week 1 — Core Engine (Days 1-7)

| Day | Dev1 (Backend) | Dev2 (Frontend) |
|-----|----------------|-----------------|
| 1 | Repo setup, FastAPI skeleton, proxy endpoint | Repo setup, Next.js skeleton, landing page hero |
| 2 | Proxy working (OpenAI + Anthropic) | Landing page sections + live demo widget (mock data) |
| 3 | PII detection (Presidio) | CLI scaffold, `scan` command |
| 4 | Jailbreak detection (regex + LLM judge) | CLI `connect` command, npm publish |
| 5 | Audit log schema + Postgres | Dashboard shell + auth |
| 6 | `/health`, `/scan`, `/stats` endpoints | Connect demo widget to real backend |
| 7 | Buffer + integration testing | Buffer + polish |

**Week 1 milestone:** `curl` the proxy, it works. CLI scans a file. Landing page is live.

---

## Week 2 — Enterprise Layer (Days 8-14)

| Day | Dev1 (Backend) | Dev2 (Frontend) |
|-----|----------------|-----------------|
| 8 | SOC-2 policy pack (YAML) | Dashboard live stats page |
| 9 | HIPAA policy pack | Audit log table + filters |
| 10 | Policy engine (evaluate rules against requests) | Policy builder UI |
| 11 | Multi-tenant (app_id + api_key) | Onboarding flow (step 1-3) |
| 12 | Rate limiting + cost caps | Onboarding flow (step 4-7 + confetti) |
| 13 | AIDepShield IOC feed integration | Stripe integration |
| 14 | Buffer + load test | Buffer + polish |

**Week 2 milestone:** Paying customer possible. Policy packs working. Onboarding <5 min.

---

## Week 3 — Demo Killer (Days 15-21)

| Day | Dev1 (Backend) | Dev2 (Frontend) |
|-----|----------------|-----------------|
| 15 | Workato connector research + scaffold | demo.gatekeeper.dev scaffold |
| 16 | Workato connector working | Demo scary examples + real-time results |
| 17 | "Explain this block" feature | SOC-2 PDF report generator |
| 18 | Policy-as-code (YAML, GitOps) | Slack alerts integration |
| 19 | Security hardening + rate limit tuning | Demo polish + mobile responsive |
| 20 | End-to-end testing | End-to-end testing |
| 21 | **DEMO DAY PREP** | **DEMO DAY PREP** |

**Week 3 milestone:** demo.gatekeeper.dev is live. Workato demo script rehearsed.

---

## Week 4 — Buffer + Polish (Days 22-30)

- Bug fixes from demo feedback
- Performance optimization (<50ms proxy latency)
- Security audit (have someone try to break it)
- Documentation
- GitHub README polish
- Product Hunt prep
- Outreach to 10 Series-A startups

---

## Communication

- **Daily standup:** Telegram (async, 9 AM)
- **Blocker escalation:** Tag me immediately, don't wait for standup
- **Decisions:** Run anything architectural by o3 first, then me
- **Code review:** PRs reviewed within 2 hours during business hours

---

## Resources

- Backend spec: `INTERN-1-BACKEND.md`
- Frontend spec: `INTERN-2-FRONTEND.md`
- AIDepShield API: `https://aidepshield-production.up.railway.app`
- Design inspiration: snyk.io, datadoghq.com, stripe.com
- Railway account: dilip@shaachi.com
- Vercel account: dilip@shaachi.com

---

## Definition of "Done" for Workato Demo

✅ demo.gatekeeper.dev loads instantly  
✅ Paste a PII-heavy prompt → see it redacted in <100ms  
✅ Paste a jailbreak → see it blocked with explanation  
✅ Click "Download SOC-2 Report" → PDF downloads  
✅ Workato connector demo working (even if mocked)  
✅ "This would have taken us 6 months to build" — that's the reaction we want  

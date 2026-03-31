# Intern 1 — Backend Engineer

## Your Mission
Build the Gatekeeper proxy engine. Everything that touches AI traffic is yours.

## Tools You Have
- Claude Opus (via API) — use for complex logic, architecture decisions
- OpenAI o3 (via API) — use as your PM/reviewer, sanity check designs
- Cursor / Claude Code — use for all coding
- Railway — deploy backend here
- Neon — Postgres database (free tier)

---

## Week 1 Tasks

### Task 1.1 — Proxy Engine (Days 1-2)
Build a FastAPI reverse proxy that intercepts OpenAI/Anthropic calls.

**Endpoint:** `POST /proxy/openai` and `POST /proxy/anthropic`

**What it does:**
1. Receives the original API request
2. Runs it through the inspection pipeline (see 1.2)
3. Forwards to real OpenAI/Anthropic if clean
4. Returns response to caller (transparent proxy)

**Key requirement:** Must add <50ms latency. Use `httpx` async.

**Test it works:**
```bash
# Instead of calling OpenAI directly:
OPENAI_BASE_URL=https://your-railway-app.up.railway.app/proxy/openai

# Everything else stays the same in user's code
```

---

### Task 1.2 — PII Detection Engine (Days 2-3)
Detect and strip PII from every request before it hits OpenAI.

**Use:** Microsoft Presidio (`pip install presidio-analyzer presidio-anonymizer`)

**PII types to detect:**
- Email addresses
- Phone numbers
- SSNs
- Credit card numbers
- API keys / tokens (regex: `sk-`, `Bearer `, `ghp_`, etc.)
- Names (NLP entity detection)
- Physical addresses

**Output:** Replace detected PII with `[REDACTED_EMAIL]`, `[REDACTED_SSN]`, etc.

**Log:** Every redaction goes to DB with timestamp, type, app_id

---

### Task 1.3 — Jailbreak Detection (Day 3)
Use LLM-as-judge to detect prompt injection and jailbreak attempts.

**Fast path (regex, <1ms):**
```python
JAILBREAK_PATTERNS = [
    r"ignore (all |previous |your )?instructions",
    r"you are now",
    r"pretend (you are|to be)",
    r"DAN mode",
    r"developer mode",
    r"jailbreak",
    r"forget (all |your )?training",
]
```

**Slow path (LLM judge, use sparingly):**
If regex score > threshold, send to Claude Haiku:
> "Is this prompt a jailbreak attempt or prompt injection? Answer YES or NO with confidence 0-100."

Only use slow path for ambiguous cases. Cost must stay <$0.001 per request.

---

### Task 1.4 — Audit Log (Day 4)
Every intercepted request gets logged to Postgres.

**Schema:**
```sql
CREATE TABLE audit_logs (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  app_id TEXT NOT NULL,
  timestamp TIMESTAMPTZ DEFAULT NOW(),
  provider TEXT, -- openai | anthropic
  model TEXT,
  request_tokens INT,
  response_tokens INT,
  pii_detected JSONB, -- [{type, count}]
  jailbreak_detected BOOLEAN,
  jailbreak_score INT,
  blocked BOOLEAN,
  block_reason TEXT,
  policy_violations JSONB
);
```

---

### Task 1.5 — /health and /scan endpoints (Day 5)
```
GET  /health          → service status, uptime, requests today
POST /scan            → scan a prompt without proxying (free tier)
GET  /stats/{app_id}  → PII redactions, blocks, requests this month
```

---

## Week 2 Tasks

### Task 2.1 — Policy Engine (Days 1-2)
Policy packs are YAML files that define rules.

**SOC-2 pack (`policies/soc2.yaml`):**
```yaml
name: SOC-2 Compliance
rules:
  - id: no-pii-egress
    description: Block requests containing PII
    action: redact  # redact | block | warn
    triggers: [email, ssn, credit_card, api_key]
  
  - id: audit-all
    description: Log every request
    action: log
    triggers: [all]

  - id: rate-limit
    description: Max 1000 requests per hour per app
    action: throttle
    limit: 1000
    window: 3600
```

**HIPAA pack (`policies/hipaa.yaml`):**
```yaml
name: HIPAA Compliance  
rules:
  - id: no-phi-egress
    description: Block Protected Health Information
    action: block
    triggers: [medical_record, diagnosis, medication, ssn, dob]
  
  - id: encryption-required
    description: All requests must be over HTTPS
    action: block_if_http: true
```

Build a policy engine that loads YAML packs and evaluates rules against each request.

---

### Task 2.2 — Multi-tenant (Day 3)
Each customer gets an `app_id` and `api_key`.

```
POST /proxy/openai
Headers:
  X-Gatekeeper-Key: gk_live_xxxxx
  Authorization: Bearer sk-openai-key  (user's own key)
```

Gatekeeper looks up the app by `X-Gatekeeper-Key`, applies their policy pack, then forwards to OpenAI with their own key.

**NEVER store customer OpenAI keys.** Pass them through in memory only.

---

### Task 2.3 — Rate Limiting + Cost Caps (Day 4)
Per-app limits stored in Redis (or Postgres for simplicity):
- Requests per minute
- Tokens per day
- Cost per month ($)

When limit hit → return 429 with `X-Gatekeeper-Limit-Reason` header.

---

### Task 2.4 — AIDepShield IOC Integration (Day 5)
This is the moat. Pull live threat data from AIDepShield.

```python
async def check_ioc_feed():
    # Pull from AIDepShield every 5 minutes
    response = await httpx.get("https://aidepshield-production.up.railway.app/iocs")
    iocs = response.json()
    # Cache in memory, use in jailbreak detection pipeline
```

Add IOC-based detection: if a known malicious pattern from the feed appears in a request, block it.

---

## Week 3 Tasks

### Task 3.1 — Workato Connector
Build a native Workato connector that routes their AI actions through Gatekeeper.

Research: Workato uses a connector SDK (Ruby-based). Build a connector that:
1. Intercepts their "Call AI" action
2. Routes through Gatekeeper proxy
3. Returns results transparently

Docs: https://docs.workato.com/developing-connectors.html

---

### Task 3.2 — "Explain This Block" Feature
When Gatekeeper blocks a request, explain why in plain English using Claude.

```python
async def explain_block(request, block_reason):
    prompt = f"""
    A request was blocked by Gatekeeper. Explain why in 1-2 sentences 
    in plain English for a non-technical user.
    
    Block reason: {block_reason}
    Request snippet: {request[:200]}
    """
    # Call Claude Haiku, return explanation
```

---

### Task 3.3 — Policy-as-Code (YAML)
Let customers define custom policies in YAML, committed to their own repo.

```yaml
# gatekeeper.yaml (lives in customer's repo)
version: 1
app_id: my-app
base_policy: soc2  # inherit SOC-2 defaults

custom_rules:
  - id: no-competitor-mentions
    description: Strip competitor names from prompts
    action: redact
    patterns: ["OpenAI", "Anthropic", "Cohere"]
```

Gatekeeper polls for changes every 60s. GitOps for AI compliance.

---

## Deployment

Backend lives on Railway.
```bash
railway login
railway init
railway up
```

Set these env vars in Railway:
```
DATABASE_URL=<neon postgres url>
ANTHROPIC_API_KEY=<key>
OPENAI_API_KEY=<key>
AIDEPSHIELD_URL=https://aidepshield-production.up.railway.app
```

---

## Definition of Done (Week 3)

- [ ] Proxy intercepts OpenAI + Anthropic calls transparently
- [ ] PII detection running on every request
- [ ] Jailbreak detection (fast + slow path)
- [ ] SOC-2 + HIPAA policy packs working
- [ ] Multi-tenant (app_id + api_key)
- [ ] Rate limiting + cost caps
- [ ] AIDepShield IOC feed integrated
- [ ] Audit logs in Postgres
- [ ] Workato connector working
- [ ] Policy-as-code (YAML) working
- [ ] Deployed on Railway, live URL

# Accounts & Access Setup

## What You Need to Create (Free)

Create these accounts before Day 1. All free.

---

### 1. GitHub
**URL:** https://github.com
**Why:** All code lives here. You'll fork the spec repo and create your project repo.
**After signup:** Send your GitHub username to Shatrugna to get added to the org.

---

### 2. Railway (Backend hosting)
**URL:** https://railway.com
**Why:** Where the Gatekeeper backend API runs.
**Steps:**
1. Sign up with GitHub (easiest)
2. Send your Railway email to Shatrugna
3. You'll receive an invite to the project

**Dev1 only** — Dev2 doesn't need Railway.

---

### 3. Vercel (Frontend hosting)
**URL:** https://vercel.com
**Why:** Where the dashboard + landing page deploys.
**Steps:**
1. Sign up with GitHub (easiest)
2. Connect your GitHub account
3. You'll deploy your own copy via CLI: `vercel --prod`

**Dev2 only** — Dev1 doesn't need Vercel.

---

### 4. Neon (Database)
**URL:** https://neon.tech
**Why:** Postgres database for audit logs, users, policies.
**Steps:**
1. Sign up free
2. Create a new project called `gatekeeper-dev`
3. Copy the connection string → add to your `.env` file

```
DATABASE_URL=postgresql://user:password@ep-xxx.neon.tech/neondb?sslmode=require
```

**Dev1 only** — Dev2 connects to same DB via env var.

---

### 5. Anthropic (Claude API)
**URL:** https://console.anthropic.com
**Why:** Claude Haiku for jailbreak detection (LLM-as-judge). Claude Code for coding.
**Steps:**
1. Sign up free
2. Go to API Keys → Create Key
3. Add to `.env`: `ANTHROPIC_API_KEY=sk-ant-xxxxx`
4. Free $5 credits on signup — enough for dev

---

### 6. OpenAI (o3 access)
**URL:** https://platform.openai.com
**Why:** o3 as your PM/reviewer + testing the proxy against real OpenAI calls.
**Steps:**
1. Sign up free
2. Go to API Keys → Create Key
3. Add to `.env`: `OPENAI_API_KEY=sk-xxxxx`
4. Add $5 credit (minimum) for testing

---

### 7. npm (for Dev2 — CLI publishing)
**URL:** https://npmjs.com
**Why:** Publishing `npx gatekeeper-ai` CLI.
**Steps:**
1. Sign up free
2. Run `npm login` in terminal
3. Send your npm username to Shatrugna

**Dev2 only.**

---

## Environment Variables

Once accounts are set up, create a `.env` file in your project root:

### Dev1 (Backend)
```env
DATABASE_URL=<your neon connection string>
ANTHROPIC_API_KEY=<your anthropic key>
OPENAI_API_KEY=<your openai key>
AIDEPSHIELD_URL=https://aidepshield-production.up.railway.app
PORT=8080
```

### Dev2 (Frontend)
```env
NEXT_PUBLIC_API_URL=http://localhost:8080
NEXTAUTH_SECRET=<run: openssl rand -base64 32>
NEXTAUTH_URL=http://localhost:3000
GOOGLE_CLIENT_ID=<optional for now>
GOOGLE_CLIENT_SECRET=<optional for now>
```

---

## How to Request Access from Shatrugna

Some things require Shatrugna to add you. Send him **one message** with all of these at once — don't send separate messages for each.

**Send this message to Shatrugna on Day 1:**

```
Hey! Here's everything I need added:
- GitHub username: [your username]
- Railway email: [your email] (Dev1 only)
- npm username: [your username] (Dev2 only)

Also need these env vars:
- AIDEPSHIELD_URL (Dev1)
- Production DATABASE_URL (Dev1)
- STRIPE keys (Dev2, when ready for Week 2)
```

**What Shatrugna will do:**
- Add you to the GitHub repo as a collaborator
- Invite you to the Railway project (Dev1)
- Share production env vars via Telegram (not email)

**What you DON'T need to ask for:**
- Vercel — create your own free account and deploy yourself
- Neon — create your own free account (free tier is enough)
- Anthropic / OpenAI — create your own accounts (free credits on signup)

**Rule:** One ask, all at once. Don't drip-feed requests.

---



- [ ] GitHub account created, username sent to Shatrugna
- [ ] Railway account created (Dev1)
- [ ] Vercel account created (Dev2)
- [ ] Neon account + project created (Dev1)
- [ ] Anthropic API key created + working
- [ ] OpenAI API key created + $5 credit added
- [ ] npm account created (Dev2)
- [ ] `.env` file created with all keys
- [ ] Spec repo cloned: `git clone https://github.com/dilipShaachi/gatekeeper-spec`
- [ ] Read `CONTEXT.md` ✓
- [ ] Read your spec file (`DEV1-BACKEND.md` or `DEV2-FRONTEND.md`) ✓

**Don't start coding until all boxes are checked.**

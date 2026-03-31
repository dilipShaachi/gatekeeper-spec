# Tooling Guide — How to Build Gatekeeper Fast

## Primary Tool: Claude Code

Claude Code is your main coding assistant. It writes production-quality code for this stack.

### Setup
```bash
# Install
npm install -g @anthropic-ai/claude-code

# Login (one time)
claude login

# Run in your project folder
claude --print --permission-mode bypassPermissions
```

### The Right Workflow

**One task at a time. Test before moving on.**

```
1. Open your spec file (INTERN-1 or INTERN-2)
2. Pick ONE task from the current day
3. Open Claude Code in your project folder
4. Paste the task description + relevant context
5. Let it build
6. Test it manually — does it actually work?
7. Fix issues, then move to next task
```

**❌ Wrong:** "Here is the entire spec, build everything"  
**✅ Right:** "Build the FastAPI proxy endpoint from Task 1.1. Here are the requirements: [paste Task 1.1 only]"

---

## Quality by Task

| Task | Tool | Notes |
|---|---|---|
| FastAPI proxy engine | Claude Code | Excellent — trust it |
| PII detection (Presidio) | Claude Code | Very good — test edge cases |
| Next.js dashboard | Claude Code | Excellent — trust it |
| CLI (Node.js + commander) | Claude Code | Excellent — trust it |
| Policy engine (YAML) | Claude Code | Very good — review logic carefully |
| Workato connector (Ruby) | Claude Code + manual review | Decent — needs extra testing |
| PDF report generator | Claude Code | Very good |
| Stripe integration | Claude Code | Excellent — well-known API |

---

## Using o3 as Your PM

When you're stuck on a design decision, ask o3 first:

```bash
curl -s https://api.openai.com/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -d '{
    "model": "o3",
    "messages": [{"role": "user", "content": "YOUR QUESTION HERE"}]
  }' | jq '.choices[0].message.content'
```

**Ask o3 when:**
- Designing system architecture
- Stuck on a hard problem >30 min
- Unsure about a tech decision
- Need a second opinion before building something big

**Ask Claude Code when:**
- Writing code
- Debugging
- Refactoring

---

## Using Claude Opus for Complex Logic

For the hardest tasks (jailbreak detection logic, policy engine evaluation), use Opus directly:

```python
import anthropic

client = anthropic.Anthropic()

message = client.messages.create(
    model="claude-opus-4-6",
    max_tokens=1024,
    messages=[
        {"role": "user", "content": "YOUR COMPLEX QUESTION"}
    ]
)
print(message.content[0].text)
```

---

## Cursor Setup (Alternative to Claude Code)

If you prefer Cursor:
1. Open project folder in Cursor
2. Use `Cmd+K` for inline edits
3. Use `Cmd+L` for chat (Claude Sonnet by default, switch to Opus for hard tasks)
4. Same rule: one task at a time

---

## Daily Workflow

```
Morning:
1. Read standup from other intern
2. Pick today's tasks from WEEK-BY-WEEK.md
3. Open Claude Code / Cursor
4. Build task by task

When blocked:
1. Try for 30 min max
2. Ask o3 for architectural help
3. Ping on Telegram — don't stay blocked

End of day:
1. Post standup update on Telegram
2. Push your code: git push
3. Note what's working and what's not
```

---

## Git Workflow

```bash
# Start of day
git pull

# During day — commit often
git add .
git commit -m "feat: proxy engine intercepts OpenAI calls"
git push

# Commit message format:
# feat: new feature
# fix: bug fix
# wip: work in progress (end of day push)
```

---

## Testing Checklist Before Marking a Task Done

- [ ] It runs without errors
- [ ] I tested it manually (not just "the code looks right")
- [ ] Edge cases work (empty input, bad input, network failure)
- [ ] I can demo it in 60 seconds

**If you can't demo it in 60 seconds, it's not done.**

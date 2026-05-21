---
name: cheatsheet
trigger: /cheatsheet
description: >
  Generate or update a personal Claude Code cheat sheet HTML file from your live
  skill list, CLAUDE.md settings, shortcuts, prompt patterns, and model routing.
  Produces a self-contained dark-theme HTML file with 7 tabs, global search,
  copy buttons, and a full filterable skill catalog — all personalized to whoever
  runs it.
---

# /cheatsheet — Personal Claude Code Cheat Sheet

Regenerates `~/Claude Code/cheatsheet.html` (fallback: `~/Desktop/claude-cheatsheet.html`)
from your live Claude Code setup. Everything is read at invocation time, so the
output always reflects your current installed skills, CLAUDE.md config, and model
routing rules.

## Sub-commands

| Command | What it does |
|---|---|
| `/cheatsheet` | Full regenerate (default) |
| `/cheatsheet update` | Same as above |
| `/cheatsheet open` | Open existing file: `open "~/Claude Code/cheatsheet.html"` |

---

## Step-by-step execution

### 1. Determine output path

```bash
[ -d "$HOME/Claude Code" ] && echo "$HOME/Claude Code/cheatsheet.html" || echo "$HOME/Desktop/claude-cheatsheet.html"
```

### 2. Read your setup (do these in parallel)

- **Live skills list** — from the `<system-reminder>` block in the current session. Every installed skill appears there. Use this as the source of truth; do not guess or use memory.
- **~/.claude/CLAUDE.md** — extract: proactive trigger table, model routing rules, About Me, stack info (framework, UI lib, auth provider, DB).
- **~/CLAUDE.md** — extract: project-level writing style, autonomy rules, stack.
- **~/.claude/rules/model-routing.md** — full Opus/Sonnet/Haiku agent/task tables.
- **~/.claude/rules/cc-patterns.md** — keybinding patterns and power patterns.

### 3. Generate the HTML

Write a single self-contained HTML file. No external CDN dependencies. Everything
inline. The file must open offline.

#### Design spec

```
bg:      #0d1117   surface: #161b22   surface2: #21262d
border:  #30363d   accent:  #58a6ff   text:     #e6edf3
muted:   #8b949e   green:   #56d364   yellow:   #e3b341
red:     #f85149   purple:  #bc8cff   orange:   #ffa657
mono:    'SF Mono', 'Fira Code', 'Cascadia Code', Consolas
```

Sticky header: logo + global search input + tab buttons.
Global search filters all visible content. Escape clears it.
Copy-to-clipboard button on every command/skill row.
Mobile responsive (no fixed widths, CSS grid/flexbox).

#### Tab 1 — Shortcuts

In-session keybindings (render as `<kbd>` visual keys):

| Keys | What it does |
|---|---|
| Shift+Tab | Toggle auto-accept edits mode |
| # text | Claude remembers it and writes to CLAUDE.md |
| ! command | Run bash locally; output into Claude's context |
| Escape | Safely stop Claude mid-task |
| Escape Escape | Jump back in conversation history |
| Ctrl+R | Show full context window output |
| ⌥T | Toggle extended thinking (macOS) / Alt+T (Win/Linux) |
| Ctrl+O | Verbose thinking output |

CLI flags:

| Command | What it does |
|---|---|
| `claude --resume` | Resume previous session |
| `claude -p "..."` | Run as Unix filter; pipe any input |
| `/model opus` | Switch to Opus for planning/architecture |
| `/model sonnet` | Return to default workhorse |
| `/model haiku` | Switch down for mechanical work (3x cheaper) |
| `/clear` | Hard-reset context; use when switching tasks |
| `/goal <condition>` | Session-scoped completion gate; Claude keeps turning until Haiku confirms |
| `/ship` | Full pipeline: lint→type-check→test→build→commit→push→PR |

Power patterns (callout boxes):
- Feedback loop first: give Claude a verifiable output (tests, screenshots, simulator output) before iterating. 2-3 self-iterations → near-perfect results. Highest-leverage pattern.
- Parallel worktrees: run multiple Claude sessions in separate git worktrees. Use `superpowers:using-git-worktrees`.
- Explore before editing: ask how things work before touching files. Claude finds usage examples, linked issues, and intent — deeper than grep.

#### Tab 2 — Skills (collapsible subcategories)

Build this tab dynamically from the live skills list in the session.

Organize into these subcategories (use `data-category` attributes for filtering):

```
Planning & Architecture
Debugging & Error Recovery
TDD & Testing
Next.js & Vercel Stack
Deployment & CI/CD
Database
AI & Claude API
Auth & Payments
Design & UI
Figma
Animation — GSAP
Animation — Three.js
Animation — Other
Hyperframes
Video & Media
Audio & Voice
Marketing
Social Media
Writing & Content
Code Quality & Review
Agentic & Autonomous
Superpowers (Discipline)
Memory & Sessions
iOS / Swift / Android
Backend Frameworks
GitHub & Git
Slack
Healthcare / Financial Plugins
Product & Operations
Plugin & Skill Development
Utilities & Misc
```

Each skill row:
- Monospace skill name (copy-to-clipboard button)
- One-line description (infer from name if not in memory; be accurate, not generic)

Collapsible sections: clicking the header toggles the body. Default: all expanded.
Show a count badge on each section header (e.g., "Planning & Architecture [10]").

#### Tab 3 — Prompt Patterns

Organize prompts by context. Each prompt is in a block with:
- Context label (e.g., "When starting any non-trivial feature")
- The prompt text in monospace
- Copy button

Include prompts for:
1. **Planning before coding** — "Before you write code, make a plan. Ask me for approval first."
2. **Fuzzy problem** — "/plan grill" (relentless interview until zero ambiguity)
3. **Options, not decisions** — "Give me 3 approaches with tradeoffs. Don't pick one."
4. **Unfamiliar codebase** — "How is [module] instantiated? Trace through the code."
5. **Debugging: feedback loop** — "Before fixing, write a test that shows the bug. Show me it fails. List hypotheses ranked by likelihood. Then fix."
6. **Stopping guesswork** — "Stop. Read [file:line] before answering. Don't paraphrase from memory."
7. **Regression** — "This worked before [X]. Look through git history for what changed."
8. **TDD** — "Write a failing test for [behavior]. Show me it fails. Then implement minimum code to pass."
9. **Security review** — "Review [file]. Focus only on security. Cite file:line for every finding."
10. **Trident audit** — "Dispatch code-reviewer and security-reviewer in parallel against this diff."
11. **Weekly git review** — "Read the git log for my username. What did I ship this week?"
12. **Commit and PR** — "commit push PR"
13. **Autonomous mode** — "/goal tests pass and build is clean"
14. **Let Claude run** — "Assume I'm asleep. Execute autonomously. Don't stop to ask."

#### Tab 4 — Precision

**Scoping edits**:
- "Only change `functionName` in `src/path/file.ts:42`. Don't touch anything else."
- "Surgical change only. Don't reformat, rename, or refactor adjacent code."
- "Change X but do not touch [files/functions]. Those are out of scope."

**Stopping guesswork**:
- "Don't invent a solution. Read `[file:line]` first. Cite what you read. Then propose a fix."
- "For every claim about existing code, quote the file:line that backs it."

**TypeScript errors**:
- Paste exact error output (file, line, type mismatch). Add: "Don't guess the types — read the definitions."

**Current diff in context**:
- `!git diff HEAD` — use the `!` prefix to pipe git diff directly into Claude's context.

**Vague vs specific error reports** — side-by-side comparison table:

| Vague (bad) | Specific (good) |
|---|---|
| "The login is broken" | "Login returns 401 on POST /api/auth/callback. Error: [verbatim]. File: src/.../route.ts:34. Expected: redirect. Actual: 401 no body." |
| "It doesn't work on mobile" | "src/middleware.ts:12 throws when cookies.get('token') is undefined. Repro: clear cookies, hit any protected route." |

**API / framework boilerplate**:
- "Check package.json for the exact version of [lib]. Fetch the docs for that version before implementing."
- "This is Next.js 15. All request APIs are async: await cookies(), headers(), params, searchParams."

**Tracer-first** (before parallel agents):
- "Run one prompt end-to-end as a tracer. Show me the result. If tracer fails, fix the plan before fanning out."

#### Tab 5 — Tokens

**Quick commands**:

| Command | Effect |
|---|---|
| `mp-caveman` | ~75% token reduction; ultra-compressed mode |
| `context-budget` | Audit context consumption across agents, skills, MCPs |
| `token-budget-advisor` | Choose response depth interactively |
| `strategic-compact` | Compress at phase boundaries |
| `/clear` | Hard-reset; use when switching unrelated tasks |

**Model routing for cost**:

| Task | Right model | Savings |
|---|---|---|
| Formatting, renaming, Tailwind, paraphrasing | Haiku | ~3x |
| shadcn/ui components, accessibility, animation | Haiku | ~3x |
| Production code, review, debugging | Sonnet | Default |
| Architecture, planning, irreversible decisions | Opus | Worth it |

**Context hygiene**:
- DO: cite file:line, use ctx_read in map/signatures mode, /clear between unrelated tasks, strategic-compact at milestones, use subagents to protect main context
- DON'T: paste entire large files, let context exceed 70% without compacting, add verbose CLAUDE.md entries, use Opus for formatting

**lean-ctx reading modes** (from CLAUDE.md):

| Situation | Mode |
|---|---|
| File you will edit | full (or native Read) |
| Debugging at any confidence | full (or native Read) |
| Unsure which mode | full (or native Read) |
| Exploring unfamiliar structure | signatures |
| Context-only, won't edit | map |
| After an edit | diff |
| Large file, one section | lines:N-M |

#### Tab 6 — Model Routing

Pull the judgment-depth rule and full agent/task tables from `~/.claude/rules/model-routing.md`.

Show three sections:
1. **Opus** — planning, architecture, irreversible decisions; agent/task table
2. **Sonnet** — production code workhorse; agent/task table
3. **Haiku** — fast, 3x cheaper, mechanical work; agent/task table
4. **Switch commands** — `/model opus`, `/model sonnet`, `/model haiku` with copy buttons

#### Tab 7 — Full Catalog

Filterable dense table of ALL skills from the live session list.

Category filter buttons at top. "All" selected by default. Clicking a category
filters the table to that category only.

Columns: Skill name (monospace, copy button) | Category badge | Description.

The search bar in the header also filters this tab in real time.

---

### 4. After writing

1. Confirm the output path to the user.
2. Run `open "[output_path]"` to open in browser (macOS).
3. Report total skill count included.

---

## Personalization notes

- Pull the user's name from `~/CLAUDE.md` (About Me section) for the title.
- Pull the stack (Next.js version, UI lib, auth, DB) from CLAUDE.md for the footer or a "My Stack" callout on the Shortcuts tab.
- If the user has a Telegram bot or Obsidian vault mentioned in CLAUDE.md, add a quick-reference row for those.
- The proactive trigger table in CLAUDE.md maps message patterns to skills — display this as a compact lookup in the Skills tab header as "Auto-triggers".

---

## Adding new skills later

Any time this skill is re-run (`/cheatsheet`), it re-reads the live skills list from
the session and overwrites the file. No manual editing required. The file stays
current as long as you run `/cheatsheet` after installing new skills.

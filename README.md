# claude-cheatsheet

A Claude Code skill that generates a **personal, self-updating HTML cheat sheet** from your live setup. Works for any Claude Code user — no configuration required.

Every time you run `/cheatsheet`, it re-reads your current session and rewrites the file. New skills installed? Run it again.

## Install

```bash
npx skills add sants2001/claude-cheatsheet
```

Or manually copy `SKILL.md` to `~/.claude/skills/cheatsheet/SKILL.md`.

## Usage

```
/cheatsheet          # Full regenerate from current setup
/cheatsheet update   # Same as above
/cheatsheet open     # Open existing file in browser
```

## What it generates

A single self-contained HTML file (`~/Claude Code/cheatsheet.html` or `~/Desktop/claude-cheatsheet.html`) with 8 tabs:

| Tab | Contents |
|---|---|
| **Shortcuts** | Keybindings, CLI flags, power patterns |
| **Skills** | All installed skills in collapsible subcategories with copy buttons |
| **Prompt Patterns** | Ready-to-copy prompts by context (debugging, TDD, reviews, autonomous) |
| **Precision** | Surgical edit/debug techniques, exact error format, stopping guesswork |
| **Tokens** | Token-saving commands, model routing for cost, lean-ctx mode guide |
| **Model Routing** | Opus/Sonnet/Haiku task tables from your model-routing.md |
| **Full Catalog** | Filterable dense table of every installed skill |
| **How Claude Works** | Meta insights: context window, hallucination, thinking modes, feedback loops, anti-patterns, suggestions |

## The "How Claude Works" tab

This is what makes the cheat sheet more than a reference doc. It covers:

- **Context window mechanics** — primacy/recency, the 70% degradation cliff, why long CLAUDE.md is wasteful
- **How Claude reads requests** — task-category inference, how to override defaults
- **Hallucination triggers** — what specifically causes them, mitigation rules
- **Thinking modes** — when extended thinking helps vs. wastes tokens
- **Feedback loops** — the single highest-leverage pattern for better Claude output
- **Subagents and parallel work** — protecting your context window, tracer-first rule
- **Anti-patterns table** — 10 common mistakes and what to do instead
- **Suggestions** — actionable tips based on your installed skill count and setup

## Personal to you

When you have CLAUDE.md files, the cheat sheet reads them for:
- Your name and stack (framework, UI lib, auth, DB)
- Proactive trigger table (message patterns → skill auto-invocations)
- Model routing rules
- Autonomy and approval policies

Without CLAUDE.md files, it uses generic defaults. The skill works for anyone.

## Design

- Dark GitHub theme (`#0d1117` bg, `#58a6ff` accent)
- No external dependencies — opens offline
- Global search across all tabs (Escape to clear)
- Copy-to-clipboard on every row
- Mobile responsive
- Collapsible skill categories with count badges

## Example output

See [`example.html`](./example.html) — generated from a setup with 560+ installed skills.

## Keeping it current

After installing new skills, run `/cheatsheet` again. The output always reflects the live session's skill list.

---

Built with [Claude Code](https://claude.ai/claude-code).

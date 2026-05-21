# claude-cheatsheet

A Claude Code skill that generates a **personal, self-updating HTML cheat sheet** from your live setup — installed skills, CLAUDE.md config, model routing rules, shortcuts, and prompt patterns.

Every time you run `/cheatsheet`, it re-reads your current session and rewrites the file. No manual maintenance.

## Install

```bash
npx skills add santinogatmaytan/claude-cheatsheet
```

Or manually copy `SKILL.md` to `~/.claude/skills/cheatsheet/SKILL.md`.

## Usage

```
/cheatsheet         # Full regenerate from current setup
/cheatsheet update  # Same as above
/cheatsheet open    # Open existing file in browser
```

## What it generates

A single self-contained HTML file (`~/Claude Code/cheatsheet.html` or `~/Desktop/claude-cheatsheet.html`) with 7 tabs:

| Tab | Contents |
|---|---|
| **Shortcuts** | Keybindings, CLI flags, power patterns |
| **Skills** | All installed skills in collapsible subcategories with copy buttons |
| **Prompt Patterns** | Ready-to-copy prompts by context (debugging, TDD, reviews, autonomous) |
| **Precision** | Surgical edit/debug techniques, exact error format, stopping guesswork |
| **Tokens** | Token-saving commands, model routing for cost, lean-ctx mode guide |
| **Model Routing** | Opus/Sonnet/Haiku task tables from your model-routing.md |
| **Full Catalog** | Filterable dense table of every installed skill |

**Personal to you**: the cheat sheet reads your CLAUDE.md for your name, stack (Next.js version, UI lib, auth, DB), model routing rules, and proactive trigger table. Each user gets a different output.

## Design

- Dark GitHub theme (`#0d1117` bg, `#58a6ff` accent)
- No external dependencies — opens offline
- Global search (Escape to clear)
- Copy-to-clipboard on every row
- Mobile responsive

## Keeping it current

After installing new skills, just run `/cheatsheet` again. The skill re-reads the live skills list from the session and regenerates everything.

## Example output

See [`example.html`](./example.html) for a sample (Santino's setup, ~560 skills).

---

Built with [Claude Code](https://claude.ai/claude-code).

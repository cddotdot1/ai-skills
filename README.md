# ai-skills

Personal AI skills I've invented. Each skill lives in `skills/<name>/SKILL.md`.

## Skills

| Skill | What it does |
| --- | --- |
| [explain-diff](skills/explain-diff/SKILL.md) | Turns a diff, branch, commit range or PR into a self-contained HTML explainer with background, intuition, code walkthrough and a quiz. |

## Installing

These skills work with [Claude Code](https://claude.com/claude-code), which loads
personal skills from `~/.claude/skills/<name>/SKILL.md`.

### 1. Clone the repo

```bash
gh repo clone cddotdot1/ai-skills ~/Development/ai-skills
```

### 2. Link the skills into Claude Code

Symlinking (rather than copying) means a `git pull` or a local edit takes effect
straight away.

One skill:

```bash
mkdir -p ~/.claude/skills
ln -s ~/Development/ai-skills/skills/explain-diff ~/.claude/skills/explain-diff
```

Every skill in the repo:

```bash
mkdir -p ~/.claude/skills
for dir in ~/Development/ai-skills/skills/*/; do ln -sfn "${dir%/}" ~/.claude/skills/"$(basename "$dir")"; done
```

If a skill with the same name is already installed, move it out of
`~/.claude/skills` first, or the link will fail (or end up nested inside it).

### 3. Check it loaded

Start a new Claude Code session and ask "what skills do you have?", or type
`/explain-diff`. Skills are picked up when a session starts.

### Notes

- The links point at whatever branch the clone has checked out. Keep the clone on
  `main`, or a skill that only exists on another branch will vanish when you switch.
- To update: `git -C ~/Development/ai-skills pull`.
- To uninstall a skill: `rm ~/.claude/skills/<name>` (removes only the link).
- Project-only install: link into `<project>/.claude/skills/` instead of
  `~/.claude/skills/`.

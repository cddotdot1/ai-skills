# ai-skills

Personal AI skills I've invented. Each skill lives in `skills/<name>/SKILL.md`.

## Skills

| Skill | What it does |
| --- | --- |
| [explain-diff](skills/explain-diff/SKILL.md) | Turns a diff, branch, commit range or PR into a self-contained HTML explainer with background, intuition, code walkthrough and a quiz. |

## Using a skill with Claude Code

Symlink it into your personal skills folder:

```bash
ln -s "$PWD/skills/explain-diff" ~/.claude/skills/explain-diff
```

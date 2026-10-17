---
name: explain-diff
description: Use when the user asks for a rich, visual explanation of a code change — a diff, a branch, a commit range, uncommitted work, or a GitHub PR. Produces one self-contained HTML page with background, intuition, a code walkthrough and an interactive quiz, saved outside the repo.
---

# Explain Diff

Turn a code change into a single, long, self-contained HTML page that teaches a
reader what changed and why — well enough that they could review it or extend it.

Inspired by Geoffrey Litt's `explain-diff` skill
(https://gist.github.com/geoffreylitt/a29df1b5f9865506e8952488eac3d524).

## 1. Work out which change to explain

Pick the first that applies, and say which one you chose in one line:

1. The user named something — a PR number or URL (`gh pr diff <n>`, `gh pr view <n>`),
   a commit or range (`git show <sha>`, `git diff <a>..<b>`), or a branch.
2. Uncommitted work exists — `git diff HEAD` (include staged and unstaged).
3. Otherwise, the current branch against its base:
   `git diff $(git merge-base HEAD origin/HEAD)...HEAD`.

If the diff is empty, stop and ask what to explain. Read the PR description and
commit messages too: they often state the intent the code alone does not.

## 2. Understand it before writing

- Read every changed file in full, not just the hunks.
- Explore the surrounding code broadly: callers, callees, data models, tests, docs.
  The Background section depends on this.
- Run nothing that changes state. This skill only reads.

## 3. Write the page

Four sections, in this order, with smooth transitions between them:

### Background
What existing system does this change live in? Start with a broad primer for a
newcomer (mark it clearly as skippable for readers who know the area), then narrow
to exactly the parts the change touches.

### Intuition
The essence of the change, not every detail. Use a concrete worked example with
small toy data, and show it before and after the change. Lean on diagrams.

### Code walkthrough
A high-level tour of the changes, grouped and ordered so they make sense to read
(usually by concept or data flow, not by file name or diff order). Show short,
relevant excerpts; link file paths; skip noise such as formatting or renames
unless they matter.

### Quiz
Five multiple-choice questions of medium difficulty: answerable only by someone who
understood the substance of the change, but never trick questions. Clicking an
answer reveals right/wrong and an explanation of why — for wrong answers too.
Show a running score.

## 4. Style

- Write in classic style, with the clarity and flow of Martin Kleppmann: plain,
  confident, concrete, engaging. Explain terms the first time they appear.
- Use callouts for key definitions, important edge cases and gotchas.
- Choose a small number of diagram families and reuse them throughout so the
  reader learns to read them once. Useful kinds:
  - A simplified mock of the UI the user sees, for UI changes.
  - A component / data-flow diagram, always annotated with example data.
  - A before/after comparison of the same toy example.
- Build diagrams from HTML and CSS (or inline SVG). Never ASCII art. Use real
  HTML lists and tables for lists and tables.

## 5. Format and output

- One self-contained `.html` file: inline CSS and JS, no external requests
  except optionally a pinned syntax-highlighting script from cdnjs.
- One long page with section headers and a table of contents at the top. No tabs
  for top-level structure.
- Responsive enough to read on a phone; supports light and dark mode via
  `prefers-color-scheme`.
- Code blocks: use `<pre><code>`. Any custom code container **must** have
  `white-space: pre` or `pre-wrap`, or newlines collapse. Before saving, scan every
  code block in the source and confirm this.
- Save outside the repo, in `~/explanations/`, named
  `YYYY-MM-DD-explain-<short-slug>.html` using today's date, so files sort by time
  and never end up in version control. Create the folder if needed.
- When done, open it (`open <path>` on macOS) and reply with the path plus a
  two-sentence summary of what the change does.

## Checklist before finishing

- [ ] Stated which change was explained (PR / range / branch / uncommitted).
- [ ] All four sections present, with a table of contents linking to each.
- [ ] At least one worked example with toy data, shown before and after.
- [ ] Five quiz questions; each option has feedback; score updates on click.
- [ ] Every code block preserves whitespace.
- [ ] File saved under `~/explanations/` with a date-prefixed name, not in the repo.

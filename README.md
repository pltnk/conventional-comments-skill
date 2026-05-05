# Conventional Comments Skill

*Keep feedback scannable, review intent clear at a glance, and every comment actionable.*

A Claude skill that formats review and feedback comments using the [Conventional Comments standard](https://conventionalcomments.org/).

When you ask Claude for a review, critique, or feedback on anything (code, docs, RFCs, designs, drafts, PRs), this skill ensures every comment uses a clear format:

```
<label> [decorations]: <subject>

[discussion]
```

The skill defines comment [format](./SKILL.md#format) and [communication practices](./SKILL.md#communication-practices) only. It does not prescribe what to comment on, how to organize the review, or how many comments to write. Those are all up to Claude's judgment based on what you're reviewing.

## What you get

- **Standard labels** with clear semantics (`praise`, `nitpick`, `suggestion`, `polish`, `issue`, `todo`, `typo`, `question`, `thought`, `chore`, `note`)
- **Standard decorations** for blocking semantics, plus support for review-specific ones like `(security)` or `(perf)`
- **Pairing rule** for `issue:`. Every problem gets a separate follow-up `suggestion:` proposing the fix
- **Communication practices** ported from the standard's [communication guide](https://conventionalcomments.org/communication/): be curious, leave actionable comments, combine similar comments, replace "you" with "we", don't restate the label
- **Multi-language support**. Labels and decorations stay English; subject and discussion match your language

## Usage

- The skill triggers on any review-shaped request, including indirect prompts like "thoughts on this?" or "what could be better?".
- You can also invoke it explicitly with `/conventional-comments`.

## Examples

A review of a Python function might produce:

> **praise:** Input validation up front is clean — explicit `ValueError`s with concrete bounds make this easy to call safely.

> **issue (blocking):** `float` arithmetic introduces accumulating precision errors for monetary calculations.
>
> The intermediate `price * discount_percent / 100` is binary-float arithmetic; `round(..., 2)` at the end masks but doesn't prevent drift.

> **suggestion (blocking):** Switch to `decimal.Decimal` end-to-end.
>
> The precision boundary then lives at the API edge — callers pass `Decimal` (or `str` we convert).

## Installation

### Claude Code

Claude Code [discovers skills](https://code.claude.com/docs/en/skills#where-skills-live) from two well-known directories. Pick the scope you want:

**Personal**, available across all your projects:

```bash
mkdir -p ~/.claude/skills
git clone https://github.com/pltnk/conventional-comments-skill.git ~/.claude/skills/conventional-comments
```

**Project**, committed to a single repo, shared with anyone who clones it:

```bash
cd path/to/your-project
mkdir -p .claude/skills
git clone https://github.com/pltnk/conventional-comments-skill.git .claude/skills/conventional-comments
```

Either way, the skill becomes available immediately. Claude Code [watches skill directories for file changes](https://code.claude.com/docs/en/skills#live-change-detection) and picks up the new skill in the current session without a restart.

### Claude.ai

Upload [SKILL.md](./SKILL.md) directly at <https://claude.ai/customize/skills>.

Refer to [Anthropic's guide on using skills in Claude](https://support.claude.com/en/articles/12512180-use-skills-in-claude) for the current UI flow.

## How the skill is structured

The skill is a single [SKILL.md](./SKILL.md) file with no scripts, no bundled resources, and no external dependencies.

## Acknowledgments

This skill ports the [Conventional Comments standard](https://conventionalcomments.org/) and its [communication guide](https://conventionalcomments.org/communication/) into a form Claude can apply consistently. All credit for the underlying format and practices goes to the Conventional Comments authors.

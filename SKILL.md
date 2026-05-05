---
name: conventional-comments
description: Format review and feedback comments using the [Conventional Comments standard](https://conventionalcomments.org/). The goal is comments that are easy to understand and act on, with a collaborative tone. Use whenever the user asks for a review, critique, or feedback on anything — code, docs, RFCs, designs, drafts, PRs, or any other artifact — including indirect prompts like "thoughts on this" or "what could be better?". Defines comment format and tone, not review content, structure, or length. Labels and decorations in English; subject and discussion in the user's language.
---

## Scope

This skill defines the **format** of each review comment and recommended **communication practices**. It does **not** define:

- **Review content.** Substance, depth, and what's worth flagging come from the artifact and the user's request, not from this skill.
- **Review structure.** This skill formats individual comments; it doesn't organize them — pick the structure you find the most suitable.
- **Review length.** The number of comments must be as much or as little as the work warrants.

## Format

Every comment follows this shape:

```
<label> [decorations]: <subject>

[discussion]
```

- **label** — a single lowercase English word signaling the kind of comment. Bold in Markdown (e.g. `**suggestion:**`).
- **decorations** *(optional)* — lowercase comma-separated qualifiers in parentheses, sitting between label and colon. Decorations classify comments that share a label. Example: `**issue (ux,non-blocking):**`.
- **subject** — the main message, on the same line as the label.
- **discussion** *(optional)* — supporting context, reasoning, examples, patches, or "why / next steps" notes. **When a discussion is present, a blank line between the subject line and the discussion is required.**

### Labels

- Use one label per comment.
- Diverging from the following list is fine if a different label fits better.

| Label | Meaning |
|---|---|
| **praise:** | Highlights something genuinely positive. Aim for at least one per review; never invent it. |
| **nitpick:** | A trivial preference-based request. Non-blocking by nature. |
| **suggestion:** | Proposes an improvement. Be explicit about *what* and *why*. Consider patches and a `(blocking)` or `(non-blocking)` decoration. |
| **polish:** | Like `suggestion:`, but for purely quality-improving changes — nothing is wrong. |
| **issue:** | A specific problem you're confident about. **Always pair with a separate follow-up `suggestion:`** that proposes the fix; never embed the fix in the `issue:` discussion. If you're not sure a problem is real, use `question:` instead of `issue:`. |
| **todo:** | A small, trivial, but necessary change. Lighter weight than `issue:` or `suggestion:`. |
| **typo:** | Like `todo:`, but specifically for misspellings. |
| **question:** | A potential concern where it isn't yet clear whether it's relevant. Ask the author for clarification. |
| **thought:** | An idea that surfaced during review. Non-blocking by nature. |
| **chore:** | A simple task required before acceptance (running a CI job, signing off a checklist). Link to the process when possible. |
| **note:** | Always non-blocking. Highlights something to be aware of. |

### Decorations

| Decoration | Meaning |
|---|---|
| **(non-blocking)** | Does not block acceptance. |
| **(blocking)** | Blocks acceptance until resolved. |
| **(if-minor)** | Resolve only if doing so turns out to be minor or trivial. |

Decorations can be review-specific — `(security)`, `(ux)`, `(test)`, `(perf)`, `(a11y)` are common. Use them when they sharpen the comment. Keep the vocabulary of decorations small and unambiguous across the whole review. Keep per-comment decorations few; if a comment needs many, consider splitting it.

## Communication practices

The standard's [communication guide](https://conventionalcomments.org/communication/) recommends:

### Be curious

Don't assume full context. When unsure whether a problem is real, prefer `question:` over `issue:` or `suggestion:`.

Avoid:

```
**suggestion:** This bug could be solved in the `Main` component. Probably less code.
```

Prefer:

```
**question:** Could we solve this in the `Main` component?

It might be a simpler fix.
```

### Leave actionable comments

Make it clear how to resolve the comment. If there's no obvious path forward, say so explicitly.

### Combine similar comments

Don't fragment one concern across many tiny comments. Batch related issues into a single comment with a patch or example.

Avoid:

Ten separate `**polish:** m_foo → foo` comments.

Prefer:

```
**polish:** Rename all `m_X` variables to `X` — the `m_` prefix isn't used elsewhere in this project.

E.g. `m_foo` → `foo`, `m_bar` → `bar`, `m_quackaroo` → `quackaroo`.
```

### Replace "you" with "we"

"You" places focus on the person; "we" focuses on the task.

Avoid:

```
**issue:** You should write tests for this.
```

Prefer:

```
**todo:** We should write tests for this.
```

### Don't restate the label

The label signals the comment's type, stance, and that something is being said. Don't restate any of that — drop hedging, introductory phrases, fillers, and pleasantries.

Avoid:

```
**issue:** I think this might be a problem — looking at the code, the function basically swallows exceptions silently. Hope this helps!
```

Prefer:

```
**issue:** The function silently swallows exceptions.
```

## Language behavior

- **Labels and decorations are always in English** — they're the canonical tokens of the standard and must not be translated.
- **Subject and discussion match the language of the user's message** — Russian message, Russian subject and discussion. Same for any other language.

Example (Russian content, English label):

```
**suggestion (non-blocking):** Стоит вынести валидацию в отдельную функцию.

Сейчас в обработчике смешаны валидация и сохранение — разделение упростит тестирование и переиспользование.
```

---
name: domain-modeling
description: Build and sharpen the project's domain model. Use when the user wants to fix domain terminology or a ubiquitous language, record an architectural decision (ADR), or when another skill needs to support the domain model.
---

# Domain Modeling

Actively build and sharpen the project's domain model as you design. This is an
*active* discipline: challenge terms, invent edge cases, and record the glossary
and decisions at the very moment they crystallize. (Merely *reading* `CONTEXT.md`
for the vocabulary is not this skill; that is a one-line habit available to any
skill. This skill is for when you are changing the model, not just using it.)

## File structure

Most repositories have a single context:

```
/
├── CONTEXT.md
├── docs/
│   └── adr/
│       ├── 0001-event-sourced-orders.md
│       └── 0002-postgres-for-write-model.md
└── src/
```

If there is a `CONTEXT-MAP.md` at the root, the repository has several contexts.
The map says where each one lives:

```
/
├── CONTEXT-MAP.md
├── docs/
│   └── adr/                       ← system-wide decisions
├── src/
│   ├── ordering/
│   │   ├── CONTEXT.md
│   │   └── docs/adr/              ← decisions for a specific context
│   └── billing/
│       ├── CONTEXT.md
│       └── docs/adr/
```

Create files lazily — only when there is something to record. If there is no
`CONTEXT.md` yet, create it when the first term is fixed. If there is no
`docs/adr/`, create it when the first ADR is needed.

## Language

Run the session in the user's working language (default — English).

In the glossary, separate the *canonical term* from its *definition*.

- **The canonical mirrors the code.** Write the canonical term the way it lives in
  the project. In a greenfield project — ask the team once and record the choice.
- **Full synonyms are co-canonical, via `/`.** If terms are identical in meaning
  and interchangeable in every scenario (repo / repository, config / configuration),
  write them in the canonical via `/` with spaces: `Order / Sale Order`. There can
  be more than two.
- **Partial synonyms go in `_Avoid_`.** If terms are similar but diverge in meaning
  in some scenario (account / user / customer), pick one canonical and move the
  rest to `_Avoid_`.
- **Ordering = a soft signal for code.** Since the working language is English, the
  canonical form naturally matches code identifiers; no separate rule is needed.
  For concepts with no code presence, order is free.
- **The definition** — in the working language, one to two sentences, "what this
  is". `_Avoid_` — a comma-separated list (order does not matter) of the rejected
  forms.

The goal is for the glossary terms to match what they are called in code and in the
team's speech. The agent reuses approved terms; the persistence layer is
`CONTEXT.md`. If the team prefers a different convention (a different house style,
abbreviations in identifiers) — fine, be consistent and record the choice in an ADR.

## During the session

### Check against the glossary

When the user uses a term against the existing language in `CONTEXT.md`, call it
out immediately: "In the glossary 'cancellation' is defined as X, but you seem to
mean Y — which is it?"

### Sharpen vague phrasings

When the user uses vague or overloaded terms, suggest a precise canonical term:
"You say 'account' — do you mean Customer or User? Those are different things."

### Establish full synonyms

When a term has synonym candidates, offer them to the user: approve, reject, or
defer ("shall we call it *grill / grilling*, or discuss later?"). "Defer" is a
dialogue escape-hatch; only approved synonyms make it into the glossary record.

The heuristic is the **substitution test**: terms are full synonyms only if they can
be swapped in any sentence and scenario without a shift in meaning. If you can
construct a scenario where they diverge, you **must surface it once, explicitly**
("here is a scenario where X and Y are different things; are we sure we merge
them?"). This is a risk warning, not a veto: the decision is the user's. If they
approve having seen the counterexample — write a full synonym via `/`. If they
reject — one canonical, the rest in `_Avoid_`. The warning is one-time, without
nagging. Reuse approved terms thereafter and record them in `CONTEXT.md` on the
spot.

### Discuss concrete scenarios

When relationships between domain concepts are discussed, test them against concrete
scenarios. Invent scenarios that probe edge cases and force the user to draw clear
boundaries between concepts.

### Check against the code

When the user claims how something works, check whether it agrees with the code. If
you find a contradiction — surface it: "The code cancels orders wholesale, but you
just said partial cancellation is possible — which is correct?"

### Update CONTEXT.md on the spot

When a term is fixed, update `CONTEXT.md` immediately. Do not batch them — record
them as they appear. Use the format from [CONTEXT-FORMAT.md](./CONTEXT-FORMAT.md).
`CONTEXT.md` must be entirely free of implementation details. Do not turn
`CONTEXT.md` into a specification, a draft, or a store of implementation decisions.
It is a glossary and nothing more.

### Propose ADRs sparingly

Propose creating an ADR only when all three hold:

1. **Hard to reverse** — the cost of changing your mind later is palpable
2. **Non-obvious without context** — a future reader will ask "why did they do it
   this way?"
3. **The result of a real trade-off** — there were real alternatives, and you chose
   one for specific reasons

If any of the three is missing — skip the ADR. Use the format from
[ADR-FORMAT.md](./ADR-FORMAT.md).

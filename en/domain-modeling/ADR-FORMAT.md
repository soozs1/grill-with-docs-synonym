# ADR Format

ADRs live in `docs/adr/` and use sequential numbering: `0001-slug.md`,
`0002-slug.md`, and so on. Create the `docs/adr/` directory lazily — only when the
first ADR is needed.

## Template

```md
# {Short decision title}

{1–3 sentences: what the context is, what was decided, and why.}
```

That is all. An ADR can be a single paragraph. The value is in recording *that* a
decision was made and *why* — not in filling out sections.

## Optional sections

Include them only when they genuinely add value. Most ADRs do not need them.

- **Status** in the frontmatter (`proposed | accepted | deprecated | superseded by
  ADR-NNNN`) — useful when decisions are revisited
- **Considered options** — only when the rejected alternatives are worth remembering
- **Consequences** — only when you need to highlight non-obvious downstream effects

## Numbering

Scan `docs/adr/` for the highest existing number and increment by one.

## When to propose an ADR

All three must hold:

1. **Hard to reverse** — the cost of changing your mind later is palpable
2. **Non-obvious without context** — a future reader will look at the code and ask
   "why did they do it this way?"
3. **The result of a real trade-off** — there were real alternatives, and you chose
   one for specific reasons

If a decision is easy to reverse — skip it, you will simply reverse it. If it
surprises no one — nobody will ask "why". If there was no real alternative — there
is nothing to record but "we did the obvious thing".

### What fits

- **Architectural shape.** "We use a monorepo." "The write model is event-sourced;
  the read model is projected into Postgres."
- **Integration patterns between contexts.** "Ordering and Billing communicate via
  domain events, not synchronous HTTP."
- **Technology choices with lock-in.** Database, message bus, authentication
  provider, deployment target. Not every library — only the ones you would change
  over a whole quarter.
- **Decisions about boundaries and responsibility.** "Customer data belongs to the
  Customer context; other contexts reference it only by ID." Explicit "no"s are as
  valuable as "yes"s.
- **Deliberate deviations from the obvious path.** "We use plain SQL instead of an
  ORM because X." Anything where a reasonable reader would assume the opposite. This
  keeps the next engineer from "fixing" what was done on purpose.
- **Constraints not visible in the code.** "We cannot use AWS because of compliance
  requirements." "Response time must be under 200 ms because of a partner API
  contract."
- **Rejected alternatives, when the rejection is non-obvious.** If you considered
  GraphQL and chose REST for subtle reasons — write it down, otherwise in six months
  someone will propose GraphQL again.

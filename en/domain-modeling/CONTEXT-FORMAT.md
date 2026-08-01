# CONTEXT.md Format

## Structure

```md
# {Context name}

{One or two sentences: what this context is and why it exists.}

## Language

**Order**: {One or two sentences: what this is}
_Avoid_: Purchase, transaction

**Invoice**: A request for payment sent to the customer after delivery.
_Avoid_: Bill, payment request

**Customer**: A person or organization that places orders.
_Avoid_: Client, buyer, account
```

## Rules

- **Distinguish full from partial synonyms.** Full synonyms (interchangeable in any
  scenario, e.g. repo / repository) are written co-canonically via `/` with spaces.
  Partial ones (diverge in some scenario, e.g. client / buyer) — one in the canon,
  the rest in `_Avoid_`.
- **For code-related concepts — English first.** If a term maps into code (class,
  function, table, file), the first variant in the canon is the English/Latin one:
  `Order`. This is a signal, not a constraint; there is no explicit "code form"
  field. (In an English working language this is automatically satisfied.)
- **Be categorical.** Fix the canon — one term or a set of full synonyms via `/` —
  and move everything extraneous to `_Avoid_`.
- **Keep definitions short.** At most one or two sentences. Define what it IS, not
  what it does. The definition is in the team's working language.
- **Include only terms specific to this project's context.** General programming
  concepts (timeouts, error types, utility patterns) do not belong here, even if the
  project uses them heavily. Before adding a term, ask: is this concept unique to
  this context, or is it a general programming concept? Only the former belongs in
  the glossary.
- **Group terms under subheadings** when natural clusters emerge. If all terms
  belong to one coherent area, a flat list is fine.

## Single- and multi-context repositories

**One context (most repositories):** a single `CONTEXT.md` at the repository root.

**Several contexts:** a `CONTEXT-MAP.md` at the root lists the contexts, where they
live, and how they relate to one another:

```md
# Context Map

## Contexts

- [Ordering](./src/ordering/CONTEXT.md) — accepts and tracks customer orders
- [Billing](./src/billing/CONTEXT.md) — issues invoices and processes payments
- [Fulfillment](./src/fulfillment/CONTEXT.md) — manages picking and shipping from
  the warehouse

## Relationships

- **Ordering → Fulfillment**: Ordering publishes `OrderPlaced` events; Fulfillment
  consumes them to start picking
- **Fulfillment → Billing**: Fulfillment publishes `ShipmentDispatched` events;
  Billing consumes them to issue an invoice
- **Ordering ↔ Billing**: Shared types for `CustomerId` and `Money`
```

The skill itself determines which structure applies:

- If there is a `CONTEXT-MAP.md` — it reads it to find the contexts
- If there is only a root `CONTEXT.md` — one context
- If there is neither — it creates a root `CONTEXT.md` lazily, when the first term
  is fixed

When there are several contexts, determine which one the current topic belongs to.
If unclear — ask.

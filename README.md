# grill-with-docs+synonym

A multilingual distribution of the `grill-with-docs` agent skill set — a
relentless, collaborative interview that sharpens a plan or design while
producing documentation (ADRs and a glossary) on the fly.

The `+synonym` suffix marks the signature feature: a **multi-synonym glossary
mechanism**. Canonical terms mirror the code; full synonyms (interchangeable in
every scenario) are recorded co-canonically via `/` (e.g. `Order / Заказ`,
`repo / repository`); partial synonyms are relegated to `_Avoid_`.

## What it does

- **English (en)** — A relentless yet collaborative interview that sharpens your
  plan or design while producing documentation on the fly: ADRs and a glossary
  with a multi-synonym mechanism (canonical terms mirror your code; full synonyms
  are joined with `/`).
- **Русский (ru)** — *source of truth.* Беспощадное, но коллаборативное интервью,
  которое оттачивает план или дизайн, попутно создавая документацию: ADR и
  глоссарий с механизмом множественных синонимов (канонические термины зеркалят
  код; полные синонимы объединяются через `/`).
- **简体中文 (zh)** — 一场毫不留情却又协作的拷问，在打磨计划或设计的同时随手产出文档：ADR
  与带有「多同义词」机制的术语表（规范术语映照代码；完全同义词以 `/` 连接）。
- **Español (es)** — Una entrevista despiadada pero colaborativa que afila tu plan
  o diseño mientras genera documentación sobre la marcha: ADR y un glosario con un
  mecanismo de sinónimos múltiples (los términos canónicos reflejan el código; los
  sinónimos completos se unen con `/`).
- **Français (fr)** — Un entretien sans pitié mais collaboratif qui affine ton plan
  ou ton design tout en produisant la documentation à la volée : des ADR et un
  glossaire doté d'un mécanisme de synonymes multiples (les termes canoniques
  reflètent le code ; les synonymes complets sont reliés par `/`).
- **Deutsch (de)** — Ein erbarmungsloses, aber kollaboratives Interview, das deinen
  Plan oder dein Design schärft und dabei nebenbei Dokumentation erzeugt: ADRs und
  ein Glossar mit einem Mehrfach-Synonym-Mechanismus (kanonische Begriffe spiegeln
  den Code; vollständige Synonyme werden mit `/` verbunden).

## Languages

| Folder | Language           | Glossary style                          |
|--------|--------------------|-----------------------------------------|
| `ru/`  | Russian            | bilingual (Russian + English) — **source of truth** |
| `zh/`  | Simplified Chinese | bilingual (Chinese + English)           |
| `es/`  | Spanish            | bilingual (Spanish + English)           |
| `fr/`  | French             | bilingual (French + English)            |
| `de/`  | German             | bilingual (German + English)            |
| `en/`  | English            | monolingual (English-only synonyms)     |

Russian is the source of truth; the other languages are derived from it.

## Install

Skills are discovered **flat** under `~/.agents/skills/` (one level deep). Copy
the three skill folders of **your** language there — do not nest the language
folder, and install only one language (the skill `name`s are identical across
languages):

```sh
# Example: Russian
cp -r ru/grill-with-docs   ~/.agents/skills/
cp -r ru/grilling          ~/.agents/skills/
cp -r ru/domain-modeling   ~/.agents/skills/
```

Resulting layout:

```
~/.agents/skills/
├── grill-with-docs/SKILL.md
├── grilling/SKILL.md
└── domain-modeling/
    ├── SKILL.md
    ├── ADR-FORMAT.md
    └── CONTEXT-FORMAT.md
```

## Skills

- **grill-with-docs** — entry point; runs a `/grilling` session using `/domain-modeling`.
- **grilling** — the interview behavior: full coverage, collaboration not
  interrogation, recommended answers, warn-then-yield.
- **domain-modeling** — builds the glossary (`CONTEXT.md`) and ADRs on the fly;
  owns the synonym mechanism.

## Fork & License

This is a fork of [mattpocock/skills](https://github.com/mattpocock/skills),
rewritten into multiple languages and extended with the multi-synonym glossary
mechanism. Original work Copyright (c) 2026 Matt Pocock.

Distributed under the [MIT License](./LICENSE) — see [LICENSE](./LICENSE) for the
full text. The original copyright and permission notice are preserved there.

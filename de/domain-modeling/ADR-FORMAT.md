# ADR-Format

ADRs liegen in `docs/adr/` und verwenden eine fortlaufende Nummerierung: `0001-slug.md`, `0002-slug.md` usw. Erzeuge das Verzeichnis `docs/adr/` lazy — erst, wenn das erste ADR gebraucht wird.

## Vorlage

```md
# {Kurzer Titel der Entscheidung}

{1–3 Sätze: welcher Kontext, was entschieden wurde und warum.}
```

Das ist alles. Ein ADR darf ein einzelner Absatz sein. Der Wert liegt darin festzuhalten, *dass* eine Entscheidung getroffen wurde und *warum* — nicht darin, Abschnitte auszufüllen.

## Optionale Abschnitte

Nimm sie nur auf, wenn sie tatsächlich Wert hinzufügen. Die meisten ADRs brauchen sie nicht.

- **Status** im Frontmatter (`proposed | accepted | deprecated | superseded by ADR-NNNN`) — nützlich, wenn man auf Entscheidungen zurückkommt
- **Geprüfte Alternativen** — nur, wenn die verworfenen Alternativen erinnerungswürdig sind
- **Konsequenzen** — nur, wenn nicht-offensichtliche Downstream-Effekte hervorzuheben sind

## Nummerierung

Durchsuche `docs/adr/` nach der höchsten vorhandenen Nummer und erhöhe sie um eins.

## Wann ein ADR vorschlagen

Alle drei Punkte müssen zutreffen:

1. **Schwer umkehrbar** — die Kosten, es später anders zu machen, sind spürbar
2. **Ohne Kontext nicht offensichtlich** — ein zukünftiger Leser schaut auf den Code und fragt «warum haben sie das so gemacht?»
3. **Ergebnis eines echten Abwägens** — es gab echte Alternativen, und ihr habt eine aus konkreten Gründen gewählt

Lässt sich die Entscheidung leicht umkehren — überspring sie, du kehrst sie einfach um. Überrascht sie nicht — wird niemand «warum» fragen. Gab es keine echte Alternative — gibt es nichts festzuhalten außer «wir haben das Offensichtliche getan».

### Was passt

- **Architekturform.** «Wir nutzen ein Monorepo.» «Das Schreibmodell ist event-sourced, das Lesemodell wird nach Postgres projiziert.»
- **Integrationsmuster zwischen Kontexten.** «Ordering und Billing kommunizieren über Domänen-Events, nicht per synchronem HTTP.»
- **Technologieentscheidungen mit Lock-in.** Datenbank, Message-Bus, Auth-Provider, Deployment-Target. Nicht jede Bibliothek — nur jene, deren Wechsel ein ganzes Quartal dauert.
- **Entscheidungen über Grenzen und Zuständigkeiten.** «Customer-Daten gehören zum Customer-Kontext; andere Kontexte verweisen nur per ID darauf.» Ein ausdrückliches «Nein» ist ebenso wertvoll wie ein «Ja».
- **Bewusste Abweichungen vom naheliegenden Weg.** «Wir nutzen pures SQL statt eines ORMs, weil X.» Alles, wobei ein vernünftiger Leser das Gegenteil annehmen würde. Das hält den nächsten Engineer davon ab, das zu «korrigieren», was absichtlich so gemacht wurde.
- **Einschränkungen, die im Code nicht sichtbar sind.** «Wir dürfen AWS aus Compliance-Gründen nicht nutzen.» «Die Antwortzeit muss wegen eines Partner-API-Vertrags unter 200 ms liegen.»
- **Verworfene Alternativen, wenn die Ablehnung nicht offensichtlich ist.** Habt ihr GraphQL erwogen und euch aus feinen Gründen für REST entschieden — haltet es fest, sonst schlägt in einem halben Jahr wieder jemand GraphQL vor.

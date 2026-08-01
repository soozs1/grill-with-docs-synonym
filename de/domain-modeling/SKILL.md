---
name: domain-modeling
description: Baue das Domänenmodell des Projekts auf und schleife es. Nutze ihn, wenn der Nutzer Domänen-Terminologie oder eine einheitliche Sprache (Ubiquitous Language) festhalten, eine Architekturentscheidung (ADR) aufzeichnen will oder wenn ein anderer Skill das Domänenmodell stützen muss.
---

# Domänenmodellierung

Baue das Domänenmodell des Projekts aktiv auf und schleife es im Zuge des Entwurfs. Dies ist eine *aktive* Disziplin: Begriffe hinterfragen, Grenzfälle ausdenken und Glossar sowie Entscheidungen genau in dem Moment festhalten, in dem sie sich herauskristallisieren. (`CONTEXT.md` bloß des Vokabulars wegen zu *lesen*, ist nicht dieser Skill; das ist eine Einzeiler-Gewohnheit, die jedem Skill offensteht. Dieser Skill greift, wenn du das Modell änderst, statt es nur zu nutzen.)

## Dateistruktur

Die meisten Repositories haben einen Kontext:

```
/
├── CONTEXT.md
├── docs/
│   └── adr/
│       ├── 0001-event-sourced-orders.md
│       └── 0002-postgres-for-write-model.md
└── src/
```

Liegt im Wurzelverzeichnis eine `CONTEXT-MAP.md`, hat das Repository mehrere Kontexte. Die Karte gibt an, wo jeder davon liegt:

```
/
├── CONTEXT-MAP.md
├── docs/
│   └── adr/                       ← systemweite Entscheidungen
├── src/
│   ├── ordering/
│   │   ├── CONTEXT.md
│   │   └── docs/adr/              ← Entscheidungen eines bestimmten Kontexts
│   └── billing/
│       ├── CONTEXT.md
│       └── docs/adr/
```

Erzeuge Dateien lazy — erst, wenn es etwas festzuhalten gibt. Gibt es noch kein `CONTEXT.md`, leg es an, sobald der erste Begriff feststeht. Gibt es noch kein `docs/adr/`, leg es an, sobald das erste ADR gebraucht wird.

## Sprache

Führe die Session in der Arbeitssprache des Nutzers (Default — German (Deutsch)).

Trenne im Glossar den *kanonischen Begriff* und seine *Definition*.

- **Der Kanon spiegelt den Code.** Halte den kanonischen Begriff so fest, wie er im Projekt lebt. Frage in einem frischen Projekt (Greenfield) einmal das Team und schlage dabei direkt bilinguale Varianten vor («nennen wir es *Order / Bestellung*?») — und halte die Wahl fest.
- **Vollständige Synonyme — ko-kanonisch, mit `/`.** Sind Begriffe bedeutungsgleich und in jedem Szenario austauschbar (review / Überprüfung, parsing / Parsen), schreib sie im Kanon mit `/` und Leerzeichen: `Order / Bestellung`. Es dürfen mehr als zwei sein.
- **Teilweise Synonyme — in `_Avoid_`.** Sind Begriffe ähnlich, divergieren aber in irgendeinem Szenario in der Bedeutung (Account / Nutzer / Kunde), wähle einen Kanon und verschiebe den Rest nach `_Avoid_`.
- **Reihenfolge = weiches Signal für Code.** Bei Konzepten mit Code-Bezug (sie bilden sich auf Klasse, Funktion, Tabelle, Modul, Datei ab) steht der englische/lateinische Begriff im Kanon zuerst: `Order / Bestellung`. Ein explizites Feld «Form für Code» gibt es nicht, und das ist keine Einschränkung: Der Agent orientiert sich bei Bezeichnern an der englischen Form, doch die Wahl der Benennungsmittel ist frei. Rein umgangssprachliche Konzepte dürfen Deutsch zuerst führen.
- **Definition** — in der Arbeitssprache, ein bis zwei Sätze, «was es ist». `_Avoid_` — eine kommagetrennte Liste (Reihenfolge egal), sie umfasst sowohl englische als auch deutsche abgelehnte Formen.

Das Ziel ist, dass die Begriffe im Glossar damit übereinstimmen, wie sie im Code und in der Sprache des Teams heißen. Bestätigte Begriffe verwendet der Agent wieder; die Persistenzschicht ist `CONTEXT.md`. Zieht das Team eine andere Konvention vor (ein rein deutschsprachiges Glossar, Transliteration in Bezeichnern) — ok, sei konsequent und halte die Wahl in einem ADR fest.

## Im Verlauf der Session

### Mit dem Glossar abgleichen

Wenn der Nutzer einen Begriff entgegen der bestehenden Sprache in `CONTEXT.md` verwendet, weise sofort darauf hin: «Im Glossar ist "Stornierung" als X definiert, aber du scheinst Y zu meinen — was davon stimmt?»

### Verschwommene Formulierungen schärfen

Wenn der Nutzer vage oder überladene Begriffe verwendet, schlage einen präzisen kanonischen Begriff vor: «Du sagst "Account" — meinst du Customer oder User? Das sind verschiedene Dinge.»

### Vollständige Synonyme festlegen

Hat ein Begriff Synonym-Kandidaten, schlag sie dem Nutzer vor: bestätigen, verwerfen oder zurückstellen («nennen wir es *grillen / grilling*, oder besprechen wir es später?»). «Zurückstellen» ist ein Escape-Hatch im Dialog; in den Glossar-Eintrag gelangen nur bestätigte Synonyme.

Die Heuristik ist der **Substitutionstest**: Begriffe sind genau dann vollständige Synonyme, wenn sie sich in jedem Satz und Szenario ohne Bedeutungsverschiebung vertauschen lassen. Kannst du ein Szenario konstruieren, in dem sie divergieren — **musst du es einmalig ausdrücklich vorbringen** («hier ist ein Szenario, in dem X und Y verschiedene Dinge sind; wirklich zusammenlegen?»). Das ist eine Risiko-Warnung, kein Veto: Die Entscheidung liegt beim Nutzer. Bestätigt er trotz Gegenbeispiel — schreiben wir das vollständige Synonym mit `/`. Verwirft er — ein Kanon, der Rest nach `_Avoid_`. Die Warnung erfolgt einmalig, ohne nachzubohren. Bestätigte Begriffe danach weiterverwenden und direkt in `CONTEXT.md` festhalten.

### Konkrete Szenarien besprechen

Wenn Beziehungen zwischen Domänenbegriffen diskutiert werden, prüfe sie an konkreten Szenarien. Denke dir Szenarien aus, die Grenzfälle abtasten und den Nutzer zwingen, die Grenzen zwischen Konzepten klar zu benennen.

### Mit dem Code abgleichen

Wenn der Nutzer behauptet, wie etwas funktioniert, prüfe, ob das mit dem Code übereinstimmt. Findest du einen Widerspruch — lege ihn offen: «Der Code storniert Bestellungen stets vollständig, aber du hast gerade gesagt, eine Teilstornierung sei möglich — was davon ist richtig?»

### CONTEXT.md direkt aktualisieren

Sobald ein Begriff feststeht, aktualisiere sofort `CONTEXT.md`. Sammle sie nicht stapelweise an — halte sie fest, sobald sie auftauchen. Nutze das Format aus [CONTEXT-FORMAT.md](./CONTEXT-FORMAT.md). `CONTEXT.md` muss vollständig frei von Implementierungsdetails sein. Mache `CONTEXT.md` weder zu einer Spezifikation noch zu einem Entwurf noch zu einem Speicher für Implementierungsentscheidungen. Es ist ein Glossar und nichts weiter.

### ADRs sparsam vorschlagen

Schlage nur dann ein ADR vor, wenn alle drei Punkte zutreffen:

1. **Schwer umkehrbar** — die Kosten, es später anders zu machen, sind spürbar
2. **Ohne Kontext nicht offensichtlich** — ein zukünftiger Leser wird fragen «warum haben sie das so gemacht?»
3. **Ergebnis eines echten Abwägens** — es gab echte Alternativen, und ihr habt eine aus konkreten Gründen gewählt

Fehlt auch nur einer der drei Punkte — überspringe das ADR. Nutze das Format aus [ADR-FORMAT.md](./ADR-FORMAT.md).

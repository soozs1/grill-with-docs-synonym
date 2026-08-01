# CONTEXT.md-Format

## Struktur

```md
# {Name des Kontexts}

{Ein bis zwei Sätze: was dieser Kontext ist und wozu er existiert.}

## Sprache

**Order / Bestellung**: {Ein bis zwei Sätze: was es ist}
_Avoid_: Purchase, transaction, Kauf

**Invoice / Rechnung**: Eine Zahlungsaufforderung, die nach der Lieferung an den Kunden gesendet wird.
_Avoid_: Bill, payment request

**Customer / Kunde**: Eine natürliche oder juristische Person, die Bestellungen aufgibt.
_Avoid_: Client, buyer, account, Käufer
```

## Regeln

- **Unterscheide vollständige und teilweise Synonyme.** Vollständige Synonyme (in jedem Szenario austauschbar: review / Überprüfung) schreib ko-kanonisch mit `/` und Leerzeichen. Teilweise (divergieren in irgendeinem Szenario: Kunde / Käufer) — eines in den Kanon, den Rest nach `_Avoid_`.
- **Bei Code-bezogenen Konzepten — Englisch zuerst.** Bildet sich ein Begriff auf Code ab (Klasse, Funktion, Tabelle, Datei), steht die englische/lateinische Variante im Kanon zuerst: `Order / Bestellung`. Das ist ein Signal, keine Einschränkung; ein explizites Feld «Form für Code» gibt es nicht.
- **Sei kategorisch.** Lege den Kanon fest — ein Begriff oder eine Menge vollständiger Synonyme mit `/` — und verschiebe alles Überflüssige nach `_Avoid_`.
- **Halte Definitionen kurz.** Höchstens ein bis zwei Sätze. Definiere, was es IST, nicht was es tut. Die Definition — in der Arbeitssprache des Teams.
- **Nimm nur Begriffe auf, die spezifisch für den Kontext dieses Projekts sind.** Allgemeine Programmierkonzepte (Timeouts, Fehlertypen, Utility-Pattern) gehören nicht hierher, selbst wenn das Projekt sie intensiv nutzt. Frag dich, bevor du einen Begriff hinzufügst: Ist dieses Konzept einzigartig für diesen Kontext oder ein allgemeines Programmierkonzept? Nur Ersteres gehört ins Glossar.
- **Gruppiere Begriffe unter Zwischenüberschriften**, wenn sich natürliche Cluster abzeichnen. Gehören alle Begriffe zu einem zusammenhängenden Gebiet, ist eine flache Liste in Ordnung.

## Repositories mit einem und mehreren Kontexten

**Ein Kontext (die meisten Repositories):** eine `CONTEXT.md` im Wurzelverzeichnis des Repositories.

**Mehrere Kontexte:** `CONTEXT-MAP.md` im Wurzelverzeichnis listet die Kontexte auf, wo sie liegen und wie sie miteinander zusammenhängen:

```md
# Kontextkarte

## Kontexte

- [Ordering](./src/ordering/CONTEXT.md) — nimmt Kundenbestellungen entgegen und verfolgt sie
- [Billing](./src/billing/CONTEXT.md) — stellt Rechnungen aus und verarbeitet Zahlungen
- [Fulfillment](./src/fulfillment/CONTEXT.md) — steuert Kommissionierung und Versand aus dem Lager

## Beziehungen

- **Ordering → Fulfillment**: Ordering publiziert `OrderPlaced`-Events; Fulfillment konsumiert sie, um die Kommissionierung zu starten
- **Fulfillment → Billing**: Fulfillment publiziert `ShipmentDispatched`-Events; Billing konsumiert sie, um eine Rechnung auszustellen
- **Ordering ↔ Billing**: Gemeinsame Typen für `CustomerId` und `Money`
```

Der Skill bestimmt selbst, welche Struktur zutrifft:

- Gibt es eine `CONTEXT-MAP.md` — liest er sie, um die Kontexte zu finden
- Gibt es nur eine `CONTEXT.md` im Wurzelverzeichnis — ein Kontext
- Gibt es weder das eine noch das andere — erzeugt er die `CONTEXT.md` im Wurzelverzeichnis lazy, sobald der erste Begriff feststeht

Gibt es mehrere Kontexte, bestimme, zu welchem das aktuelle Thema gehört. Ist es unklar — frag nach.

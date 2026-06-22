# AI-Prompts zur Testfallgenerierung – Billing Module

Die folgenden Prompts wurden mit ChatGPT/Claude verwendet, um Testfälle systematisch zu generieren.

---

## Prompt 1: Testfall-Generierung

```
Du bist QA-Ingenieur für eine SaaS-Billing-Plattform.
Erstelle vollständige Testfälle für folgende Anforderung:

B-REQ-02: "Monatliche/jährliche Abrechnung automatisch generieren"

Erstelle für jeden Testfall:
- ID (TC-BILL-XXX)
- Titel (max. 80 Zeichen)
- Vorbedingungen
- Testdaten
- Schritte (nummeriert)
- Erwartetes Ergebnis

Berücksichtige: Grenzfälle (erster Tag des Monats, Schaltjahr),
verschiedene Abonnement-Typen (Basic, Pro, Enterprise), 
Währungen, und Statusübergänge.
```

**Output:** 12 Testfälle (TC-BILL-001 bis TC-BILL-012)

---

## Prompt 2: Risikoanalyse

```
Analysiere folgende Anforderungen auf Risiken:

1. B-REQ-05: Stornierte/fehlgeschlagene Zahlungen behandeln
2. B-REQ-06: Mehrwertsteuer-Konformität für EU-Länder

Bewerte jedes Risiko (Niedrig/Mittel/Hoch) und schlage
Gegenmaßnahmen vor. Fokussiere auf:
- Datenkonsistenz
- Compliance-Risiken
- UX bei Fehlern
- Performance-Impacts
```

**Ergebnis:** Siehe `risk-analysis.md`

---

## Prompt 3: Negative Testfälle

```
Generiere NEGATIVE Testfälle für das Billing-Modul.
Fokus auf Edge Cases und feindliche Eingaben:

- Ungültige Kreditkartennummern (Luhn-Check)
- Abgelaufene Karten
- Nicht-unterstützte Länder
- SQL-Injection-Versuche in Rechnungsdaten
- Gleichzeitige Doppelbuchungen (Race Conditions)
- Sonderzeichen in Firmen-/Adressfeldern

Pro Szenario: Kurzbeschreibung + Erwartung
```

**Output:** 8 negative Testfälle dokumentiert in `test-cases.xlsx`

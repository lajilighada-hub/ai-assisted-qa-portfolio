# Defect Reports – Billing Module

## BUG-001: Rechnungs-PDF enthält falsches Datum

| Feld | Wert |
|------|------|
| **ID** | BUG-BILL-001 |
| **Titel** | Rechnungs-PDF zeigt Datum des Generierungszeitpunkts statt Rechnungsstellungsdatum |
| **Schweregrad** | Medium |
| **Priorität** | High |
| **Status** | ✅ Fixed |
| **Getestet von** | Ghassen A. |
| **Datum** | 2026-06-15 |

**Schritte zum Reproduzieren:**
1. Rechnung manuell für 01.06.2026 erstellen
2. PDF-Vorschau öffnen
3. Datumsfeld prüfen

**Tatsächlich:** PDF zeigt "16.06.2026" (Generierungsdatum)
**Erwartet:** "01.06.2026" (Rechnungsstellungsdatum)
**Root Cause:** Template-Variable `{{generated_at}}` statt `{{invoice_date}}`

---

## BUG-002: MwSt-Berechnung Schweiz falsch

| Feld | Wert |
|------|------|
| **ID** | BUG-BILL-002 |
| **Titel** | Schweiz (CH) wird mit deutschem MwSt-Satz berechnet |
| **Schweregrad** | High |
| **Priorität** | Critical |
| **Status** | ✅ Fixed |
| **Datum** | 2026-06-15 |

**Schritte:**
1. Kunde mit Rechnungsadresse Schweiz anlegen
2. Rechnung generieren
3. Steuerberechnung prüfen

**Tatsächlich:** 19 % (deutscher Satz) statt 8.1 % (CH)
**Erwartet:** 8.1 % Mehrwertsteuer Schweiz
**Root Cause:** Fallback auf Default-Länder-Steuersatz (DE) bei unbekanntem Land

---

## BUG-003: PayPal-Zahlung ohne Rückmeldung bei Timeout

| Feld | Wert |
|------|------|
| **ID** | BUG-BILL-003 |
| **Titel** | Bei PayPal-Timeout bleibt UI in "Verarbeitung" hängen |
| **Schweregrad** | Medium |
| **Priorität** | Medium |
| **Status** | 🔄 Open |
| **Datum** | 2026-06-16 |

**Schritte:**
1. Im Testmodus PayPal künstlich verlangsamen
2. Zahlung auslösen
3. Browser-Tab überwachen

**Tatsächlich:** Button zeigt endlos "Verarbeitung..." ohne Timeout
**Erwartet:** Nach 30s: "Zahlung fehlgeschlagen – bitte erneut versuchen"
**Root Cause:** Kein Frontend-Timeout implementiert; API-Call hängt

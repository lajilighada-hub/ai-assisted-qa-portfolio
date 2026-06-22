# Risk-Analyse – Billing Module

## Risikobewertung: Zahlungsausfälle (B-REQ-05)

| Risiko | Wahrscheinlichkeit | Auswirkung | Risikostufe | Gegenmaßnahme |
|--------|-------------------|------------|-------------|---------------|
| Karte abgelehnt (unzureichendes Guthaben) | Hoch | Mittel | **Hoch** | Retry-Logik + Benachrichtigung |
| Doppelabbuchung bei Retry | Mittel | Hoch | **Hoch** | Idempotenz-Key pro Zahlung |
| Abbuchung nach Account-Deaktivierung | Niedrig | Hoch | **Mittel** | Status-Check vor Zahlung |
| PayPal-API-Timeout | Mittel | Mittel | **Mittel** | Timeout-Konfiguration + Queue |
| SEPA-Lastschrift-Rücklastschrift | Niedrig | Hoch | **Mittel** | Gebühren-Weitergabe, Mahnwesen |

## Risikobewertung: EU-Mehrwertsteuer (B-REQ-06)

| Risiko | Wahrscheinlichkeit | Auswirkung | Risikostufe | Gegenmaßnahme |
|--------|-------------------|------------|-------------|---------------|
| Falscher Steuersatz (Länderupdate verpasst) | Mittel | Sehr Hoch | **Hoch** | Autom. Steuersatz-API (z.B. VIES) |
| Kein VAT-ID-Check bei B2B | Niedrig | Hoch | **Mittel** | VAT-Validierung via EU-API |
| Steuersatz-Änderung zum 01.07. eines Jahres | Mittel | Mittel | **Mittel** | Stichtagskonfiguration + Test |
| Falsche Währungsumrechnung | Niedrig | Mittel | **Niedrig** | Tagesaktueller Kurs + Rundungslogik |

## Empfohlene Teststrategie

1. **Smoke-Tests** – Kernablauf: Zahlung → Rechnung → PDF
2. **Negative Tests** – Alle Fehlerszenarien abdecken
3. **Integrationstests** – Stripe/PayPal-Sandbox
4. **Compliance-Tests** – MwSt-Sätze je EU-Land
5. **Performance** – 100 gleichzeitige Rechnungen

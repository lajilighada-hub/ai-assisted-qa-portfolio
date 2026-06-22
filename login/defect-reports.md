# Defect Reports – Login Module

## BUG-004: Remember-Me-Token wird bei Passwortänderung nicht invalidiert

| Feld | Wert |
|------|------|
| **ID** | BUG-LOG-001 |
| **Titel** | Remember-Me bleibt nach Passwortänderung gültig |
| **Schweregrad** | High |
| **Priorität** | Critical |
| **Status** | ✅ Fixed |
| **Datum** | 2026-06-14 |

**Schritte:**
1. Einloggen mit "Angemeldet bleiben"
2. Passwort über "Passwort vergessen" ändern
3. Alten Browser-Tab schließen, neuen öffnen
4. Seite laden (automatischer Login via Remember-Me-Cookie)

**Tatsächlich:** Benutzer bleibt automatisch eingeloggt
**Erwartet:** Alle Sessions (außer aktuelle) sollten invalidiert werden
**Root Cause:** Remember-Me-Token prüft nur Existenz, nicht `token_version` im User-Objekt
**Fix:** `token_version`-Feld in DB erhöhen bei Passwortänderung

---

## BUG-005: Google Login – Abbruch schlägt nicht fehl genug

| Feld | Wert |
|------|------|
| **ID** | BUG-LOG-002 |
| **Titel** | Abgebrochener Google Login zeigt "Erfolgreich" |
| **Schweregrad** | Medium |
| **Priorität** | High |
| **Status** | ✅ Fixed |
| **Datum** | 2026-06-14 |

**Schritte:**
1. "Mit Google anmelden" klicken
2. Im Google-Popup auf "Abbrechen" klicken
3. Redirect abwarten

**Tatsächlich:** Frontend zeigt "Erfolgreich angemeldet" + leeres Dashboard
**Erwartet:** "Anmeldung abgebrochen" + Zurück zum Login-Formular
**Root Cause:** Kein Error-Handling für `access_denied`-Response vom OAuth-Provider

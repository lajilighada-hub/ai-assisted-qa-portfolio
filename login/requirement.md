# Login Module – Anforderungsspezifikation

## Übersicht

Authentifizierungssystem mit E-Mail/Passwort, Social Login (Google, GitHub) und optionalem 2FA.

## Funktionale Anforderungen

| ID | Beschreibung | Priorität |
|----|-------------|-----------|
| L-REQ-01 | Login mit E-Mail und Passwort | Hoch |
| L-REQ-02 | Social Login (Google OAuth 2.0) | Hoch |
| L-REQ-03 | Social Login (GitHub OAuth) | Mittel |
| L-REQ-04 | Passwort vergessen – Reset per E-Mail | Hoch |
| L-REQ-05 | 2-Faktor-Authentifizierung (TOTP) | Mittel |
| L-REQ-06 | Session-Timeout nach 30 Minuten Inaktivität | Hoch |
| L-REQ-07 | Login-Versuch-Sperre nach 5 Fehlversuchen | Hoch |
| L-REQ-08 | "Angemeldet bleiben"-Funktion (Remember Me) | Niedrig |
| L-REQ-09 | Logout von allen Geräten | Mittel |
| L-REQ-10 | Passwort-Komplexitätsregeln (min. 8 Zeichen, Sonderzeichen) | Hoch |

## Nicht-funktionale Anforderungen

| ID | Beschreibung |
|----|-------------|
| L-NFR-01 | Login-Vorgang < 2s (bei Normalbetrieb) |
| L-NFR-02 | Brute-Force-Schutz via Rate-Limiting |
| L-NFR-03 | Passwörter gehasht (bcrypt, Kostenfaktor ≥ 12) |
| L-NFR-04 | DSGVO-konforme Session-Handhabung |

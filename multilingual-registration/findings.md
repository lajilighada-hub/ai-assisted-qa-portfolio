# Findings – Multilingual Registration

## Lokalisierungs-Testergebnisse

### Sprach-UI Korrektheit

| Sprache | UI-Texte | Fehlermeldungen | Placeholder | RTL |
|---------|----------|-----------------|-------------|-----|
| 🇩🇪 Deutsch | ✅ | ✅ | ✅ | – |
| 🇬🇧 English | ✅ | ✅ | ✅ | – |
| 🇫🇷 Français | ✅ | ✅ | ⚠️ 2 fehlerhafte | – |
| 🇪🇸 Español | ✅ | ✅ | ✅ | – |
| 🇮🇹 Italiano | ✅ | ⚠️ 1 unvollständig | ✅ | – |
| 🇳🇱 Nederlands | ⚠️ 3 fehlende Keys | ⚠️ 1 fehlend | ✅ | – |
| 🇵🇱 Polski | ✅ | ✅ | ✅ | – |
| 🇸🇦 العربية | ✅ | ✅ | ✅ | ✅ |

### RTL-Findings

**FIND-RTL-01:** Arabische UI spiegelt korrekt – jedoch bleibt Loading-Spinner links
→ **Severity:** Low | **Status:** 🔄 Open

**FIND-RTL-02:** Kalender-Widget zeigt Wochentage von links nach rechts
→ **Severity:** Medium | **Status:** ✅ Fixed (Locale-Prop hinzugefügt)

### Validierungs-Findings

| Finding | Sprache | Beschreibung | Status |
|---------|---------|-------------|--------|
| PLZ-Format | 🇫🇷 Frankreich | Erwartet 5-stellig, akzeptiert auch 4-stellig | ✅ Fixed |
| Telefon-Präfix | 🇳🇱 Niederlande | +31-Code nicht im Dropdown | ✅ Fixed |
| Datum-Parser | 🇵🇱 Polen | dd.mm.yyyy wird als mm.dd.yyyy gelesen | 🔄 Open |
| Namenssonderzeichen | 🇸🇦 Arabisch | ß/é/ü im Namen blockiert | ✅ Fixed |

### Empfehlungen

1. **i18next-Dateien automatisiert auf Vollständigkeit prüfen** (GitHub Action)
2. **RTL-Testing in BrowserStack** auf echten arabischen Geräten
3. **CrowdIn/POEditor** für Community-Übersetzungen
4. **Lokalisierte Screenshot-Tests** in der CI-Pipeline

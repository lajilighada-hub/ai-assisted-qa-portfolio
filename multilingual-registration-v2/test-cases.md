# Multilingual Customer Registration – Test Cases

## Test Data per Language

| Field | EN | DE | FR | AR |
|-------|----|----|----|----|
| Title | Mr / Ms | Herr / Frau | M. / Mme | السيّد / السيّدة |
| First Name | John | Max | Jean | يوسف |
| Last Name | Smith | Müller | Dupont | عبدالله |
| Email | john@test.com | max@test.de | jean@test.fr | youssef@test.sa |
| Phone | +1 555 123456 | +49 171 1234567 | +33 6 12 34 56 78 | +966 50 123 4567 |
| Street | 123 Main St | Hauptstr. 42 | 12 Rue de Paris | شارع الملك فهد |
| City | New York | Berlin | Paris | الرياض |
| ZIP | 10001 | 10115 | 75001 | 11564 |
| Date | 06/23/2026 | 23.06.2026 | 23/06/2026 | 2026/06/23 |
| Currency | $1,234.50 | 1.234,50 € | 1 234,50 € | ١٬٢٣٤٫٥٠ ر.س |

## Test Cases (15)

### Character Display & Encoding

| ID | Title | Language | Steps | Expected Result | Type |
|----|-------|----------|-------|----------------|------|
| LOC-001 | All UI strings render in English | EN | 1. Select EN 2. Observe all labels, buttons, errors | Full English UI: "Register", "First Name", "Submit" | Positive |
| LOC-002 | All UI strings render in German | DE | 1. Select DE 2. Observe all labels, buttons, errors | Full German UI: "Registrierung", "Vorname", "Absenden" | Positive |
| LOC-003 | All UI strings render in French | FR | 1. Select FR 2. Observe all labels, buttons, errors | Full French UI: "Inscription", "Prénom", "Envoyer" | Positive |
| LOC-004 | Arabic characters display correctly | AR | 1. Select AR 2. Observe all text | Arabic script renders correctly, no boxes/????, right-aligned UI | Positive |
| LOC-005 | Special French characters (é, è, ê, ï, ô, û, ç) | FR | Enter: "François Noël Côte-d'Île" in name field | Characters display correctly, no encoding issues | Positive |
| LOC-006 | Special German characters (ä, ö, ü, ß) | DE | Enter: "Müller Straße Ärger" in name/street field | Umlauts and ß display and save correctly | Positive |
| LOC-007 | Arabic diacritics (tashkeel) | AR | Enter: "مُحَمَّد" with diacritics | Characters with tashkeel display and save correctly | Positive |

### Right-to-Left (RTL) Arabic

| ID | Title | Steps | Expected Result | Type |
|----|-------|-------|----------------|------|
| LOC-008 | Full RTL layout for Arabic | 1. Select AR 2. Observe layout | All text right-aligned, input fields right-to-left, arrows/icons flipped | Positive |
| LOC-009 | Mixed RTL/LTR content (phone number) | 1. AR mode 2. Enter "+966 50 123 4567" | Phone number left-to-right inside RTL form, correct alignment | Positive |
| LOC-010 | RTL date picker | 1. AR mode 2. Open date picker | Days displayed right-to-left, Arabic month names, Saturday first | Positive |

### Mandatory Fields & Validation

| ID | Title | Language | Steps | Expected Result | Type |
|----|-------|----------|-------|----------------|------|
| LOC-011 | All mandatory fields required in each language | EN, DE, FR, AR | 1. Select each language 2. Submit empty form | 4 error messages in correct language, all mandatory fields highlighted | Positive |
| LOC-012 | Mandatory field indicator position in RTL | AR | 1. AR mode 2. Observe required field markers | Asterisk * on left side of label (opposite of LTR), visually correct | Positive |

### Error Message Translations

| ID | Title | Steps | Expected Result | Type |
|----|-------|-------|----------------|------|
| LOC-013 | Error messages in selected language | 1. Select DE 2. Enter invalid email "abc" 3. Submit | Error in German: "Bitte geben Sie eine gültige E-Mail-Adresse ein" | Positive |
| LOC-014 | Error messages switch on language change | 1. Submit with errors in FR 2. Switch to EN without clearing | Errors update to English immediately, no page reload needed | Positive |

### Date & Number Formats

| ID | Title | Steps | Expected Result | Type |
|----|-------|-------|----------------|------|
| LOC-015 | Locale-specific date format validation | 1. Switch to DE 2. Enter "01.02.2026" | Accepted (DD.MM.YYYY in DE). Switch to EN: "02/01/2026" needed | Positive |

## Traceability Matrix

| Requirement | Test Case IDs | Coverage |
|-------------|--------------|----------|
| English UI | LOC-001, LOC-011, LOC-013, LOC-014, LOC-015 | ✅ |
| German UI | LOC-002, LOC-006, LOC-011, LOC-013, LOC-014, LOC-015 | ✅ |
| French UI | LOC-003, LOC-005, LOC-011, LOC-013, LOC-014 | ✅ |
| Arabic UI | LOC-004, LOC-007, LOC-008, LOC-009, LOC-010, LOC-011, LOC-012 | ✅ |
| Character display (accents, umlauts, Arabic) | LOC-005, LOC-006, LOC-007 | ✅ |
| RTL layout | LOC-008, LOC-009, LOC-010, LOC-012 | ✅ |
| Mandatory field validation | LOC-011, LOC-012 | ✅ |
| Error message translations | LOC-013, LOC-014 | ✅ |
| Date and number formats | LOC-015 | ✅ |

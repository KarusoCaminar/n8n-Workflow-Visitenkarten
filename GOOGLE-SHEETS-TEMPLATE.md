# 📊 Google Sheets Template

## Schnell-Anleitung

1. **Erstelle neues Google Sheet**
2. **Benenne das Sheet:** `Contacts` (wichtig - case-sensitive!)
3. **Erstelle folgende Spalten in Zeile 1:**

---

## Spalten-Struktur

### Header-Zeile (Row 1):

| A | B | C | D | E | F | G | H | I | J | K | L | M |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| name | company | title | email | phone | website | address | notes | quality | plausibility_score | created_at | last_updated | scan_count |

---

## Spalten-Details

### name (A)
- **Type:** Text
- **Beschreibung:** Vollständiger Name (Vor- und Nachname)
- **Beispiel:** `Max Mustermann`

### company (B)
- **Type:** Text
- **Beschreibung:** Firmenname
- **Beispiel:** `Kortex System GmbH`

### title (C)
- **Type:** Text
- **Beschreibung:** Jobtitel / Position
- **Beispiel:** `CEO`, `Software Engineer`, `Marketing Manager`

### email (D)
- **Type:** Email (Formatierung empfohlen)
- **Beschreibung:** E-Mail-Adresse
- **Beispiel:** `max@kortex-system.de`
- **Formatierung:** Format → Number → Plain text (oder Email)

### phone (E)
- **Type:** Text
- **Beschreibung:** Telefonnummer mit Ländercode
- **Beispiel:** `+49 123 456789`, `+1-555-123-4567`

### website (F)
- **Type:** URL (Formatierung empfohlen)
- **Beschreibung:** Firmen-Website
- **Beispiel:** `https://kortex-system.de`
- **Formatierung:** Format → Number → Plain text

### address (G)
- **Type:** Text
- **Beschreibung:** Vollständige Adresse
- **Beispiel:** `Hauptstraße 123, 12345 Berlin, Deutschland`

### notes (H)
- **Type:** Text (Multiline)
- **Beschreibung:** Zusätzliche Notizen, Besonderheiten
- **Beispiel:** `VIP Kunde`, `Follow-up in 2 Wochen`

### quality (I)
- **Type:** Text
- **Beschreibung:** Qualitäts-Score (automatisch berechnet)
- **Werte:** `✅ Sehr gut`, `⚠️ OK`, `❌ Niedrig`

### plausibility_score (J)
- **Type:** Number
- **Beschreibung:** Numerischer Score (0-100)
- **Beispiel:** `85`, `60`, `45`
- **Formatierung:** Format → Number → Number

### created_at (K)
- **Type:** Date/Time
- **Beschreibung:** Zeitstempel der Erstellung
- **Beispiel:** `2025-10-30T12:30:45.000Z`
- **Formatierung:** Format → Number → Date time

### last_updated (L)
- **Type:** Date/Time
- **Beschreibung:** Zeitstempel der letzten Aktualisierung
- **Beispiel:** `2025-10-30T15:20:10.000Z`
- **Formatierung:** Format → Number → Date time

### scan_count (M)
- **Type:** Number
- **Beschreibung:** Anzahl der Scans (bei Updates erhöht)
- **Beispiel:** `1`, `3`, `5`
- **Formatierung:** Format → Number → Number

---

## 🎨 Empfohlene Formatierung

### Header-Zeile schön machen:
1. **Zeile 1 markieren**
2. **Format → Text → Bold**
3. **Hintergrundfarbe:** Hellblau oder Grau
4. **Text-Farbe:** Weiß (bei dunklem Hintergrund)
5. **Freeze:** View → Freeze → 1 row (Header bleibt oben)

### Spalten-Breite:
- **A (name):** 150px
- **B (company):** 150px
- **C (title):** 120px
- **D (email):** 180px
- **E (phone):** 120px
- **F (website):** 150px
- **G (address):** 200px
- **H (notes):** 200px
- **I (quality):** 100px
- **J (plausibility_score):** 80px
- **K (created_at):** 150px
- **L (last_updated):** 150px
- **M (scan_count):** 80px

### Bedingte Formatierung (Quality):
1. **Spalte I markieren**
2. **Format → Conditional formatting**
3. **Rule 1:** Text contains `✅` → Hintergrund Grün (#d4edda)
4. **Rule 2:** Text contains `⚠️` → Hintergrund Orange (#fff3cd)
5. **Rule 3:** Text contains `❌` → Hintergrund Rot (#f8d7da)

---

## 📋 Beispiel-Daten (zum Testen)

Füge in Zeile 2+ ein:

| name | company | title | email | phone | website | address | notes | quality | plausibility_score | created_at | last_updated | scan_count |
|------|---------|-------|-------|-------|---------|---------|-------|---------|-------------------|------------|--------------|------------|
| Max Mustermann | Kortex System | CEO | max@kortex.de | +49 123 456789 | https://kortex-system.de | Berlin, DE | Demo Entry | ✅ Sehr gut | 95 | 2025-10-30T10:00:00Z | 2025-10-30T10:00:00Z | 1 |

---

## 🔒 Berechtigungen

### Option 1: Privat (empfohlen)
- Nur du hast Zugriff
- n8n nutzt OAuth2 (dein Google-Account)
- Sicher für echte Kundendaten

### Option 2: Eingeschränkt
- **Share → Add people:** Nur bestimmte Emails
- **Role:** Editor (für n8n)
- Viewer (für andere Team-Mitglieder)

### Option 3: Öffentlich (nur für Demo!)
- **Share → Get link → Anyone with the link → Viewer**
- ⚠️ **NICHT für echte Kundendaten!**

---

## 🔍 Sheet-ID finden

1. Öffne dein Google Sheet
2. URL sieht so aus:
```
https://docs.google.com/spreadsheets/d/1AbC2dEf3GhI4jKl5MnO6pQr7StU8vWx9YzA/edit
                                        ↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑
                                        Das ist deine SHEET-ID!
```
3. Kopiere diesen Teil
4. Füge in n8n ein (alle Google Sheets Nodes)

---

## 📊 Erweiterte Features

### Charts erstellen
1. **Insert → Chart**
2. **Chart type:** Bar chart
3. **Data range:** B2:B (Company column)
4. **Aggregate:** COUNT
5. Zeigt: Top-Firmen nach Anzahl Kontakte

### Pivot Table
1. **Data → Pivot table**
2. **Rows:** Company
3. **Values:** COUNT of name
4. Zeigt: Kontakte pro Firma

### Filter Views
1. **Data → Filter views → Create new filter view**
2. **Filter by:** Quality = "✅ Sehr gut"
3. Zeigt: Nur hochwertige Kontakte

---

## 🚨 Wichtige Hinweise

### ⚠️ Spalten NICHT umbenennen!
Der n8n Workflow erwartet **exakt diese Spaltennamen**:
- `name` nicht `Name` oder `Full Name`
- `email` nicht `Email` oder `E-Mail`
- etc.

### ⚠️ Sheet-Name muss "Contacts" sein!
- Nicht `Kontakte`, `contacts`, oder `Sheet1`
- Genau: `Contacts` (mit großem C)

### ⚠️ Erste Zeile = Header!
- Zeile 1 sind die Spalten-Namen
- Daten starten in Zeile 2
- n8n überspringt automatisch Header

---

## ✅ Quick Setup Checklist

- [ ] Google Sheet erstellt
- [ ] Sheet umbenannt zu `Contacts`
- [ ] Alle 13 Spalten in Zeile 1 eingefügt
- [ ] Spalten-Namen exakt wie Template (copy-paste!)
- [ ] Header-Zeile formatiert (Bold, Farbe)
- [ ] Zeile 1 fixiert (Freeze)
- [ ] Sheet-ID kopiert
- [ ] Berechtigung geprüft (du hast Editor-Rechte)
- [ ] Test-Zeile eingefügt (optional)

---

## 🎉 Fertig!

Dein Google Sheet ist jetzt bereit für den n8n Workflow!

**Nächster Schritt:** Sheet-ID in n8n Workflow einfügen (siehe SETUP-ANLEITUNG.md)

---

*Template Version 1.0 - Kortex System - 30.10.2025*

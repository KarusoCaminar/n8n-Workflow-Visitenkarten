# 🎴 Kortex Visitenkarten Scanner - Setup-Anleitung

## 📋 Übersicht

Dieser Workflow scannt Visitenkarten mit **Vertex AI (Gemini 2.5 Flash)**, extrahiert automatisch alle Kontaktdaten, prüft Plausibilität und speichert sie in **Google Sheets** mit automatischer Deduplizierung.

---

## 🚀 Schnellstart (3 Schritte)

### 1️⃣ Google Sheets vorbereiten

**Erstelle ein neues Google Sheet mit folgenden Spalten:**

| name | company | title | email | phone | website | address | notes | quality | plausibility_score | created_at | last_updated | scan_count |
|------|---------|-------|-------|-------|---------|---------|-------|---------|-------------------|------------|--------------|------------|

**Sheet-Name:** `Contacts` (wichtig!)

**Tipps:**
- Erste Zeile = Header (Namen genau wie oben)
- Spalten können formatiert werden (z.B. Email als Link)
- Sheet kann öffentlich lesbar gemacht werden oder privat bleiben

**Sheet-ID finden:**
- URL deines Sheets: `https://docs.google.com/spreadsheets/d/DEINE_SHEET_ID/edit`
- Kopiere die SHEET_ID

---

### 2️⃣ n8n Workflow importieren

1. **In n8n einloggen:** https://n8n2.kortex-system.de/
   - Email: `austen.bittner@kortex-system.com`
   - Password: `openKS1N8n.`

2. **Workflow importieren:**
   - Klicke oben rechts auf **"+"** → **"Import from File"**
   - Wähle: `n8n-workflow-visitenkarten-vertex.json`
   - Workflow wird geladen

3. **Credentials konfigurieren:**

#### A) Vertex AI Credentials
   - Node: **"Call Vertex AI (Gemini 2.5 Flash)"**
   - Du hast bereits Vertex AI Credentials eingerichtet
   - Stelle sicher: 
     - **Model:** `gemini-2.0-flash-exp` (oder `gemini-1.5-flash`)
     - **Region:** z.B. `us-central1` oder `europe-west1`
     - **Project ID:** Deine GCP Project ID

#### B) Google Sheets Credentials
   - Nodes: **"Lookup Existing Contacts"**, **"Update Row"**, **"Append New Row"**, **"Get All Contacts"**
   - Credential-Type: `Google Sheets OAuth2 API`
   - Falls noch nicht vorhanden:
     1. Klicke auf **"+ New Credential"**
     2. Wähle **"Google Sheets OAuth2 API"**
     3. Folge OAuth2-Flow (Google-Login)
     4. Berechtigung erteilen

4. **Sheet-ID eintragen:**
   - Suche in allen Google Sheets Nodes nach: `YOUR_GOOGLE_SHEET_ID`
   - Ersetze mit deiner echten Sheet-ID
   - Betrifft 4 Nodes:
     - "Lookup Existing Contacts"
     - "Update Row"
     - "Append New Row"
     - "Get All Contacts"

5. **Workflow aktivieren:**
   - Oben rechts Toggle auf **"Active"**
   - Webhook wird automatisch erstellt

---

### 3️⃣ Frontend deployen

1. **Datei:** `frontend-kortex.html`

2. **Optionen:**
   - **Hostinger Upload:** Lade `frontend-kortex.html` in dein Web-Verzeichnis hoch
   - **Subdomain:** z.B. `https://visitenkarten.kortex-system.de/`
   - **Test lokal:** Öffne HTML-Datei direkt im Browser (funktioniert auch!)

3. **Fertig!** 🎉
   - Öffne die Seite
   - Klicke auf Demo-Karten oder lade eigene hoch
   - Kontakte landen automatisch in Google Sheets

---

## 📊 Endpoints

### 1. Visitenkarten-Upload
**URL:** `https://n8n2.kortex-system.de/webhook/business-card`  
**Methode:** POST  
**Payload:** `multipart/form-data`
- `data` (file) - Für File-Upload
- `imageUrl` (string) - Für URL-basierte Bilder

**Response:**
```json
{
  "success": true,
  "action": "create|update",
  "message": "Neuer Kontakt erstellt",
  "data": {
    "name": "Max Mustermann",
    "company": "Beispiel GmbH",
    "email": "max@beispiel.de",
    ...
  },
  "timestamp": "2025-10-30T12:00:00.000Z",
  "table_url": "https://n8n2.kortex-system.de/webhook/view-contacts"
}
```

### 2. Kontakte-Tabelle anzeigen
**URL:** `https://n8n2.kortex-system.de/webhook/view-contacts`  
**Methode:** GET  
**Response:** Schöne HTML-Tabelle mit allen Kontakten

---

## 🔧 Workflow-Details

### Ablauf:

1. **Webhook Start** → Empfängt Bild (Upload oder URL)
2. **Check Input Type** → Unterscheidet zwischen File und URL
3. **Fetch/Prepare** → Lädt Bild oder bereitet Upload vor
4. **Merge** → Vereint beide Pfade
5. **Convert to Base64** → Konvertiert für Vertex AI
6. **Call Vertex AI** → Gemini 2.5 Flash analysiert Visitenkarte
7. **Parse Response** → Extrahiert JSON-Felder
8. **Plausibility Check** → Validiert Email, Name, Phone, Website
9. **Lookup Sheets** → Sucht nach Duplikaten
10. **Dedupe & Merge** → Entscheidet: Update oder Create
11. **Update/Create** → Schreibt in Google Sheets
12. **Build Response** → Erstellt Antwort für Frontend
13. **Respond to Webhook** → Sendet JSON zurück

### Zweiter Workflow (Tabellen-Ansicht):

1. **Webhook View** → GET-Request
2. **Get All Contacts** → Lädt alle Zeilen aus Sheet
3. **Generate HTML** → Erstellt schöne Tabelle
4. **Respond HTML** → Zeigt Tabelle im Browser

---

## 🔐 Sicherheit & Performance

### ✅ Best Practices

1. **Rate Limiting:**
   - n8n hat eingebautes Rate Limiting
   - Für Production: Webhook mit IP-Whitelist absichern

2. **Credentials:**
   - ✅ Niemals in Code hardcoden
   - ✅ Nutze n8n Credentials Manager
   - ✅ Vertex AI Key ist sicher gespeichert

3. **CORS:**
   - ✅ Aktuell: `Access-Control-Allow-Origin: *`
   - Für Production: Beschränke auf deine Domain

4. **Kosten sparen:**
   - Vertex AI = Pay-per-Use
   - Gemini 2.5 Flash = günstigstes Modell
   - Bilder werden vor Upload optimiert (Base64)

5. **Google Sheets Limits:**
   - Max 5 Million Zellen pro Sheet
   - Max 50.000 Zeilen empfohlen
   - Bei mehr: Wechsel zu Postgres/MySQL

---

## 🐛 Troubleshooting

### Problem: Workflow startet nicht
**Lösung:**
- Prüfe ob Workflow **aktiviert** ist (Toggle oben rechts)
- Prüfe Webhook-URL: `https://n8n2.kortex-system.de/webhook/business-card`
- Test mit CURL:
```bash
curl -X POST https://n8n2.kortex-system.de/webhook/business-card \
  -F "imageUrl=https://example.com/card.jpg"
```

### Problem: Vertex AI Error
**Lösung:**
- Prüfe Credentials: Node "Call Vertex AI" → Credentials
- Prüfe Region in der URL (muss mit Credentials übereinstimmen)
- Prüfe Model-Name: `gemini-2.0-flash-exp` verfügbar?
- Fallback: `gemini-1.5-flash` nutzen

### Problem: Google Sheets Error
**Lösung:**
- Prüfe Sheet-ID korrekt eingefügt
- Prüfe Sheet-Name = `Contacts` (case-sensitive!)
- Prüfe Spalten exakt wie Template
- Prüfe Credentials haben Schreibrechte

### Problem: Frontend zeigt Fehler
**Lösung:**
- Öffne Browser Console (F12)
- Prüfe CORS-Fehler (dann n8n Webhook CORS anpassen)
- Prüfe Webhook-URL korrekt
- Teste Webhook direkt (siehe oben)

### Problem: Duplikate werden nicht erkannt
**Lösung:**
- Prüfe ob Email-Feld gefüllt ist
- Normalisierung prüft: Email > Name+Company
- Code in "Dedupe & Merge" Node anpassen wenn nötig

### Problem: Plausibility Score zu niedrig
**Lösung:**
- Bild-Qualität verbessern (höhere Auflösung)
- Prompt in "Call Vertex AI" anpassen
- Schwellwert in "Plausibility Check" anpassen (Zeile 50)

---

## 🎨 Anpassungen

### Frontend anpassen

**Logo ändern:**
```html
<div class="logo">🏢</div>  <!-- Emoji hier ändern -->
```

**Farben ändern:**
```css
background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
/* Ersetze mit deinen Brand-Colors */
```

**Mehr Demo-Karten:**
```javascript
const DEMO_CARDS = {
    card1: 'URL1',
    card2: 'URL2',
    card3: 'URL3'  // Neue Karte
};
```
Dann HTML-Block duplizieren und `card3` verwenden.

### Workflow anpassen

**Mehr Felder extrahieren:**
1. Prompt in "Call Vertex AI" erweitern:
```
...LinkedIn URL, Fax, Mobilnummer, etc.
```

2. Parse-Logic in "Parse Vertex Response" erweitern:
```javascript
linkedin: parsedData.linkedin || null,
fax: parsedData.fax || null,
```

3. Google Sheet um Spalten erweitern

4. Dedupe-Logic anpassen (optional)

**Anderes Modell nutzen:**
- "Call Vertex AI" → URL ändern:
```
gemini-2.0-flash-exp → gemini-1.5-pro
```
(Pro = teurer aber genauer)

**Email-Benachrichtigung hinzufügen:**
1. Node hinzufügen: "Send Email"
2. Nach "Build Response" einfügen
3. Template mit Kontaktdaten erstellen

---

## 📈 Monitoring & Analytics

### Execution Log
- n8n zeigt alle Executions
- **Executions** Tab → Siehst du alle Runs
- Bei Fehlern: Error-Details anschauen

### Google Sheets als Dashboard
- Nutze Google Sheets **Charts** für Visualisierung
- Beispiele:
  - Kontakte pro Tag
  - Qualitäts-Score Distribution
  - Top-Firmen

### Export
- Google Sheets → File → Download → CSV
- Oder nutze Sheets API für automatischen Export

---

## 🚀 Production Deployment Checklist

- [ ] Google Sheet erstellt mit korrekten Spalten
- [ ] Sheet-ID in allen Nodes eingetragen
- [ ] Vertex AI Credentials geprüft
- [ ] Google Sheets Credentials geprüft
- [ ] Workflow aktiviert
- [ ] Webhook getestet (CURL oder Postman)
- [ ] Frontend hochgeladen auf Hostinger
- [ ] Demo-Bilder funktionieren
- [ ] Upload-Funktion getestet
- [ ] Tabellen-Ansicht funktioniert
- [ ] CORS konfiguriert (falls nötig)
- [ ] Error-Handling getestet
- [ ] Deduplizierung getestet

---

## 📞 Support

**n8n Instanz:** https://n8n2.kortex-system.de/  
**Google Sheets Template:** Erstelle selbst (siehe oben)  
**Workflow JSON:** `n8n-workflow-visitenkarten-vertex.json`  
**Frontend:** `frontend-kortex.html`

---

## 🎉 Los geht's!

1. ✅ Sheets erstellt? → Weiter
2. ✅ Workflow importiert & konfiguriert? → Weiter
3. ✅ Frontend hochgeladen? → **FERTIG!**

**Test jetzt deine erste Visitenkarte! 🚀**

---

*Version 1.0 - Erstellt für Kortex System - 30.10.2025*

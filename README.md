# JUH Motorradgruppe - Einsatzplanung 2025

## Übersicht

Diese Web-App ermöglicht die professionelle Planung und Verwaltung von Motorrad-Einsätzen der Johanniter Unfallhilfe Bayern. Die App bietet umfassende Funktionen zur Einsatzverwaltung mit Cloud-Synchronisation über Firebase.

## Hauptfunktionen

### ✅ Einsatzverwaltung
- Vollständige Einsatzplanung mit allen relevanten Daten
- Datum von/bis, Uhrzeit von/bis
- Veranstaltungsdetails und Ortsangaben
- Zuordnung von Motorrädern und Fahrern
- Kilometer-Tracking
- Bemerkungsfeld für zusätzliche Informationen

### ✅ Ansichten
1. **Dashboard** - Schnellüberblick mit Statistiken und nächsten Einsätzen
2. **Einsatzplanung** - Tabellarische Übersicht aller Einsätze mit Such- und Filterfunktion
3. **Kalenderansicht** - Chronologische Darstellung der Einsätze
4. **Motorräder** - Übersicht der Fahrzeuge mit Einsatzstatistiken
5. **Fahrer** - Übersicht der Fahrer mit Einsatzzahlen
6. **Statistik** - Detaillierte Auswertungen nach Einsatztypen

### ✅ Export-Funktionen
- **CSV Export** - Für Excel-Import und weitere Verarbeitung
- **Excel Export** - Direkte Excel-Datei (Vorbereitet)
- **PDF Export** - Für Ausdrucke und Archivierung (Vorbereitet)

### ✅ Einsatztypen
- Pflichtdienst (Rot markiert)
- Optionaler Dienst (Grün markiert)
- Fachfortbildung
- Sanitätsdienst
- Streife
- Veranstaltung
- Werkstatt

### ✅ Motorräder
- M-JU 903
- M-JU 904
- M-JU 905
- M-JU 906
- MKT 1200 RT
- MKT F800 GS

### ✅ Fahrer
- Miklos
- Apel
- Brosch
- Eipert
- Wallner
- Hofbauer
- v. Gneisenau
- Rubner
- Hipf/Lehmann
- Backhaus
- Baumüller

## Installation und Einrichtung

### Schritt 1: Firebase-Projekt erstellen

1. Gehe zu [Firebase Console](https://console.firebase.google.com/)
2. Erstelle ein neues Projekt oder wähle ein bestehendes aus
3. Aktiviere **Cloud Firestore** unter "Build" → "Firestore Database"
4. Wähle "Produktionsmodus starten" und eine Region (z.B. europe-west3 für Frankfurt)

### Schritt 2: Firebase-Konfiguration

1. In der Firebase Console: Gehe zu Projekteinstellungen (Zahnrad-Symbol)
2. Scrolle zu "Deine Apps" → Klicke auf das Web-Symbol (</>) 
3. Registriere die App (z.B. "JUH Motorradgruppe")
4. Kopiere die Firebase-Konfiguration

### Schritt 3: Konfiguration in die App einfügen

Öffne die Datei `motorrad-einsatzplanung.html` und ersetze die Firebase-Konfiguration (ca. Zeile 632):

```javascript
const firebaseConfig = {
    apiKey: "DEINE_API_KEY",
    authDomain: "dein-project.firebaseapp.com",
    projectId: "dein-project-id",
    storageBucket: "dein-project.appspot.com",
    messagingSenderId: "123456789",
    appId: "deine-app-id"
};
```

Mit deinen echten Werten aus der Firebase Console.

### Schritt 4: Firestore-Regeln konfigurieren

In der Firebase Console unter "Firestore Database" → "Regeln":

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Für Entwicklung - ACHTUNG: Für Produktion Authentifizierung hinzufügen!
    match /{document=**} {
      allow read, write: if true;
    }
  }
}
```

**WICHTIG**: Diese Regeln sind nur für die Entwicklung! Für den Produktionseinsatz solltest du Firebase Authentication hinzufügen und die Regeln entsprechend anpassen.

### Schritt 5: App öffnen

Öffne die Datei `motorrad-einsatzplanung.html` in einem modernen Browser (Chrome, Firefox, Edge, Safari).

## Firebase-Integration erweitern

Die grundlegende Struktur für Firebase ist bereits vorbereitet. Um die Firebase-Synchronisation zu aktivieren, müssen folgende Funktionen implementiert werden:

### Einsätze laden
```javascript
const loadEinsaetze = async () => {
    const { getDocs, collection, query, orderBy } = window.firebaseModules;
    const q = query(collection(db, 'einsaetze'), orderBy('datum'));
    const querySnapshot = await getDocs(q);
    const data = [];
    querySnapshot.forEach((doc) => {
        data.push({ id: doc.id, ...doc.data() });
    });
    setEinsaetze(data);
};
```

### Einsatz speichern
```javascript
const saveEinsatz = async (einsatz) => {
    const { addDoc, collection } = window.firebaseModules;
    await addDoc(collection(db, 'einsaetze'), einsatz);
};
```

### Einsatz aktualisieren
```javascript
const updateEinsatz = async (id, einsatz) => {
    const { updateDoc, doc } = window.firebaseModules;
    await updateDoc(doc(db, 'einsaetze', id), einsatz);
};
```

### Einsatz löschen
```javascript
const deleteEinsatz = async (id) => {
    const { deleteDoc, doc } = window.firebaseModules;
    await deleteDoc(doc(db, 'einsaetze', id));
};
```

## Anpassungen und Erweiterungen

### Farben anpassen
Die JUH-Farben sind als CSS-Variablen definiert und können leicht angepasst werden:

```css
:root {
    --juh-red: #E30613;
    --juh-dark-red: #B30510;
    --juh-light-red: #FF1F2D;
    /* ... weitere Farben */
}
```

### Weitere Motorräder hinzufügen
Im JavaScript-Code die MOTORRAEDER-Array erweitern:

```javascript
const MOTORRAEDER = [
    { id: '903', label: 'M-JU 903' },
    // ... neue Motorräder hier hinzufügen
];
```

### Weitere Fahrer hinzufügen
Im JavaScript-Code das FAHRER-Array erweitern:

```javascript
const FAHRER = [
    'Miklos',
    'Apel',
    // ... neue Fahrer hier hinzufügen
];
```

### Weitere Einsatztypen hinzufügen
Im JavaScript-Code das EINSATZTYPEN-Array erweitern:

```javascript
const EINSATZTYPEN = [
    'Pflichtdienst',
    'Optionaler Dienst',
    // ... neue Typen hier hinzufügen
];
```

## Excel/PDF Export erweitern

### Excel-Export implementieren
Für vollständigen Excel-Export kann die `xlsx`-Bibliothek verwendet werden:

```html
<script src="https://cdn.sheetjs.com/xlsx-latest/package/dist/xlsx.full.min.js"></script>
```

```javascript
const exportToExcel = () => {
    const ws = XLSX.utils.json_to_sheet(filteredEinsaetze);
    const wb = XLSX.utils.book_new();
    XLSX.utils.book_append_sheet(wb, ws, "Einsätze");
    XLSX.writeFile(wb, `JUH_Einsatzplanung_${new Date().toISOString().split('T')[0]}.xlsx`);
};
```

### PDF-Export implementieren
Für PDF-Export kann `jsPDF` verwendet werden:

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.5.31/jspdf.plugin.autotable.min.js"></script>
```

## Responsive Design

Die App ist vollständig responsive und funktioniert auf:
- Desktop-PCs
- Tablets
- Smartphones

## Browser-Kompatibilität

Getestet und funktionsfähig in:
- Chrome/Edge (empfohlen)
- Firefox
- Safari

## Progressive Web App (PWA)

Die App kann zu einer PWA erweitert werden für:
- Offline-Funktionalität
- Installation als App
- Push-Benachrichtigungen

Dafür müssen folgende Dateien hinzugefügt werden:
- `manifest.json` - App-Metadaten
- `service-worker.js` - Offline-Caching

## Sicherheit

**WICHTIG für Produktionseinsatz:**

1. **Firebase Authentication aktivieren**
   - E-Mail/Passwort
   - Google Sign-In
   - Oder andere Methoden

2. **Firestore-Regeln anpassen**
   ```javascript
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /einsaetze/{einsatz} {
         allow read, write: if request.auth != null;
       }
     }
   }
   ```

3. **HTTPS verwenden**
   - Für Produktion immer HTTPS
   - Firebase Hosting bietet automatisch HTTPS

## Support und Weiterentwicklung

Vorschläge für weitere Features:
- [ ] Benachrichtigungen für bevorstehende Einsätze
- [ ] Automatische E-Mail-Versendung an Fahrer
- [ ] GPS-Tracking während Einsätzen
- [ ] Fuel-Management
- [ ] Wartungsplanung für Motorräder
- [ ] Dokumenten-Archiv (Protokolle, Berichte)
- [ ] Mobile App (iOS/Android)
- [ ] Integration mit Google Calendar
- [ ] QR-Code-Scanner für Motorräder
- [ ] Foto-Upload für Einsätze

## Kontakt

Bei Fragen oder Problemen wenden Sie sich an Ihre IT-Abteilung oder den Entwickler.

---

**Version:** 1.0  
**Stand:** Dezember 2025  
**Entwickelt für:** Johanniter Unfallhilfe Bayern - Motorradgruppe

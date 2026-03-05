# 🎬 Jellyfin Header Button Injector

> Ein benutzerdefiniertes JavaScript für Jellyfin, das die übergroße Suchleiste und den „Request Media"-Button durch zwei kompakte, native Jellyfin-Icons ersetzt — mit vollem Funktionsumfang und eigenem Suchmodal mit Live-Dropdown.

Entwickelt als Erweiterung für das **[Ratings Plugin von K3ntas](https://github.com/K3ntas/jellyfin-plugin-ratings)**, das bereits ein vollständiges Bewertungssystem, Medien-Anfragen, Benachrichtigungen und Admin-Verwaltung mitbringt.

---

## ✨ Was macht dieser Code?

Das Script greift in die Jellyfin-Oberfläche ein und verändert folgendes:

### 🔍 Neuer Suche-Button
- Ersetzt die große eingebettete Suchleiste durch ein **kompaktes Lupen-Icon** im nativen Jellyfin-Stil
- Klick öffnet ein **eigenes Suchmodal** (ähnlich dem Request-Modal)
- Beim Tippen erscheint der **native Jellyfin-Dropdown** mit Cover-Bildern, Titeln, Typ-Badge und Jahr — direkt im Modal
- `Enter` navigiert zur vollständigen Suchergebnisseite
- `ESC` oder Klick außerhalb schließt das Modal

### ▶️ Neuer Request-Button
- Ersetzt den großen „Request Media"-Button durch ein **Play + Plus Icon**
- Klick öffnet exakt dasselbe **„Medien Anfordern"-Formular** wie vorher
- Unterstützt alle Felder des Ratings-Plugins: Medientitel, Typ, IMDB-Code, IMDB-Link, Notizen

### 📐 Platzsparend auf allen Geräten
- Beide Buttons landen **neben dem Würfel-Icon** in der rechten Headerleiste
- Auf Mobile (≤925px) automatisch verkleinert
- Die originalen Elemente werden per CSS ausgeblendet — **nichts wird gelöscht**

---

## 📸 Vorher / Nachher

### Desktop — Startseite

| Vorher | Nachher |
|--------|---------|
| Große Suchleiste + „Request Media"-Button im Header | Zwei kompakte Icons neben dem Würfel |
| ![Vorher Desktop](images/Befor_Web_Search.png) | ![Nachher Desktop](images/After_Home_Web.png) |

---

### Mobile — Startseite

| Vorher | Nachher |
|--------|---------|
| Suchleiste und Request-Button außerhalb der Headerleiste | Beide Icons sauber im Header integriert |
| ![Vorher Mobil](images/Befor_smarthphone.PNG) | ![Nachher Mobil](images/After_home_Smartphone.PNG) |

---

### Suchmodal mit Live-Dropdown

Klick auf die Lupe öffnet das Suchfenster. Beim Tippen erscheint der native Jellyfin-Dropdown:

![Suchmodal](images/After_Web_Search.png)

---

## 📦 Voraussetzung: Ratings Plugin

Dieser Code ist als Erweiterung für das **Ratings Plugin** gedacht, das folgende Features mitbringt:

| Feature | Beschreibung |
|---------|--------------|
| ⭐ Bewertungssystem | 1–10 Sterne pro Medium, Durchschnitt auf Cards |
| 📬 Medien-Anfragen | Nutzer können Filme/Serien anfragen (mit IMDB-Feldern) |
| 🔔 Benachrichtigungen | Push-Notifications bei neuen Medien (Browser + Native Apps) |
| 🗂️ Media Management | Admin-Ansicht mit Statistiken, Lösch-Planung, Badges |
| 🔍 Dropdown-Suche | Native Suche mit Vorschlägen direkt im Header |
| 🛎️ Admin-Anfragenverwaltung | Anfragen annehmen, ablehnen, snoozen, löschen |

### Plugin installieren

1. Jellyfin Dashboard → **Plugins** → **Katalog** → Suche nach „Ratings"  
   **oder** manuell über das Manifest:

```
https://raw.githubusercontent.com/K3ntas/jellyfin-plugin-ratings/main/manifest.json
```

> Dashboard → Plugins → Repositories → `+` → URL einfügen → Speichern → Katalog neu laden → „Ratings" installieren

2. Jellyfin **neu starten**

---

## 🚀 Installation des Header Injectors

### Methode 1 — Benutzerdefiniertes JavaScript (empfohlen)

1. Als **Admin** einloggen
2. **Dashboard** → **Einstellungen** → **Allgemein**
3. Ganz nach unten scrollen: **„Benutzerdefiniertes JavaScript"**
4. Inhalt von [`jellyfin-icon-injector.js`](./jellyfin-icon-injector.js) einfügen
5. **Speichern** → Seite mit `F5` / `Cmd+R` neu laden

> ⚠️ Benötigt **Administratorrechte**.

---

### Methode 2 — JavaScript Injector Plugin

Falls du ein Plugin wie den **JavaScript Injector** nutzt (z. B. aus dem Ratings-Plugin bereits integriert):

1. `jellyfin-icon-injector.js` als Injection-Datei hinterlegen
2. Jellyfin neu starten

---

## ⚙️ Wie es funktioniert (technisch)

```
Seite lädt
  → Script wartet 900ms auf React-Render
  → CSS wird in <head> injiziert
      → #headerSearchField   { display: none }
      → #requestMediaBtn     { display: none }
  → Zwei neue <button>-Elemente werden in .headerRight eingefügt
  → MutationObserver überwacht DOM-Änderungen (SPA-Navigation)
      → Falls Buttons verschwinden → werden neu eingefügt
```

### Suche intern

```
Klick auf Lupe
  → jbi-search-overlay.show wird gesetzt
  → #jbi-search-input fokussiert

Eingabe im Modal-Input
  → Wert wird in #headerSearchInput gespiegelt
  → Native input/change Events werden gefeuert
  → Jellyfin rendert #searchDropdown
  → Script verschiebt #searchDropdown in #jbi-dropdown-wrap (im Modal)
  → Dropdown zeigt Thumbnails, Titel, Typ-Badge, Jahr

Enter
  → Modal schließt sich
  → Navigation zu /web/#/search.html?query=...
  → Dropdown wird zurück in body verschoben
```

### Request intern

```
Klick auf Play+Plus Icon
  → document.getElementById("requestMediaBtn").click()
  → Ratings-Plugin öffnet #requestMediaModal
  → Formular erscheint: Titel, Typ, IMDB-Code, IMDB-Link, Notizen
```

---

## 🎨 Anpassen

### Button-Position ändern

In der Funktion `inject()` am Ende des Scripts:

```js
// Aktuell: .headerRight (neben Würfel-Icon, rechts)
const target = document.querySelector(".headerRight") || ...

// Für linke Seite (neben Logo):
const target = document.querySelector(".headerLeft") || ...
```

### Icons tauschen

Suche nach `SEARCH_SVG` oder `REQUEST_SVG` und ersetze den SVG-Inhalt:

```js
const SEARCH_SVG = `<svg viewBox="0 0 24 24">
  <!-- Dein SVG hier, kein fill, stroke für Linien -->
  <circle cx="10.5" cy="10.5" r="6.5"/>
  <line x1="15.5" y1="15.5" x2="21" y2="21"/>
</svg>`;
```

### Originale sichtbar lassen

Um die originalen Buttons **nicht** auszublenden (beide zeigen):

```css
/* Diese Zeilen im CSS-Block entfernen oder auskommentieren: */
#requestMediaBtn,
#headerSearchField {
  display: none !important;
}
```

---

## 📱 Kompatibilität

| Plattform | Status |
|-----------|--------|
| Jellyfin Web — Desktop | ✅ |
| Jellyfin Web — Mobile Browser | ✅ |
| Jellyfin iOS App | ❌ Kein Custom JS Support |
| Jellyfin Android App | ❌ Kein Custom JS Support |
| Docker / Synology NAS | ✅ (getestet) |
| Reverse Proxy Setup | ✅ |

Getestet mit **Jellyfin 10.11** + **Ratings Plugin v1.0.273** + **JellyTweaks 3.1** + **Jellyfin Enhanced 11.1**.

---

## 📁 Repo-Struktur

```
jellyfin-header-injector/
├── README.md
├── jellyfin-icon-injector.js
└── images/
    ├── After_Home_Web.png
    ├── After_home_Smartphone.PNG
    ├── After_Web_Search.png
    ├── Befor_Web_Search.png
    └── Befor_smarthphone.PNG
```

---

## 📜 Lizenz

MIT — frei verwendbar, veränderbar und weiterzugeben.

---

## 🙏 Credits

- **[K3ntas/jellyfin-plugin-ratings](https://github.com/K3ntas/jellyfin-plugin-ratings)** — Das Ratings Plugin, dessen Request-Modal und Suchfeld-Infrastruktur dieser Code nutzt
- **[jellyfin/jellyfin-web](https://github.com/jellyfin/jellyfin-web)** — Jellyfin Open-Source Media Server

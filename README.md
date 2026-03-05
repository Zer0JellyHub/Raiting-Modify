# 🎬 Jellyfin Header Button Injector

> A custom JavaScript for Jellyfin that replaces the oversized search bar and "Request Media" button with two compact, native-style icons — with full functionality and a custom search modal with live dropdown.

Built as an extension for the **[Ratings Plugin by K3ntas](https://github.com/K3ntas/jellyfin-plugin-ratings)**, which provides a complete rating system, media requests, notifications and admin management.

---

## ✨ What does this do?

### 🔍 New Search Button
- Replaces the large embedded search bar with a **compact magnifier icon** in native Jellyfin style
- Click opens a **custom search modal** (similar to the Request modal)
- While typing, the **native Jellyfin dropdown** appears with cover images, titles, type badge and year — directly inside the modal
- `Enter` navigates to the full search results page
- `ESC` or click outside closes the modal

### ▶️ New Request Button
- Replaces the large "Request Media" button with a **Play + Plus icon**
- Click opens exactly the same **"Request Media" form** as before
- Supports all Ratings Plugin fields: title, type, IMDB code, IMDB link, notes

### 📐 Space-saving on all devices
- Both buttons appear **next to the grid icon** in the right header bar
- Automatically scaled down on mobile (≤925px)
- Original elements are hidden via CSS — **nothing is deleted**

---

## 📸 Before / After

### Desktop — Home

| Before | After |
|--------|-------|
| Large search bar + "Request Media" button in header | Two compact icons next to the grid button |
| ![Before Desktop](images/Befor_Web_Search.png) | ![After Desktop](images/After_Home_Web.png) |

---

### Mobile — Home

| Before | After |
|--------|-------|
| Search bar and request button outside the header | Both icons cleanly integrated in the header |
| ![Before Mobile](images/Befor_smarthphone.PNG) | ![After Mobile](images/After_home_Smartphone.PNG) |

---

### First iteration — Lupe & R? in the header

After the first injection the two buttons appeared as a magnifier and "R?" text in the header bar:

![First version](images/Bildschirmfoto_2026-03-05_um_09_00_00.png)

---

### Search Modal with Live Dropdown

Click the magnifier to open the search window. While typing, the native Jellyfin dropdown with thumbnails appears directly in the modal:

| Search modal | Live dropdown |
|-------------|---------------|
| ![Search modal](images/Bildschirmfoto_2026-03-05_um_09_18_08.png) | ![Live dropdown](images/Bildschirmfoto_2026-03-05_um_09_21_28.png) |

Final result with full dropdown integration:

![Final search](images/After_Web_Search.png)

---

### Request Modal

Click on the Play+Plus icon to open the media request form:

![Request modal](images/Bildschirmfoto_2026-03-05_um_08_54_06.png)

---

## 📦 Requirement: Ratings Plugin

This script is designed as an extension for the **Ratings Plugin**, which includes:

| Feature | Description |
|---------|-------------|
| ⭐ Rating System | 1–10 stars per item, average shown on cards |
| 📬 Media Requests | Users can request movies/series (with IMDB fields) |
| 🔔 Notifications | Push notifications for new media (Browser + Native Apps) |
| 🗂️ Media Management | Admin view with stats, deletion scheduling, badges |
| 🔍 Dropdown Search | Native search with suggestions directly in the header |
| 🛎️ Admin Request Management | Accept, reject, snooze, delete requests |

### Install the Plugin

1. Jellyfin Dashboard → **Plugins** → **Catalog** → search for "Ratings"  
   **or** manually via the manifest:

```
https://raw.githubusercontent.com/K3ntas/jellyfin-plugin-ratings/main/manifest.json
```

> Dashboard → Plugins → Repositories → `+` → paste URL → Save → reload Catalog → install "Ratings"

2. **Restart** Jellyfin

---

## 🚀 Installation

### Method 1 — Custom JavaScript (recommended)

1. Log in as **Admin**
2. **Dashboard** → **Settings** → **General**
3. Scroll to the bottom: **"Custom JavaScript"**
4. Paste the contents of [`jellyfin-icon-injector.js`](./jellyfin-icon-injector.js)
5. **Save** → reload the page with `F5` / `Cmd+R`

> ⚠️ Requires **admin rights**.

---

### Method 2 — JavaScript Injector Plugin

If you use a plugin like the **JavaScript Injector** (e.g. already integrated in the Ratings Plugin):

1. Set `jellyfin-icon-injector.js` as the injection file
2. Restart Jellyfin

---

## ⚙️ How it works (technical)

```
Page loads
  → Script waits 900ms for React render
  → CSS is injected into <head>
      → #headerSearchField   { display: none }
      → #requestMediaBtn     { display: none }
  → Two new <button> elements are inserted into .headerRight
  → MutationObserver watches DOM changes (SPA navigation)
      → If buttons disappear → re-injected automatically
```

### Search internally

```
Click on magnifier
  → jbi-search-overlay.show is set
  → #jbi-search-input focused

Typing in modal input
  → Value is mirrored to #headerSearchInput
  → Native input/change events are fired
  → Jellyfin renders #searchDropdown
  → Script moves #searchDropdown into #jbi-dropdown-wrap (in modal)
  → Dropdown shows thumbnails, titles, type badge, year

Enter
  → Modal closes
  → Navigation to /web/#/search.html?query=...
  → Dropdown moved back to body
```

### Request internally

```
Click on Play+Plus icon
  → document.getElementById("requestMediaBtn").click()
  → Ratings Plugin opens #requestMediaModal
  → Form appears: title, type, IMDB code, IMDB link, notes
```

---

## 🎨 Customization

### Change button position

In the `inject()` function at the bottom of the script:

```js
// Default: .headerRight (next to grid icon, right side)
const target = document.querySelector(".headerRight") || ...

// For left side (next to logo):
const target = document.querySelector(".headerLeft") || ...
```

### Swap icons

Find `SEARCH_SVG` or `REQUEST_SVG` and replace the SVG content:

```js
const SEARCH_SVG = `<svg viewBox="0 0 24 24">
  <!-- Your SVG here, no fill, use stroke for lines -->
  <circle cx="10.5" cy="10.5" r="6.5"/>
  <line x1="15.5" y1="15.5" x2="21" y2="21"/>
</svg>`;
```

### Keep originals visible

To keep the original buttons visible (show both):

```css
/* Remove or comment out these lines in the CSS block: */
#requestMediaBtn,
#headerSearchField {
  display: none !important;
}
```

---

## 📱 Compatibility

| Platform | Status |
|----------|--------|
| Jellyfin Web — Desktop | ✅ |
| Jellyfin Web — Mobile Browser | ✅ |
| Jellyfin iOS App | ❌ No Custom JS support |
| Jellyfin Android App | ❌ No Custom JS support |
| Docker / Synology NAS | ✅ (tested) |
| Reverse Proxy Setup | ✅ |

Tested with **Jellyfin 10.11** + **Ratings Plugin v1.0.273** + **JellyTweaks 3.1** + **Jellyfin Enhanced 11.1**.

---

## 📁 Repository Structure

```
jellyfin-header-injector/
├── README.md
├── jellyfin-icon-injector.js
└── images/
    ├── After_Home_Web.png
    ├── After_home_Smartphone.PNG
    ├── After_Web_Search.png
    ├── Befor_Web_Search.png
    ├── Befor_smarthphone.PNG
    ├── Bildschirmfoto_2026-03-05_um_08_54_06.png
    ├── Bildschirmfoto_2026-03-05_um_09_00_00.png
    ├── Bildschirmfoto_2026-03-05_um_09_18_08.png
    └── Bildschirmfoto_2026-03-05_um_09_21_28.png
```

---

## 📜 License

MIT — free to use, modify and distribute.

---

## 🙏 Credits

- **[K3ntas/jellyfin-plugin-ratings](https://github.com/K3ntas/jellyfin-plugin-ratings)** — The Ratings Plugin whose request modal and search field infrastructure this script uses
- **[jellyfin/jellyfin-web](https://github.com/jellyfin/jellyfin-web)** — Jellyfin Open Source Media Server

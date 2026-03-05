# 🎬 Jellyfin Header Button Injector

> A custom JavaScript for Jellyfin that replaces the oversized search bar and "Request Media" button with two compact, native-style icons — full functionality, custom search modal with live dropdown.

Built as an extension for the **[Ratings Plugin by K3ntas](https://github.com/K3ntas/jellyfin-plugin-ratings)**.

---

## 📸 Before / After

### Desktop

| Before | After |
|--------|-------|
|<img width="1429" height="796" alt="Befor_Home_Web" src="https://github.com/user-attachments/assets/16609e2e-8e15-4283-b0c2-694ab314945d" /> |<img width="1429" height="777" alt="After_Home_Web" src="https://github.com/user-attachments/assets/7b90f20c-87a7-43a6-869e-173c9cfdc705" />
 

### Search — Before / After

| Before (original big search bar) | After (modal with live dropdown) |
|----------------------------------|----------------------------------|
|<img width="1434" height="785" alt="Befor_Web_Search" src="https://github.com/user-attachments/assets/0b000763-ee88-4970-bebf-8398bd6a03fe" /> |<img width="1430" height="779" alt="After_Web_Search" src="https://github.com/user-attachments/assets/5050ab5a-1c74-4b96-95aa-8fec5df4cf6a" />

### Mobile

|<img width="910" height="1010" alt="Befor and After" src="https://github.com/user-attachments/assets/47690419-19f2-4b43-b060-655055638e4f" />|




---

## ✨ Features

### 🔍 Search Button
- Compact magnifier icon replaces the large search bar
- Click opens a **search modal** in native Jellyfin style
- Typing shows the **native Jellyfin dropdown** with cover images, titles, type badges and year — directly inside the modal
- `Enter` navigates to the full search results page
- `ESC` or click outside closes the modal

### ▶️ Request Button
- Play + Plus icon replaces the large "Request Media" button
- Opens exactly the same **"Request Media" form** as before
- All Ratings Plugin fields supported: title, type, IMDB code, IMDB link, notes

### 📐 Works everywhere
- Both buttons sit **next to the grid icon** in the right header bar
- Automatically scaled down on mobile (≤925px)
- Original elements are hidden via CSS — nothing is deleted

---

## 📦 Requirement: Ratings Plugin

This script extends the **[Ratings Plugin by K3ntas](https://github.com/K3ntas/jellyfin-plugin-ratings)**.

Install via manifest URL:
```
https://raw.githubusercontent.com/K3ntas/jellyfin-plugin-ratings/main/manifest.json
```
> Dashboard → Plugins → Repositories → `+` → paste URL → Save → reload Catalog → install "Ratings" → restart Jellyfin

---

## 🚀 Installation

1. Log in as **Admin**
2. **Dashboard** 
3. Scroll to *JavaScript Injector"** (If you don#t have install it)
4. Paste the contents of [`jellyfin-icon-injector.js`](./jellyfin-icon-injector.js)
5. **Save** → reload with `F5` / `Cmd+R`

---

## ⚙️ How it works

| Element | Change |
|---------|--------|
| `#headerSearchField` | Hidden via CSS |
| `#requestMediaBtn` | Hidden via CSS |
| `.headerRight` | Two new icon buttons inserted at the front |

**Search flow:** typing in the modal mirrors input to `#headerSearchInput` → fires native Jellyfin events → native `#searchDropdown` renders inside the modal → `Enter` navigates to `/web/#/search.html?query=...`

**Request flow:** click triggers `#requestMediaBtn.click()` → Ratings Plugin opens `#requestMediaModal`

---

## 📱 Compatibility

| Platform | Status |
|----------|--------|
| Jellyfin Web — Desktop | ✅ |
| Jellyfin Web — Mobile Browser | ✅ |
| Docker / Synology NAS | ✅  |
| Jellyfin iOS  | ✅ |

Tested with **Jellyfin 10.11** + **Ratings Plugin v1.0.273** + **JellyTweaks 3.1** + **Jellyfin Enhanced 11.1**

---

## 🙏 Credits

- **K3ntas/jellyfin-plugin-ratings](https://github.com/K3ntas/jellyfin-plugin-ratings)**
- **(https://github.com/n00bcodr/Jellyfin-JavaScript-Injector)**
- **[jellyfin/jellyfin-web](https://github.com/jellyfin/jellyfin-web)**

---

## 📜 License

MIT — free to use, modify and distribute.

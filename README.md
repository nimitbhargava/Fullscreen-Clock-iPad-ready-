# Fullscreen Clock (iPad‑ready)

A single‑file, production‑ready fullscreen clock optimized for iPad (works great on desktop too). It supports tap‑to‑fullscreen, optional screen wake lock, precise ticking, and a customizable UI (12/24‑hour, date visibility, font, and theme) with settings saved in local storage.

## 💡 Recommended repository name

**`fullscreen-clock`**
(Alternatives: `ipad-fullscreen-clock`, `big-clock`, or `wall-clock-web`)

## 📦 What’s in this repo

```
fullscreen-clock/
├─ index.html         # Production build (single file)
├─ README.md          # This file
├─ LICENSE            # MIT (recommended)
└─ .gitignore         # Optional (node_modules, build artifacts if you add tooling later)
```

> The project is dependency‑free and runs directly from `index.html`. No build step required.

## 🚀 Quick start

**Option A — Double‑click locally**

1. Clone the repo.
2. Open `index.html` in any modern browser.

**Option B — Serve locally (recommended for testing wake lock/fullscreen)**

```bash
# Any simple static server works
python3 -m http.server 8080
# then open http://localhost:8080
```

## 📱 iPad setup

* **Add to Home Screen** in Safari for a chrome‑less fullscreen experience:

  1. Open the page in Safari → Share → **Add to Home Screen**.
  2. Launch from the Home Screen icon.
* Tap the clock (or press **F** on hardware keyboards) to toggle fullscreen.
* Use the **Keep Awake** button to prevent sleep (supported on iPadOS 16+ Safari; otherwise set *Settings → Display & Brightness → Auto‑Lock → Never* while using the app).

## ⚙️ Features

* Large centered clock + optional date
* **Fullscreen** toggle (buttons, tap, key `F`)
* **Screen Wake Lock** toggle (button, key `K`) with graceful fallback
* **Settings modal** (seconds/date, 12/24‑hour, font, theme) stored in LocalStorage (key: `clock.settings.v1`)
* Precise second‑aligned ticking and no layout jitter
* Accessible: ARIA labels, keyboard shortcuts
* Mobile‑safe: safe‑area insets, double‑tap zoom prevention
* No external dependencies; works offline once loaded

## 🔧 Configuration

You can override defaults via **query parameters** on first load:

```
?seconds=0|1&date=0|1&h24=0|1&font=sans|mono&theme=dark|light|amoled
```

Examples:

* `index.html?h24=1&seconds=0` → 24‑hour, hide seconds
* `index.html?theme=amoled&font=mono` → AMOLED theme, monospace font

## 🌐 Deploy to GitHub Pages

1. Create a **public** repo named `fullscreen-clock`.
2. Commit `index.html`, `README.md`, and `LICENSE`.
3. Push to `main`.
4. In **Settings → Pages**:

   * **Source**: *Deploy from a branch*.
   * **Branch**: `main` / root (`/`).
5. Visit the URL GitHub shows (e.g., `https://<your-user>.github.io/fullscreen-clock`).

> Tip: If you prefer a custom domain, set `CNAME` in the repo root and add a DNS CNAME record to `your-user.github.io`.

## 🧩 Optional PWA bundle (if you want true install + offline)

This project is intentionally single‑file. If you want install banners and offline caching, add these optional files:

```
public/
├─ index.html
├─ manifest.webmanifest
├─ sw.js
└─ icons/
   ├─ 192.png
   └─ 512.png
```

**manifest.webmanifest** (example)

```json
{
  "name": "Fullscreen Clock",
  "short_name": "Clock",
  "start_url": "/?source=pwa",
  "display": "standalone",
  "background_color": "#0b0b0e",
  "theme_color": "#0b0b0e",
  "icons": [
    { "src": "/icons/192.png", "sizes": "192x192", "type": "image/png" },
    { "src": "/icons/512.png", "sizes": "512x512", "type": "image/png" }
  ]
}
```

**sw\.js** (very small cache‑first example)

```js
const CACHE = 'clock-v1';
self.addEventListener('install', e => {
  e.waitUntil(caches.open(CACHE).then(c => c.addAll(['/', '/index.html'])));
});
self.addEventListener('fetch', e => {
  e.respondWith(caches.match(e.request).then(res => res || fetch(e.request)));
});
```

Add to `<head>` of `index.html` when using the PWA variant:

```html
<link rel="manifest" href="/manifest.webmanifest">
<script>
  if ('serviceWorker' in navigator) {
    window.addEventListener('load', () => navigator.serviceWorker.register('/sw.js'));
  }
</script>
```

## 🧪 Testing checklist

* [ ] Fullscreen toggles via button, tap, and `F` key
* [ ] Wake Lock toggles via button and `K` key; fallback message appears if unsupported
* [ ] Settings modal opens/saves; survives reload (LocalStorage)
* [ ] Date and 12/24‑hour formatting reflect your locale and settings
* [ ] Looks correct in portrait and landscape on iPad
* [ ] Works from iPad Home Screen shortcut

## 📜 License

MIT. See `LICENSE`.

## 🤝 Contributing (optional)

1. Fork the repo, create a feature branch.
2. Make changes (keep it dependency‑free and accessible).
3. Open a PR with a clear description and screenshots.

## 🙋 FAQ

**Q: How do I hide the date?**
A: Use the Settings modal or append `?date=0` the first time you open it.

**Q: Will it stay awake on my iPad?**
A: On recent iPadOS/Safari versions, the Wake Lock API works. Otherwise, use fullscreen and set Auto‑Lock to *Never* while using the app.

**Q: Can I change the font size?**
A: The size is responsive, but you can tweak `clamp()` in `.time` within `index.html` if you want a different min/max.

**Q: Can I run it on a TV or kiosk?**
A: Yes—open in a modern browser, go fullscreen, optionally enable Keep Awake, and hide mouse via fullscreen mode.

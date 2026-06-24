# Hand — iOS build notes

This is the original `index.html` game wrapped in **Capacitor** so it can ship to the
App Store. The game code and structure are unchanged — only offline fonts and iOS
safe-area CSS were added.

## Layout
- `www/` — the web app served inside the native shell (`index.html` + bundled `fonts/`).
  Edit the game here. The original untouched `index.html` is kept at the repo root for reference.
- `ios/` — the generated Xcode project (build this on macOS / Codemagic).
- `capacitor.config.json` — appId / appName / `webDir: www`.
- `app-icon-1024.png` — master icon for App Store Connect upload.

## ⚠️ Set before first submission
- **Bundle ID** is currently a placeholder: `com.binghanem.hand`. Change it in
  `capacitor.config.json` (`appId`) **and** in Xcode → target → Signing → Bundle Identifier
  to match the App ID registered in your Apple Developer account.
- **Version / build number**: set `MARKETING_VERSION` (e.g. 1.0.0) and
  `CURRENT_PROJECT_VERSION` (e.g. 1) in Xcode → target → General.
- **Signing**: select your Team and a provisioning profile in Xcode (or Codemagic).

## Build (macOS or Codemagic — CocoaPods + Xcode required)
```bash
npm install
npx cap sync ios     # regenerates ios/App/App/public from www and runs pod install
npx cap open ios     # opens Xcode  →  Product > Archive  →  upload to App Store Connect
```

On Codemagic, use the same `npm install && npx cap sync ios` step before the Xcode
archive step (same flow as your Let's Meet app).

## What was changed vs the original `index.html`
- Google Fonts `@import` → local `fonts/fonts.css` (works fully offline; no CDN call).
- `viewport-fit=cover` + safe-area insets on header, bottom bar, modal, and reset bar
  (clears the notch and home indicator).
- iOS web-app meta tags, no-zoom, no tap-highlight / callout (native feel).
- No game logic, scoring, translations, or persistence was altered.

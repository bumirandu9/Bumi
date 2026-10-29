# BacaBuku Android (Maven Pro)

Offline-first personal ebook library and reader built with Vite + Capacitor. The APK is compiled in GitHub Actions so you can use an Android phone without a PC.

## Current prototype features
- Import EPUB and PDF files (also TXT/HTML as simple text reading).
- Imported files and metadata are copied into the app's IndexedDB storage.
- Library grid with cover thumbnails, title/author editing, search, recent, favorites and finished filters.
- EPUB cover metadata extraction; PDF first-page thumbnail; custom cover image selection.
- EPUB.js and PDF.js bundled by npm/Vite for local/offline reading.
- Font size, font-family choices, Light/Dark/Sepia themes, in-reader brightness filter.
- Reading progress, EPUB CFI/PDF page state, bookmarks, and table of contents when available.
- Export original imported file.

## Important limitations of this prototype
- Maven Pro currently loads through Google Fonts CSS. For a *fully offline font*, download the Maven Pro WOFF2 font file with appropriate license and place it in `www/fonts/`, then replace the CSS `@import` with a local `@font-face` rule. Until that is done, Maven Pro may fall back to a system font while offline.
- The brightness control changes the reader content brightness; it does not change the Android system screen brightness. System brightness control requires a native Android plugin/permission and should be added in a later pass.
- Direct DOCX/RTF/MOBI/AZW3 conversion is not implemented. Convert those to EPUB with Calibre first. PDF-to-EPUB conversion is not implemented because reliable conversion needs extraction/OCR and manual review.
- EPUB bookmark locations are stored as CFIs, PDF bookmarks as page numbers. EPUB progress depends on EPUB.js location generation and may be approximate until the location map has finished generating.
- IndexedDB is app-private WebView storage, but uninstalling or clearing app data deletes imported books. Add an export/backup feature before relying on it as the only copy.
- This is a prototype and should be tested with real books before treating it as a finished reader.

## Build an APK using only your Android phone
You do not need Android Studio installed on the phone. GitHub Actions builds the APK on a cloud runner.

1. Extract this ZIP on your phone.
2. In a browser, create a new empty repository on GitHub, for example `bacabuku-android`.
3. Upload the **contents** of this folder to the repository (not the ZIP itself). Make sure `.github/workflows/build-apk.yml` is included. If the GitHub web uploader hides dot-folders or is awkward on mobile, use GitHub's web editor to create that workflow file, or use a mobile Git client that preserves `.github`.
4. Open the repository's **Actions** tab and select **Build BacaBuku APK**. If asked, enable Actions. Press **Run workflow**. A push to `main` also starts a build.
5. Wait for the workflow to finish successfully. Open the completed run and find **Artifacts** near the bottom. Download `BacaBuku-debug-apk` and extract it; it contains `app-debug.apk`.
6. Open the APK on the Poco F4. If Android blocks installation, allow “Install unknown apps” for the browser or file manager you used, then retry. Only install APKs from your own repository/build.

## Local development (optional)
Requires Node.js and Android SDK/JDK 21 on a computer:

```bash
npm install
npm run build
npx cap add android
npx cap sync android
npx cap open android
```

## Privacy
No account or ad SDK is included. Books are stored locally in IndexedDB in the installed app. The current Maven Pro stylesheet requests the font from Google Fonts until the font asset is bundled locally; other reader assets are bundled in the app build.

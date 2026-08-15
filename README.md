# Masul V1.32 — Firebase Freeze Fix

## Critical fix
V1.31 could make Chrome show **Page Unresponsive** immediately after loading the GitHub Pages build.

### Cause
A `MutationObserver` watched the entire page. Its callback called `refreshCloudUI()`, which changed DOM classes/text. Those changes triggered the same observer again, creating a continuous feedback loop.

### V1.32 fix
- Removed the whole-page MutationObserver.
- Cloud buttons are bound directly once.
- Cloud status updates are idempotent: text/classes are changed only when necessary.
- Firebase Authentication and Firestore features from V1.31 are retained.
- Local Masul browser data is not reset by this update.

## After uploading
1. Replace GitHub `index.html` with the V1.32 file.
2. Keep `masul-icon.png`.
3. Wait for GitHub Pages to redeploy.
4. Hard refresh the live page (`Ctrl + F5`).
5. Confirm the dashboard opens normally.
6. Only then continue to Cloud & Account setup.

Do not upload/restore cloud data until the live page is stable.

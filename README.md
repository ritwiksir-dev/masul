# Masul V1.34 — Live Sync Stability

## Critical hotfix
V1.33 could freeze with `Maximum call stack size exceeded`.

### Root cause
Cloud status refresh called the full dashboard renderer:

Cloud status refresh
→ full dashboard render
→ saveState()
→ cloud save hook
→ cloud status refresh
→ …

### V1.34 fix
- Cloud status refresh now updates only save/cloud indicators.
- It no longer calls the full dashboard renderer.
- Added a re-entry guard around the cloud status UI.
- Firebase Authentication, live Firestore listeners, automatic cloud loading and automatic cloud saving remain enabled.
- Firebase data is not modified merely by installing this file.
- No Firestore rule change is required.

## Deployment
Replace only GitHub Pages `index.html`.

Then:
1. Close all old V1.33 Masul tabs.
2. Wait for GitHub Pages to redeploy.
3. Open Chrome first and hard refresh with Ctrl+F5.
4. Confirm the version badge says V1.34 Live Sync Stability.
5. Wait for the cloud state to reach Synced.
6. Only then open Edge and hard refresh there.

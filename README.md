# Masul V1.33 — Live Cloud Sync

## Major change
V1.33 moves Masul from manual cloud migration to automatic Firebase synchronization.

### After sign-in
- Masul opens the Firebase organisation automatically.
- A fresh browser with no meaningful local data loads Firebase automatically.
- If the local browser and Firebase contain the same data, live sync starts silently.
- If both contain meaningful but different data, Masul pauses and shows a safety reconciliation screen instead of guessing.

### Automatic saving
Normal Masul `saveState()` actions now:
1. save the browser working copy immediately,
2. mark cloud changes as pending,
3. debounce briefly,
4. write only changed/added/deleted Firestore documents.

The whole collection is not blindly deleted/re-uploaded for every normal edit.

### Real-time receive
Firestore `onSnapshot()` listeners watch:
- organisation
- config/app
- batches
- people
- fees
- paymentRequests
- payments
- historyImports

Changes arriving from another signed-in browser update the local working copy automatically.

### Status
Masul now shows:
- Synced
- Syncing…
- Pending sync
- Offline • local safe
- Cloud issue

### Safety protections
- Remote updates do not apply while a modal is open; they wait until the modal closes.
- Remote state application does not trigger a write-back loop.
- JSON backup restore while signed in first disconnects/signs out, so a restored backup cannot silently overwrite Firebase.
- Reset Local Data while signed in first disconnects/signs out and keeps the Firebase copy unchanged.
- Reload Cloud Copy downloads a JSON backup before replacing the browser working copy.
- If a browser copy and Firebase differ at first connection, Masul asks which copy to use. Replacing Firebase requires typing `CLOUD`.

### New owner accounts
A newly created owner organisation uses the current browser setup as the initial Firebase copy automatically.

## Current limitations
- V1.33 remains owner-only under the current Phase 1 Firestore rules.
- Simultaneous editing of the exact same record on different devices is still effectively last-write-wins. Multi-user roles and stronger conflict/audit handling belong to the next collaboration phase.
- Public/customer payment links are not yet opened through anonymous/public Firestore access.
- The browser's existing Masul localStorage remains the local working/safety copy. Firestore persistent disk cache is not automatically enabled because fee/student data may be used on shared devices.

## Deployment
Replace only GitHub Pages `index.html`. The existing `masul-icon.png` can remain unchanged. No Firestore rules change is required for V1.33 under the current owner-only rules.

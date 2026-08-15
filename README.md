# Masul V1.31 — Firebase Cloud Foundation

Firebase project:
- Project ID: `masul-900d7`
- Web app: Masul Web
- Authentication: Email/Password
- Firestore: Production mode
- Security: owner-only Phase 1 rules

## What this build adds

### Owner authentication
Admin Dashboard → Settings → Cloud & Account

- Sign In
- Create Owner Account
- Password reset
- Sign Out

Creating an owner account also creates an isolated Firebase organisation owned by that Firebase UID.

### Safe first migration
Cloud sync is deliberately **manual** in V1.31.

- **Upload Current Data**: current browser data becomes the Firebase copy.
- **Restore from Cloud**: Firebase data replaces this browser's local copy.
- Both workflows offer a JSON backup first.
- Signing in by itself does not upload, download or overwrite data.

### Cloud structure

`users/{uid}`

`organizations/{orgId}`
- owner UID
- organisation settings
- cloud schema metadata

Subcollections:
- `batches`
- `people`
- `fees`
- `paymentRequests`
- `payments`
- `historyImports`
- `config/app`

### Logo safety
Firestore documents have a size limit. A very large base64 organisation logo is therefore omitted from the organisation Firestore document rather than risking a failed migration. The browser copy is preserved. A later Storage/asset strategy can provide full cross-device logo sync.

### Phase 1 limitation
V1.31 does NOT yet enable automatic real-time two-way synchronization. The first objective is to:
1. create the owner account,
2. upload the trusted local copy,
3. verify it,
4. restore it in another browser/device,
5. then enable automatic synchronization in a subsequent build.

## Next after successful migration
- automatic cloud writes
- multi-device live sync
- role model beyond owner
- secure customer payment-link access
- App Check
- backend functions for sensitive operations
- provider webhook verification

The published Phase 1 Firestore rules are also included as `firestore.rules.txt` for reference.

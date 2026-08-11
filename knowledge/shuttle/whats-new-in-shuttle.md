# What's New in Shuttle

Shuttle Went Multiplayer #overview
  - Your Looms learned to travel — across your devices, and to other people
  - Two commands, two very different things
    - Sync My Devices — all your stuff, on your devices [>>](device-sync)
    - Share This Loom Live — one document, with other people [>>](live-sharing)
  - No account required for any of it [>>](identity)
  - Everything here is live today at wwwshuttle.app

---

Sync My Devices #device-sync
  - Your whole workspace, live on every device
  - Edit on your laptop, glance at your phone — changes appear within moments, both directions
  - Zero signup
    - The first device creates the sync room
    - Other devices join with a link or QR code
    - Under the hood: each device carries its own cryptographic identity [>>](identity)
  - Offline is a first-class citizen
    - Edits made on the train queue quietly in a persistent outbox
    - They reconcile automatically when you're back
    - A crash or a dead battery never costs you a change
  - Command palette → "Sync My Devices"

---

Share This Loom Live #live-sharing
  - Share a single Loom — just that one — live with another person
  - They see your edits as you make them, and they can edit too
  - The rest of your workspace stays private
  - The recipient's side
    - They get a link
    - On accept, a toast appears with an "Open" button — straight into the shared Loom
  - You stay in control
    - Revoke someone's access, or freeze them to read-only, at any moment
    - Enforced by the server, effective immediately — not just hidden in the UI
  - Prefer a frozen copy instead? [>>](snapshots)
  - Command palette → "Share This Loom Live"

---

Identity Without Accounts #identity
  - No signup, no password, no email — your device *is* your identity
  - Each device carries its own cryptographic keypair
    - Created quietly the first time it's needed
    - Stored only on that device
  - Joining a room means answering a signed challenge
    - The room learns your public-key fingerprint — nothing else about you
  - This is what makes zero-signup sync possible [>>](device-sync)

---

Undo That Survives Collaboration #undo
  - Cmd+Z undoes *your* last change — never a collaborator's
  - Works even while their edits stream in [>>](live-sharing)
  - Never corrupts the merged document
  - Most collaborative editors get this wrong
    - Shuttle keeps a private history of your own edits on your device

---

Images That Don't Weigh You Down #images
  - Paste images straight into a node
  - The bytes live in dedicated blob storage
    - Not inside the database, not inside the sync stream
  - So an image-heavy Loom syncs as fast as a text-only one [>>](device-sync)

---

Snapshot Sharing #snapshots
  - The frozen-copy alternative to live sharing [>>](live-sharing)
  - Share a read-only snapshot of a Loom — or your whole workspace
  - A short link, expiring after a time you choose
    - Up to 30 days; 7 by default
  - Live sharing is a conversation; a snapshot is a photograph

---

Also Recent #also-recent
  - Map View understands official Google Maps URLs
  - Multi-tab safe — open as many tabs as you like; they coordinate invisibly

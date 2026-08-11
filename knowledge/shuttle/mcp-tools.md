# Shuttle MCP Server — Tool Reference

> Mirror for LLM consumption. Source of truth: `app/docs/mcp-tools.md` in the Shuttle app repository (current as of app v0.10.1, 2026-08-11).

Companion to the [Using Shuttle guide](https://weaving.wwwshuttle.app/tutorials/using-shuttle/). Code-verified against `app/mcp-server/` (package `@wwwshuttle-app/mcp` v0.1.0) and the app-side bridge (`src/services/mcpBridge.ts`).

## Setup

```
claude mcp add wwwshuttle -- npx @wwwshuttle-app/mcp
```

The MCP server runs a WebSocket server on `ws://localhost:3847`; the Shuttle app connects to it as a client. Two environments:

- **`http://localhost:5173` (dev):** the app connects automatically in a dev build. No extension needed.
- **`https://wwwshuttle.app` (production):** requires (1) the Chrome bridge extension ("Download Extension" in the app menu → Claude Code section — it relays around the HTTPS→localhost mixed-content block), (2) App menu → Claude Code → **"Enable MCP"** toggled on, and (3) a **page refresh** — the connection gate only runs at startup.

Chromium-only (MV3 extension). All data lives in the browser tab's IndexedDB — the server holds no state, so **every tool fails if no Shuttle tab is open**.

## Tools (11)

Read-only: `shuttle_status`, `shuttle_list_looms`, `shuttle_get_loom`, `shuttle_get_thread`, `shuttle_search`, `shuttle_get_context`. Write: `shuttle_create_loom`, `shuttle_create_thread`, `shuttle_add_nodes`, `shuttle_update_node`, `shuttle_import_markdown`. **There are no destructive tools** — no delete, move, reorder, or rename; writes are additive or whole-content replacement.

| Tool | Input | What it does |
|---|---|---|
| `shuttle_status` | — | Reports whether a client holds the socket. Call first. Caveat: in extension mode the *extension* is the client, so this can report connected with zero Shuttle tabs open — later tools then fail with "No wwwshuttle tab found". |
| `shuttle_list_looms` | — | All looms as `- {emoji} **{title}** (id: …)`, in sidebar order. |
| `shuttle_get_loom` | `loomId` | Loom details + its threads (`nodeId`, `title`, `orderValue`, recursive `nodeCount`). Counts only — use `shuttle_get_thread` for contents. |
| `shuttle_get_thread` | `threadNodeId` | **Reads a thread's full contents**: every node in tree order as an indented outline, each with its node ID (usable with `shuttle_update_node` / as `parentNodeId` in `shuttle_add_nodes`). Reads from the database, so it works for threads that aren't currently open. |
| `shuttle_create_loom` | `title`, `emoji?`, `themeId?` (`default\|ocean\|forest\|sunset\|lavender`) | New loom, ordered last. Created with **no threads** — follow with `shuttle_create_thread` or `shuttle_import_markdown`. |
| `shuttle_create_thread` | `loomId`, `title` | New thread at the end of the loom; the title becomes the thread root node. Its description mandates adding content right after — never leave an empty thread. |
| `shuttle_add_nodes` | `threadNodeId`, `nodes: [{content, parentNodeId?}]` | Appends nodes (markdown content) under the thread (or under `parentNodeId`). Not transactional; returns a count, **not node IDs**, so you can't nest onto just-created nodes — use `shuttle_import_markdown` for hierarchy. |
| `shuttle_update_node` | `nodeId`, `content` | Wholesale content replacement (no append/patch mode). The only way to alter existing content. |
| `shuttle_search` | `query`, `loomId?` | Case-insensitive **substring** match over node content (not fuzzy). Returns snippets with loom/thread context; renders at most 10 results even when reporting a larger count. |
| `shuttle_get_context` | — | What the user is looking at: `{loomId, loomTitle, threadNodeId, threadTitle, focusedNodeId, mode}` (`viewing`/`writing`). Call before relying on `shuttle_import_markdown`'s current-thread default. |
| `shuttle_import_markdown` | `markdown`, `threadId?` | **Preferred write path** for anything structured. With a leading `# Title` → creates a whole new loom (threads split on `---`); without → appends bullets to `threadId` or the current thread. Full support for 2-space-indent hierarchy, `#anchor` declarations, and `[>>](anchor)` depth links. Goes through the same op funnel as typing, so it syncs to tabs/rooms/devices. Returns node/thread/depth-link counts + warnings. |

### Import-markdown rules that matter

- Write depth links in three passes *within one import*: content first, then `#anchor` targets, then `[>>](anchor)` references — an "Unmatched anchor link" warning means the raw `[>>](name)` text is rendered in the UI and must be fixed (re-import or `shuttle_update_node`).
- Silent-failure edge: no `# Title`, no `threadId`, and no current thread → the import has no destination. Check `shuttle_get_context` first.
- Anchors may be referenced by multiple links; unused anchors are harmless.

## Resources

- **`shuttle://usage`** (text/markdown) — the canonical usage map (the app repo's `app/docs/usage-map.md`, bundled into the package at build time). Agents should read it before advising users or generating Shuttle content.

### Reading content

Typical read flow: `shuttle_list_looms` → `shuttle_get_loom` (thread list) → `shuttle_get_thread` (contents). `shuttle_search` is for finding where something lives, not for bulk reading.

## Reliability notes

- One client at a time: the newest connection (tab or extension) wins silently. With multiple Shuttle tabs, extension-relayed commands go to an arbitrary matching tab — keep one tab open for predictable writes.
- 30-second timeout at every hop; in-flight requests reject with "PWA disconnected" if the tab closes.
- Port 3847 is hardcoded; a second server instance logs `Port 3847 is already in use` but still starts the MCP side, so tools fail at call time.
- The WebSocket has no authentication — it trusts localhost.

## Known quirks (as of 0.1.0)

- MCP server identity is `shuttle` while the documented alias is `wwwshuttle` (cosmetic).
- Earlier drifts (stale install comment, hardcoded localhost in `shuttle_status`'s message, dead `includeNodes` param) were fixed post-0.9.1 along with the addition of `shuttle_get_thread`.

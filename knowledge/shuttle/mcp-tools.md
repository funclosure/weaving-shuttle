# Shuttle MCP Server — Tool Reference

> Mirror for LLM consumption. Source of truth: `app/docs/mcp-tools.md` in the Shuttle app repository (current as of app v0.50.0 and `@wwwshuttle-app/mcp` 0.7.0, 2026-11-19).

Companion to the [Using Shuttle guide](https://weaving.wwwshuttle.app/tutorials/using-shuttle/). Code-verified against `app/mcp-server/` (package `@wwwshuttle-app/mcp` v0.1.0) and the app-side bridge (`src/services/mcpBridge.ts`).

## Setup

```
claude mcp add wwwshuttle -- npx @wwwshuttle-app/mcp
```

The MCP server runs a WebSocket server on `ws://localhost:3847`; the Shuttle app connects to it as a client. Two environments:

- **`http://localhost:5173` (dev):** the app connects automatically in a dev build. No extension needed.
- **`https://wwwshuttle.app` (production):** requires (1) the Chrome bridge extension ("Download Extension" in the app menu → Claude Code section — it relays around the HTTPS→localhost mixed-content block), (2) App menu → Claude Code → **"Enable MCP"** toggled on, and (3) a **page refresh** — the connection gate only runs at startup.

Chromium-only (MV3 extension). All data lives in the browser tab's IndexedDB — the server holds no state, so **every tool fails if no Shuttle tab is open**.

## Where the tools live

The app side of every tool below is `src/services/agentToolTable.ts`; the bridge (`src/services/mcpBridge.ts`) only looks commands up there. The same table is registered on the browser's WebMCP model context when App menu → Claude Code → **Browser agents** is on, so an in-browser agent sees every tool below other than `shuttle_status` (server-only), under their unprefixed names (`list_looms`, not `shuttle_list_looms`). `src/services/mcpToolParity.test.ts` fails when this package's tool names or params drift from the table.

Every app-side tool is declared by this package: `get_open_thread` and `get_usage_guide` since 0.4.0, the Loom chat's three (`read_chat`, `post_chat`, `save_chat_as_thread`) since 0.6.0 (app v0.49.0), `select_node`, `sync_status` and `add_image` since 0.6.0 as published (app v0.49.2), and the chat's `edit_chat`, `delete_chat` and `react_chat` from 0.7.0 (app v0.50.0).

## Tools (22)

Read-only: `shuttle_status`, `shuttle_get_open_thread`, `shuttle_list_looms`, `shuttle_get_loom`, `shuttle_get_thread`, `shuttle_search`, `shuttle_get_context`, `shuttle_get_usage_guide`, `shuttle_read_chat`, `shuttle_sync_status`. Navigation: `shuttle_select_node`. Write: `shuttle_create_loom`, `shuttle_create_thread`, `shuttle_add_nodes`, `shuttle_update_node`, `shuttle_import_markdown`, `shuttle_post_chat`, `shuttle_edit_chat`, `shuttle_delete_chat`, `shuttle_react_chat`, `shuttle_save_chat_as_thread`, `shuttle_add_image`. **There are no destructive tools for content** — no delete, move, reorder, or rename of looms, threads or nodes; writes are additive or whole-content replacement. In the Loom chat (talk, not content) an agent can delete only its own message (`shuttle_delete_chat`), and `shuttle_save_chat_as_thread` clears a chat only by keeping every message as a thread.

| Tool | Input | What it does |
|---|---|---|
| `shuttle_status` | — | Reports whether a client holds the socket. Call first. Caveat: in extension mode the *extension* is the client, so this can report connected with zero Shuttle tabs open — later tools then fail with "No wwwshuttle tab found". |
| `shuttle_get_open_thread` | — | **Call first** (0.4.0). What the person is looking at: the open loom, the open thread as an outline with each node's id and rev, the loom's other threads (ids, node counts), the mode and the focused node. |
| `shuttle_get_usage_guide` | — | (0.4.0) Shuttle's compact usage guide: shortcuts, sharing, node formatting, and how `rev` / `expectedRev` guard edits. |
| `shuttle_list_looms` | — | All looms as `- {emoji} **{title}** (id: …)`, in sidebar order. |
| `shuttle_get_loom` | `loomId` | Loom details + its threads (`nodeId`, `title`, `orderValue`, recursive `nodeCount`). Counts only — use `shuttle_get_thread` for contents. |
| `shuttle_get_thread` | `threadNodeId`, `depth?`, `under?`, `preview?`, `since?` | **Reads a thread's full contents** (for a long one: `depth`/`preview` for an outline, `under` for one subtree, 0.5.0; `since` with the snapshot token of an earlier read for only what changed, 0.5.0): every node in tree order as an indented outline, each with its node ID and rev (the ID for `shuttle_update_node` / as `parentNodeId` in `shuttle_add_nodes`; the rev as `shuttle_update_node`'s `expectedRev`). Reads from the database, so it works for threads that aren't currently open. |
| `shuttle_create_loom` | `title`, `emoji?`, `themeId?` (`default\|teal\|forest\|amber\|gold\|indigo\|rose\|mono`, the app's presets; before 0.4.0 the server listed ids that don't exist) | New loom, ordered last. Created with **no threads** — follow with `shuttle_create_thread` or `shuttle_import_markdown`. |
| `shuttle_create_thread` | `loomId`, `title` | New thread at the end of the loom; the title becomes the thread root node. Its description mandates adding content right after — never leave an empty thread. |
| `shuttle_add_nodes` | `threadNodeId`, `nodes: [{content, parentNodeId?}]` | Appends nodes (markdown content) under the thread (or under `parentNodeId`). Not transactional. Lists each created node with its ID and rev (0.3.0), so you can nest onto or edit just-created nodes; for hierarchy prefer `shuttle_import_markdown`. |
| `shuttle_update_node` | `nodeId`, `content`, `expectedRev?` | Wholesale content replacement (no append/patch mode). The only way to alter existing content. Pass `expectedRev` (the node's rev from a read, 0.3.0): if the node changed since, nothing is written and the error gives its current content and rev. |
| `shuttle_search` | `query`, `loomId?` | Case-insensitive **substring** match over node content (not fuzzy). Returns snippets with loom/thread context; renders at most 10 results even when reporting a larger count. |
| `shuttle_get_context` | — | What the user is looking at: `{loomId, loomTitle, threadNodeId, threadTitle, focusedNodeId, mode}` (`viewing`/`writing`). Call before relying on `shuttle_import_markdown`'s current-thread default. |
| `shuttle_import_markdown` | `markdown`, `threadId?` | **Preferred write path** for anything structured. With a leading `# Title` → creates a whole new loom (threads split on `---`); without → appends bullets to `threadId` or the current thread. Full support for 2-space-indent hierarchy, `#anchor` declarations, and `[>>](anchor)` depth links. Goes through the same op funnel as typing, so it syncs to tabs/rooms/devices. Returns node/thread/depth-link counts, the created thread IDs and nodes (ID and rev, first 200; 0.3.0) + warnings. |
| `shuttle_read_chat` | `loomId?`, `after?` | (0.6.0; 0.7.0 adds edits, reactions and removals) The Loom chat of a shared Loom (default: the open Loom): the last 50 messages (`id`, author, time, text, `replyTo`; your own marked `(you)`, edited ones marked, reactions listed) and a `cursor`. Pass the cursor back as `after` for what happened since ("Nothing new" when none): new messages, messages edited or reacted to, ids deleted or cleared, and the thread a saved chat went to. `reset` means the cursor predates a reload of the app: a fresh read, skip ids already seen. Shared Looms only. |
| `shuttle_post_chat` | `loomId?`, `text`, `replyTo?` | (0.6.0) Posts to the Loom chat under the workspace's sync name (what the members see). Markdown, up to 4000 characters; `replyTo` (a message id) answers that message, one level deep. Refused when the chat holds 200 messages: save it first. |
| `shuttle_edit_chat` | `id`, `text`, `loomId?` | (0.7.0) Replaces the text of one of your own chat messages; members see it marked edited. Refused on another member's message or one that is gone. |
| `shuttle_delete_chat` | `id`, `loomId?` | (0.7.0) Deletes one of your own chat messages for everyone; it cannot be undone. Replies to it stay under "Message deleted". Refused on another member's message. |
| `shuttle_react_chat` | `id`, `emoji`, `action?`, `loomId?` | (0.7.0) Reacts to a chat message with one of 👍 ✅ 👀 ❤️ 😂 🎉: a quick "seen" or "agreed" that adds no message. `action: "remove"` takes it back. |
| `shuttle_save_chat_as_thread` | `loomId?`, `title?` | (0.6.0) Keeps the Loom chat as a new thread at the end of its Loom (one node per message, `**Name** · time: text` with its reactions, replies as child nodes), then clears the chat for everyone. Returns the thread id. |
| `shuttle_select_node` | `nodeId` | (app v0.49.2) Moves the view to a node and selects it in viewing mode, so keyboard shortcuts act on it (⌥↑/⌥↓ move, Tab/⇧Tab indent, ⌘⌫ delete, Enter writes). Switches Loom and thread as needed, leaves a ⌘← history step, takes the keyboard out of any text field. Refused while an editor is open. A thread root opens its thread with nothing selected. |
| `shuttle_sync_status` | — | (app v0.49.2) Whether this device has caught up with its rooms and whether its own writes reached them: per room (the workspace room, each shared Loom) its state, `caughtUp` (online: the catch-up replay and outbox drain finished) and `unconfirmed` (batches the room has not acked; 0 means delivered), plus `allCaughtUp` / `allDelivered`. In a tab that is not the leader, it says another tab holds the rooms. |
| `shuttle_add_image` | `url?`, `data?`, `mimeType?`, `alt?`, `afterNodeId?`, `parentNodeId?`, `threadNodeId?` | (app v0.49.2) Adds a picture as a new row, stored like a pasted image (compressed to at most 2 MB, synced to rooms): `url` (http(s), `file://` or an absolute path, read by this server) or `data` (base64). Right after `afterNodeId`, or at the end of `parentNodeId` / `threadNodeId` (default: the open thread). Returns the row's id and rev and the asset id (`![alt](asset:<id>)` shows it elsewhere). Over WebMCP the page fetches `url` itself, so many sites refuse; pass `data` there. |

### Import-markdown rules that matter

- Write depth links in three passes *within one import*: content first, then `#anchor` targets, then `[>>](anchor)` references — an "Unmatched anchor link" warning means the raw `[>>](name)` text is rendered in the UI and must be fixed (re-import or `shuttle_update_node`).
- Silent-failure edge: no `# Title`, no `threadId`, and no current thread → the import has no destination. Check `shuttle_get_context` first.
- Anchors may be referenced by multiple links; unused anchors are harmless (see usage-map §format notes).

## Resources

- **`shuttle://usage`** (text/markdown) — the canonical usage map (this repo's `app/docs/usage-map.md`, bundled into the package at build time). Agents should read it before advising users or generating Shuttle content.

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

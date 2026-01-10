# Shuttle Markdown Import Format

> **Source**: This spec is synced with `app/src/components/ImportMarkdownDialog.tsx`

## Import Modes
1. **New Loom**: Start with `# Loom Title` → creates new Loom with threads
2. **Current Loom**: No title + `---` separators → adds threads to current loom
3. **Current Thread**: No title, no separators → adds nodes to focused thread

## Syntax

### Structure
- Use bullet lists with `-` for nodes
- **Root nodes can omit the `-`** — plain text becomes the thread title
- Indent with **2 spaces** for child nodes
- Separate threads with `---` (horizontal rule)
- Use `- ## Section` for section headers inside bullets

```markdown
# My Loom

Slash Commands           <- root node (no bullet needed)
  - Child node           <- children use bullets
  - Another child

---

Another Thread Title     <- root node for second thread
  - Its children
```

### Depth Links (Bidirectional Connections)
- **Declare anchor**: `- Node content #anchor-name` (must be at END of line)
- **Link to anchor**: `- Another node [>>](anchor-name)`
- Anchors: lowercase letters, numbers, hyphens, underscores (Unicode supported)
- Case-sensitive; unmatched links show warning

## Example - Cross-Thread Links
```markdown
# Knowledge Base

- Overview #overview
  - Key concepts explained here
  - See details in [>>](deep-dive)
- Quick Reference
  - Summary points [>>](overview)

---

- Deep Dive #deep-dive
  - Detailed explanation
  - Based on [>>](overview)
```

## Example - Video Summary
Thread title: `https://youtube.com/watch?v=...` (timestamps become clickable)
```markdown
- ## Timestamps
- [0:00] Introduction #ts-intro
- [5:30] Key concept [>>](concept-1) #ts-key
- ## Key Concepts
- **Important Concept** #concept-1
  - Explanation [>>](ts-intro)
```

## URL Patterns
| Pattern | Use |
|---------|-----|
| `/i/v1/{lz-compressed}` | Inline import (small content, LZ-String compressed) |
| `/import?url={url}` | Fetch from URL (Gist, GitHub raw) |
| `/import?url={url}&loom=current` | Add threads to current loom |
| `/import?url={url}&thread=current` | Add nodes to current thread |

### Inline Import Limits
- Max recommended URL length: ~2000 characters
- Original content up to ~4000 characters compresses well
- Use `LZString.compressToEncodedURIComponent(markdown)` for compression

## Content Guidelines
- **3-7 threads** per loom; each thread = one topic
- **1-3 depth links** per thread creates connected feel
- **2-3 nesting levels** is ideal; keep nodes scannable
- First bullet becomes thread title in sidebar
- Anchor names: short but descriptive (`#intro`, `#key-insight`, `#conclusion`)

---
name: to-shuttle
description: Package research or content for Shuttle import. Use when user wants to save findings to Shuttle, export notes as a Loom, or send content to their Shuttle app. Creates an import link or formatted markdown.
---

# To Shuttle

Package content for import into Shuttle (https://wwwshuttle.app).

> **Reference**: See [shuttle-markdown-spec.md](./shuttle-markdown-spec.md) for the complete format specification.

## Shuttle Markdown Format

```markdown
# Loom Title

Thread Title                     <- root node (no bullet needed)
  - Child node with details      <- children use bullets
  - Another child
  - ```bash                      <- code blocks work inside bullets
    example command
    ```

---

Another Thread #anchor-name
  - Links to anchor above [>>](anchor-name)
  - Creates bidirectional depth connection
```

### Default: Add Threads to Current Loom

**Most common use case** — user wants to add content to their existing loom.

```markdown
Thread Title
  - Content here
  - More content

---

Another Thread
  - Its content
```

No `# Title` = adds threads to current loom (via ⌘I paste).

### Create New Loom (when requested)

Only add `# Title` when user explicitly wants a new loom:

```markdown
# New Loom Title

Thread Title
  - Content...
```

With `# Title` = can use URL import (`/i/v1/...`).

### Syntax Rules

| Element | Syntax | Purpose |
|---------|--------|---------|
| Loom title | `# Title` | Creates new Loom (only if requested) |
| Root nodes | Plain text | Thread title — no `-` needed |
| Child nodes | `- content` | Bullet points become child nodes |
| Hierarchy | 2-space indent | Creates parent-child relationships |
| Code blocks | ` ``` ` | Fenced code blocks work inside bullets |
| Thread separator | `---` | Starts a new thread |
| Anchor declaration | `#anchor-name` at end | Marks a node as linkable target |
| Depth link | `[>>](anchor-name)` | Creates bidirectional link to anchor |

## Content Design Principles

### Writing Style (Tailwind/Nextra-inspired)
- **Friendly, conversational tone** — "just run" not "can be executed"
- **Short, punchy sentences** — scannable, not walls of text
- **Code blocks as visual anchors** — break up text, show real examples
- **Progressive disclosure** — what/why first, details later

### Thread Structure
- **Each thread = one topic/chapter** — threads appear in left sidebar
- **Root node = thread title** — plain text, no bullet needed
- **Nest for hierarchy** — use indentation for sub-points, definitions, examples

### Depth Links (The Magic)
Depth links create Shuttle's two-dimensional navigation:
- **Horizontal**: Moving between threads (← →)
- **Vertical**: Diving into depth links (⌘→) and surfacing back (⌘←)

**Best practices for depth links:**
- Link related concepts across different threads
- Link from specific examples back to general principles
- Link from questions to their explorations
- Create a web of connections, not just linear references

### Content Organization Patterns

**Pattern 1: Documentation (with code blocks)**
```markdown
# API Guide

Getting Started
  - Quick setup — just three steps
  - ```bash
    npm install my-package
    ```

Configuration #config
  - ```json
    { "key": "value" }
    ```
  - See advanced options [>>](advanced)

---

Advanced Options #advanced
  - Builds on basic config [>>](config)
  - For power users
```

**Pattern 2: Concept Web**
```markdown
# Topic Overview

Main Concept #main
  - Definition
  - Why it matters [>>](application)

---

Application #application
  - Real-world example [>>](main)
  - How to implement
```

**Pattern 3: Question → Exploration**
```markdown
# Research Topic

Key Question #question
  - Initial hypothesis
  - What we're exploring

---

Deep Dive #exploration
  - Detailed analysis [>>](question)
  - Conclusions that answer [>>](question)
```

## Output Strategy

### Step 1: Design the Content Structure

Before formatting:
1. Identify 3-7 main threads (topics/sections)
2. Plan depth link connections between related concepts
3. Choose meaningful anchor names (lowercase, hyphens)

### Step 2: Format as Shuttle Markdown

Transform content into the format above:
- `# Title` as the loom name
- `- ` bullets for each point
- 2-space indentation for hierarchy
- `---` between threads
- `#anchors` on linkable nodes
- `[>>](anchor)` for connections

### Step 3: Generate Import Link

**IMPORTANT:** The inline import (`/i/v1/`) requires a `# Title` line to create a new loom. Without it, the import will fail.

**For content under ~4000 characters (original):**

Use LZ-String compression to create inline URL. The compression typically achieves 60-80% of original size.

```javascript
// Generate and write link to file (REQUIRED - terminals break long URLs)
const LZString = require('lz-string');
const fs = require('fs');
const markdown = `# Your Title Here\n\n- Content...`;
const compressed = LZString.compressToEncodedURIComponent(markdown);
const url = `https://wwwshuttle.app/i/v1/${compressed}`;
fs.writeFileSync('shuttle-import-link.txt', url);
```

**For larger content:**

Option A: Create a GitHub Gist
1. Create new Gist with `.md` extension
2. Get the "Raw" URL
3. Generate URL: `https://wwwshuttle.app/import?url={raw-gist-url}`

Option B: Provide markdown for manual paste
```
To import into Shuttle:
1. Open Shuttle → Menu → Import (or press ⌘I)
2. Select "Paste Text" tab
3. Paste the markdown below
```

### Step 4: Ask Delivery Preference

**For adding to current loom (default):**
```
Ready to add to your current loom. Output the markdown for you to paste (⌘I)?
```

**For creating new loom:**
```
How would you like the Shuttle link?
- Clipboard (pbcopy) — paste in browser
- File — open and copy URL
- QR terminal — scan with phone (small content only)
- QR image — shareable PNG (small content only)
```

### Step 5: Deliver the Link

**CRITICAL:** Never output the raw URL directly — terminals wrap long URLs across lines, breaking them.

**Option A: Copy to clipboard (preferred on macOS)**
```bash
node -e "
const LZString = require('lz-string');
const markdown = \`# Loom Title

Thread Title
  - Content...
\`;
const compressed = LZString.compressToEncodedURIComponent(markdown);
console.log('https://wwwshuttle.app/i/v1/' + compressed);
" | pbcopy && echo "Link copied to clipboard!"
```

**Option B: Write to file**
```bash
node -e "
const LZString = require('lz-string');
const fs = require('fs');
const markdown = \`# Loom Title

Thread Title
  - Content...
\`;
const compressed = LZString.compressToEncodedURIComponent(markdown);
fs.writeFileSync('shuttle-import-link.txt', 'https://wwwshuttle.app/i/v1/' + compressed);
console.log('Link written to shuttle-import-link.txt');
"
```

**Option C: QR code in terminal (scan with phone)**
```bash
node -e "
const QRCode = require('qrcode');
const LZString = require('lz-string');
const markdown = \`# Loom Title

Thread Title
  - Content...
\`;
const url = 'https://wwwshuttle.app/i/v1/' + LZString.compressToEncodedURIComponent(markdown);
QRCode.toString(url, {type: 'terminal', small: true}, (err, str) => console.log(str));
"
```
Requires: `pnpm add -D qrcode`

**Note:** QR codes work best for small looms (~500 chars original). Longer content produces dense QR codes that are hard to scan. Use pbcopy or file for larger content.

**Option D: QR as PNG image (shareable)**
```bash
node -e "
const QRCode = require('qrcode');
const LZString = require('lz-string');
const markdown = \`# Loom Title

Thread Title
  - Content...
\`;
const url = 'https://wwwshuttle.app/i/v1/' + LZString.compressToEncodedURIComponent(markdown);
QRCode.toFile('shuttle-qr.png', url, { width: 400 }, (err) => {
  if (err) console.error(err);
  else console.log('QR saved to shuttle-qr.png');
});
"
```
Opens with `open shuttle-qr.png` — shareable via Slack, email, etc.

**Then tell the user:**
```
Done! Link copied to clipboard (or scan QR / open file).

This loom contains:
- X threads covering [topics]
- Y depth links connecting [concepts]
```

**If markdown fallback (for adding to existing loom):**
```
Here's your content formatted for Shuttle:

[Include the markdown]

To import:
1. Open your loom in Shuttle
2. Press ⌘I (Import)
3. Paste the markdown above
```

Note: To add threads to an existing loom (without creating new), omit the `# Title` and use manual paste.

## Complete Example: Technical Documentation

**Input:** "Create a loom about slash commands in Claude Code"

**Output:**

```markdown
# Slash Commands

Slash Commands
  - Your own shortcuts for Claude Code
  - Create a file, run a command — that's it

The file
  - Drop a `.md` in `.claude/commands/`
  - ```text
    .claude/commands/my-command.md
    ```

Run it
  - ```bash
    /my-command
    /my-command with-args
    ```

The structure #structure
  - ```markdown
    ---
    description: What it does
    argument-hint: [args]
    allowed-tools: Bash(git:*)
    ---

    Your instructions here...
    ```

Frontmatter options
  - `description` — shows when you type `/`
  - `argument-hint` — like `[patch|minor|major]`
  - `allowed-tools` — skip permission prompts [>>](patterns)

Tool patterns #patterns
  - ```text
    Bash(gh:*)      # any gh command
    Bash(git:*)     # any git command
    ```
  - Works with the structure above [>>](structure)

Team sharing
  - Commit `.claude/commands/` to git
  - Everyone gets the same commands
```

This creates:
- Single thread with clean hierarchy
- Code blocks as visual anchors
- Depth links connecting related sections
- Friendly, scannable content

## URL Patterns Reference

| Pattern | Use Case |
|---------|----------|
| `/i/v1/{compressed}` | Inline import (recommended for most content) |
| `/import?url={url}` | URL import (for very large content via Gist) |
| `/import?url={url}&loom=current` | Add threads to user's current loom |
| `/import?url={url}&thread=current` | Add nodes to user's current thread |

## Quick Tips

- **Thread count**: 3-7 threads works well; more becomes hard to navigate
- **Anchor names**: Keep short but descriptive (`#intro`, `#key-insight`, `#conclusion`)
- **Link density**: Aim for 1-3 depth links per thread for a connected feel
- **Hierarchy depth**: 2-3 levels of nesting is ideal; deeper gets hard to read
- **Content length**: Each node should be scannable (1-3 sentences typically)

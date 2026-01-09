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

- First node in first thread #anchor-name
  - Nested child node (2-space indent)
  - Another child
- Second root node

---

- First node in second thread
- This links to the anchor above [>>](anchor-name)
  - The link creates bidirectional depth connection
```

### Syntax Rules

| Element | Syntax | Purpose |
|---------|--------|---------|
| Loom title | `# Title` | Creates new Loom (required) |
| Nodes | `- content` | Bullet points become nodes |
| Hierarchy | 2-space indent | Creates parent-child relationships |
| Thread separator | `---` | Starts a new thread |
| Anchor declaration | `#anchor-name` at end | Marks a node as linkable target |
| Depth link | `[>>](anchor-name)` | Creates bidirectional link to anchor |

## Content Design Principles

### Thread Structure
- **Each thread = one topic/chapter** - threads appear in left sidebar
- **First bullet = thread title** - shown in thread list, keep it concise
- **Nest for hierarchy** - use indentation for sub-points, definitions, examples

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

**Pattern 1: Concept Web**
```markdown
# Topic Overview

- Main Concept #main
  - Definition
  - Why it matters [>>](application)

---

- Application #application
  - Real-world example [>>](main)
  - How to implement
```

**Pattern 2: Question → Exploration**
```markdown
# Research Topic

- Key Question #question
  - Initial hypothesis
  - What we're exploring

---

- Deep Dive #exploration
  - Detailed analysis [>>](question)
  - Supporting evidence
  - Conclusions that answer [>>](question)
```

**Pattern 3: Timeline with Cross-References**
```markdown
# Historical Events

- Event A (1900) #event-a
  - Context
  - Impact on [>>](event-b)

---

- Event B (1950) #event-b
  - How it built on [>>](event-a)
  - New developments
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

**For content under ~4000 characters (original):**

Use LZ-String compression to create inline URL. The compression typically achieves 60-80% of original size.

```javascript
// In browser console or Node.js with lz-string package:
const LZString = require('lz-string'); // or import in browser
const compressed = LZString.compressToEncodedURIComponent(markdown);
const url = `https://wwwshuttle.app/i/v1/${compressed}`;
console.log(url);
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

### Step 4: Present to User

**If link generated:**
```
Here's your Shuttle import link:
https://wwwshuttle.app/i/v1/...

Click to import "{Loom Title}" into Shuttle.

This loom contains:
- X threads covering [topics]
- Y depth links connecting [concepts]
```

**If markdown fallback:**
```
Here's your content formatted for Shuttle:

[Include the markdown]

To import:
1. Open Shuttle → Menu → Import (⌘I)
2. Paste the markdown above
```

## Complete Example: Research Notes

**Input:** "Create a loom about the science of habits"

**Output:**

```markdown
# The Science of Habits

- What is a habit? #habit-def
  - Automatic behavior triggered by context
  - Requires minimal conscious thought
  - Built through repetition over time
- The Habit Loop #habit-loop
  - Cue: The trigger that initiates the behavior
  - Routine: The behavior itself
  - Reward: The benefit you get [>>](dopamine)

---

- The Neuroscience #neuroscience
  - Basal ganglia stores habit patterns
  - Prefrontal cortex disengages as habits form [>>](habit-def)
  - Dopamine and reward prediction #dopamine
    - Anticipation drives motivation [>>](habit-loop)
    - Unexpected rewards strengthen habits

---

- Changing Habits #change
  - Identify the cue [>>](habit-loop)
  - Keep the same reward
  - Replace only the routine
- The 21-Day Myth
  - Actually takes 18-254 days (avg 66)
  - Depends on complexity [>>](neuroscience)
  - Consistency matters more than duration

---

- Practical Strategies
  - Habit stacking: attach new to existing [>>](habit-loop)
  - Environment design: modify cues [>>](change)
  - Implementation intentions: "When X, I will Y"
  - Start tiny: 2-minute versions first
```

This creates:
- 4 threads (Definition, Neuroscience, Change, Strategies)
- 8 depth links connecting concepts across threads
- Rich hierarchical structure within each thread

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

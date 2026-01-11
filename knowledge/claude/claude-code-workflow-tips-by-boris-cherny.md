# Claude Code Workflow Tips by Boris Cherny

- [Boris Cherny: I'm Boris and I created Claude Code. Lots of people have asked...](https://x.com/bcherny/status/2007179832300581177)
  - [Open with Shuttle](https://wwwshuttle.app/import?url=https://raw.githubusercontent.com/funclosure/weaving-shuttle/refs/heads/main/knowledge/claude/claude-code-workflow-tips-by-boris-cherny.md)

---

Quick Summary #summary
- Plan first — use Plan mode before executing [>>](plan-mode)
- Verify aggressively — tests make quality 2-3x better [>>](verification)
- Parallelize everywhere — 5+ local + 5-10 web sessions [>>](parallel-sessions)
- Build shared memory — CLAUDE.md + commands + hooks [>>](shared-memory)
- Trust bigger models — Opus 4.5 with thinking [>>](model-choice)
- Source: [Claude Code Docs](https://code.claude.com/docs)

---

Parallel Sessions #parallel-sessions
- ## Terminal (5 sessions)
  - Use numbered tabs 1-5 in iTerm
  - Enable system notifications for when input is needed
  - [iTerm 2 notifications setup](https://code.claude.com/docs/en/terminal-config#iterm-2-system-notifications)
- ## Web (5-10 sessions)
  - Run additional Claudes on [claude.ai/code](https://claude.ai/code)
  - Hand off local → web using `&` prefix [>>](terminal-to-web)
  - Teleport web → local using `--teleport` [>>](web-to-terminal)
- ## Mobile
  - Start sessions from Claude iOS app throughout the day
  - Check in on progress later from any device

---

Model Choice #model-choice
- Always use **Opus 4.5** with thinking mode enabled
- Boris: "It's the best coding model I've ever used. Even though it's bigger & slower than Sonnet, since you have to steer it less and it's better at tool use, it is almost always faster in the end."
- ## When to Use Each Model
  - **Opus 4.5**: Complex reasoning, code migration, heavy agentic workflows
  - **Sonnet 4.5**: Most dev work, long-context tasks, orchestrating multi-agent systems
  - **Haiku 4.5**: High-volume simple tasks, executor in multi-agent systems, real-time needs
- ## Pricing (per million tokens)
  - Haiku: $1 input / $5 output
  - Sonnet: $3 input / $15 output
  - Opus: $5 input / $25 output

---

Shared Team Memory #shared-memory
- ## CLAUDE.md #claude-md
  - Single file checked into git, whole team contributes
  - Add mistakes/patterns so Claude learns not to repeat them [>>](compounding)
  - Tag `@.claude` in PRs to suggest updates (via GitHub action)
  - Hierarchical loading: enterprise → project → user level
  - Use `/init` to bootstrap, `/memory` to edit
  - [Memory docs](https://code.claude.com/docs/en/memory)
- ## Slash Commands #slash-commands
  - Store in `.claude/commands/` as markdown files
  - Turn frequent workflows into commands (e.g., `/commit-push-pr`)
  - Frontmatter options: `description`, `argument-hint`, `allowed-tools`
  - Use `$ARGUMENTS` for user input
  - [Slash commands docs](https://code.claude.com/docs/en/slash-commands)
- ## Hooks #hooks
  - PostToolUse hook → auto-format code (handles the last 10%)
  - Pre-allow safe commands in `.claude/settings.json` → avoid constant prompts
  - [Hooks docs](https://code.claude.com/docs/en/hooks)

---

Plan & Verify #plan-verify
- ## Plan Mode #plan-mode
  - Enter with shift+tab x2
  - Iterate your plan with Claude until solid
  - Then switch to auto-accept edits — often becomes one-shots
- ## Verification #verification
  - Give Claude reliable ways to check its own work
  - Unit tests, browser tests, Chrome extension, simulators
  - Verification → 2-3x better final quality
  - For long tasks: background verification agent, Stop hook, or ralph-wiggum plugin

---

Claude Code Web #claude-web
- [claude.ai/code](https://claude.ai/code) — research preview
- Available for Pro, Max, Team premium, and Enterprise premium users
- ## Use Cases
  - Questions about code architecture
  - Bug fixes and routine tasks
  - Parallel work on multiple bugs
  - Repos not on your local machine
  - Backend changes (write tests → write code)
  - Async tasks that don't need frequent steering
- ## Terminal → Web #terminal-to-web
  - Prefix message with `&` to send to web
  - Or: `claude --remote "Fix the auth bug"`
  - Each `&` creates independent web session
  - Use `/tasks` to check progress
- ## Web → Terminal #web-to-terminal
  - Use `/teleport` or `/tp` for interactive picker
  - Or: `claude --teleport` or `claude --teleport <session-id>`
  - From `/tasks` command: press `t` to teleport
  - Requires: clean git state, correct repo, branch pushed to remote

---

Compounding Engineering #compounding
- Philosophy from [Dan Shipper](https://every.to/chain-of-thought/compound-engineering-how-every-codes-with-agents)
- Each feature should make subsequent features easier to build, not harder
- ## The Four-Step Loop
  - **Plan**: Agents research and synthesize implementation plans
  - **Work**: Agents write code and tests
  - **Review**: Engineer reviews output and lessons learned
  - **Compound**: Feed results back to make next loop better [>>](claude-md)
- ## Time Distribution
  - 80% in Plan and Review phases
  - 20% in Work and Compound phases
  - Inverts traditional engineering where coding dominates
- ## Key Principles
  - Build imperfect 60% solutions first
  - Use immediately on real work — don't wait until "ready"
  - Feel the friction, note the gaps
  - Small improvements compound over time
  - Review architecture early when changes are cheap
- ## Why It Works with AI
  - Iteration cost approaches zero
  - Improving a skill/command takes minutes, not hours
  - Learning compounds across sessions
  - Skills compose and improvements propagate
- Source: [every.to/chain-of-thought](https://every.to/chain-of-thought/compound-engineering-how-every-codes-with-agents)

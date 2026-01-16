# Claude Code Workflow Tips by Boris Cherny

- [Boris Cherny: I'm Boris and I created Claude Code. Lots of people have asked...](https://x.com/bcherny/status/2007179832300581177)
  - [Open with Shuttle](https://wwwshuttle.app/import?url=https://raw.githubusercontent.com/funclosure/weaving-shuttle/refs/heads/main/knowledge/claude/claude-code-workflow-tips-by-boris-cherny.md)

---

Boris's 13 Tips #the-13-tips
- 1/ Run multiple Claudes in parallel in terminal (5 tabs numbered 1-5), use system notifications to track when input is needed [>>](parallel-sessions)
  - [@bcherny](https://x.com/bcherny/status/2007179833990885678)
- 2/ Run additional Claudes on claude.ai/code in parallel, hand off sessions between terminal and web, start sessions from phone app [>>](claude-web)
  - [@bcherny](https://x.com/bcherny/status/2007179836704600237)
- 3/ Use Opus 4.5 with thinking enabled for everything — slower but requires less steering, faster overall than smaller models [>>](model-choice)
  - [@bcherny](https://x.com/bcherny/status/2007179838864666847)
- 4/ Share a single CLAUDE.md (checked into git), update collaboratively when Claude makes errors to prevent repeats [>>](claude-md)
  - [@bcherny](https://x.com/bcherny/status/2007179840848597422)
- 5/ During code reviews, tag @.claude on PRs to update the docs file, using the Claude Code GitHub action [>>](compounding)
  - [@bcherny](https://x.com/bcherny/status/2007179842928947333)
- 6/ Start most sessions in Plan mode (shift+tab x2), refine iteratively, then switch to auto-accept edits for one-shot implementation [>>](plan-mode)
  - [@bcherny](https://x.com/bcherny/status/2007179845336527000)
- 7/ Use slash commands for repetitive workflows (in .claude/commands/), like /commit-push-pr, with inline bash for efficiency [>>](slash-commands)
  - [@bcherny](https://x.com/bcherny/status/2007179847949500714)
- 8/ Employ subagents for common tasks — code-simplifier to refine code, verify-app for end-to-end testing [>>](subagents)
  - [@bcherny](https://x.com/bcherny/status/2007179850139000872)
- 9/ Use a PostToolUse hook to auto-format Claude's code, handling the last 10% of formatting to avoid CI errors [>>](hooks)
  - [@bcherny](https://x.com/bcherny/status/2007179852047335529)
- 10/ Avoid --dangerously-skip-permissions; use /permissions to pre-allow safe bash commands in .claude/settings.json [>>](permissions)
  - [@bcherny](https://x.com/bcherny/status/2007179854077407667)
- 11/ Let Claude use your tools autonomously — Slack via MCP, BigQuery queries, Sentry logs (configs in .mcp.json) [>>](mcp-tools)
  - [@bcherny](https://x.com/bcherny/status/2007179856266789204)
- 12/ For long tasks: background agent verification, Stop hooks, or ralph-wiggum plugin; use --permission-mode=dontAsk in sandboxes [>>](ralph-wiggum)
  - [@bcherny](https://x.com/bcherny/status/2007179858435281082)
- 13/ Provide Claude a verification method (e.g., Chrome extension for UI) to 2-3x quality — invest in domain-specific feedback loops [>>](verification)
  - [@bcherny](https://x.com/bcherny/status/2007179861115511237)

---

Background #background
- Boris started Claude Code as a side project in September 2024
- Initially just a terminal tool that told him what song was playing
- Breakthrough: gave Claude access to filesystem and bash
- "Claude began exploring my codebase on its own... mind-blowing"
- His manager's advice: "Don't build for the model of today, build for the model six months from now"
- ## Results (30 days)
  - 259 PRs landed
  - 497 commits
  - 40k lines added, 38k removed
  - 80-90% of Claude Code is now written by Claude Code
  - Anthropic productivity per engineer grew 70%
- Source: [VentureBeat](https://venturebeat.com/technology/the-creator-of-claude-code-just-revealed-his-workflow-and-developers-are), [Every.to Podcast](https://every.to/podcast/how-to-use-claude-code-like-the-people-who-built-it)

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
  - **Each session uses its own git checkout** (not branches or worktrees)
  - This avoids merge conflicts between parallel sessions
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
  - **Boris's CLAUDE.md is only ~2.5k tokens** — quality over quantity
  - Add mistakes/patterns so Claude learns not to repeat them [>>](compounding)
  - Tag `@.claude` in PRs to suggest updates (via GitHub action)
  - Hierarchical loading: enterprise → project → user level
  - Use `/init` to bootstrap, `/memory` to edit
  - [Memory docs](https://code.claude.com/docs/en/memory)
- ## Slash Commands #slash-commands
  - Store in `.claude/commands/` as markdown files
  - `/commit-push-pr` runs dozens of times daily
  - Pre-compute context with inline bash (git status, etc.) to reduce model back-and-forth
  - Frontmatter options: `description`, `argument-hint`, `allowed-tools`
  - Use `$ARGUMENTS` for user input
  - [Slash commands docs](https://code.claude.com/docs/en/slash-commands)
- ## Hooks #hooks
  - PostToolUse hook → auto-format code (handles the last 10%)
  - Pre-allow safe commands in `.claude/settings.json` → avoid constant prompts
  - [Hooks docs](https://code.claude.com/docs/en/hooks)
- ## Permissions #permissions
  - Don't use `--dangerously-skip-permissions`
  - Use `/permissions` to pre-allow safe bash commands
  - Share allowlist in `.claude/settings.json` with team

---

Subagents #subagents
- Specialized AI personas with separate context windows
- Keep main thread focused, give each subagent clear mandate
- ## Boris's Subagent Files
  - `code-simplifier.md` — cleans up architecture post-work
  - `verify-app.md` — end-to-end testing instructions
  - `build-validator.md` — validates builds
  - `code-architect.md` — architecture decisions
  - `oncall-guide.md` — oncall assistance
- ## code-simplifier Example
  - Tools: Read, Edit, Grep, Glob
  - "You are a code simplification expert. Goal: make code more readable and maintainable without changing functionality."
  - Principles: reduce complexity/nesting, extract repeated logic, meaningful variable names, simplify conditionals
- [Subagents docs](https://code.claude.com/docs/en/sub-agents)

---

Plan & Verify #plan-verify
- ## Plan Mode #plan-mode
  - Enter with shift+tab x2
  - Iterate your plan with Claude until solid
  - "A good plan is really important!"
  - Then switch to auto-accept edits — often becomes one-shots
- ## Verification #verification
  - **Most important thing for great results**
  - Give Claude a way to verify its work → 2-3x quality
  - ## Chrome Extension Example
    - Claude tests every UI change using Claude Chrome extension
    - Opens browser, tests the UI, iterates until code works and UX feels good
    - Domain-specific feedback loop for frontend work
  - Other options: unit tests, browser tests, simulators
  - For long tasks: background verification agent, Stop hook, or ralph-wiggum [>>](ralph-wiggum)

---

MCP Tools #mcp-tools
- Let Claude use your tools autonomously
- ## Examples Boris Uses
  - **Slack** — search and post via MCP server
  - **BigQuery** — run queries via `bq` CLI for analytics
  - **Sentry** — grab error logs for debugging
- ## Setup
  - Configure in `.mcp.json`
  - Check configs into git to share with team
- [MCP docs](https://code.claude.com/docs/en/mcp)

---

Ralph Wiggum #ralph-wiggum
- Autonomous loop for long-running tasks
- Named after Ralph Wiggum from The Simpsons — persistent iteration despite setbacks
- ## How It Works
  - Give Claude a task, it works on it
  - When Claude tries to exit, Stop hook blocks exit (exit code 2)
  - Original prompt gets re-injected
  - Files from previous iteration still there — each iteration builds on the last
- ## Installation
  - `/plugin marketplace add anthropics/claude-code`
  - `/plugin install ralph-wiggum@claude-plugins-official`
  - `/cancel-ralph` to stop an active loop
- ## Cost Warning
  - 50 iterations on medium codebase can cost $50-100+
  - Always use `--max-iterations` as cost control
  - Start with 20-30 iterations and adjust
- ## Best Practices
  - Success criteria must be specific and verifiable
  - Bad: "good code quality"
  - Good: "ESLint passes with zero warnings" or "All unit tests pass"
- Source: [Ralph Wiggum Plugin](https://github.com/anthropics/claude-code/blob/main/plugins/ralph-wiggum/README.md)

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

# Claude Code Workflow Tips by Boris Cherny

- Claude Code Tips from Boris Cherny #claude-code-tips-from-boris-ch-91a517
  - ![Boris Cherny: I'm Boris and I created Claude Code. Lots of people have asked ...](https://x.com/bcherny/status/2007179832300581177) #boris-cherny-i-m-boris-and-a87977
  - [@bcherny](https://x.com/bcherny/status/2007179833990885678) #bcherny-https-x-com-bcher-ed8cd1
    - I run 5 Claudes in parallel in my terminal. I number my tabs 1-5, #i-run-5-claudes-in-parallel-in-9c7aea
    - and use system notifications to know when a Claude needs input #and-use-system-notifications-t-270b1e
    - [Cluade Docs - iTerm 2 system notifications](https://code.claude.com/docs/en/terminal-config#iterm-2-system-notifications) #cluade-docs-iterm-2-system-8f0405
    - <img src="https://pbs.twimg.com/media/G9rtc4EasAELEzh?format=jpg&name=4096x4096" height="200"> #img-src-https-pbs-twimg-co-27a558
  - [@bcherny](https://x.com/bcherny/status/2007179836704600237) #bcherny-https-x-com-bcher-184f70
    - 2/ I also run 5-10 Claudes on http://claude.ai/code, in parallel with my local Claudes. #2-i-also-run-5-10-claudes-on-7728ed
    - As I code in my terminal, I will often hand off local sessions to web (using &), or manually kick off sessions in Chrome, and sometimes I will --teleport back and forth. [>>](moving-tasks-between-web-bb03ec) #as-i-code-in-my-terminal-i-wi-984c57
    - I also start a few sessions from my phone (from the Claude iOS app) every morning and throughout the day, and check in on them later. #i-also-start-a-few-sessions-fr-f9ab71
  - [@bcherny](https://x.com/bcherny/status/2007179838864666847) #bcherny-https-x-com-bcher-c06666
    - 3/ I use `Opus 4.5` with thinking for everything. It's the best coding model I've ever used, and even though it's bigger & slower than Sonnet, since you have to steer it less and it's better at tool use, it is almost always faster than using a smaller model in the end. [>>](opus-4-5-complex-reasoning-h-93af77) #3-i-use-opus-4-5-with-think-2984c0
  - [@bcherny](https://x.com/bcherny/status/2007179840848597422) #bcherny-https-x-com-bcher-f7fca8
    - 4/ Our team shares a single `CLAUDE.md` for the Claude Code repo. We check it into git, and the whole team contributes multiple times a week. Anytime we see Claude do something incorrectly we add it to the `CLAUDE.md`, so Claude knows not to do it next time. [>>](claude-md-is-a-memory-file-tha-149caf) #4-our-team-shares-a-single-c-1abb13
    - Other teams maintain their own `CLAUDE.md`'s. It is each team's job to keep theirs up to date. #other-teams-maintain-their-own-d86388
    - <img src="https://pbs.twimg.com/media/G9rfKYRbkAA6Q3w?format=jpg&name=medium" height="300"> #img-src-https-pbs-twimg-co-8daec5
  - [@bcherny](https://x.com/bcherny/status/2007179842928947333) #bcherny-https-x-com-bcher-c2e552
    - 5/ During code review, I will often tag @.claude on my coworkers' PRs to add something to the `CLAUDE.md` as part of the PR. #5-during-code-review-i-will-7410e1
    - We use the Claude Code Github action (/install-github-action) for this. #we-use-the-claude-code-github-039630
    - It's our version of [@danshipper](https://x.com/danshipper)'s Compounding Engineering [>>](core-philosophy-3527be) #it-s-our-version-of-danshipp-19edc7
  -  #node-90106d

---

- Workflow Summary #workflow-summary-0eb570
  - # Quick summary #quick-summary-933aff
    - Plan first #plan-first-eb8413
    - Verify aggressively #verify-aggressively-4d94b0
    - Parallelize sessions everywhere #parallelize-sessions-everywher-ab17c1
    - Build shared memory (CLAUDE.md + commands + subagents) #build-shared-memory-claude-md-72e30b
    - Trust bigger models with good feedback loops #trust-bigger-models-with-good-9f9a62
  - # Tips #tips-72d19c
    - ## Run multiple parallel Claudes in terminal #run-multiple-parallel-claud-08da1b
      - Use 5 numbered tabs (1-5) #use-5-numbered-tabs-1-5-16ea9a
      - Enable system notifications so you know when input is needed #enable-system-notifications-so-39d020
        - [Cluade Docs - iTerm 2 system notifications](https://code.claude.com/docs/en/terminal-config#iterm-2-system-notifications) #cluade-docs-iterm-2-system-8133ed
    - ## Run even more sessions across platforms #run-even-more-sessions-acro-41560b
      - 5–10 additional Claudes on claude.ai/code (web) #5-10-additional-claudes-on-cla-232f86
      - Start/check sessions from phone (iOS app) throughout the day #start-check-sessions-from-phon-31d264
      - Hand off between local ↔ web using `&` or `--teleport` #hand-off-between-local-web-u-614360
    - ## Model choice #model-choice-33e88d
      - Always use Opus 4.5 with thinking mode enabled #always-use-opus-4-5-with-think-e02440
      - Bigger/slower per response → much faster overall (better tool use & less steering needed) #bigger-slower-per-response-m-96695f
    - ## Maintain shared team knowledge #maintain-shared-team-knowle-372675
      - Keep a single `CLAUDE.md` file checked into git #keep-a-single-claude-md-file-7fd906
      - Whole team contributes — document mistakes/patterns so Claude learns not to repeat them #whole-team-contributes-docum-fc83ab
      - Tag @.claude in PRs to suggest CLAUDE.md updates (via GitHub action) #tag-claude-in-prs-to-suggest-0ad495
    - ## Start most work in Plan mode #start-most-work-in-plan-mod-4bf7a2
      - Use Plan mode (shift+tab ×2) for goals like writing a PR #use-plan-mode-shift-tab-2-f-b702fd
      - Iterate plan with Claude until solid → then switch to auto-accept edits (often 1-shots) #iterate-plan-with-claude-until-584ad4
    - ## Create reusable slash commands #create-reusable-slash-comma-339bd8
      - Store in `.claude/commands` #store-in-claude-commands-0eaa9a
      - Turn frequent inner-loop workflows into commands (e.g. `/commit-push-pr`) #turn-frequent-inner-loop-workf-94511d
    - ## Leverage subagents for repetitive quality steps #leverage-subagents-for-repe-1ef775
      - Examples: `code-simplifier`, `verify-app` (end-to-end testing) #examples-code-simplifier-004ff6
      - Automate common post-implementation workflows for PRs #automate-common-post-implement-1ee7f7
    - ## Use inline bash in commands to pre-compute info & reduce model back-and-forth #use-inline-bash-in-commands-559589
      - Use `!` to run bash directly #use-to-run-bash-directly-a0a28a
      - ```shell #shell-echo-you-can-run-c-b69ad9
        ! echo "you can run command from Claude with !"
        ```
      - [Claude Docs - Bash Command Execution](https://code.claude.com/docs/en/slash-commands#bash-command-execution) #claude-docs-bash-command-ex-330725
    - ## Use hooks for polish & safety #use-hooks-for-polish-safe-5f5784
      - PostToolUse hook → auto-format code (handles the last 10%) #posttooluse-hook-auto-format-506683
      - Pre-allow safe commands via `/permissions` (in `.claude/settings.json`) → avoid constant prompts #pre-allow-safe-commands-via-04e9f1
      - [Claude Docs - Hooks](https://code.claude.com/docs/en/hooks) #claude-docs-hooks-https-e03025
    - ## Critical: Build strong verification feedback loops #critical-build-strong-veri-811181
      - Give Claude reliable ways to check its own work (unit tests, browser tests, Chrome extension, simulators…) #give-claude-reliable-ways-to-c-afc328
      - Verification → 2–3× better final quality #verification-2-3-better-fin-3bff27
      - For long tasks: background verification agent, Stop hook, or ralph-wiggum plugin #for-long-tasks-background-ver-08b487
  -  #node-bb73f1
  - ### Source #source-d7d903
    - https://code.claude.com/docs #https-code-claude-com-docs-8ffab0
  -  #node-51cdd6

---

- Slash Commands #slash-commands-f260f0
  - Single markdown files in `.claude/commands/`Reusable workflows triggered by `/command-name`Team-shared via gitFile structure #single-markdown-files-in-cla-c1fa7d
    - Location: `.claude/commands/my-command.md` #location-claude-commands-my-754c30
    - Frontmatter with metadata at top #frontmatter-with-metadata-at-t-8f456b
    - Body contains instructions for Claude #body-contains-instructions-for-61f4ff
    - Run with `/my-command` or `/my-command args` #run-with-my-command-or-my-038525
  - Frontmatter options #frontmatter-options-dff767
    - `description` - shown when typing `/` #description-shown-when-typ-2ef1e3
    - `argument-hint` - usage hint like `[patch|minor]` #argument-hint-usage-hint-l-bb6ee0
    - `allowed-tools` - pre-approve tools to skip prompts #allowed-tools-pre-approve-ab9b46
  - allowed-tools patterns #allowed-tools-patterns-dc4841
    - `Bash(gh:*)` - any gh command #bash-gh-any-gh-command-ae83f1
    - `Bash(pnpm:*)` - any pnpm command #bash-pnpm-any-pnpm-comm-2bc4fe
    - `Bash(git:*)` - any git command #bash-git-any-git-comman-fea80e
    - Can also set globally in `.claude/settings.json` #can-also-set-globally-in-cla-edf628
  - Common patterns #common-patterns-fbc9bc
    - Validation: run type-check, lint, tests #validation-run-type-check-li-6084ac
    - Git workflow: stage, commit, push, PR #git-workflow-stage-commit-p-f74f64
    - Build/deploy: run build, open preview URL #build-deploy-run-build-open-2efb7c
    - Use `$ARGUMENTS` for user input #use-arguments-for-user-inpu-17e8c8
  - Example: `bump-version.md` #example-bump-version-md-c50db4
    - `description: Trigger GitHub workflow` #description-trigger-github-w-417109
    - `argument-hint: [patch|minor|major]` #argument-hint-patch-minor-m-cb8336
    - `allowed-tools: Bash(gh:*)` #allowed-tools-bash-gh-1cdbfd
    - Body tells Claude to run `gh workflow run...` #body-tells-claude-to-run-gh-w-65e458

---

- Compounding Engineering by Dan Shipper #compounding-engineering-by-dan-be67a9
  - ## Core Philosophy #core-philosophy-3527be
    - Each feature should make subsequent features easier to build, not harder #each-feature-should-make-subse-142af7
      - Contrast to traditional engineering: each feature increases complexity and makes next features harder #contrast-to-traditional-engine-78ecf2
      - In compound engineering: complexity still grows, but so does the AI's knowledge of the codebase #in-compound-engineering-compl-c6a71a
    - Creates a learning loop where AI and humans both improve from each build cycle #creates-a-learning-loop-where-829bce
  - ## The Four-Step Loop #the-four-step-loop-90d8bd
    - Plan: Agents read issues, research approaches, and synthesize information into detailed implementation plans #plan-agents-read-issues-rese-13a9b3
    - Work: Agents write code and create tests according to those plans #work-agents-write-code-and-cr-635f19
    - Review: The engineer reviews the output itself and the lessons learned from the output #review-the-engineer-reviews-t-81a487
    - Compound: The engineer feeds the results back into the system to make the next loop better by helping the system learn from successes and failures #compound-the-engineer-feeds-t-caac48
  - ## Time Distribution #time-distribution-1556ae
    - Roughly 80% of compound engineering is in the Plan and Review phases #roughly-80-of-compound-engine-ded170
    - Only 20% is in the Work and Compound phases #only-20-is-in-the-work-and-co-4d98a0
    - This inverts traditional engineering where code-writing dominates #this-inverts-traditional-engin-22a42b
  - ## Knowledge Accumulation #knowledge-accumulation-8edec2
    - Every bug, failed test, or problem-solving insight gets documented and used by future agents #every-bug-failed-test-or-pro-d41274
    - Results are fed back into CLAUDE.md (or equivalent shared documentation) #results-are-fed-back-into-clau-b48104
    - The codebase becomes increasingly "self-teaching" #the-codebase-becomes-increasin-aca876
    - Team members contribute continuously—entire teams maintain shared prompt libraries #team-members-contribute-contin-dc68b3
  - ## Practical Benefits at Every #practical-benefits-at-every-ed7c86
    - Running 5+ software products, each primarily built and run by a single person #running-5-software-products-f52f15
    - Products are used by thousands of people daily for important work #products-are-used-by-thousands-304916
    - Two engineers can ship like a team of 15+ with proper compound engineering setup #two-engineers-can-ship-like-a-27d6fc
  - ## Key Implementation Principles #key-implementation-principl-7b2856
    - Build imperfect 60% solutions first, don't anticipate every edge case #build-imperfect-60-solutions-a70165
    - Use immediately on real work—don't wait until "ready" #use-immediately-on-real-work-d-861b7d
    - Feel the friction and note the gaps to signal what needs improvement #feel-the-friction-and-note-the-c58528
    - Small, targeted improvements compound over time (minutes per cycle, impact over months) #small-targeted-improvements-c-2736a6
    - Review architecture early when changes are cheap #review-architecture-early-when-e2c018
    - Improve iteratively and repeat—each cycle informs the next #improve-iteratively-and-repeat-eb2015
  - ## Why It Works with AI #why-it-works-with-ai-598306
    - Iteration cost approaches zero compared to human-written code #iteration-cost-approaches-zero-910e06
    - Improving a skill/command takes minutes, not hours #improving-a-skill-command-take-812f4b
    - The AI remembers the philosophy (via philosophy documents like `cover-design-philosophy.md`) #the-ai-remembers-the-philosoph-759f47
    - Learning compounds across sessions #learning-compounds-across-sess-148df7
    - Skills compose—they can call other skills, and improvements propagate #skills-compose-they-can-call-o-b0d6d9
    - Usage is the curriculum: each time you use a skill, the AI observes what works and what doesn't #usage-is-the-curriculum-each-1c7133
  - --- #node-b9d728
  - ### Source #source-5e6618
    - https://every.to/chain-of-thought/compound-engineering-how-every-codes-with-agents #https-every-to-chain-of-thou-a78c65
    - https://every.to/guides/agent-native #https-every-to-guides-agent-b9c0f7
  -  #node-6c37f8

---

- Claude 4.5 Model Comparison #claude-4-5-model-comparison-67e436
  - ## Claude 4.5 Model Comparison Table #claude-4-5-model-comparison-53ef29
  - ## When to Use Each Model #when-to-use-each-model-d825e8
    - Opus 4.5: Complex reasoning, heavy-duty agentic workflows, code migration/refactoring #opus-4-5-complex-reasoning-h-93af77
      - @bcherny: I use `Opus 4.5` with thinking for everything. It's the best coding model I've ever used, and even though it's bigger & slower than Sonnet, since you have to steer it less and it's better at tool use, it is almost always faster than using a smaller model in the end. #bcherny-i-use-opus-4-5-wit-5c44fe
    - Sonnet 4.5: Most development work, long-context tasks, orchestrating multi-agent workflows #sonnet-4-5-most-development-w-7e3f3a
    - Haiku 4.5: High-volume simple tasks, executor in multi-agent systems, real-time responsiveness needs #haiku-4-5-high-volume-simple-f03aaf
    - Multi-Model Strategy: Sonnet 4.5 orchestrates, Haiku 4.5 executes in parallel (cost-effective for scale) #multi-model-strategy-sonnet-4-7050d4
  - ## Claude 4.5 Model Comparison Table #claude-4-5-model-comparison-fa4050
    - | **Aspect** | **Claude Haiku 4.5** | **Claude Sonnet 4.5** | **Claude Opus 4.5** | #aspect-claude-haiku-3c3e94
      | --- | --- | --- | --- |
      | **Release Date** | October 15, 2025 | September 27, 2025 | November 24, 2025 |
      | **Primary Use Case** | Ultra-fast model optimized for real-time responsiveness and cost efficiency | Best balance of intelligence, speed, and cost for most use cases, with exceptional performance in coding and agentic tasks | Most intelligent model to date, sets a new standard across coding, agents, computer use, and enterprise workflows |
      | **Speed/Latency** | Fastest model, great for UI scaffolding, small prompts, and quick fixes | Exceptionally fast and responsive, engineered to provide answers with minimal delay, ideal for real-time chat | Comparatively slower with higher latency, labeled as "Moderate" speed |
      | **Context Window** | 200,000 token context window with up to 64,000 output tokens | 200,000 token context (1M available in beta) | 200,000 token context |
      | **Input Pricing** | $1 per million tokens | $3 per million tokens | $5 per million tokens |
      | **Output Pricing** | $5 per million tokens | $15 per million tokens | $25 per million tokens |
      | **Coding Performance** | 90% of Sonnet 4.5's performance on agentic coding evaluations | Frontier model and the best coding model in the world | 80.9% accuracy on SWE-bench Verified, outperforming Sonnet 4.5 (77.2%) |
      | **Extended Thinking** | First Haiku model to include extended thinking capability | Yes | Yes |
      | **Computer Use** | Included | Yes | Yes |
      | **Vision/Multimodal** | Text and image input | Text and image input | Text and image input |
      | **Safety Level** | ASL-2 (less restrictive) | ASL-3 (more restrictive) | ASL-3 (more restrictive) |
      | **Best For** | UI scaffolding and prototypes; executor in multi-agent workflows orchestrated by Sonnet | Long-context and multi-step engineering tasks, consistently maintaining accuracy across extended workflows | High-quality code and heavy-duty agentic workflows; code migration and code refactoring |
      | **Reasoning Capability** | Speed and scale focus, built to handle high-volume, real-time workloads | Depth and reasoning focus, tackling complex analytical and creative tasks | Holds a slight edge in reasoning depth and consistency on the most challenging problems |
      | **Context Awareness** | Supported | Introduced Sept 29, 2025 | Supported |
      | **Special Features** | — | Supports 1M token context window in beta | Memory Tool (beta) for storing/retrieving information beyond context window |
  -  #node-6627ff

---

- Claude Code Web #claude-code-web-b2bb26
  - https://code.claude.com/docs/en/claude-code-on-the-web #https-code-claude-com-docs-e-d76a66
  - ## Overview & Key Features #overview-key-features-51a417
    - **Status**: Currently in research preview #status-currently-in-resea-514b9d
    - **What it does**: Developers kick off Claude Code from the Claude app for answering questions about code architecture, bug fixes and routine tasks, parallel work, repositories not on your local machine, and backend changes #what-it-does-developers-k-20d9a5
    - **Multi-platform**: Available on the Claude iOS app for kicking off tasks on the go and monitoring work in progress #multi-platform-available-2e22e7
    - **Session mobility**: Send tasks from your terminal to run on the web with the & prefix, or teleport web sessions back to your terminal to continue locally #session-mobility-send-tas-9d88e2
  - ## Use Cases #use-cases-71b894
    - Answering questions about code architecture and features #answering-questions-about-code-d174fa
    - Bug fixes and routine, well-defined tasks #bug-fixes-and-routine-well-de-8f242c
    - Parallel work: tackle multiple bugs at the same time #parallel-work-tackle-multiple-9c68ef
    - Working on repositories not locally checked out #working-on-repositories-not-lo-bb5d2d
    - Backend changes (write tests → write code to pass tests) #backend-changes-write-tests-705837
    - Asynchronous tasks that don't need frequent steering #asynchronous-tasks-that-don-t-d24ba6
  - ## Accessibility #accessibility-ee7dcc
    - **Pro users**: Available #pro-users-available-69fbeb
    - **Max users**: Available #max-users-available-2f0998
    - **Team premium seat users**: Available #team-premium-seat-users-a-c8b8f8
    - **Enterprise premium seat users**: Available #enterprise-premium-seat-user-5c179c
    - **Access point**: Visit claude.ai/code #access-point-visit-claude-d2a582
  - ## Getting Started #getting-started-85bc69
    - Visit [claude.ai/code](https://claude.ai/code) #visit-claude-ai-code-https-be78f4
    - Connect your GitHub account #connect-your-github-account-b241fb
    - Install the Claude GitHub app in your repositories #install-the-claude-github-app-ce6d2c
    - Select your default environment #select-your-default-environmen-f7c63a
    - Submit your coding task #submit-your-coding-task-140d92
    - Review changes and create a pull request in GitHub #review-changes-and-create-a-pu-9a57a0
  - ## How It Works (Execution Flow) #how-it-works-execution-flo-5ef79a
    - **Repository cloning**: Cloned to Anthropic-managed virtual machine #repository-cloning-cloned-471b5e
    - **Environment setup**: Secure cloud environment prepared #environment-setup-secure-5321ef
    - **Network configuration**: Internet access configured based on settings #network-configuration-int-61842d
    - **Task execution**: Analyze code, make changes, run tests, check work #task-execution-analyze-co-2dce2f
    - **Completion**: Notified when finished #completion-notified-when-6e140d
    - **Results**: Changes pushed to branch, ready for PR #results-changes-pushed-to-aaaa51
  - ## Moving Tasks Between Web & Terminal [>>](2-i-also-run-5-10-claudes-on-7728ed) #moving-tasks-between-web-bb03ec
    - **Teleport one-way**: Can pull web sessions to terminal, not the reverse #teleport-one-way-can-pull-30cf78
    - ### From terminal to web #from-terminal-to-web-9e134c
      - Start message with `&` to send to web: #start-message-with-to-send-4ec8ad
        - `& Fix the authentication bug in src/auth/login.ts` #fix-the-authentication-bug-3e52c4
      - Or from command line: `claude --remote "Fix the authentication bug in src/auth/login.ts"` #or-from-command-line-claude-2fa9f9
      - Runs in cloud while you continue locally #runs-in-cloud-while-you-contin-006d1c
      - Use `/tasks` to check progress #use-tasks-to-check-progress-fb39d3
      - **Plan-then-execute pattern**: Plan locally with `claude --permission-mode plan`, then send to web with `&` #plan-then-execute-pattern-c1fd0e
      - **Parallel tasks**: Each `&` creates its own independent web session #parallel-tasks-each-c-a887f7
    - ### From web to terminal #from-web-to-terminal-ba56c3
      - **Using `/teleport` or `/tp`**: Interactive picker of web sessions #using-teleport-or-tp-ee28ad
      - **Using `--teleport`**: From command line with `claude --teleport` or `claude --teleport <session-id>` #using-teleport-from-c-1b50be
      - **From `/tasks` command**: Press `t` to teleport into a session #from-tasks-command-pre-13f285
      - **From web interface**: Click "Open in CLI" to copy command #from-web-interface-click-152c6c
    - ### Teleport requirements #teleport-requirements-c6bba1
      - Clean git state (no uncommitted changes) #clean-git-state-no-uncommitte-6c00fa
      - Correct repository (not a fork) #correct-repository-not-a-fork-5c06f9
      - Branch available (pushed to remote) #branch-available-pushed-to-re-bb37c5
      - Same account authentication #same-account-authentication-58046e
  - ## Environment Configuration #environment-configuration-3eed40
    - **Step 1 — Preparation**: Clone repo + run SessionStart hooks #step-1-preparation-clon-35d675
    - **Step 2 — Network**: Configure internet access (limited by default, customizable) #step-2-network-configur-64dc38
    - **Step 3 — Execution**: Claude runs code, tests, verifies work #step-3-execution-claude-df4a94
    - **Step 4 — Results**: Branch pushed to remote, ready for PR #step-4-results-branch-p-dfc3b4
    - **Environment selection**: Click environment to open selector, add new or update existing #environment-selection-cli-08cf08
    - **Terminal default**: Use `/remote-env` to choose default for `&` and `--remote` #terminal-default-use-re-4a4243
    - ### Environment variables #environment-variables-c3a831
      - Set as key-value pairs in `.env` format #set-as-key-value-pairs-in-en-295f1f
      - Example: `API_KEY=your_api_key` or `DEBUG=true` #example-api-key-your-api-key-a4e0f7
  -  #node-14fc1b

---

- CLAUDE.md #claude-md-8fe1dc
  - CLAUDE.md is a memory file that stores project-specific instructions, coding standards, and preferences that Claude Code reads automatically at startup. #claude-md-is-a-memory-file-tha-149caf
  - ## How it works: #how-it-works-effd33
  - Automatic loading: Claude Code recursively reads CLAUDE.md files from your current directory up to the root, loading them into context when launched.   #automatic-loading-claude-code-ef8e76
    Hierarchical structure: Files higher in the hierarchy take precedence:
    - Enterprise policy (`/Library/Application Support/ClaudeCode/CLAUDE.md` on macOS) #enterprise-policy-library-a-71bb17
    - Project memory (`./CLAUDE.md` or `./.claude/CLAUDE.md`) #project-memory-claude-md-7c5d6e
    - User memory (`~/.claude/CLAUDE.md`) #user-memory-claude-claude-a7a517
  - Imports: `CLAUDE.md` files can import other files using @path/to/file syntax, with recursive imports up to 5 levels deep. #imports-claude-md-files-can-646617
  - Subtree discovery: CLAUDE.md files nested in subdirectories are included when Claude reads files in those subtrees. #subtree-discovery-claude-md-f-f916ef
  - Use /init to bootstrap a CLAUDE.md, or /memory to edit memory files directly. #use-init-to-bootstrap-a-claud-fccf56
  - Want to know more about memory management? These pages may help: #want-to-know-more-about-memory-7bc8f5
  - [Memory management ](https://code.claude.com/docs/en/memory) #memory-management-https-c-0ee6d9
  - [GitHub Actions integration](https://code.claude.com/docs/en/github-actions) #github-actions-integration-h-9a7a4c
  -  #node-394bf3

---

- How to Use Claude Code Like Boris #how-to-use-claude-code-like-bo-675840
  - ## Core Workflow #core-workflow-b9ece1
    - Run Multiple Parallel Clauses #run-multiple-parallel-clauses-0b0ed2
      - Use 5 numbered tabs (1-5) in your terminal for local sessions #use-5-numbered-tabs-1-5-in-y-821220
      - Run 5-10 additional Claudes on claude.ai/code (web) in parallel #run-5-10-additional-claudes-on-e06330
      - Start sessions from your phone (iOS app) throughout the day and check in later #start-sessions-from-your-phone-64802d
      - Enable system notifications so you know when input is needed #enable-system-notifications-so-2c99f4
    - Model Choice #model-choice-4d2ba8
      - Always use Opus 4.5 with thinking mode for everything #always-use-opus-4-5-with-think-e24e12
      - Boris: "It's the best coding model I've ever used, and even though it's bigger & slower than Sonnet, since you have to steer it less and it's better at tool use, it is almost always faster than using a smaller model in the end." #boris-it-s-the-best-coding-m-b4ca78
  - ## Build Shared Team Memory #build-shared-team-memory-34013a
    - Create a CLAUDE.md File #create-a-claude-md-file-9d2588
      - Your team shares a single `CLAUDE.md` checked into git #your-team-shares-a-single-cla-42c748
      - Every time Claude does something incorrectly, add it to the file so Claude learns not to repeat it #every-time-claude-does-somethi-c99b51
      - During code review, tag @.claude on PRs to suggest updates to the `CLAUDE.md` #during-code-review-tag-clau-1e7d59
      - Use the Claude Code GitHub action to keep the file up to date #use-the-claude-code-github-act-af9d24
      - Other teams can maintain their own `CLAUDE.md` files #other-teams-can-maintain-their-f096a1
    - Create Reusable Slash Commands #create-reusable-slash-commands-a1c520
      - Store in `.claude/commands/` as markdown files #store-in-claude-commands-a-c94d47
      - Turn frequent workflows into commands (e.g., `/commit-push-pr`) #turn-frequent-workflows-into-c-c06820
      - Team-shares via git #team-shares-via-git-9736fc
  - ## Optimization Techniques #optimization-techniques-6dbd6e
    - Hand off between local ↔ web using `&` prefix or `--teleport` #hand-off-between-local-web-u-4cfa26
    - Enable Plan mode (shift+tab ×2) for goals before execution #enable-plan-mode-shift-tab-2-d8c0e7
    - Pre-allow safe commands via `/permissions` in `.claude/settings.json` to avoid constant prompts #pre-allow-safe-commands-via-1d8930
    - Use PostToolUse hooks for auto-formatting code (handles the last 10%) #use-posttooluse-hooks-for-auto-82982c
    - Verify aggressively with unit tests, browser tests, or other verification methods — this makes final quality 2–3× better #verify-aggressively-with-unit-0bbc7a
    - Iterate your plan with Claude until solid, then switch to auto-accept edits (often one-shots after that) #iterate-your-plan-with-claude-bf93bb
  - ## Why It Works: Compounding Engineering #why-it-works-compounding-e-26f010
    - This approach is Boris's implementation of Compounding Engineering — where each feature and lesson learned makes the next feature easier to build, not harder #this-approach-is-boris-s-imple-72a6ed
    - Every bug, failed test, or problem-solving insight gets documented and fed back into the system #every-bug-failed-test-or-pro-e8cb94
    - The codebase becomes increasingly "self-teaching" as you compound improvements #the-codebase-becomes-increasin-aaf003
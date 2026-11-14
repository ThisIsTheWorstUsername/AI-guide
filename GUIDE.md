# Becoming a Power User of Agentic AI and Claude Code

A step-by-step curriculum. Work through it alone, or hand [`COACH.md`](COACH.md) to an AI model and let it guide you level by level.

Each level has a **goal**, the **ideas** you need, **exercises** to do on a real project, and a **checkpoint** that tells you when to move on. Do the exercises; reading alone will not make you better.

> Claude Code changes quickly. Commands and shortcuts named here were correct at the time of writing; if one does not work, run `/help` or check the docs at <https://code.claude.com/docs>.

---

## Part 1 — What agentic AI is, and what to use it for

### The one-paragraph mental model

A chatbot answers a message. An **agent** is a model running in a loop with tools: it reads files, runs commands, searches, edits, checks the result, and repeats until the job is done. You stop being the person who types every step and become the person who **sets the goal, supplies context, and judges the result**. Your skill as a user is mostly the skill of a good manager: clear briefs, good checks, and knowing what to delegate.

### What it is good for

| Use | Examples |
|---|---|
| **Understanding** | "How does login work in this codebase?", "Why is this test flaky?", onboarding to an unfamiliar project |
| **Building** | Features, scripts, prototypes, small tools, whole apps when you supply the direction |
| **Fixing** | Reproduce a bug, find the cause, fix it, prove the fix with a test |
| **Chores** | Refactors, renames, dependency upgrades, migrations, writing tests, updating docs |
| **Review** | Reviewing a diff for bugs, security problems, or needless complexity |
| **Research and analysis** | Reading many sources or files and producing a summary, comparison, or report |
| **Data work** | Cleaning spreadsheets, querying databases, making charts |
| **Automation** | Recurring jobs: triage issues, summarize a week of commits, check a deploy, draft release notes |
| **Non-code computer work** | Organizing files, batch-converting documents, drafting from your notes |

### What makes a task a good fit

A task suits an agent when:

1. **You can say what "done" looks like.** A passing test, a matching screenshot, a file that exists.
2. **The agent can check its own work.** Something it can run or look at.
3. **Mistakes are cheap to undo.** The work is in git, or in a sandbox, or is a draft.

A task is a poor fit, or needs you close by, when:

- The result cannot be verified by anyone but you (taste, strategy, a decision that is yours to own).
- An error is irreversible or outward-facing: deleting data, sending email, deploying to production, spending money.
- You do not understand the area well enough to notice a confident wrong answer.

You can still use an agent for those. You just stay in the loop and approve each step.

---

## Part 2 — The levels

### Level 0 — Setup and first contact

**Goal:** Claude Code is running on a project you care about and you have delegated one real thing.

**Ideas**
- Claude Code runs in a terminal, desktop app, web app (claude.ai/code), and IDE extensions. Pick one; the skills transfer.
- It works inside a project folder and can read, edit, and run things there.
- It asks permission before risky actions. Read the prompts at first; they teach you what it is doing.

**Exercises**
1. Install Claude Code and open it in a project (any project; a small personal one is ideal).
2. Ask three questions about the project without asking for any changes. For example: "Give me a tour of this codebase", "Where is X handled?", "What would break if I removed Y?"
3. Ask for one small change. Read the diff before accepting it.

**Checkpoint:** You have accepted one change that you read and understood.

---

### Level 1 — Briefing well

**Goal:** Your first prompt gets you a correct result most of the time.

**Ideas**
- A good brief has four parts: **goal** (what and why), **context** (files, examples, constraints), **done criteria** (how either of you will know it worked), and **boundaries** (what not to touch).
- Point at things. Mention files with `@path/to/file`, paste the error text, attach the screenshot.
- Say the "why". "Make the list faster" is weak. "The list takes 4 seconds with 10,000 rows; users scroll it constantly; get it under 200 ms" is strong.
- Vague prompts are fine for exploring. Precise prompts are for building.

**Exercises**
1. Take a vague request you might have typed ("fix the search") and rewrite it with all four parts. Run both versions in separate sessions and compare.
2. Give a task with an example to imitate: "Add an endpoint like the one in `@users.py`, but for orders."
3. Ask the agent to interview you: "Before you start, ask me whatever you need to know." Notice which questions you should have answered up front.

**Checkpoint:** You can explain why a prompt failed in terms of what was missing from the brief.

---

### Level 2 — Explore, plan, then build

**Goal:** You stop the agent from solving the wrong problem.

**Ideas**
- For anything bigger than a few lines, split the work: **explore → plan → implement → verify**.
- **Plan mode** makes Claude research and propose a plan without changing anything. Cycle permission modes with `Shift+Tab`.
- Review the plan the way you would review a colleague's design. Fixing a plan costs seconds; fixing a wrong implementation costs an hour.
- Interrupt early. Press `Esc` the moment you see it going the wrong way, then redirect.
- Skip the plan for small, obvious changes. Planning a one-line fix is waste.

**Exercises**
1. Pick a medium feature. Use plan mode. Push back on at least one thing in the plan before approving.
2. Deliberately interrupt a running task with `Esc` and change its direction.
3. Ask for two alternative approaches with trade-offs, and choose one yourself.

**Checkpoint:** You have caught a wrong approach at the plan stage, before any code was written.

---

### Level 3 — Give the agent a way to check itself

**Goal:** The agent verifies its work, so you review finished results instead of debugging drafts.

This is the single highest-value habit in the whole guide.

**Ideas**
- An agent with a feedback loop iterates until it is right. An agent without one stops when the output *looks* right.
- Feedback loops: tests, type checker, linter, build, running the app, a screenshot compared against a mock-up, a script whose output can be checked.
- Test-first works very well: have it write a failing test, confirm it fails, then implement until it passes.
- For bugs: "reproduce it first, then fix it, then show the reproduction now passes."
- Git is your safety net. Commit before a big task so anything can be undone. Claude Code also keeps checkpoints you can roll back to with `/rewind` (or `Esc` twice).
- You still review. Read the diff. Ask "what did you not test?" A second, fresh session reviewing the first one's work catches a lot (`/code-review`).

**Exercises**
1. Fix a bug using reproduce → fix → prove.
2. Build a small feature test-first.
3. For something visual, give it a screenshot or mock-up and have it compare its own result.
4. Make a mess on purpose and recover it with `/rewind` or git.

**Checkpoint:** Every task you give includes a way to verify it, and you have recovered from a bad change without panic.

---

### Level 4 — Manage context

**Goal:** Long sessions stay sharp, and Claude knows your project without being told every time.

**Ideas**
- The **context window** is the agent's working memory: your messages, every file it read, every command output. As it fills, quality drops and early instructions fade.
- **One task per session.** Use `/clear` when you switch to something unrelated.
- If you have corrected the same mistake twice, the context is polluted. Clear it and write a better first prompt using what you learned.
- `/compact` summarizes a long session so you can continue. `/context` shows what is using the space.
- **`CLAUDE.md`** is a file at the project root that is loaded into every session. Put in it the things Claude cannot guess: build and test commands, conventions, gotchas, things never to do. `/init` generates a first draft.
- Keep `CLAUDE.md` short. For each line ask: "would Claude make a mistake without this?" If not, delete it. A bloated file gets ignored.
- When Claude gets something wrong that it will get wrong again, add a line to `CLAUDE.md`. That is how the project gets easier over time.
- Resume earlier work with `claude --continue` or `/resume`.

**Exercises**
1. Run `/init`, then cut the result down to what truly matters.
2. Over a week, each time you correct Claude, decide whether the correction belongs in `CLAUDE.md`.
3. Run `/context` in a long session and see what is consuming it.
4. Practise the discipline: finish a task, `/clear`, start the next.

**Checkpoint:** You have a `CLAUDE.md` under about 100 lines that you have edited at least three times from real experience.

---

### Level 5 — Customize your setup

**Goal:** Things you do repeatedly take one command, and Claude can reach the systems you work with.

**Ideas**
- **Permissions:** allow safe, frequent commands (tests, lint, git status) so you are not clicking "yes" all day, and keep prompts for risky ones. Manage them with `/permissions`.
- **Skills** (custom slash commands): a saved set of instructions for a repeated workflow, such as "fix a GitHub issue", "write release notes", "review this PR our way". They live in `.claude/skills/` and can be shared with your team through git.
- **Hooks:** scripts the harness runs automatically at fixed moments, for example formatting after every edit or blocking edits to a protected folder. Use a hook when something must happen *every time*; instructions in `CLAUDE.md` are advice, hooks are guarantees.
- **MCP servers / connectors:** plug Claude into outside systems such as GitHub, a database, your issue tracker, a browser, or your docs. See `/mcp`.
- **Subagents:** helpers with their own separate context. Use them for heavy searching or reading so the mess stays out of your main session. See `/agents`.

**Exercises**
1. Allowlist three safe commands you approve constantly.
2. Turn a prompt you have typed more than three times into a skill.
3. Add one hook (auto-format after edits is a good first one).
4. Connect one external tool through MCP and use it in a real task.
5. Ask Claude to "use a subagent to investigate X" and notice how little of your context it uses.

**Checkpoint:** You have one skill, one hook, and one connector that you actually use.

---

### Level 6 — Scale and automate

**Goal:** Work happens in parallel and without you watching.

**Ideas**
- **Parallel sessions:** run several Claudes on separate tasks at once. Use **git worktrees** so each has its own copy of the code and they do not collide.
- **Writer and reviewer:** one session writes, a fresh one reviews. Fresh context finds what the author missed.
- **Headless mode:** `claude -p "prompt"` runs without a chat interface, so you can use it in scripts, pipelines, and CI.
- **Fan-out:** for a large migration, have Claude list the files, then loop over them with one headless run each.
- **Cloud and background sessions:** hand off a task, close your laptop, and come back to a branch or pull request.
- **Scheduled tasks:** recurring jobs such as a morning summary of new issues or a nightly dependency check.
- **Auto modes and sandboxes:** less interruption, in exchange for trusting your guardrails. Only loosen permissions where a mistake is contained.
- The limit at this level is your attention for reviewing, so invest in automatic checks (Level 3) before scaling up.

**Exercises**
1. Run two sessions at once on unrelated tasks in separate worktrees.
2. Write a one-line script using `claude -p`, for example to explain the last commit or to lint a commit message.
3. Hand a task to a cloud or background session and review the result as a pull request.
4. Set up one scheduled or CI-triggered job that saves you real time each week.

**Checkpoint:** Something useful now happens every week without you typing a prompt.

---

### Level 7 — Judgment

**Goal:** You know when to trust, when to check, and when to do it yourself.

**Ideas**
- **You own the result.** "The AI wrote it" is not an excuse. Do not ship what you cannot explain.
- **Match trust to risk.** A throwaway prototype needs a glance. A payment change needs a careful read and tests.
- **Security:** never paste secrets into prompts; keep credentials out of the repo; be wary of content the agent reads from the web or from strangers, because it can contain instructions meant to hijack it (prompt injection); confirm before anything destructive or public.
- **Watch for common failures:** confident wrong answers, tests weakened to pass, more code than was needed, work reported as done that was not verified. Ask "show me the output" when in doubt.
- **Cost and speed:** large contexts and many parallel agents cost more. Use the strongest model for hard thinking and smaller ones for routine work (`/model`).
- **Keep learning.** Ask Claude to explain what it did and why. Using an agent should make you understand more of your system, not less.

**Exercises**
1. After a large task, ask: "What are you least sure about in this change?" Check that part yourself.
2. Write down your personal rules: what you always review by hand, and what you never delegate.
3. Once a month, redo something you automated to confirm you still understand it.

**Checkpoint:** You can state your own delegation rules and have a recent example of catching a problem the agent missed.

---

## Part 3 — Habits of power users

1. Give every task a way to be verified.
2. Explore and plan before building anything non-trivial.
3. One task per session; clear between tasks.
4. Correct early. After two failed corrections, restart with a better prompt.
5. Feed mistakes back into `CLAUDE.md`, skills, and hooks so they do not recur.
6. Commit often; treat git as the undo button.
7. Read the diff.
8. Automate what you repeat three times.
9. Scale only after your checks are solid.
10. Stay the owner of the work.

## Quick reference

| What | How |
|---|---|
| Interrupt | `Esc` |
| Roll back | `/rewind` or `Esc` twice |
| Switch permission / plan mode | `Shift+Tab` |
| Reference a file | `@path` |
| Run a shell command yourself | start the line with `!` |
| Fresh context | `/clear` |
| Summarize a long session | `/compact` |
| See context use | `/context` |
| Create project memory | `/init`, then edit `CLAUDE.md` |
| Resume earlier work | `claude --continue`, `/resume` |
| Permissions | `/permissions` |
| Connectors | `/mcp` |
| Subagents | `/agents` |
| Model | `/model` |
| Non-interactive run | `claude -p "prompt"` |
| Everything else | `/help` |

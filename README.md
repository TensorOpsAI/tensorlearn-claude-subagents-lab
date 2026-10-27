# Give subagents the right context

A notification feature spans a service and a portal. Provider constraints, UI rules and old advice live in separate documents. Broad prompts repeatedly lose a requirement between research and implementation.

In this repository the notification service is `src/notification.mjs`; the portal is where patients read their results and is not included. The documents are in `fixtures/research/`: the provider constraints in `provider.md`, the rules for what a message may say in `privacy.md`, and the old advice in `archived-integration.md`. `buildNotification` receives a result such as `{reviewed, released, testName, value}` (the shape used in `fixtures/verification/planted-diff.patch`).

**What you'll practise:** delegating to Claude Code subagents with deliberately limited context. Your main Claude Code session is the coordinator. Each role runs as a separate subagent that sees only its own instructions and the files its `requestedContext` names, and the only thing passed from research to implementation is `handoff.json`.

**In the course slides:** Chapter 1, Lesson 6 (Parallel sessions and subagents). In the slides a subagent is a Markdown file in `.claude/agents/` (name, description, tools) committed to the repo; here the graded roles are JSON files in `agents/`, and the optional `/agents` step lets you try the real thing without committing it. Running a plan in parallel worktrees is not part of this lab.

## Task

1. Read the task and role contracts in `contracts.md`.
2. Write prompts for researcher, implementer and verifier in `agents/`: replace the `TODO` in each `instructions` and keep `purpose` and `requestedContext` as they are. Model and tool choices are not part of these files.
3. Run research and produce `handoff.json` with source citations, decisions and unresolved items.
4. Implement `src/notification.mjs` from the handoff using the restricted implementation role.
5. Run a blind verifier with the approved requirements and diff (`fixtures/verification/`), resolve its findings and submit.

**Running a role in Claude Code.** Ask your main session to launch a subagent for the role, for example: *Launch a subagent with the instructions in `agents/researcher.json`. Give it only `fixtures/research/`. Have it return the content for `handoff.json`.* The coordinator delegates; it does not write `src/` itself. Each `requestedContext` name stands for these files:

| Name | Files |
|---|---|
| `research` | `fixtures/research/` |
| `implementation` | `fixtures/implementation/README.md` and `src/` |
| `handoff` | `handoff.json` |
| `verification` | `fixtures/verification/requirements.md` |
| `diff` | `fixtures/verification/planted-diff.patch`, plus your own change (`git diff main -- src/`) |
| `narrative` | `fixtures/author-narrative.txt` (no role should receive it) |

Optional: create real subagents with `/agents` and paste in each role's instructions. They are saved under `.claude/agents/`; do not commit that folder (any file outside the allowed paths makes the submission invalid). In your PR description, note which roles actually ran as subagents and paste the verifier's findings.

This lab often takes longer than the listed time the first time through.

## Working rules

The coordinator cannot implement. Each role receives only its assigned fixture pack. Researcher and verifier cannot write source. Role files supply prompts and context requests only: adding fields such as `model` or `tools` breaks the role schema.

Edit only `agents/**`, `handoff.json`, `src/**` and `tests/student/**`. Use Node 22; there is no `package.json`, so run code with `node` directly.

## Acceptance targets

- Role instructions separate research, implementation and blind verification; rehearse all three locally and report which actually ran.
- The handoff preserves current provider and privacy constraints with citations.
- The implementation uses the handoff and passes independent tests.
- The verifier identifies the planted defect without the author narrative.

## Done when

- All three files in `agents/` have written `instructions` (no `TODO`) and still have exactly `purpose`, `instructions` and `requestedContext`, with the starter `requestedContext` values.
- `handoff.json` cites `provider.md` and `privacy.md`, and leaves the channel decision unresolved with its owners.
- `node -e "import('./src/notification.mjs').then(m=>console.log(m.buildNotification({reviewed:true,released:true,testName:'glucose',value:'7.2'}),m.buildNotification({reviewed:true,released:false}),m.scheduleBatches(Array.from({length:45},(_,i)=>i)).map(b=>b.length)))"` prints an eligible neutral message with no test name or value, `{ eligible: false, body: '' }`, and `[ 20, 20, 5 ]`.
- If you added tests in `tests/student/` (files ending in `.test.mjs`), `node --test tests/student/*.test.mjs` passes.
- The verifier found the planted defect in `planted-diff.patch` without seeing `author-narrative.txt`, and your PR description lists its findings and which roles ran.
- `git status` shows changes only in `agents/`, `handoff.json`, `src/` and `tests/student/`.

## Graded format

- Role files: exactly `purpose`, `instructions` (at least 80 characters, no `TODO`) and `requestedContext`. Keep the starter `requestedContext` lists: the implementer must not request `research`, `verification` or `narrative`, and the verifier must not request `narrative`. The researcher's instructions should say to cite sources and to prefer current documents over archived or superseded ones.
- `handoff.json`: exactly `requirements`, `unresolved` and `sources`. Each requirement has a unique `id`, a `statement` and a `source` naming the fixture file it comes from (for example `fixtures/research/provider.md`). Include at least three requirements, covering the provider's batch size and rate limit (with their numbers) and the privacy rules (review and release; no clinical content).
- `unresolved` items have a `question` and an `owner`; `sources` items have a `path` and a `section`.
- Do not carry the archived note's advice into the requirements or cite `archived-integration.md` as a requirement's source.
- `src/notification.mjs`: both functions are synchronous. `buildNotification` returns exactly `{eligible: false, body: ''}` unless both `reviewed` and `released` are `true`. `scheduleBatches` fills each batch to the maximum before starting the next (45 items become 20, 20, 5), keeps the order and does not change its input.

## How to submit

1. Fork this repository to your personal GitHub account. Use fictional information
   only, keep the fork public, and keep `main` as its default branch. In your fork's
   **Actions** tab, click **I understand my workflows, go ahead and enable them** if shown.
2. On the lab page, use **Connect GitHub for scoring** to install the read-only grading
   App on your fork, then **Recheck connection**.
3. Download the TensorLearn connector from the lab page and extract it outside your fork.
   From your fork folder, run the page's `--code` connection command and paste your token
   only into the hidden terminal prompt. This opens a Claude Code session connected to the lab.
4. In that session, run `/load`. Run `/load` again after your instructor launches the round.
5. Create a branch such as `subagents/first-draft` and complete the task above. Change only the
   files the task allows: any other change, including `CLAUDE.md`, `.claude/` settings or
   agents, or notes files, makes the submission invalid.
6. Commit and push your branch. Open a PR **inside your own fork**, targeting its `main`
   branch (GitHub may suggest the upstream repository: select your fork instead). Check
   that **Check subagents submission** starts in the PR's checks. Keep the PR open; no merge
   is required.
7. Run **`/submit`** in the connected Claude Code session, then `/check` (or refresh the
   lab page) to read your saved score. The Actions run is useful feedback, but it does not
   submit your work.

A change to your answer needs a new commit, push and `/submit`. Keep `main` unchanged:
the grader compares your PR with the released starter.

## What is graded

The pinned grader in `.github/workflows/grade.yml` reads your files as data and runs your
code only in an isolated, offline container. Every check is a fixed rule; no AI model
judges your work. Your score is out of 100. Your facilitator separately reviews
clarity and reasoning.

- The three role files keep their fields, written instructions and separate contexts.
- handoff.json follows the schema, cites the current sources, keeps the provider limits, review-and-release and privacy rules, and leaves the channel open.
- Your notification code runs against fixed cases: it requires review and release, sends a neutral message and batches at most 20 in order.
- Using the archived note, breaking the gate, leaking clinical detail or exceeding 20 per batch caps the score at 25/100.

# Give subagents the right context

A notification feature spans a service and a portal. Provider constraints, UI rules and old advice live in separate documents. Broad prompts repeatedly lose a requirement between research and implementation.

## Task

1. Read the task and role contracts.
2. Write prompts for researcher, implementer and verifier in `agents/`. The instructor selects models and tools.
3. Run research and produce `handoff.json` with source citations, decisions and unresolved items.
4. Implement from the handoff using the restricted implementation role.
5. Run a blind verifier with the approved requirements and diff, resolve its findings and submit.

## Working rules

The coordinator cannot implement. Each role receives only its assigned fixture pack and approved tools. Researcher and verifier cannot write source. Candidate role files supply prompts and context requests, not authority to select arbitrary models or tools. Model overrides are rejected by the instructor runtime.

## Acceptance targets

- Role instructions separate research, implementation and blind verification; rehearse all three locally and report which actually ran.
- The handoff preserves current provider and privacy constraints with citations.
- The implementation uses the handoff and passes independent tests.
- The verifier identifies the planted defect without the author narrative.

## How to submit

1. Fork this repository to your personal GitHub account. Use fictional information
   only, keep the fork public, and keep `main` as its default branch.
2. Clone your fork and enable GitHub Actions in its **Actions** tab. Use **Connect
   GitHub for scoring** on the lab page to install the read-only grading App on your
   fork. Return to the lab and use **Recheck connection** if needed.
3. Download the TensorLearn client from the lab page and extract it outside your fork.
   Follow the page's connection command from your fork folder. Paste your learner
   token only into the hidden terminal prompt. Run `/load` from your fork folder to
   connect the repository. Run `/load` again after the facilitator launches the round.
4. Create a branch such as `subagents/first-draft` and complete the task above. Change
   only the files the task allows; any other change makes the submission invalid.
5. Commit and push your branch. Open a PR **inside your own fork**, targeting its
   `main` branch. GitHub may suggest the upstream repository: select your fork instead.
   Opening or updating this PR starts the checks automatically.
6. Keep the PR open. When ready, run **`/submit`** in the connected Claude Code session
   to submit that exact pushed revision for the current round. **No merge is required.**
7. Use `/check` or refresh the lab page to read the saved result. The Actions run and
   `lab-score/score.json` are useful feedback, but an Actions check alone does not
   submit your work or prove that the server saved a score.

A change to your answer requires a new commit, push, and explicit `/submit`. Keep
`main` unchanged: the grader compares your PR with the released starter.

## What is graded

The pinned grader in `.github/workflows/grade.yml` reads your files as data and runs
your code only in an isolated, offline container. Every check is a fixed rule; no AI
model judges your work. Your facilitator separately reviews clarity and reasoning.

- The three role files keep their fields, written instructions and separate contexts.
- handoff.json follows the schema, cites the current sources, keeps the provider limits, review-and-release and privacy rules, and leaves the channel open.
- Your notification code runs against fixed cases: it requires review and release, sends a neutral message and batches at most 20 in order.
- Using the archived note, breaking the gate, leaking clinical detail or exceeding 20 per batch caps the score at 25/100.

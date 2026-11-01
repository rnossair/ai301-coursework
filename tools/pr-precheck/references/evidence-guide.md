# Evidence guide: where evidence lives in a PR package

## Plan fidelity (harness category: silent-drift)

**Where it lives.**
- *Eval bundle:* the plan-context block holds the plan's scope pair (what is in and out of scope), its intended fix, and its deviation notes. The candidate PR's diff shows what actually changed. The description is in the candidate PR section.
- *Live:* `plan.md` (scope, fix, deviation notes), `git diff main...HEAD` for the change, and the draft description.

**What good looks like.** Every changed file and hunk falls inside the plan's stated boundary or a deviation note, and every plan item has a hunk or a disclosed omission. An honest deviation re-ties a mismatch: a note or description line that names what differs and why. Silent drift runs both ways: the diff does more than the plan (extra files or behavior, no note), or less (a plan item missing, no note). A description that claims more or less than the diff delivers is also drift, not a comms issue.

## Test evidence (harness category: not-tested)

**Where it lives.**
- *Eval bundle:* the candidate PR's test-evidence section, read against the plan-context block's test plan and the issue's reproduction steps; the repo-facts block for the repo's own checks.
- *Live:* the captured test output or before/after notes you provide, the plan's test plan, the issue's repro, and the repo's CONTRIBUTING or CI config for the stated checks.

**What good looks like.** Decisive evidence names an observable behavior, states the before result and expected-after result, and shows the after result for the same scenario as the repro or test plan. The outcome of the repo's checks is visible as a command and result. "Tests pass" or "works locally" with nothing observable is not evidence.

## Diff quality (harness category: unreviewable)

**Where it lives.** The unified diff and the commit list (eval: candidate PR section; live: `git diff main...HEAD`, `git log main..HEAD`).

**What good looks like.** The fix is visible at a glance and nothing unrelated rides along. Debris tells: debug prints or console logs, stray TODOs, dead or commented-out code, whitespace or formatting-only hunks on untouched lines, drive-by edits to unrelated files, unrelated generated or lock-file changes, and WIP, fixup, or merge commits that bury the change.

## Standards and comms (harness category: standards-wall)

**Where it lives.**
- *Eval bundle:* the repo-facts block (PR template sections, contributing instructions, stated policy including AI-use disclosure) and the issue thread for maintainer direction. The PR description is in the candidate PR section.
- *Live:* the repo's PR template (`.github/PULL_REQUEST_TEMPLATE*`), CONTRIBUTING.md, the issue thread, and the draft description. `scope.md` lists the house rules.

**What good looks like.** Every template section is present with real, specific content, and any asked-for disclosure is present and truthful. Explicit maintainer direction in the thread is followed or openly addressed. Compliant is specific; boilerplate, empty sections, or a visibly ignored ask is not. (Whether the description's claims match the diff is plan fidelity, above.)

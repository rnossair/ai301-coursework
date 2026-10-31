# Procedure: how this skill grades a plan package

Modes: **eval mode** grades a bundle and uses ONLY the bundle text (fetch nothing). **Live mode** gathers issue-side evidence per `references/evidence-guide.md`. Where a step differs by mode, it says so.

## Read order

Read in this order, noting the listed facts as you go. The repro is read before the plan so the plan's diagnosis is judged against the evidence, not against the plan's own framing.

1. **Live mode only:** read `scope.md` first, then `voice-guide.md`. In eval mode, skip both.
2. **Issue information** (eval: the issue-context block). Note the reported symptom or error, the affected versions, and any maintainer requests or conventions in the thread.
3. **Repro evidence** (eval: the repro-evidence block; live: the student's posted repro comment, or the house repro pack as quoted in the drafts). Note the exact input or command, the observed output or error, and the environment and version. If none is present, record that absence.
4. **Files named in the issue or repro.** Live mode: read only what the drafts quote or name. Eval mode: do not fetch; use only what the bundle quotes.
5. **The plan.** Note the stated cause, the files it will change, what it says it won't touch, the approach and order of work, the tests, and any stated risks or unknowns.
6. **The plan comment.** Note what it claims, which issue specifics it names, and the next step it commits to.

## Evidence gathering

For each family, pull the fact from this location and record it before grading. Eval mode uses the bundle block named; live mode uses the location in the evidence guide.

- **Diagnosis and grounding:** the plan's stated cause, plus the symptom, input, and version from the repro evidence.
- **Scope:** the plan's scope statement: files or areas named, and the not-in-scope line.
- **Executability:** the plan's approach and order of work, read against the files it names.
- **Test plan:** the plan's tests, plus the repro steps and artifact they should reproduce.
- **Honesty:** the plan's stated risks and unknowns, and any certainty or timeline claims in the plan or comment.
- **Repo policy:** the repo-facts block (contribution policy, AI policy, templates). Record whether it requires AI-use disclosure.
- **Comms:** the plan comment, plus the maintainer signals in the issue thread.

If a family's evidence is absent from the package, record "absent" and handle it per Check execution below. Do not go looking outside the package to fill it.

## Check execution

1. Run every check in the rubric, top to bottom. A fail does not stop the run, so the summary is complete.
2. A check may be graded from its own evidence without re-reading the whole package when it is self-contained:
   - `scope-is-bounded`: the plan only.
   - `respects-repo-policy`: the repo-facts block, the plan, and the plan comment.
   - `unknowns-are-honest`: the plan and the plan comment.
3. These checks must cross-reference, so re-read the repro evidence: `diagnosis-matches-repro`, `test-is-automated-and-observable`, and `executable-by-stranger` (the plan against the files it names). `comment-is-thread-aware` reads the plan comment against the thread.
4. If evidence for a check is absent:
   - Repro evidence missing and the plan quotes none: `diagnosis-matches-repro` and `test-is-automated-and-observable` are `fail`.
   - Repro evidence missing but the plan quotes some: grade against what is quoted.
   - Repo states no AI or contribution rule: `respects-repo-policy` passes.
   - Any other absence: `unclear`.
5. Every grade carries one line naming the fact or quote that decided it. "Looks fine" is not evidence.
6. Apply the rubric's pass condition as written. If it passes by the rubric but feels wrong, it passes; note the tension in the summary.

## Verdict assembly

1. Apply the rubric's verdict rule: `accept` only if every `required` check passes; any required `fail` is `reject`.
2. `unclear` on a required check counts as `fail`. `preferred` checks never change the verdict.
3. In the output, quote the evidence line for each failing required check (the deciding checks). On `accept`, quote the evidence for each required pass.
4. List preferred-check failures as suggestions in the summary.
5. Live mode only: report any voice-guide rule the plan comment breaks, quoting the rule. This does not change the verdict.
6. Report any place where this procedure was silent, as a gap.
7. Emit the short readable summary, then the fenced JSON block last, in the format SKILL.md defines.

## After a deviation (live mode)

If the build deviated from the posted plan and `plan.md` was updated to record what changed and why, re-run this procedure on the updated package before any follow-up comment is posted. A deviation recorded in the plan is graded like the rest of it.

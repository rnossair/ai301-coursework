# Evidence guide: where evidence lives in a plan package

In eval mode, the only evidence is the bundle's blocks: issue context, repo-facts, repro-evidence, candidate plan, candidate plan comment. Nothing is fetched. In live mode, the drafts are the student's `plan.md` and draft plan comment, and issue-side evidence comes from the issue URL (via `gh`, the GitHub API, or the web).

## Diagnosis and grounding

**Where it lives:**
- The plan's stated cause: the plan's cause or diagnosis statement.
- The behavior it must explain: the repro evidence's output excerpt or artifact, its exact input or command, and its environment and version record. Eval: the repro-evidence block. Live: the student's posted repro comment on the issue; for the house issue, the house repro pack as quoted in the drafts.
- The reported symptom: the issue body (eval: issue-context block).

**What good looks like:** The stated cause names a specific mechanism (a function, condition, or data path) and explains the exact error or symptom the repro artifact shows, on the same input and a version the issue covers. It fails when the cause only restates the symptom, explains a different symptom, or contradicts the artifact.

## Scope

**Where it lives:** The plan's in-scope statement (the files or areas it will change) and its not-in-scope line.

**What good looks like:** Every file or area to be changed is named, a not-in-scope line says what stays untouched, and everything in the plan serves the one fix. Drive-by refactors, unrelated cleanups, and extra features are scope creep.

## Executability

**Where it lives:** The plan's approach and order of work, read against the files it names.

**What good looks like:** A stranger could start from the plan without asking the author anything. It says where the change goes, what the change is, and what order to do it in. Steps like "investigate" or "fix the bug" with no direction do not count.

## Test plan

**Where it lives:** The plan's test plan, read against the repro evidence's input, steps, and artifact.

**What good looks like:** It names an automated test that exercises the same input and behavior the repro shows. The test fails before the fix and passes after. Manual-only checks, or tests that would pass without the fix, prove nothing observable.

## Honesty

**Where it lives:** The plan's stated risks and unknowns, plus any claims of certainty, timelines, or guaranteed fixes in the plan or the plan comment. Live mode: also the deviation note recorded in `plan.md` after a build.

**What good looks like:** Unknowns are stated as unknowns, and claims match what the repro evidence backs up. False confidence means certainty the evidence doesn't support, a guaranteed timeline or fix, or open questions left out. A mid-build deviation is honest only if it is recorded in the plan with what changed and why.

## Comms

**Where it lives:**
- The plan comment.
- The issue's maintainer signals: eval, the issue-context block (thread highlights). Live, the issue thread.
- The repo-facts block's stated templates, contribution asks, and contribution policy, including any AI-use disclosure requirement. Live: the repo's CONTRIBUTING file and docs.

**What good looks like:**
- The comment names this issue's specifics and states the next step.
- It reflects what maintainers asked for in the thread, and it follows the repo's templates and rules.
- If the repo requires AI-use disclosure, the plan and comment disclose it, naming the tool and extent. Course packages count as AI-assisted work.
- If the repo states no such rule, this passes.
- Boilerplate that would fit any issue, or a comment that ignores a maintainer request, does not pass.

# Rubric: is this plan ready to post and build from?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here (via your procedure.md). It ships empty on
purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the plan's scope statement, the test plan read against
     the repro evidence's steps, the plan comment read against the
     thread highlights, the repo-facts block) or a location from your
     references/evidence-guide.md. "The plan" is not a source; "the
     plan's stated cause read against what the repro evidence shows"
     is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (is
     this one bounded change? could a stranger start executing it?),
     never the write-up's shape (how many sections it has, how long it
     is, whether it uses headings). Structure-shaped checks are what
     make graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad plans posted. The lecture named the
failure families: the diagnosis ignores or contradicts the reproduced
evidence, the change is unbounded (scope creep), the plan targets the
symptom while the evidence points at the cause, a stranger could not
start executing it, the test plan proves nothing observable, the
unknowns are dressed up as certainty, and the comment ignores what the
thread or the repo's stated conventions ask. A rubric that ignores a
family will fail eval packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diagnosis-matches-repro | The plan's stated cause, read against the repro evidence (output excerpt, steps, environment record) and the issue's reported symptom | The plan names a specific cause of the bug, and that cause explains the behavior the repro evidence actually shows (same error, same symptom, same input and version). Fails if the plan gives no cause, only restates the symptom, targets the symptom while the evidence points at a different cause, or contradicts or ignores the repro evidence. An honestly stated unknown with a concrete way to find out passes only if the plan's first step is that investigation. | required |
| scope-is-bounded | The plan's scope statement (files or areas it will change, and what it will not touch) | Names each file or area it will change and says what it won't touch, so the change is one bounded fix. Fails if files are left unnamed, the not-in-scope line is missing, or the plan includes drive-by refactors, unrelated cleanups, or feature additions beyond the issue. | required |
| executable-by-stranger | The plan's approach and order of work, read against the files it names | A stranger could start executing from the plan without asking the author anything: the approach, the location of the change, and the order of work are concrete. Fails if key steps are left as "investigate" or "fix the bug" with no direction. | required |
| test-is-automated-and-observable | The plan's test plan, read against the repro evidence's steps and artifact | The plan adds an automated test that fails before the fix and passes after, and that exercises the same input and behavior the repro evidence shows. Fails if the tests are manual only, absent, or would pass without the fix (prove nothing observable). | required |
| unknowns-are-honest | The plan's stated risks and unknowns, read against the repro evidence | Uncertainty is stated as uncertainty, and only modest claims are made. Fails if the plan asserts certainty the repro evidence does not back up, guarantees a timeline or fix, or hides open questions. | required |
| respects-repo-policy | The repo-facts block (contribution policy, AI policy, templates), read against the plan and the plan comment | The plan and comment follow the repo's stated rules. If the repo requires disclosure of AI-assisted work, they disclose it (tool and extent); treat course packages as AI-assisted work. If the repo states no such rule, this passes. | required |
| comment-is-thread-aware | The plan comment, read against the issue thread (maintainer signals, thread highlights) | The comment names this issue's specifics, reflects what maintainers asked for in the thread, and states the next step. Fails if it is boilerplate that would fit any issue, or ignores a maintainer request or convention in the thread. | preferred |

## Verdict rule

Accept (ready to post and build from) if every `required` check passes. Reject (hold) if any `required` check fails. `preferred` checks never change the verdict, but note failures as suggestions. `unclear` on a required check counts as fail.

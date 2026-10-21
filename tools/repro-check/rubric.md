# Rubric: is this reproduction package ready to post?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here. It ships empty on purpose: the judgment is your
work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the claim comment, the repro report's environment
     record, the artifacts read against the issue's description, the
     repo-facts block) or a location from your
     references/evidence-guide.md. "The report" is not a source; "the
     output excerpt read against the error the issue describes" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (does
     the artifact show the issue's behavior?), never the write-up's
     shape (how many steps it has, how long it is, whether it uses a
     template's headings). Structure-shaped checks are what make
     graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad packages posted. The lecture named the
proof families: the environment is recorded, the steps are complete
and followable, the behavior shown matches the issue (not an adjacent
one), the outcome is stated honestly (an evidenced cannot-reproduce is
a pass, a confident wrong-target is not), and the words respect the
repo's conventions. A rubric that ignores a family will fail eval
packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
includes-version | Environment or System section, or similar in body | includes release version being tested | required
includes-environment | Environment or System section, or similar in body | includes testing environment | preferred
problem-detail | Body, expected behavior and actual behavior section or similar | includes problem and expected behavior | required
Includes-steps | “Steps” or “How to reproduce” section, or similar | a stranger could reproduce the steps without guessing | required
artifact-matches-issue | The output excerpt or artifact in the repro report, read against the error or behavior the issue describes, plus the environment record read against the issue's affected versions | the artifact shows the same behavior the issue reports (same error, same symptom), on the same input and a version the issue covers or the report says why it differs. Fails if the reporter changed the input or command so it no longer matches the issue, shows an adjacent symptom (a different error, a graceful failure instead of a crash), or deviates in version or environment without acknowledging it. An honest, evidenced cannot-reproduce passes. | required
claim-is-specific-and-honest | The claim comment, read against the issue | names this issue's specifics and a concrete next step, and makes only modest promises. Fails if it is interchangeable boilerplate that would fit any issue, guarantees a timeline or fix, asks to reserve or be assigned the issue without a plan, or asserts certainty the repro report does not back up. | required
respects-repo-policy | The repo-facts block (contribution policy, AI policy) read against the claim comment and repro report | the comments follow the repo's stated rules. If the repo requires disclosure of AI-assisted work, the comments disclose it (tool and extent). Treat course packages as AI-assisted work. If the repo states no such rule, this passes. | required


## Verdict rule

All required checks need to pass for a repro to be approved. Preferred checks are only to rank approved issues.

Grade a check `unclear` only when the package truly lacks the information to decide it. An `unclear` on a required check is treated as a fail, so the verdict is reject.

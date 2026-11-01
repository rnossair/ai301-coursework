# Procedure: how this tool grades a PR package

Execute the stages in order. Do not skip a stage, and do not grade any check before Read order and Evidence gathering are complete.

## Read order

1. **Live mode only:** read `scope.md` first and apply the scope seam from SKILL.md (stop on an unfilled `Repo:` placeholder or an out-of-scope repo). Skip this in eval mode.
2. Read `rubric.md`, then `references/evidence-guide.md`. If the rubric has no checks or verdict rule, or this procedure were empty, refuse per SKILL.md.
3. Read the **issue** (live: thread, template, policy; eval: the issue and repo-facts blocks). Note: the reported problem, the reproduction steps, any maintainer instruction, the PR template's section list, and the stated policy including AI-use disclosure and the repo's own checks.
4. Read the **plan** (live: `plan.md`; eval: the plan-context block) before the diff. Note: the stated scope/boundary as a list of files or areas, the intended fix, the acceptance goal, the test plan, and every deviation note. The plan goes before the diff and description because every later check measures them against it; reading the diff first would let it define what "in scope" means.
5. Read the **diff** and commit list (live: `git diff main...HEAD` and `git log main..HEAD`; eval: the candidate PR's diff and commits). Note: each changed file, what each hunk does, and any debris.
6. Read the **description and title**, then the **test evidence**. Note: each concrete claim the description makes, each template section's content, any disclosure, and what the evidence shows.

## Evidence gathering

Gather per family, using the locations in `references/evidence-guide.md`. Record each fact as you gather it so each check has a quote ready.

- **Diff vs plan scope:** list the diff's changed files. List the plan's scope files/areas and deviation notes. Pair them: every changed file maps to the scope or a note, or it is an extra. Record extras and record plan items with no matching change.
- **Plan delivered:** take the plan's intended fix and acceptance goal. Locate the hunk(s) that implement each. Record any plan item with no hunk and whether a deviation note or description disclosure covers it.
- **Description vs diff:** extract every concrete claim from the description (what changed, what it fixes, tests added, "no other changes", fidelity to the plan). For each, find the supporting hunk or note its absence. Also list material diff changes the description never mentions.
- **Test evidence vs test plan:** take the plan's test plan and the reproduction steps. Record the PR's before result, expected-after result, and shown after result, and whether they describe the same scenario as the plan's.
- **Repo checks:** record the repo's stated checks (test command, linter, CI) from the repo facts or CONTRIBUTING, then the command and result shown in the evidence or description.
- **Diff quality:** scan every hunk for debug leftovers, dead or commented-out code, whitespace/formatting-only hunks, files unrelated to the fix, and unrelated generated/lock files. Scan commits for WIP or fixup debris.
- **Standards:** compare the template's section list against the description section by section; mark each filled with specific content, empty, or boilerplate. Record the disclosure statement if one is asked for.
- **Maintainer direction:** list explicit instructions from maintainers in the thread, then find where the PR follows or addresses each.

## Check execution

- Run the checks in the rubric's table order.
- Grade each check from its recorded evidence against its pass condition, and write the one-line deciding fact or quote. You need not re-read the whole package for a check whose evidence is already recorded.
- If the evidence for a check is genuinely absent (the package has no test-evidence section, or the plan states no boundary), grade it per the rubric's pass condition. Where the condition does not cover absence, grade `unclear` and say what is missing in the evidence line. Never fill the gap with an assumption.
- Do not let one check's result move another's. A failure in one family does not make a neighboring check fail.
- Grade the artifact, not its polish. Do not raise or lower a grade for length, formatting, or confident tone.
- Live mode only, after all checks are graded: read `voice-guide.md`, hold the title and description against its rules, and list each broken rule with the offending phrase in the summary. Do not change any grade because of it.
- If a step you need is not covered here, list it as a procedure gap in the summary and continue without inventing the step.

## Verdict assembly

1. Collect the grade of every check in rubric order.
2. Apply the rubric's verdict rule: `accept` only if every `required` check is `pass`; `reject` if any required check is `fail` or `unclear`. Preferred checks never change the verdict.
3. If the verdict is `reject`, the **deciding check** is the first failing or unclear required check in rubric order. Quote its deciding fact in the readable summary. List the other failures after it, briefly.
4. If the verdict is `accept`, say so plainly and note any preferred-check misses and voice findings as non-blocking.
5. Emit the fenced JSON block last: `item` is the PR URL or bundle id, every check appears once in rubric order with its grade and one-line evidence, and `verdict` is the result of step 2. Nothing follows the block.

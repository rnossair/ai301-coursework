# Rubric: is this pull request ready to submit?

Four failure families drive the checks: silent drift, not tested, unreviewable, and standards wall. A package that fails none of them is a clear accept. A PR that honestly discloses a shortfall or deviation can be ready; "less than everything" alone is never a failure.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Diff within plan scope | The diff's changed files and hunks, read against the plan's stated scope/boundary and its deviation notes | Every changed file and hunk falls inside the plan's stated scope or is covered by a deviation note. Fail if the diff touches anything outside both. Unclear if the plan states no boundary to compare against. | required |
| Plan delivered or disclosed | The plan's stated fix and acceptance goal, read against what the diff actually changes, plus the deviation notes and the description's disclosures | The diff implements what the plan promised, OR every piece it omits or changes is named in a deviation note or in the description. Fail if part of the plan is silently missing, or the diff does something materially different without a note. | required |
| Description matches diff | The description's claims (what changed, what it fixes, "no other changes", fidelity to the plan), read against the diff contents | Every concrete claim in the description is true of the diff, and nothing material in the diff is unmentioned. Fail if the description claims more, less, or something else than the diff delivers (for example "adds tests" with no test files in the diff). | required |
| Observable test evidence | The PR's test-evidence section, read against the plan's test plan and the reproduction's own steps | The evidence names an observable behavior, states the before result and the expected-after result, and the after result is shown (output, log, or described observation) for the reproduction steps or the plan's test plan. Fail on bare claims such as "tests pass" or "tested locally" with no observable behavior, or evidence for a different scenario than the plan's. | required |
| Repo checks run | The test evidence and description, read against the repo's own checks (test suite, linter, CI as stated in the repo-facts/CONTRIBUTING) | The outcome of the repo's stated checks is visible (command and result), and failures are fixed or explicitly disclosed. Fail if the stated checks were never run or their failing result is hidden. If the repo states no checks, pass on that fact. | required |
| Reviewable diff | The unified diff and the commit list | The fix is visible and nothing unrelated rides along: no debug leftovers (prints, console logs, stray TODOs), no dead or commented-out code, no formatting churn on untouched lines, no drive-by edits, no generated or lock files unrelated to the fix, no merge or fixup debris that buries the change. Fail if any such debris appears; one stray hunk is enough. | required |
| Template and disclosure honored | The repo's PR template sections and stated policy (including AI-use disclosure), read against the PR description | Every template section is present with real, specific content (no empty sections, placeholders, or boilerplate left in), and any disclosure the repo asks for is present and truthful. Fail if a section is skipped or boilerplate, or a stated ask is visibly ignored. | required |
| Maintainer direction engaged | The issue/thread, read against the PR description and diff | Any explicit maintainer instruction in the thread (a requested approach, a constraint, a "do not") is followed or openly addressed. Pass if the thread contains no such direction. Fail if direction is ignored. | required |
| Title communicates the change | The PR title, read against the diff | A maintainer could tell from the title alone what the change is and it is accurate to the diff. Fail if it is vague ("fix bug", "updates") or misdescribes the diff. | preferred |
| Commits tell a coherent story | The commit list | Commit messages describe their changes and are not WIP, "fix fix", or unrelated to the diff. | preferred |

## Verdict rule

- **accept** if every `required` check passes.
- **reject** if any `required` check is `fail`.
- `unclear` counts as `fail`: an unverifiable claim is a failing one. A required check graded `unclear` therefore rejects.
- `preferred` checks are graded and reported but never change the verdict, whatever their grade.
- The deciding check is the first failing (or unclear) required check in table order.

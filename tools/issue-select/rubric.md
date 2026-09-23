# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Active Repository | last 6 default-branch commit dates | A commit has been made in the past 2 months | required |
| Active Issue | issue comments | A comment has been made in the last 6 months (if issue older than 6 months) | required |
| Issue Unclaimed | issue comments | This issue has not been claimed by someone else (that is not a student) | preferred |
| Issue Open | issue comments, pull requests | This issue has no current open linked PRs | required |
| Bounded | issue comments, issue description | This issue is limited to one or two tasks | required |
| Familiar Techstack | issue body, repo readme | This issue is related to the familiar techstack, or close enough to learn on the fly | preferred |

## Verdict rule

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict, they
rank accepted issues; unclear counts as fail." -->

accept if all "required" check passes, "preferred" checks don't change the verdict but are used for ranking accepted issues. If unclear on a "required" check, count as fail.

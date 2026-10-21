# Evidence guide: where proof lives in a reproduction package

<!--
THIS IS THE PART YOU WRITE (new this week: Unit 1 handed you this file
finished; the scaffolding fades). The skill uses this guide as its map:
for every kind of proof a rubric check names, this file says WHERE to
find it in a package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repo-facts block, the
  claim comment, the repro report and its parts). In live mode (where
  on GitHub or in the draft: the issue thread, the repo's docs, the
  student's draft comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the versions named match what the
  issue targets, or the difference is called out") over adjectives
  ("environment is thorough").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute; the rubric swap showed you what that feels
like. Write the map you wish your grader had.
-->

## Environment

<!-- Where the environment record lives, and what a sufficient one
looks like against the issue's stated target. -->
- **In eval bundle:** "repro_report_markdown" under "thread_highlights"
- **In live mode:** Under the issue comments (sometimes within a repro comment) or inside the issue body itself

Should name the specific version, build or commit tested, and should match what the issue targets (or reports the difference).

## Steps

<!-- Where the reproduction steps live, and what makes them followable
by a stranger, starting state to trigger. -->
- **In eval bundle:** "repro_report_markdown" under "thread_highlights" or restated in a claim comment
- **In live mode:** Either inside the issue body or in a repro comment somewhere below. A stranger with no other context could execute the steps in order

## Behavior shown

<!-- Where the artifacts live (output excerpts, logs, screenshots),
and what it means for an artifact to show the issue's behavior rather
than an adjacent one. -->
- **In eval bundle:** the candidate claim comment and repro report text, read against the repo-facts block (contribution policy, issue/comment template, AI-use disclosure requirement)
- **In live mode:** pasted output blocks, log excerpts, or linked screenshots in the repro comment, read against the issue body's description (including any stated expected-vs-actual)

## Honesty

<!-- Where claims and their backing meet: how to tell a report that
says exactly what happened (including an honest cannot-reproduce) from
one that claims more than its evidence shows. -->
- **Where it lives**
  - In eval bundle: the claim comment's stated outcome, read against the repro report's artifacts in the same bundle
  - In live mode: the draft claim comment's stated outcome, read against the draft repro report's artifacts
- **What good looks like:** the claimed outcome (reproduced / could not reproduce / reproduced something different) is exactly what the artifacts support — no stronger, no softer. An evidenced "could not reproduce on v2.3" is a pass. A confident "reproduced" claim resting on an artifact that actually shows an adjacent or unrelated failure is not, regardless of how detailed or well-written the surrounding report is.

## Comms

<!-- Where the words meet the repo: the claim comment against the
issue, the comments against the repo's stated templates and
contribution policy (including AI-use disclosure requirements), and
what specific-and-honest looks like next to boilerplate. -->
- **Where it lives**
  - In eval bundle: the candidate claim comment and repro report text, read against the repo-facts block (contribution policy, issue/comment template, AI-use disclosure requirement)
  - In live mode: the draft comment(s), read against the repo's CONTRIBUTING file, issue template, and any stated AI-disclosure policy
- **What good looks like:** the comment addresses this issue's specific behavior and evidence in its own words, content that couldn't be pasted unchanged onto a different issue.
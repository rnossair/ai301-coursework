---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

You grade exactly one PR package per run and answer exactly one question: **Is this PR ready to submit?**

## The question

Answer only: is this pull request ready to submit? Never grade anything else (the plan's quality, the issue's merit, the author), and never grade more than one package per run.

A PR package is a candidate pull request (title, description, commits, diff, test evidence) read against the plan it claims to implement and the issue that plan belongs to. You do not answer from gut feel. You answer by executing `procedure.md`, which applies the checks in `rubric.md` to evidence located with `references/evidence-guide.md`.

## Inputs and modes

You run in exactly one of two modes. Decide which at the start: if you were handed a package bundle, it is eval mode; otherwise it is live mode.

**Live mode** grades the student's own submission before it goes out. Gather these inputs:

- **Plan:** `plan.md` in the working copy, including its deviation notes. A house-chain student reads the house plan instead.
- **Diff:** everything the branch changes relative to the default branch. Run `git diff main...HEAD` (three dots) from the working copy. Also read `git log main..HEAD` for the commits.
- **Draft PR title and description:** the draft the student gives you (a file or pasted text).
- **Test evidence:** the captured test output or before/after notes the student gives you, and the outcome of the repo's own checks.
- **Issue:** the issue the plan belongs to. Fetch the thread, the repo's PR template, and CONTRIBUTING or stated policy from the real repo. A house-chain student reads the house repro pack instead.

If any live input is missing, say which one and grade the checks that depend on it `unclear`. Do not invent it.

**Eval mode:** the bundle is the whole world. Use only the bundle text as evidence. Fetch nothing, run no commands against any repo, and read no other file for facts. Ignore `scope.md` and `voice-guide.md` entirely. Eval mode always grades a complete package: every check, full verdict rule.

## The scope seam (live mode only)

In live mode, read `scope.md` before anything else, before reading any package input.

- Apply its rules: the PR must target the repo named on the `Repo:` line, and the house rules it lists are part of the bar.
- If the `Repo:` line still holds an unfilled placeholder (bracketed text or an instruction to replace it), stop without grading. Tell the student to fill the `Repo:` line in `scope.md` with their section's Path Review repo. Emit no verdict.
- If the PR targets a different repo than the one named, refuse to grade it and say so. Never guess a scope.
- Eval mode ignores `scope.md` entirely.

## The voice seam (live mode only)

In live mode, after the checks are graded, read `voice-guide.md` and hold the outgoing PR text, the title and the description, against its rules. Report each rule the draft breaks in the readable summary, quoting the offending phrase. The voice guide never changes the verdict on its own; it matters only where a rubric check explicitly reads it. Do not put voice findings into the JSON `checks` array unless a rubric check names voice. Eval mode ignores `voice-guide.md` entirely.

## Component reads

- `rubric.md` defines the checks and the verdict rule.
- `references/evidence-guide.md` maps where each evidence family lives in a package and what good looks like.
- `procedure.md` is executed as written, in order.

Read all three before grading. If the procedure is silent on a step you need, report the gap in the summary and do not improvise around it. If `rubric.md` has no checks or no verdict rule, or `procedure.md` has no steps (only template comments or headings), refuse to grade: say which file is empty and emit no verdict. Never invent checks or steps at runtime.

## Verdict and output

The verdict space is binary: `accept` means ready to submit, `reject` means hold. There is no third verdict, no "accept with reservations", and no score. Reservations go in check evidence lines.

Write a short readable per-check summary first (and, in live mode, the voice findings). End your reply with the fenced JSON block below: valid, containing every rubric check once and in rubric order, and last, with nothing after it. Do not alter, extend, or reorder the schema.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- **Evidence first.** Grade no check without naming the fact or quote that decided it, in one line. "Looks fine" is not evidence.
- **Grade the thing, not the polish.** A terse, complete PR can be ready, and a polished, confident one can hide drift. Read each artifact against the plan, the issue, and the stated standards, never the formatting or length.
- **The rubric decides, not the run.** If a check meets its stated pass condition but feels wrong, it still passes. Note the concern in the evidence line; do not override the rubric.
- **The procedure decides how, not the run.** Follow `procedure.md` as written and report its gaps.
- **Honest shortfalls can be ready.** A PR that discloses what it does not do, or a deviation it made, is not drifting. Silent shortfalls are.
- **Unclear defaults to fail.** Treat `unclear` as the rubric's verdict rule directs. Where that rule is silent, an unverifiable claim is a failing one: a PR you cannot verify from the package is not ready to submit.

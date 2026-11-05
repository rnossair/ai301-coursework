# Contribution: feat(tool-bridge): pass MCP tool annotations to Pi registerTool definition

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Rayane Nossair
**GitHub Username:** rnossair
**Issue:** https://github.com/irahardianto/pi-mcp-extension/issues/253
**Pull Request:** https://github.com/irahardianto/pi-mcp-extension/pull/265

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: issue-scout search queries, and a rubric fix for new maintainer-filed issues**

I wrote three search queries based on my fit profile: AI-engineering terms in Python and TypeScript, plus a React/Next/Express fallback. I ranked the results by dropping assigned, stale, and crowded issues, capping each repo at 2 slots, and putting AI-related titles first. Every query returned zero results on the first scan, because I had wrapped the keywords in double quotes ("llm OR agent OR rag") and gh searched for that exact phrase instead of separate keywords joined by OR. I removed the quotes so each term is a separate argument, and I logged a rubric gap the scan exposed: amd/gaia#4779 passed even though a merged PR had already done most of the work.

**Unit 6: voice guard hook, repo memory and repro-check on my claim and repro**

I installed a voice-guard hook that blocks `gh issue comment` until repro-check has passed the draft, and I wrote `CLAUDE.local.md` in my clone as repo memory (48 lines: the lint limits, the smoke-test file list, PR and CHANGELOG rules, the subsystem notes for #253, and the AI-policy note). It is excluded through `.git/info/exclude`, so it never reaches the PR. repro-check passed my claim and my repro, but the voice guide flagged "I expect to open a PR within a few days" as breaking my no-promised-timelines rule, and the rubric passed it because "expect" is hedged. I overrode the voice guide myself and posted the claim with that sentence, because I judged it a reasonable estimate for a small change and the claim also says I will release it if I cannot continue. The override was my call, and I would tighten the claim check to fail any stated timeframe so the rubric and my voice guide agree. One small leftover: the `@AGENTS.md` hint comment is still in `CLAUDE.local.md`.

**Unit 7: ship-gate and manifest, a coverage claim I had overstated, and a hook that never ran**

I installed the ship-gate hook and filled in `ship-manifest.md` in my clone: five must-pass checks (`npm ci`, lint, typecheck, tests with the 95/85/83 coverage floors, `npm pack --dry-run`), each sourced to CONTRIBUTING.md, AGENTS.md or `package.json`, and five dated exclusions. Four of the exclusions are tools I don't have installed locally (semgrep, gitleaks, qlty, actionlint), each noted with the CI job that covers it. The fifth replaces `npm run test:coverage` with the same command and a double-quoted glob, because on Windows the single-quoted glob selects no files and the floors pass on nothing. Filling in the manifest is what exposed that: my earlier PR draft said "coverage floors met" on an empty report, so I re-ran with the corrected glob and fixed the line.

The ship-gate itself never ran on my PR. Its hook in `~/.claude/settings.json` was installed as `python3 ".../check_gate.py"`, but on my Windows machine `python3` is only the Microsoft Store shortcut, which prints "Python was not found" and exits with code 49. Claude Code stops a command only when a hook exits with code 2, so the failed hook let the PR command through without any checks. I only noticed afterwards: PR #265 had already opened, and `~/.claude/ship-gate/gate-log.md`, which the gate writes on every run, did not exist. I changed the hook command to `python` (which resolves to my Python 3.14 install). I did not re-run the gate, because the PR was already submitted, so my gate evidence for this PR is the manifest and the repo checks I ran by hand, not a gate pass. The lesson is that a hook failing for a setup reason fails open, so after installing one I should test it once with a harmless command and look for its log.

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

I found this issue by running my issue-scout on all of GitHub. It uses three queries built from my fit profile: AI-engineering terms in Python and TypeScript, plus a React/Next/Express fallback. The scan returned 300 unique results. It then dropped assigned issues, stale ones and crowded threads, capped each repo at 2 slots, and graded the top 5. Those five were pi-mcp-extension#253, pi-mcp-extension#256, amd/gaia#4872, amd/gaia#4779 and ix-infrastructure/Peer2Peer#20. The scout accepted the first four and rejected Peer2Peer#20 on the Active Issue check. For #253 it passed all 8 checks and its verdict was accept. Its difficulty note said the scope is small, a single helper function and one new field, that it is a direct hit on "Agent tools, MCP", and that the repo's AGENTS.md is written for AI coding agents, so AI-assisted work is welcome.

I chose #253 over #256 because the work is smaller and the owner already wrote the full design in the issue. The plan is to add a pure `buildToolAnnotations()` helper in `src/tool-bridge.ts`. It copies only the four boolean hints (`readOnlyHint`, `destructiveHint`, `idempotentHint`, `openWorldHint`) and is passed to `registerTool`. The work also needs tests and `README.md` / `CHANGELOG.md` notes. It fits me because it is TypeScript and MCP tool plumbing, which is the direction I'm moving in as I go from MERN full-stack work toward AI engineering. I hope to learn how MCP tool metadata flows into an agent's permission logic, and what a repo with strict lint and complexity limits expects in a PR.

The scout held up, but it needed fixes during a real wild search. Every query returned zero results at first because I had quoted the keywords, and gh searched for the exact phrase. I removed the quotes, which gave 300 results, and logged that the file still has the old form. The scout first rejected #253 on Active Issue because the thread had zero comments, even though the owner had filed it minutes earlier. I changed the rubric so the check also passes when the issue was opened by an Owner, Member or Collaborator within the last 30 days. Re-grading flipped #253 to accept, so I did not choose against my own tool. Two things still bother me: every top candidate was updated "today", so the recency ranking mostly reflects churn, and the rubric has no check for an issue that a merged PR has already mostly fixed (amd/gaia#4779).

Claim comment: https://github.com/irahardianto/pi-mcp-extension/issues/253#issuecomment-6620434361 (posted 2026-11-04)

**Toolkit**

- Tool: issue-scout (my search strategy and rubric). I ran it live against all of GitHub and graded the top five candidates against the rubric.
- What it produced: A funnel from 300 results down to a graded top five. Four candidates passed and one was rejected. For #253 it produced an accept verdict with 8 passing checks.
- Difficulties: The quoted OR query returned zero results. The Active Issue check wrongly rejected a brand-new maintainer-filed issue. Recency ranking was noisy. The rubric missed partially fixed issues.
- What changed: I unquoted the search terms and added the maintainer-authored 30-day clause to Active Issue. I logged the ranking and "already mostly fixed" problems in scan-notes.md for later, and `search-strategy.md` still needs the query fix.

---

## Understanding the Issue

**Problem Description**

pi-mcp-extension bridges MCP servers' tools into the Pi coding agent. MCP tools can carry annotations, four boolean hints (`readOnlyHint`, `destructiveHint`, `idempotentHint`, `openWorldHint`) that say what a tool does. The bridge turned those hints into text in the tool's description but never gave Pi the structured data, so anything in Pi that decides whether to confirm a call (a permission extension, a `beforeToolCall` hook) could not tell a read-only tool from a destructive one.

**Expected Behavior**

Each bridged tool is registered with Pi's `registerTool` carrying an `annotations` object with the boolean hints the MCP server sent. The tool's label and description text stay as they are, and a tool with no usable hints registers exactly as before, with no `annotations` field.

**Current Behavior**

`ToolBridge._registerTool` registers every tool without an `annotations` field. In my repro a tool with `destructiveHint: true` produced the description `Delete a record [⚠️ destructive]` and `annotations: undefined`.

**Affected Components**

`src/tool-bridge.ts` (`_registerTool` and a new `buildToolAnnotations` helper), `tests/tool-bridge.test.ts`, `README.md`, `CHANGELOG.md`, and `SECURITY.md` (the supported-version line the repo asks for on a `feat`). Not touched: `src/index.ts`, whose resource tools already pass annotations, and `src/tool-discovery.ts`.

**Toolkit**

- Tool: Claude Code, used to read the issue, `src/tool-bridge.ts` and the Pi type definitions. No grading skill ran in this phase.
- What it produced: the plain-language problem statement above and the list of affected files, which I checked against the issue's own "files to touch" list.
- Difficulties: none met.
- What changed: nothing.

---

## Reproduction Process

**Environment Setup**

- OS: Windows 11 Pro 10.0.26200
- Node: v24.13.1
- Project: `irahardianto/pi-mcp-extension`, `main` at commit `f6e1d5f1a97ece89e25341a1088550e2d0bd6a8e` ("fix(schema-refs): percent-decode RFC 6901 §6 fragment segments in resolveRefTarget (#250)")
- Pi (`@earendil-works/pi-coding-agent`): 1.3.0, installed by `npm ci`
- Setup: check out that commit and run `npm ci`. I made no other changes to `src/`.

Challenges: none came up in setup. The only decision was how to observe the bug. I wrote a throwaway unit test against the bridge's output instead of using the RPC smoke test, because `tests/mock-server.ts` sets no annotations and RPC mode has no command that dumps a tool's `annotations`.

**Steps to Reproduce**

1. Run `git checkout f6e1d5f` (or current `main`), then `npm ci`.
2. Create a temporary, untracked file `tests/zz-repro-253.test.ts`. It builds a `ToolBridge` with a mock Pi (`registerTool` pushes into an array) and a mock MCP client whose `tools/list` returns one tool, `delete_record`, with `annotations: { title: "Delete", destructiveHint: true, readOnlyHint: false }`. It calls `bridge.refreshTools("myserver", mockClient)`, prints the registered tool's keys, `description` and `annotations`, then asserts `annotations` deep-equals `{ destructiveHint: true, readOnlyHint: false }`.
3. Run `node --import tsx/esm --test tests/zz-repro-253.test.ts`.
4. Observed: exit code 1. The registered tool has no `annotations` key, so the assertion fails with `undefined` where the hints were expected.

**Reproduction Evidence**

- **Repro comment posted upstream:** https://github.com/irahardianto/pi-mcp-extension/issues/253#issuecomment-6621235976 (2026-11-04), with the full test in a collapsed block
- **Commit showing reproduction:** none (the repro test was temporary and deleted; `git status` shows no repro files in the repo)
- **Screenshots/logs:**

```text
registered keys : name, label, description, promptSnippet, parameters, execute
description     : Delete a record [⚠️ destructive]
annotations     : undefined
▶ repro #253
  ✖ registerTool receives the structured annotations from tools/list (2.7507ms)
ℹ tests 1
ℹ pass 0
ℹ fail 1

  AssertionError [ERR_ASSERTION]: Expected values to be strictly deep-equal:
  + actual - expected

  + undefined
  - {
  -   destructiveHint: true,
  -   readOnlyHint: false
  - }
```

- **My findings:** Reproduced. The bridge turns the `destructiveHint` into text in `description` (`[⚠️ destructive]`) but never passes the structured `annotations` object to `pi.registerTool`, so a bridged tool looks like `annotations: undefined` to Pi. Pi 1.3.0 does support the field: `ToolAnnotations` is declared at L400 and `annotations?: ToolAnnotations` at L492 and L1580 of `node_modules/@earendil-works/pi-coding-agent/dist/core/extensions/types.d.ts`. Existing tests only check the description text ("includes annotation hints in tool description" in `tests/tool-bridge.test.ts`), which is why this went unnoticed. Limits: this is a unit-level repro against the bridge's output. I did not check how a real permission extension reacts, since none is installed here. The issue is labelled `enhancement`, so this shows a missing passthrough, not a crash.

**Toolkit**

- Tool: repro-check (my rubric, evidence guide and voice guide), run live against my claim draft and repro draft for issue #253. I also had it check the repo's CONTRIBUTING.md, AGENTS.md and PR template for an AI-disclosure rule.
- What it produced: an accept verdict with all 7 checks passing (version, environment, problem detail, steps, artifact matches issue, claim specific and honest, repo policy). The `artifact-matches-issue` pass rested on the output showing `annotations : undefined` for a tool with `destructiveHint: true`. It found no AI-disclosure rule in the repo, so the policy check passed.
- Difficulties: my claim was already posted when I ran it, so it graded the claim as it stands on the thread, not as a draft. The voice guide flagged "I expect to open a PR within a few days" as close to my no-promised-timelines rule. The rubric passed it because the phrase is hedged, so the rubric and my voice guide disagree on that line.
- What changed: nothing in the rubric yet. I would tighten the claim check to fail any stated timeframe, and in future I will run repro-check on the claim before posting it.

---

## Solution Approach

**Analysis**

The root cause is in `ToolBridge._registerTool` in `src/tool-bridge.ts`. It builds the `registerTool` object from `name`, `label`, `description`, `promptSnippet`, `parameters` and `execute` only. It reads `tool.annotations` for the label (the `title`) and for `buildToolDescription` (the text hints) but never copies the hint booleans into an `annotations` field. Pi 1.3.0 already declares `annotations?: ToolAnnotations` on its tool definition, so the field is accepted and simply never sent. Not verified: how a real permission extension reacts, since none is installed here.

**Proposed Solution**

Add a pure helper, `buildToolAnnotations`, that copies the four hints only when the value is strictly a boolean (so `false` is kept), ignores `title` and non-boolean values, and returns `undefined` when nothing applies. `_registerTool` spreads its result into `registerTool` only when defined, which satisfies `exactOptionalPropertyTypes` and omits the field instead of sending `{}`. Deliberately not changing: `buildToolDescription`, `label`, `promptSnippet`, `src/index.ts`, `src/tool-discovery.ts`, any refactor of `tool-bridge.ts`, or dependencies.

**Implementation Plan**

1. Write the tests first in `tests/tool-bridge.test.ts` and run the passthrough test against unchanged `src/` to see it fail.
2. Add `buildToolAnnotations` and the conditional spread in `src/tool-bridge.ts`, and update the file's header comment.
3. Update `README.md` (the features line and the spec table row) and add a `### Added` entry under `[Unreleased]` in `CHANGELOG.md`.
4. Update the `SECURITY.md` supported-version table, which `AGENTS.md` requires for a `feat`.
5. Run lint, typecheck, test coverage and `npm pack --dry-run`, then open the PR with `Closes #253`.

Deviations recorded in my plan: the new tests went into their own `describe("ToolBridge annotations passthrough")` block instead of next to the existing description-hint test, with no behavior difference; and `SECURITY.md` moved the supported line to `1.14.x` instead of `1.13.x`, because `1.13.0` shipped (#261) while I was working.

**Toolkit**

- Tool: plan-check (my rubric, procedure and voice guide), run live on `plan.md` and `plan-comment.md` for #253, using my posted repro comment as the evidence.
- What it produced: a reject on the first run, then an accept two minutes later. The first run failed `diagnosis-matches-repro` because `plan.md` stated a goal but no cause and cited my local `repro-draft.md` instead of the posted comment. It also found no `plan-comment.md` to grade, and it noted that the passthrough test is the only new test that fails before the fix. After I added a Cause section, linked the posted comment, reworded the test step and wrote the plan comment, all required checks passed.
- Difficulties: `procedure.md` is silent on a missing plan-comment draft in live mode, and on a plan that cites an unposted file. I graded both conservatively as `unclear` and `fail`. The voice guide again flagged the timeline in my posted claim.
- What changed: I rewrote the plan as above. I would add both missing cases to `procedure.md`.

---

## Testing Strategy

Before and after, on the same test. With the `...(toolAnnotations ? { annotations: toolAnnotations } : {})` line commented out, the new passthrough test fails with `undefined` where `{ destructiveHint: true, readOnlyHint: false }` was expected, the same failure as my repro. With the line restored it passes. Four of the seven new tests fail without the spread (passthrough, all four hints, title not copied, non-boolean values ignored while valid hints survive). The other three (key omitted with no annotations, key omitted with only a `title`, description hints unchanged) are regression guards and pass either way.

The seven new tests cover the repro input, all four hints including `false`, omission when a tool has no annotations or only a `title`, `title` not copied into `annotations`, non-boolean values ignored, and the description text unchanged. Repo checks on Windows 11 / Node v24.13.1 / Pi 1.3.0:

- `npm run lint`: exit 0
- `npm run typecheck`: exit 0
- tests with coverage floors (95/85/83): exit 0, 657 tests, 653 passed, 0 failed, 4 platform skips; `tool-bridge.ts` 98.27% lines / 94.59% branches / 95.65% functions
- `npm pack --dry-run`: exit 0, 35 files

On Windows npm's single-quoted `--test-coverage-include='src/**'` selects no files, so I ran the script's Node command with the glob double-quoted.

**Toolkit**

- Tool: the repo's own checks (`npm run lint`, `typecheck`, `test:coverage`, `npm pack --dry-run`), and pr-precheck, which re-ran the tests with the spread commented out.
- What it produced: the red-then-green evidence and the 4-of-7 count in the Testing Strategy above. The repo checks all exited 0.
- Difficulties: the npm coverage script selects no files on Windows because of its single-quoted glob.
- What changed: I ran the script's Node command directly with the glob double-quoted, and said so in the PR.

---

## Implementation Notes

**2026-11-03 to 2026-11-04**

Scouted the issue, posted my claim, then posted the repro comment. The repro was a throwaway unit test against the bridge's output, not the RPC smoke test, because `tests/mock-server.ts` sets no annotations.

**2026-11-04**

Wrote `plan.md`, with the test-first order of work. Created the branch `feat/253-pass-mcp-annotations`. Wrote the failing passthrough test first and kept its output. Added `buildToolAnnotations` using a `readonly` key list and a loop, to stay inside the repo's lint complexity limits, and the conditional spread in `_registerTool`.

While I was working, `main` moved to v1.13.0 (#261), so I rebased onto it and changed the `SECURITY.md` supported line to `1.14.x` instead of `1.13.x`. I also put the seven tests in their own `describe` block instead of next to the existing description-hint test. Both are recorded as deviations in my plan.

The coverage check failed to select any files on Windows because of the single-quoted glob in the npm script. I ran its Node command with the glob double-quoted. Committed as `6f12dbc` (`feat(tool-bridge): pass MCP tool annotations to Pi registerTool definition`) and opened PR #265 that evening.

**Toolkit**

- Tool: Claude Code, for the edits and test runs, with git on the `feat/253-pass-mcp-annotations` branch.
- What it produced: commit `6f12dbc` with the helper, seven tests and the doc changes.
- Difficulties: `main` moved to v1.13.0 while I worked, and the Windows coverage glob failed.
- What changed: I updated the `SECURITY.md` version to 1.14.x and recorded both in the plan's Deviations section.

---

## Pull Request

**PR Link:** https://github.com/irahardianto/pi-mcp-extension/pull/265

**PR Description:** Title `feat(tool-bridge): pass MCP tool annotations to Pi registerTool definition`, `Closes #253`, one commit (`6f12dbc`) on top of v1.13.0 from `rnossair:feat/253-pass-mcp-annotations`. It explains the gap and the helper, lists the scope (`src/tool-bridge.ts`, `tests/tool-bridge.test.ts`, `README.md`, `CHANGELOG.md`, `SECURITY.md`), and gives the release impact. The Verification section has the lint, typecheck, test and pack results and the red-then-green output. It says why the pre-merge smoke test was not run (`tool-bridge.ts` is not on the runtime-behavior list in `AGENTS.md`) and offers to run it. It states the limits, a self-review checklist, and an AI-use disclosure (Claude Code, Sonnet 5.5).

**Maintainer Feedback:**

- 2026-11-04: none yet; PR #265 has no comments or reviews.

**Status:** Awaiting review

**Toolkit**

- Tool: pr-precheck (my rubric and procedure), run live on `pr-draft.md` against `plan.md`, the diff and the repo's PR template.
- What it produced: a reject, then an accept on the corrected draft with all 10 checks passing. The first run failed "Description matches diff": my draft said only the first two new tests fail before the change, but with the spread disabled four of seven fail. It also failed "Template and disclosure honored" because I had left out the template's Self-review section.
- Difficulties: the wrong claim in the description was a mistake in my draft, caught only because the tool re-ran the tests. `procedure.md` says to diff against `main`, but my local `main` was stale, so the run used `origin/main`. The placeholder rule text also still mentions a "Path Review repo".
- What changed: I corrected the test count, added the Self-review section and updated `plan.md` for the two drifts. I would fix the `main` versus `origin/main` step and the placeholder text in `procedure.md`.

---

## Learnings and Reflections

**Technical Skills Gained**

I learned how a conditional spread keeps a TypeScript object valid under `exactOptionalPropertyTypes`, and how to write a helper inside a repo's lint complexity limits. I also learned how MCP tool annotations flow from `tools/list` into an agent's tool registry, and that the MCP SDK already validates them as booleans, so my non-boolean test is reachable only through a mock. On the process side I learned the repo's release rules: Conventional Commit titles, a `CHANGELOG.md` entry under `[Unreleased]`, and the `SECURITY.md` table for a `feat`.

**Challenges Overcome**

The scout's first scan returned zero results until I unquoted the OR query, and it first rejected #253 until I changed the Active Issue rule (see Why I Chose This Issue). While building, `main` moved to v1.13.0, which changed the `SECURITY.md` version I needed, and the npm coverage script selected no files on Windows because of a quoted glob. I recorded the first as a plan deviation and worked around the second by running the Node command directly.

**What I'd Do Differently Next Time**

I would run repro-check on the claim before posting it, so my voice guide and the rubric agree on timelines first. I would also push the repro test to a fork branch so "Commit showing reproduction" has a real link, and write down in the plan which files the repo's release rules touch (here `SECURITY.md`) before building, not after.

---

## Resources Used

- [CONTRIBUTING.md](https://github.com/irahardianto/pi-mcp-extension/blob/main/CONTRIBUTING.md): Conventional Commit PR titles, the `CHANGELOG.md` rule, and the smoke-test recipe
- [AGENTS.md](https://github.com/irahardianto/pi-mcp-extension/blob/main/AGENTS.md): lint complexity limits, the `SECURITY.md` rule for a `feat`, and the runtime-behavior list for the smoke test
- [Issue #253](https://github.com/irahardianto/pi-mcp-extension/issues/253): the owner's full design (helper, conditional spread, test cases)
- `types.d.ts` in `@earendil-works/pi-coding-agent` 1.3.0: shows that Pi declares `annotations?: ToolAnnotations`
- [PR #261](https://github.com/irahardianto/pi-mcp-extension/pull/261): showed the Windows coverage-glob workaround and the 1.13.0 release that moved my `SECURITY.md` version

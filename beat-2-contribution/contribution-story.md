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
**Pull Request:** [GitHub PR link, once opened]

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

**Unit 5: [one line on the tool work]**

I wrote three search queries based on my fit profile: AI-engineering terms in Python and TypeScript, plus a React/Next/Express fallback. I ranked the results by dropping assigned, stale, and crowded issues, capping each repo at 2 slots, and putting AI-related titles first. Every query returned zero results on the first scan, because I had wrapped the keywords in double quotes ("llm OR agent OR rag") and gh searched for that exact phrase instead of separate keywords joined by OR. I removed the quotes so each term is a separate argument, and I logged a rubric gap the scan exposed: amd/gaia#4779 passed even though a merged PR had already done most of the work.

**Unit 6: repro-check and my voice guide, run on my claim and repro drafts**

I ran repro-check first in claim-only mode on my claim comment, then on the full claim-plus-repro package, and held both drafts against my voice guide. Both came back accept, and I confirmed from the repo's CONTRIBUTING.md and AGENTS.md that there is no AI-disclosure rule. The voice guide caught something the rubric did not: "I expect to open a PR within a few days" breaks my no-promised-timelines rule, but the rubric passed it because "expect" is hedged, so the two disagree on that line. I overrode the voice guide myself and posted the claim with that sentence in it, because I judged the hedged "expect ... within a few days" to be a reasonable estimate for a small change and the claim also says I will release it if I cannot continue. The override was my call, not the tool's, and I would tighten the claim check to fail any stated timeframe so the rubric and my voice guide agree next time. plan-check also refused to grade, correctly, because I had no `plan.md` for #253 and no posted repro comment yet, which showed me the order of work: post the repro first, then write the plan. I have since posted the repro comment on #253.

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

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

[In your own words, what is broken or missing?]

**Expected Behavior**

[What should happen?]

**Current Behavior**

[What actually happens?]

**Affected Components**

[Which parts of the codebase are involved?]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

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

[Your analysis of the root cause: what is causing the issue, traced
to the files and functions involved.]

**Proposed Solution**

[High-level description of your fix approach, and what you are
deliberately NOT changing.]

**Implementation Plan**

1. [Modify file X to do Y]
2. [Add or change Z]
3. [Update tests]

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**[Date]**

[Entry]

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]

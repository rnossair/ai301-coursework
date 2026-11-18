# Passing MCP safety hints to Pi: my first merged open-source PR, and the toolkit that got me there

## Overview

pi-mcp-extension bridges MCP servers' tools into Pi, a coding-agent harness. MCP tools can carry safety hints such as "destructive" or "read-only", and the extension wrote them into the tool's description text but never passed them to Pi as structured data, so Pi permission extensions saw `annotations: undefined`. I found the issue with a scout I built and tuned (my own search strategy and rubric), claimed it, reproduced it with a failing test, planned and built the fix with plan-check, pr-precheck, a voice guide and a ship manifest that I also wrote, and opened [PR #265](https://github.com/irahardianto/pi-mcp-extension/pull/265). The maintainer merged it on 2026-11-06, about a day after I opened it, and [issue #253](https://github.com/irahardianto/pi-mcp-extension/issues/253) is now closed.

## What happened

**Finding the issue.** My issue-scout ran three queries from my fit profile (AI-engineering terms in Python and TypeScript, plus a web-stack fallback). The first scan returned zero results for every query, because I had wrapped the keywords in double quotes and `gh` searched for the exact phrase `"llm OR agent OR rag"`. I removed the quotes and got 300 results. The scout then rejected #253, the issue I ended up fixing, on its Active Issue check: the thread had zero comments, even though the repo owner had filed the issue minutes earlier. The rule was wrong, not the issue, so I changed my rubric. Before, the check passed only if an Owner, Member or Collaborator had commented on the issue or on another recent one. After, it also passes when one of them opened the issue within 30 days:

```
Active Issue | ... | someone with an Owner/Member/Collaborator badge commented on this issue,
or replied to another recent issue, within the last 30 days;
OR this issue was opened by an Owner/Member/Collaborator within the last 30 days. | required
```

Re-grading flipped #253 to accept, with all 8 checks passing. That is my first change of my own work.

**Claim and reproduction.** I claimed the issue on 2026-11-04 and then reproduced it with a throwaway test that registers one tool with `annotations: { title: "Delete", destructiveHint: true, readOnlyHint: false }` through a mock client. The test failed on `main` at `f6e1d5f`, and the output showed the description carried the hint while the structured field was missing:

```
description     : Delete a record [⚠️ destructive]
annotations     : undefined
AssertionError [ERR_ASSERTION]: Expected values to be strictly deep-equal:
+ undefined
- { destructiveHint: true, readOnlyHint: false }
```

That output is what I posted on the issue as my repro comment. My repro-check tool had accepted it first, and it also read the repo's CONTRIBUTING.md and AGENTS.md for an AI-disclosure rule (there is none, and I disclosed my use of Claude Code anyway).

**The plan, and a second version of it.** My first `plan.md` listed files, steps and tests, and plan-check rejected it. Its deciding line was: "`plan.md` never states the cause... The Reproduced line points to the local `repro-draft.md`, not the posted comment." I added a Cause section (`_registerTool` builds the `registerTool` literal without `annotations`), linked the posted repro, and reworded my test step because only the passthrough test fails before the fix. plan-check accepted it two minutes later.

**The fix.** It is one pure helper and one conditional spread, which satisfies `exactOptionalPropertyTypes` and omits the field instead of sending `{}`:

```ts
function buildToolAnnotations(annotations): ToolAnnotations | undefined {
  if (!annotations) return undefined;
  const result: ToolAnnotations = {};
  for (const key of ANNOTATION_HINT_KEYS) {
    const value = annotations[key];
    if (typeof value === "boolean") result[key] = value;
  }
  return Object.keys(result).length > 0 ? result : undefined;
}
// in _registerTool:
...(toolAnnotations ? { annotations: toolAnnotations } : {}),
```

I wrote seven tests first. With the spread commented out, four of the seven fail (passthrough, all four hints, title not copied, non-boolean values ignored). The other three, which check that the key is omitted and the description is unchanged, are regression guards and pass either way.

**A catch by my tool, and a second version of my PR description.** My PR draft said: "Only the first two fail before the change; the other five are regression guards and pass either way." pr-precheck re-ran the tests with the spread disabled, found four failures, and rejected the draft for a description that did not match the diff. It also rejected it because I had left out the PR template's Self-review section. I corrected the count, added the section, and the second run passed all 10 checks. I also found while filling in my ship manifest that `npm run test:coverage` selects no files on Windows because of its single-quoted glob, so my "coverage floors met" had passed on an empty report. I re-ran it with a double-quoted glob and got real numbers (`tool-bridge.ts` 98.27% lines, 94.59% branches, 95.65% functions) before the PR went out.

**What landed.** I opened PR #265 on 2026-11-05 with one commit (`6f12dbc`, five files, +118/-6). The maintainer's review comment, about a day later, was: "thank you for the contributions @rnossair , merging this now!" The PR merged on 2026-11-06, and on the issue the maintainer wrote that I could "feel free to take on other issues."

## Limitations and what's next

My fix only proves what the bridge hands to `registerTool`. I never ran a real Pi permission extension against it, so I have not seen a permission prompt actually change because of these annotations. My non-boolean test is reachable only through a mock, because the MCP SDK already validates the hints as booleans.

The toolkit also let me down in two places. My ship-gate hook was installed as `python3`, which on my Windows machine is only the Microsoft Store shortcut and fails with "Python was not found" and exit code 49. Claude Code blocks a command only when a hook exits with code 2, so the gate failed open and PR #265 opened without any gate run. I found it afterwards because the gate's log file did not exist, and I changed the hook to `python`. My scout is also still noisy: every top candidate was "updated today", so recency ranking mostly reflects churn, and it has no check for an issue a merged PR has already mostly fixed (amd/gaia#4779 passed that way). Finally, my voice guide and my repro-check rubric disagreed about "I expect to open a PR within a few days". The rubric passed it because "expect" is hedged, my voice guide forbids it, and I overrode the guide and posted it.

Next, I would test any new hook once with a harmless command and look for its log before trusting it. I would add a "partially fixed already" check to the scout rubric and rank by creation date, and make my claim check fail any stated timeframe so the rubric and the voice guide agree. The maintainer has invited me to take another issue, so I would run the improved scout again and aim for one where I can test against a real permission extension.

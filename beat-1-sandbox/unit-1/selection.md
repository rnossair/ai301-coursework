# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/57

**Verdict output**

#69 — Output parser crashes on JSON array fallback (RAG generator)

- Active Repository: pass — last commit 2026-09-16, 6 days ago
- Active Issue: pass — created 2026-09-10, comment as recent as 2026-09-19
- Issue Unclaimed (preferred): pass — claimed only by student jacho15 (association NONE); house rule says student claims don't block
- Issue Open: pass — no linked/mentioned PRs against #69
- Bounded: pass — single fix, 2 named files, 2–4hr estimate
- Familiar Techstack (preferred): pass — RAG generator, Python; matches stated RAG interest
- Verdict: accept

```
{
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/57",
    "checks": [
      {"name": "Active Repository", "grade": "pass", "evidence": "Most recent default-branch commit dated 2026-09-16 (repo pushed_at), 6 days before today 2026-09-22"},
      {"name": "Active Issue", "grade": "pass", "evidence": "Issue created 2026-09-10 (12 days old), well under the 6-month age that would trigger the comment requirement"},
      {"name": "Issue Unclaimed", "grade": "pass", "evidence": "comments: 0 — no claim comments at all"},
      {"name": "Issue Open", "grade": "pass", "evidence": "No open or closed PR in the repo's PR list mentions or closes #57"},
      {"name": "Bounded", "grade": "pass", "evidence": "Body scopes one bug in tech_detector.py with a runnable repro and two named failing tests"},
      {"name": "Familiar Techstack", "grade": "pass", "evidence": "Labeled 'agent' (Agent tools & orchestration), Python — matches stated interest in 'Agent tools'"}
    ],
    "verdict": "accept"
  }
```

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

- 8/20
- 17/20
- 16/20
- 18/20


**Issue analysis**

issue-09 — rubric's decision: **reject**; gold label: **accept**. From `eval-run.txt`:

```
issue-09  accept  reject   NO     failed: Active Issue, Issue Unclaimed (preferred), Familiar Techstack (preferred)
```

Reasoning: the rubric's verdict rule states "accept if all 'required' check
passes, 'preferred' checks don't change the verdict but are used for ranking
accepted issues." 

**Check rationale**

`| Active Repository | last 6 default-branch commit dates | A commit has been made in the past 2 months | required |`

This seems to be be balanced enough to do the trick.

**Trade-offs**

This does mean it would drop any repository that has not been maintained in 2 months, which might be a little too short for some smaller repositories that still have maintainers.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**


1. The issue's fit to your interests and to the time available.

   Yes, the issue fits my interest by working on an issue related to agent tools, and seems to be fixable in a reasonable amount of time.

2. What the verdict identified correctly, and what you weighed that the rubric could not.

   The rubric identified the tech stack and activity correctly, but can't really weigh how long it would take to fix.

3. The anticipated difficulty in claiming it.

   Moderate. Very Doable.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.

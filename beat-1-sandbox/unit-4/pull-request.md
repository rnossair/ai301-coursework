# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s1/pull/246

**Branch**

fix/57-skip-root-relative-vendor-paths

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

- 19/20

**Package analysis**

pkg-20 (category: standards-wall). Gold label: reject. My rubric said accept, so it is my one disagreement in the final run (the note in `eval-run.txt` is `graded accept`). The other standards-wall package, pkg-01, was rejected correctly, so the category scored 1/2.

My rubric accepted it because every `required` check passed, and the verdict rule is "**accept** if every `required` check passes". A standards-wall package is one that gets past the correctness checks (diff within scope, plan delivered, description matches the diff, observable test evidence) but breaks the repo's own standards. The checks that are meant to catch that are `Repo checks run` and `Template and disclosure honored`, and both graded pass on this package, so nothing in my rubric held it.

**Check rationale**

> | Template and disclosure honored | The repo's PR template sections and stated policy (including AI-use disclosure), read against the PR description | Every template section is present with real, specific content (no empty sections, placeholders, or boilerplate left in), and any disclosure the repo asks for is present and truthful. Fail if a section is skipped or boilerplate, or a stated ask is visibly ignored. | required |

It reads that way because the standards-wall family is about the repo's stated rules, not about how the description looks. The pass condition asks for real, specific content in each template section and for the disclosure to be present and truthful, so a grader looks at what is written and whether it is true, not just whether a heading exists. A check that only counted headings would pass boilerplate, which is the failure this family is built around.

**Trade-offs**

This check is where my rubric still misses a case: pkg-20 is gold reject but my rubric accepts it, which is why the final score is 19/20 and not 20/20 and why standards-wall is 1/2. The check judges stated rules against the description, so a standards problem that is not visible in the repo-facts or template text the package quotes will pass it. I accept that miss for now instead of making the check stricter, because every clear-accept package (7/7) still accepts, and a stricter wording risks rejecting honest PRs that disclose a shortfall, which the rubric says can be ready.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.

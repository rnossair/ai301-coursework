# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

rnossair

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/57#issuecomment-6300371224

```
Hello, I am a developer contributing to this repo as part of AI301.

I'll be running this issue's test snippets targeting `agent/tools/tech_detector.py` as well as the two tests `test_node_modules_excluded,` `test_build_directory_excluded`, then use the provided example to reproduce the issue on my system before proposing a fix. I will be providing environment and system information, as well as relevant versions and step by step informations.

I'm using Claude for diagnosing a potential fix, as well as assisting in drafting a proper Claim and repro comment.
```


**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/57#issuecomment-6327815837

==============================================================================
==============================================================================
==============================================================================

Reproducing #57 using the issue's file list and its two named tests.

## Environment

Commit: `f89c06fc3ff292df2a04a39ac51319d32a76b779`
OS: `Windows 11 (10.0.26200)`
Python: `3.14.0 (project venv)`
Packages: `pytest 9.1.1, structlog 26.1.0`
Fork link: `https://github.com/rnossair/pathreview-ai301-fa26-s1`

## Steps to reproduce

**Setup** (Windows 11, Git Bash or PowerShell):

1. `git clone https://github.com/rnossair/pathreview-ai301-fa26-s1 && cd pathreview-ai301-fa26-s1`
2. `git checkout f89c06fc3ff292df2a04a39ac51319d32a76b779`
3. `python -m venv .venv`, then `.venv/Scripts/pip install -e ".[dev]"`

All commands below run from the repo root.

### Control: no vendored files (expect `Python`)

4. Save as `control_57.py`:

```python
from agent.tools.tech_detector import TechDetector

t = TechDetector()
files = ['main.py', 'core/app.py']
print(t.execute({'files': files}).data)
```

5. Run `.venv/Scripts/python control_57.py`. Expect `'primary_language': 'Python'`.

Output:

```
$ .venv/Scripts/python control_57.py
2026-10-20 16:02:00 [info     ] tech_detected                  frameworks_count=0 languages_count=1 primary_lang=Python
{'primary_language': 'Python', 'all_languages': ['Python'], 'frameworks': []}
```

### Reproduction: the issue's file list (expect the bug)

6. Save as `repro_57.py` (the issue's snippet with its exact file list; I print the whole result dict instead of only `['primary_language']`, so `all_languages` is visible too):

```python
from agent.tools.tech_detector import TechDetector

t = TechDetector()
files = ['main.py', 'core/app.py',
         'node_modules/lib/index.js', 'node_modules/lib/util.js',
         'node_modules/x/a.js', 'node_modules/y/b.js',
         'build/bundle.js', 'build/vendor.js']

print(t.execute({'files': files}).data)
```

7. Run `.venv/Scripts/python repro_57.py`. Actual: `'primary_language': 'JavaScript'`.

The only change from the control run is the six vendored paths under `node_modules/` and `build/`.

Output:

```
$ .venv/Scripts/python repro_57.py
2026-10-20 15:29:05 [info     ] tech_detected                  frameworks_count=0 languages_count=2 primary_lang=JavaScript
{'primary_language': 'JavaScript', 'all_languages': ['JavaScript', 'Python'], 'frameworks': []}
```

**Expected:** `Python`
**Actual:** `JavaScript`

### Tests the issue names

8. Run:

`.venv/Scripts/pytest tests/unit/test_tech_detector.py -k "node_modules_excluded or build_directory_excluded" -v --runxfail`

### Output 

Run with `--runxfail`

```
------------------------------------------------------------------- Captured stdout call --------------------------------------------------------------------
2026-10-20 15:31:14 [info     ] tech_detected                  frameworks_count=0 languages_count=2 primary_lang=JavaScript
================================================================== short test summary info ==================================================================
FAILED tests/unit/test_tech_detector.py::TestTechDetector::test_node_modules_excluded - AssertionError: assert 'JavaScript' == 'Python'
FAILED tests/unit/test_tech_detector.py::TestTechDetector::test_build_directory_excluded - AssertionError: assert 'JavaScript' == 'Python'
============================================================= 2 failed, 25 deselected in 1.05s ==============================================================
```

Run without `--runxfail`

```
tests/unit/test_tech_detector.py::TestTechDetector::test_node_modules_excluded XFAIL (issue #57: tech detector counts vendored/build-output files)     [ 50%]
tests/unit/test_tech_detector.py::TestTechDetector::test_build_directory_excluded XFAIL (issue #57: tech detector counts vendored/build-output files)  [100%]

============================================================= 25 deselected, 2 xfailed in 0.80s =============================================================
```

## Conclusion

Reproduced on commit `f89c06f`. With the issue's file list, `primary_language` is `JavaScript` instead of the expected `Python`, and both `test_node_modules_excluded` and `test_build_directory_excluded` fail with `assert 'JavaScript' == 'Python'` when run with `--runxfail`. Without the six vendored paths (control run), the result is `Python`, so the `node_modules/` and `build/` paths are what change the answer.

## AI disclosure

I used Claude (Claude Code) to help me understand the code, walk through the reproduction steps, and review and edit this report. I ran the commands and tests on my own machine, and the outputs above are from those runs.

==============================================================================
==============================================================================
==============================================================================

## Eval iterations

**Run history**

17/20, 20/20

**Package analysis**

pkg-20 (ghostty-org/ghostty#13604). Gold label: reject. My first rubric said accept, and the final rubric says reject. The repro itself is good. It records ghostty 1.3.1, Fedora 42, GTK and GNOME 48. It shows `^[[?997;2n` for the single-theme run and `^[[?997;1n` for the control, and it states the expected and actual results. My first four checks (`includes-version`, `includes-environment`, `problem-detail`, `Includes-steps`) all passed, so the package was accepted.

The package fails on the repo's rules. The repo-facts block says "All AI usage in any form must be disclosed, stating the tool used and the extent of the assistance", and neither the claim comment nor the repro report discloses anything. The rubric now has a `respects-repo-policy` check that compares the comments with the repo-facts block, so it reads the package as a disclosure failure and rejects it.

**Check rationale**

> respects-repo-policy | The repo-facts block (contribution policy, AI policy) read against the claim comment and repro report | the comments follow the repo's stated rules. If the repo requires disclosure of AI-assisted work, the comments disclose it (tool and extent). Treat course packages as AI-assisted work. If the repo states no such rule, this passes. | required

My first rubric had only four checks, and all of them looked at the repro report's sections. Nothing read the repo-facts block or the claim comment, so pkg-20 (disclosure), pkg-19 (boilerplate claim comment) and pkg-08 (wrong-target artifact) were all accepted. I added this check to cover the "words respect the repo's conventions" proof family. The pass condition asks for an outcome: do the comments follow the repo's stated rule? It does not ask whether a heading is present. It also passes when the repo states no AI policy, so repos like vuejs/core and jqlang/jq are not penalized. I rejected a flat "must include an AI disclosure" rule because it would fail the clear accepts in repos with no AI policy.

**Trade-offs**

All eight clear-accept packages still accept (8/8), and so do pkg-09 and pkg-10, the honest cannot-reproduce cases, so the "passes if the repo states no such rule" clause did not cost any accepts. The added checks changed only the three false accepts (pkg-08, pkg-19, pkg-20).

What this check will miss: it only enforces policy that appears in the captured repo-facts block. A repo whose AI rule is stated somewhere the block doesn't capture would pass undetected. Treating all course packages as AI-assisted also means a repo with a disclosure rule rejects every package unless it discloses, even if the author wrote it by hand.

This is one run with a pinned `sonnet` model. I did not re-run it or use `--only` on a canary, so I can't say how stable the 20/20 is.


---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.

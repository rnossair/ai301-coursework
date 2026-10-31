# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

rnossair

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/57#issuecomment-6500997484

# Plan for #57: tech detector counts vendored and build-output files

Branch: `fix/57-skip-root-relative-vendor-paths` on my fork (`rnossair/pathreview-ai301-fa26-s1`), cut from `main` at `f89c06f`.

## Diagnosis

My repro (posted on #57) with the issue's exact 8-file list returns `primary_language: 'JavaScript'` (`all_languages: ['JavaScript', 'Python']`). With only `main.py` and `core/app.py` it returns `Python`, so the six paths under `node_modules/` and `build/` are what flip the result.

Cause: `TechDetector._should_skip_file` (`agent/tools/tech_detector.py`, line 142) skips a file when any pattern such as `"/node_modules/"` or `"/build/"` is a substring of the path. A root-relative path like `node_modules/lib/index.js` has no leading `/`, so the substring test is false and the file is not skipped. I checked directly on my machine:

- `_should_skip_file('node_modules/lib/index.js')` returns `False`
- `_should_skip_file('a/node_modules/lib/index.js')` returns `True`

Both failing tests (`test_node_modules_excluded`, `test_build_directory_excluded`) use root-relative paths, which matches the cause. `test_vendor_files_excluded` has root-relative `vendor/` paths too, but it asserts nothing, so it passes whatever the detector returns.

## Change (in scope)

1. `agent/tools/tech_detector.py`, `_should_skip_file` only: replace the substring patterns with a directory-name check. Split the path on `/`, ignore the last part (the file name), and skip the file if any remaining part is in the set `{node_modules, vendor, dist, build, .git, __pycache__, .venv, venv}`. This is the same list the method has today. Matching whole directory names fixes root-relative paths without the false positives of simply deleting the leading slash (`"build/"` is also a substring of `src/rebuild/x.js`).
2. `tests/unit/test_tech_detector.py`:
   - delete the two `@pytest.mark.xfail(strict=True, ...)` markers on `test_node_modules_excluded` and `test_build_directory_excluded` (the repo's CONTRIBUTING says removing the marker is part of the fix, since `strict=True` turns a passing test into a CI failure);
   - add `test_issue_57_root_relative_vendor_paths`: the issue's exact 8-file list, asserting `primary_language == "Python"` and `all_languages == ["Python"]`;
   - add `test_vendor_like_directory_names_not_skipped`: `src/rebuild/app.js` and `src/node_modules_utils/a.js` must still be counted, so the fix does not skip directories that merely contain a skip word.

## Not in scope (will not touch)

- How `primary_language` is chosen. It currently takes the alphabetically first detected language (`sorted(languages)[0]`) and does not count files. That is a separate bug. Once vendored files are filtered, the issue's example returns `Python` without changing it. I will mention it as a follow-up, not fix it here.
- `EXT_TO_LANG`, `CONFIG_INDICATORS`, framework detection, and `agent/orchestrator.py`.
- Backslash (Windows-style) paths and any other path normalization.
- Lint or type baseline cleanup, and the `pyproject.toml` suppressions (none relate to this issue).

## Order of work

1. Branch `fix/57-skip-root-relative-vendor-paths` from `main`.
2. Add the two new tests first and run them. Both must fail on unmodified code (the issue's list returns `JavaScript`). This is my failing-before evidence. The rebuild/node_modules_utils test will pass before the fix, since nothing is skipped wrongly today. It is a guard against over-skipping, not a before/after test.
3. Change `_should_skip_file`.
4. Delete the two xfail markers.
5. Run `pytest tests/unit/test_tech_detector.py` (all 27 existing tests plus 2 new must pass), then `make check && make test-unit`.
6. Commit as `fix(agent): skip root-relative vendored and build paths in tech detector`, with a `Fixes #57` footer. Push to my fork and note the plan deviations here if there are any.

## Test plan

- Automated: the new `test_issue_57_root_relative_vendor_paths` uses the same input as my repro and fails before the fix (`JavaScript`), then passes (`Python`).
- The two formerly-xfail tests become normal passing tests.
- The guard test covers the false-positive risk of the new matching.
- Manual re-run of `repro_57.py` on the branch, to compare against my posted output.

## Risks and unknowns

- I don't know what path format real callers send. `agent/orchestrator.py` passes `profile_data["files"]` straight through, and I haven't traced where that list comes from. The fix handles relative paths and paths with a leading directory; leading `/`, `./` and Windows `\` forms are untested in the real pipeline.
- Skipping any directory named `build` or `dist` also skips legitimate source directories with those names. That is existing behaviour for nested paths, and I'm not changing the list.
- Other students have posted a similar diagnosis on #57. I reproduced it independently, and my plan differs in the matching approach (directory names instead of removing the slash).
- I'm confident about the cause for the issue's input. I'm not claiming this makes language detection correct in general, and I'm not promising a delivery date.

## AI disclosure

I'm using Claude (Claude Code) to help me investigate, draft this plan and review the change. I run the code and tests myself and I'll review every line before I push.

---

## Your branch

**Branch**

`fix/57-skip-root-relative-vendor-paths`

**Evidence**

Same `repro_57.py` as my Unit 2 repro comment (the issue's exact 8-file list), run from the repo root with `.venv/Scripts/python repro_57.py`.

Before (unmodified `_should_skip_file`, `main` at `f89c06f`):

```
$ .venv/Scripts/python repro_57.py
2026-10-29 10:46:35 [info     ] tech_detected                  frameworks_count=0 languages_count=2 primary_lang=JavaScript
{'primary_language': 'JavaScript', 'all_languages': ['JavaScript', 'Python'], 'frameworks': []}
```

After (branch `fix/57-skip-root-relative-vendor-paths`, commit `4abc301`):

```
$ .venv/Scripts/python repro_57.py
2026-10-29 10:46:36 [info     ] tech_detected                  frameworks_count=0 languages_count=1 primary_lang=Python
{'primary_language': 'Python', 'all_languages': ['Python'], 'frameworks': []}
```

Tests on the branch:

```
$ .venv/Scripts/pytest tests/unit/test_tech_detector.py -q
29 passed in 0.54s
```

Before the fix, the new `test_issue_57_root_relative_vendor_paths` failed with `AssertionError: assert 'JavaScript' == 'Python'`. `make check` is clean and `make test-unit` gives 379 passed, 51 xfailed.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

18/20

**Package analysis**

pkg-13 (category: clear-accept). Gold label: accept. My rubric said reject, so it is one of my two disagreements (the other is pkg-14). The note in `eval-run.txt` is `failed: test-is-automated-and-observable`, and that was the only failing check.

My rubric read it that way because of the pass condition on that check: "The plan adds an automated test that fails before the fix and passes after, and that exercises the same input and behavior the repro evidence shows." The check wants the plan to show the test failing on the unfixed code. The gold label treats the package as ready, so the package's test plan was good enough for a human grader but did not meet that wording as my grader applied it. The check is stricter than the gold standard.

**Check rationale**

> | test-is-automated-and-observable | The plan's test plan, read against the repro evidence's steps and artifact | The plan adds an automated test that fails before the fix and passes after, and that exercises the same input and behavior the repro evidence shows. Fails if the tests are manual only, absent, or would pass without the fix (prove nothing observable). | required |

It reads that way because a test plan is only worth anything if it can tell fixed code from unfixed code, so the pass condition is about outcome (fails before, passes after, same input as the repro) and not about whether a "Tests" heading exists. I rejected a looser "the plan mentions tests" version because it would pass tests that prove nothing. The cost of the strict version is the two false rejects (pkg-13, pkg-14).

**Trade-offs**

This check changes the result for two packages: pkg-13 and pkg-14 are gold accept, and my rubric rejects both on `test-is-automated-and-observable`. That is why the run scored 18/20, exactly on the bar (18/20: PASS), instead of 20/20. The other 18 packages agree with gold, including all 4 wrong-cause, all 4 scope-creep, all 3 unbuildable and both thread-convention packages, so the check is not what is catching those. I accept the two false rejects for now because a plan that cannot show a test failing before the fix should not be built from, and loosening the check risks letting through the packages designed around the "proves nothing" family.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.

# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s3/pull/228

**Branch**

`fix/72-malformed-hash`

## Eval iterations

**Run history**

One run:

1. Full run, 20 packages: **20/20**, bar PASS, all five categories matched (`clear-accept 7/7`, `not-tested 4/4`, `silent-drift 4/4`, `standards-wall 2/2`, `unreviewable 3/3`). This is the run in `eval-run.txt`.

A first attempt before it crashed on startup under the machine's default Python 3.9.6, because the harness uses `Path | None` annotations that need 3.10 or later. It graded nothing, so I do not count it as a run. The run above used Python 3.11. There was no `--only` loop and no revision to the skill files after the run, because the first full run matched all 20. The files were written in one working session, so I have no calibration history to report. I saved per-check grades for this run with `--out`, and the Package analysis below reads them.

**Package analysis**

`pkg-01` (pandas-dev/pandas#66657, category `standards-wall`). My rubric decided `reject` and the gold label is `reject`. The gold note reads: "solid C-parser fix with decisive ASAN evidence, but pandas' required PR checklist is visibly ignored: no closes #xxxx, no checklist, and the stated whatsnew entry is absent from the diff"

The saved per-check grades show three failures. The one the gold label names is `standards-respected`: "Description has no closing issue reference and no doc/source/whatsnew/vX.X.X.rst entry, both required by the repo's PR template for a bug fix." The repo's template asks for a closing reference and a whatsnew entry, the description has neither, and the check rejects on a stated template ask however good the fix is. The package's fix itself is sound: `plan-fidelity`, `description-faithful` and `test-decisive` all pass, with "Evidence re-runs the issue's own trigger script showing before=99999/after=100000, plus a clean ASAN re-run and full parser suite pass."

The other two failures are mine to explain. `repo-checks-reported` failed ("Template's 'all code checks passed (pre-commit)' item is never addressed in the description or test evidence."), but it is preferred, so it cannot change a verdict. `diff-reviewable` also failed, which the gold note does not mention: "New block duplicates the immediately-following make_stream_space(self, self->datalen - self->datapos) call and its identical malloc/snprintf('out of memory') error block verbatim." Reading the diff, that is real: the new block repeats the existing four-line out-of-memory handler right next to it. But repeated error-handling code is a code-quality quirk and not what that check is for, so on this package it is a second reason to reject that the gold label does not endorse. It does not change the verdict, since `standards-respected` already rejects the package.

Compare `pkg-20` (ghostty-org/ghostty#13604), the other `standards-wall` package. It fails `standards-respected` alone, on a different ask: "AI_POLICY requires 'All AI usage in any form must be disclosed, stating the tool used and the extent of assistance,' and the PR description contains no such disclosure." Its gold note says every package in the set is treated as AI-assisted work. So the category is two different asks, a template in one package and an AI-use disclosure in the other, and one check reads the repo's stated asks for both.

**Check rationale**

From the `rubric.md` uploaded to `tools/pr-precheck/`, the `standards-respected` pass condition as it now reads:

> The description satisfies what the repo states of a pull request. It names the issue the way the template or contribution docs ask (a closing reference), fills the template's required sections and checklist truthfully, and meets any AI-use disclosure the policy requires of pull requests or their descriptions. Read the policy's scope before its content: a disclosure duty written for issue comments does not govern the PR description, a duty covering all AI use or AI-assisted contributions does, and a human-voice-only rule is met by a specific description in the author's own words. Silence in the repo facts imposes nothing. Fail when a stated template or disclosure ask is unmet, however good the fix is.

I did not revise this check in this unit; it was written once, before the run. Its scope-first reading carries over from the `conventions-respected` check in Units 2 and 3, turned around for a pull request: there a duty written for pull requests did not govern an issue comment, and here a duty written for issue comments does not govern the PR description. I added the template asks (a closing reference, the required sections, a truthful checklist) because the category has a package, `pkg-01`, that fails on those and not on AI use. I put the template asks and the disclosure duty in one check because both are the repo's stated asks of a pull request, and the category has exactly one package of each kind. The cost of one check is that it has two failure modes, which the grade's evidence line has to name.

**Trade-offs**

The weakest check is `diff-reviewable`, and `pkg-01` shows where. Its pass condition lists "duplicated blocks" as debris. That phrase reached a legitimate four-line error handler that repeats an adjacent one, and failed the package on a quirk a reviewer might only comment on. The verdict was already `reject` through `standards-respected`, so no label changed. On a PR that is otherwise fine, the same wording would hold a small duplicate that no maintainer would ask to remove. I accept that for now. Tightening it to debug output, commented-out code and unrelated hunks would be the revision.

What I cannot say: I ran the full set once. In Unit 2 a rubric graded one package differently on two runs with no edit in between (`pkg-09`), so one 20/20 is a single measurement and says nothing about grader variance. Because nothing was revised, there was no changed check to protect with canaries, and I ran none.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.

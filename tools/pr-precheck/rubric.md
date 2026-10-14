# Rubric: is this pull request ready to submit?

A ready PR does what its plan said and no more, says truthfully what it did,
proves the change with evidence a reviewer can see, arrives without debris that
buries the fix, and meets what the repository itself asks of a pull request. A
PR that honestly discloses a shortfall can be ready.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| plan-fidelity | The diff's changed files and hunks, read against the plan's in-scope list, out-of-scope list, files, approach, and its Deviations section, and against the description's Changes list | Every substantive change in the diff is named by the plan or its Deviations, and every in-scope item the plan promised is present in the diff or its absence is disclosed in the description or Deviations. Tests and the smallest plumbing the fix needs count as in scope. Fail when the diff does more than the plan (a new option, flag, or setting, a rewrite beyond the planned site, an unrequested behavior change, a rename or cleanup pass) or does less with no disclosure (a promised part missing and not mentioned). A deliberate shortfall passes when the description or Deviations says what was left out and why. | required |
| description-faithful | Every claim in the PR title and description (what changed, what was added, what was tested), read against what the diff contains | Each claim the description makes about the change is true of the diff: a file, test, doc update, flag, or behavior it says is there is there, and a deferral it states is real. Fail when the description claims something the diff lacks (a docs update that is not in the diff, a fix wider than the diff shows) or presents the PR as faithful to the plan while the diff contradicts the plan. A short plain description passes; a confident, well-formatted one proves nothing. | required |
| test-decisive | The test evidence, read against the plan's test plan and the issue's reproduction: the commands, the inputs, the before output and the after output | The evidence re-runs the issue's own trigger, the path that actually failed, through the changed code, and shows an observable before and after: specific output, exit status, state, or artifact. It keeps a control or regression assertion where the plan names one. Fail when the evidence exercises only an adjacent path that never failed (a control case, a different method or input), or is only a claim ("tested locally", "works on my machine", "tests pass") with nothing observable, or leaves out a case the plan's test plan names without saying so. A broad suite may supplement the trigger but cannot replace it. | required |
| diff-reviewable | The diff's hunks, read for debug output, commented-out code, duplicated blocks, and edits unrelated to the fix | The diff contains only hunks a reviewer can tie to the fix. Fail when it carries debug prints or logging added for the author's own use, commented-out code or leftover experiments, duplicated blocks, unrelated import reordering, formatting churn outside the touched lines, or incidental renames, such that a reviewer would have to separate the fix from them. Formatting a repo-required formatter produced on touched lines is not debris. | required |
| standards-respected | The repo-facts block's PR template, contribution rules, and AI policy, read against the PR title and description | The description satisfies what the repo states of a pull request. It names the issue the way the template or contribution docs ask (a closing reference), fills the template's required sections and checklist truthfully, and meets any AI-use disclosure the policy requires of pull requests or their descriptions. Read the policy's scope before its content: a disclosure duty written for issue comments does not govern the PR description, a duty covering all AI use or AI-assisted contributions does, and a human-voice-only rule is met by a specific description in the author's own words. Silence in the repo facts imposes nothing. Fail when a stated template or disclosure ask is unmet, however good the fix is. | required |
| repo-checks-reported | The test evidence and description, read against the repo's own checks named in the PR template or contribution docs | The package shows the repo's named checks run with their results, or states why one was not run. A failing check reported with its reason passes. This never changes a verdict. It separates a PR that ran what the maintainers asked from one that only proved its own trigger. | preferred |

## Verdict rule

`accept` (ready to submit) when every **required** check passes. Any required
`fail` produces `reject` (hold).

`unclear` means the named evidence is absent or cannot decide the check. It is
not an established failure, but on a required check it still produces `reject`:
a PR that cannot be verified from its package is not ready to submit. The
summary must say a hold of this kind is for insufficient evidence, not for bad
evidence. An `unclear` on a required check that keeps recurring means the check
asks for evidence the package does not carry, which is a flaw in the check, not
in the PR.

`preferred` checks never change the verdict, whether they pass, fail, or are
unclear. Report their grades.

An honestly disclosed shortfall never fails a check by itself: the checks above
fail on what the PR hides, not on what it admits.

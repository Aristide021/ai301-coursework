# Procedure: how this tool grades a PR package

## Read order

1. **Gate.** Confirm the mode. In live mode read `scope.md`, then stop if its
   `Repo:` line is a placeholder or the issue or PR is outside it. Confirm the
   diff from `git diff main...HEAD` is not empty. In eval mode skip `scope.md`
   and `voice-guide.md` entirely. Confirm `rubric.md` and this procedure have
   content, and stop if not.
2. Read `rubric.md` and `references/evidence-guide.md`. List the checks and the
   verdict rule.
3. Read the issue and its thread. Note the behavior the issue asks for, the
   issue's own reproduction, and any maintainer direction.
4. Read the plan before anything the author wrote about the PR. Note, as three
   lists: the in-scope items and files, the out-of-scope items, and every item in
   the test plan. Then read the plan's Deviations section and add whatever it
   records to the in-scope list. The plan goes before the diff and the
   description because every later check is a side-by-side against it. Reading
   the description first lets its framing set what you expect to find.
5. Read the repo facts: the PR template, the contribution rules, the AI policy.
   Note each thing the repo says a PR must contain, and the scope each rule
   covers (issue comments, pull requests, or all AI use).
6. Read the diff. List every changed file, and classify each hunk as fix, test,
   plumbing the fix needs, or other. Note any added debug output, commented-out
   code, duplicated block, unrelated import change, or formatting-only hunk.
7. Read the PR title and description. List each claim it makes about what
   changed, what was added or deferred, and what was tested.
8. Read the test evidence last, with the plan's test-plan list beside it.

## Evidence gathering

- **plan-fidelity.** Put the diff's file list and hunk classes next to the plan's
  in-scope list (with Deviations). Record: changes the plan does not name, and
  planned items with no matching change. For each missing planned item, record
  whether the description or Deviations discloses it.
- **description-faithful.** For each claim in the description, find the file or
  hunk in the diff that makes it true. Record every claim with no match, and any
  statement that the PR follows the plan when the plan-fidelity gather found a
  mismatch.
- **test-decisive.** Take the issue's reproduction and the plan's test plan.
  Find, in the test evidence, the command that exercises that same trigger, and
  record whether a before output and an after output are both shown. Record each
  test-plan item the evidence covers and each it skips, and whether the evidence
  runs the path that failed or only an adjacent one.
- **diff-reviewable.** From step 6, record every hunk classed as other and any
  debris found. Check each against the plan before calling it debris.
- **standards-respected.** Match the repo's asks from step 5 to the description:
  the closing issue reference, each required template section, the checklist
  boxes against what the evidence shows, and any AI-use disclosure whose scope
  reaches a pull request. Record what is missing.
- **repo-checks-reported.** List the repo's named checks from step 5 and find
  each in the evidence or description with its result or a stated reason.

## Check execution

Run the checks in the rubric's order. Grade each from the evidence gathered for
it, quoting the deciding fact or line in one sentence. Do not re-read the whole
package for each check: go back to the package only to settle a specific fact
you did not record. When the evidence a check names is genuinely absent from the
package (not merely hard to find), grade it `unclear` and say what is missing.
Do not grade `fail` for absence of evidence you did not look for, and do not
grade `pass` on a guess. A claim that passes by the rubric's wording passes even
if it feels wrong. Note the tension in the summary and leave the grade. Do not
let one check's finding grade another: a debris hunk is `diff-reviewable`, and
it is only `plan-fidelity` if it also changes behavior beyond the plan.

## Verdict assembly

Apply the rubric's verdict rule. Count the required checks. If every one is
`pass`, the verdict is `accept`. If any required check is `fail`, the verdict is
`reject`. If none failed but any required check is `unclear`, the verdict is
`reject` for insufficient evidence, and the summary says so in those words. Never
count a preferred check. When more than one required check failed, quote the
first failing required check in rubric order as the deciding check, and still
list every other grade. In live mode, after the verdict, report the voice-guide
rules the title and description break, which do not change the verdict. Emit
the summary, then the JSON block last, with nothing after it.

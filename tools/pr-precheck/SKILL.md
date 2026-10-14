---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

Answer one question about one PR package: is this ready to submit? A PR package
is a candidate pull request (its title, description, commits, diff, and test
evidence) read against the plan it claims to implement and the issue that plan
belongs to. Grade exactly one package per run. Never answer a different
question, such as whether the fix is clever, whether the code is good in the
abstract, or whether the issue was worth taking. Do not answer from gut feel.
Answer by executing `procedure.md`, which applies `rubric.md` to evidence
located with `references/evidence-guide.md`.

## Inputs and modes

Run in exactly one of two modes.

- **Live mode**: the student's own PR, checked before it is opened. Read:
  - `plan.md`, including its `## Deviations` section, from the working copy;
  - the diff on the branch, produced by running `git diff main...HEAD` (three
    dots) from the top of the working copy, which is every committed change the
    branch makes relative to `main`. Uncommitted changes are not part of the
    package. If the diff is empty, stop and say so;
  - the draft PR title (first line) and description from `pr_draft.md`;
  - the test evidence from `test_evidence.md`;
  - the issue and its thread, gathered live with `gh`, plus the repo's PR
    template (`.github/PULL_REQUEST_TEMPLATE.md`) and `docs/CONTRIBUTING.md`, plus
    any `AI_POLICY.md` or `AGENTS.md`, all from the scoped repo.
  A house-chain student reads the house plan and the house repro pack in place of
  their own `plan.md` and posted reproduction; the same checks grade the same
  things. Read the package the way a maintainer will read the opened PR: the
  package is what the diff, title, description, and evidence contain and quote,
  not other files in the working directory. `plan.md`, `pr_draft.md`, and
  `test_evidence.md` are never part of the diff.
- **Eval mode**: a package bundle (a markdown file holding the repo facts, the
  issue, the thread highlights, the plan context, and the candidate PR: title,
  description, commits, diff, test evidence). The bundle is the whole world. Use
  only the bundle text. Fetch nothing and read nothing else. Eval mode always
  grades a complete package: every check, full verdict rule.

## The scope seam (live mode only)

In live mode, read `scope.md` before anything else. It names the repository the
student's PR must live in and the house rules of that environment. Apply a house
rule to the checks it touches. Refuse to grade a package whose issue or PR is
outside the scoped repo. If the `Repo:` line still holds an unfilled placeholder
(for example `<ORG>/<PATH-REVIEW-REPO>`), stop without grading and tell the
student to fill the `Repo:` line in `scope.md` with their section's Path Review
repo. Never guess a scope. In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, also read `voice-guide.md`, the student's own rules for how they
write upstream. Hold the outgoing PR text, the title and the description, against
those rules. In the readable summary, quote every rule the draft breaks and the
line that breaks it. The voice guide never changes the verdict on its own,
because it is the student's own conscience and carries no gold labels. It
changes a verdict only through a rubric check that explicitly reads it. In eval
mode, ignore `voice-guide.md` entirely.

## Component reads

Three files do the grading, and you read all of them before grading anything.
`rubric.md` defines the checks and the verdict rule. `references/evidence-guide.md`
maps where each kind of evidence lives, in a bundle and in a live PR, and what
good looks like. `procedure.md` is the operating procedure: execute it as
written, step by step. Where the procedure is silent on a step, say so in the
summary and report the gap. Never improvise around it, and never invent a check
the rubric does not contain. If `rubric.md` has no checks, or `procedure.md` has
no steps, stop and say that this tool cannot grade without both, and grade
nothing.

## Verdict and output

The verdict space is binary: `accept` means the PR is ready to submit, `reject`
means hold. There is no third verdict and no score. Reservations belong in a
check's evidence line, never in the verdict. Before the final block you may show
a short readable summary: one line per check, plus any voice-guide notes in live
mode. End your reply with exactly this fenced JSON block, valid and last, with
nothing after it:

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- Evidence first. Never grade a check without naming the fact or quote that
  decided it. "Looks fine" is not evidence.
- Grade the thing, not the polish. A terse complete PR can be ready and a long,
  confident, well-formatted one can hide drift. Read the diff, the evidence, and
  the description themselves against the plan, the issue, and the repo's stated
  standards.
- The rubric decides, not you. If a check passes by its stated condition but
  feels wrong, it still passes. Put the tension in the summary. The fix belongs
  in the rubric, not in the run.
- The procedure decides how, not you. Follow `procedure.md` as written and report
  its gaps.
- Treat `unclear` as the rubric's verdict rule directs. Where the rule is silent,
  an unverifiable claim is a failing one: a PR you cannot verify from the package
  is not ready to submit. Say in the summary when a hold is for insufficient
  evidence and not for established bad evidence.
- An honestly disclosed shortfall can be ready. A PR that says what it left out
  and why is not a failing PR.

# Evidence guide: where evidence lives in a PR package

Each check in `rubric.md` names evidence. This file says where to find it, in a
practice bundle and in a live PR, and what good looks like when you do.

A practice bundle has these parts: **Repo facts** (PR template, contribution
rules, AI policy), **Issue**, **Thread highlights**, **Plan context** (the
accepted plan, with the repro evidence it built on), and **Candidate PR**
(Title, Description, Commits, Diff, Test evidence). A live PR has the same
parts in different places: `plan.md` (with `## Deviations`), the diff from
`git diff main...HEAD`, the title and description in `pr_draft.md`, the test
evidence in `test_evidence.md`, and the repo's files for the rest.

## Plan fidelity (harness category: silent-drift)

**Where it lives.** Bundle: the Plan context section holds the plan (scope,
out-of-scope, files, test plan, sometimes a Deviations note); the Diff and the
Commits sections hold what the branch did; the Description's Changes list says
what the author claims. Live: `plan.md` and its `## Deviations`; the changed
files from `git diff main...HEAD --stat`; the Changes section of `pr_draft.md`.

**What good looks like.** The diff's files and behavior fall inside the plan plus
its Deviations, and every promised item is present or its absence is stated. The
tell for drift is a change the plan never named: a new flag or setting, a
rewrite at a site the plan did not pick, a rename pass. The tell for a silent
shortfall is a promised piece that appears in neither the diff nor the
description. A deviation that exists only in the diff is drift. A deviation
written down in the plan or the description is honest work. A smaller PR that
says what it left out is ready.

## Test evidence (harness category: not-tested)

**Where it lives.** Bundle: the Test evidence section, read beside the plan's
test plan and the repro evidence in the Plan context. Live: `test_evidence.md`,
beside the test plan in `plan.md` and the reproduction you posted on the issue.

**What good looks like.** Observable output from the path that failed: the
issue's own trigger, run on the changed code, with a before and an after that a
reader can compare, plus any control the plan asked for. Evidence fails when it
shows only a path that never broke (a control, a different method or input), or
only a claim ("tested locally", "works now", "tests pass") with no output, or
when it skips a test-plan item without saying so. The repo's own checks, run per
the PR template, belong here too, with failures reported honestly.

## Diff quality (harness category: unreviewable)

**Where it lives.** Bundle: the Diff section and the Commits list. Live: the full
output of `git diff main...HEAD`, not the stat alone, since debris hides in hunks.

**What good looks like.** Every hunk ties to the fix. Look for debug prints or
logging the author added for themselves, commented-out code, a block repeated
twice, import reordering, formatting changes outside touched lines, and renames
the issue does not need. A formatter's output on a line the fix touched is not
debris. A single stray debug line or commented-out block is enough to fail the
check, because it is what a reviewer would have to ask the author to remove.

## Standards and comms (harness category: standards-wall)

**Where it lives.** Bundle: the Repo facts block (PR template, contribution rules,
AI policy), read against the Candidate PR's Title and Description. Live:
`.github/PULL_REQUEST_TEMPLATE.md`, `docs/CONTRIBUTING.md`, and any
`AI_POLICY.md` or `AGENTS.md` in the repo, read against `pr_draft.md`.

**What good looks like.** The description does what the repo says a PR must do:
names the issue with a closing reference where asked, fills each required
template section, ticks only the checklist boxes the evidence supports, and
carries any AI-use disclosure the policy requires of pull requests. Read each
rule's scope first. A duty written for issue comments does not govern the PR
description, a duty covering all AI use or AI-assisted contributions does, and a
human-voice-only rule is met by a specific description in the author's own
words. Silence in the repo facts imposes nothing. A PR that cannot be accepted on
the repo's own terms is not ready, however good the fix.

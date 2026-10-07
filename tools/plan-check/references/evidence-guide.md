# Evidence guide: where evidence lives in a plan package

## Diagnosis and grounding

**Where it lives.** In eval mode, read the issue context and thread highlights,
then the repro-evidence block's steps, raw artifacts, controls, expected result,
actual result, and stated limits. Compare those with the candidate plan's
diagnosis, proposed fix layer, files, and approach. In live mode, use the issue
body and thread plus the student's posted reproduction comment; the draft
`plan.md` and comment are the candidate side. A claim that appears only in a
local source file but is not quoted in the drafts is not package evidence.

**What good looks like.** The diagnosis explains all load-bearing observations,
especially controls that distinguish one layer from another, and the proposed
change occurs early enough to prevent the shown failure. When the evidence only
narrows the cause, the plan labels the remaining hypothesis and gives a bounded
way to confirm it.

## Scope

**Where it lives.** In eval mode, use the plan's in-scope and out-of-scope
statements, proposed changes, files or areas, approach, risks, and deferrals,
then compare them with the issue request and thread direction. Also read the
plan comment because it may promise work the plan omits. In live mode, use the
same sections of `plan.md` and `comment.md` against the live issue thread.

**What good looks like.** One reviewable change fixes the reproduced behavior,
with tests and only necessary plumbing. The plan explicitly defers unrelated
migrations, redesigns, settings, UI work, broad upgrades, or adjacent symptoms.
A narrow option chosen from several thread proposals can pass when the choice
and deferrals are explained.

## Executability

**Where it lives.** Read the plan's files or code areas, ordered approach, and
implementation decision. Cross-check thread highlights for an already settled
site or strategy. In live mode, repository inspection may confirm that named
paths exist, but the grading evidence is still what the drafts tell a reader.

**What good looks like.** Another contributor can name the first place to open,
the behavior to alter, and the selected strategy. Exact functions may remain
unknown if the plan includes a specific trace that will locate the boundary and
a rule for choosing the final site. Open-ended profiling or a list of possible
layers with no decision is not executable.

## Test plan

**Where it lives.** In eval mode, map the candidate plan's test plan to the
repro-evidence block's commands or inputs, raw artifacts, controls, expected
result, and actual result. In live mode, map the draft to the student's posted
repro comment and any test direction in the issue thread.

**What good looks like.** The real changed code receives the reproduced trigger
and produces a stated observable outcome such as exact output, exit status,
rendered state, artifact, bounded timing, or absence of the original exception.
A relevant control stays unchanged when it is needed to show isolation. A full
suite is useful regression coverage but does not replace the issue-specific
oracle.

## Honesty

**Where it lives.** Read the plan's diagnosis language, risks, unknowns,
deferrals, and `## Deviations`, then compare them with limitations stated in the
repro evidence and unresolved questions in the thread. In a pre-build package,
an intentionally empty Deviations body is not itself a failure; after a build,
the updated plan must say what changed and why or state that the plan held.

**What good looks like.** Observations, inferences, and untested boundaries are
not blended together. Material uncertainty is named and contained with a check,
platform boundary, fallback, or explicit deferral. Confidence is proportional
to the package evidence, not the author's tone.

## Comms

**Where it lives.** In eval mode, read all thread highlights and the repo-facts
block's bug-report template and contribution-policy statements, then compare
them with the candidate plan comment and any promises in the plan. In live
mode, read the complete issue thread, linked active work material to the plan,
the repository's `CONTRIBUTING`/AI policy and relevant comment template, and the
draft comment. Apply Path Review's classmate rule from `scope.md`: another
student's plan does not block this plan, but the draft cannot piggyback it.

**What good looks like.** The comment is specific to this issue, acknowledges
maintainer direction and active prior work, and does not silently replace the
requested fix with a workaround. It follows policies according to their scope:
comment-wide disclosure rules require disclosure in the comment; PR-only rules
do not. With no stated rule or material thread direction, no ritual disclaimer
or invented response is required.


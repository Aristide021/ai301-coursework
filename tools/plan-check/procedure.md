# Procedure: how this skill grades a plan package

## Read order

1. Determine the mode. In live mode, read `scope.md`, confirm the issue is in
   the scoped repository, and read `voice-guide.md`; stop if the repo line is
   still a placeholder or the issue is outside scope. In eval mode, use only
   the package bundle and do not fetch live material.
2. Read `rubric.md` and `references/evidence-guide.md`. Record every check, its
   evidence family, its weight, and the verdict rule before reading the
   candidate so the candidate's polish does not set the standard.
3. Read the issue context and thread chronologically. Record the requested
   behavior, material maintainer direction, rejected approaches, active prior
   work, and any requested tests.
4. Read the repro evidence before the plan. Record the exact target input or
   command, actual result, expected result, controls, environment boundaries,
   and what those controls rule in or rule out.
5. Read the entire candidate plan, then the candidate plan comment. Record the
   diagnosis, chosen fix layer, in/out scope, files or areas, implementation
   decision, test oracle, risks, unknowns, deferrals, and any deviation.
6. Read the repo-facts block for template and contribution-policy rules. In
   live mode, gather those facts from the locations named by the evidence
   guide. Do not infer a policy that the package or live repository does not
   state.

## Evidence gathering

1. Build a diagnosis ledger with three columns: reproduced fact or control,
   plan claim, and relationship (`supports`, `does not establish`, or
   `contradicts`). Include every control that distinguishes layers or causes.
2. Build a scope ledger: list each proposed production change, dependency or
   migration, new option or UI change, refactor, test change, and deferred
   item. Mark whether the issue or material thread direction requires it.
3. Build an execution ledger: record the first named file or code area, the
   chosen behavioral change, the ordered work, and every decision the plan
   leaves for build time. Distinguish a bounded trace-to-confirm step from an
   open-ended investigation.
4. Build a test ledger mapping each reproduced trigger and relevant control to
   the planned post-fix command or check and its observable expected result.
   Record broad suites separately; they are supporting evidence, not the fix
   oracle.
5. Build an honesty ledger from explicit risks, unknowns, evidence boundaries,
   deferrals, and deviations. Compare it with gaps visible in the repro and
   thread rather than rewarding a risks heading by itself.
6. Build a communications ledger from material thread directions and repo
   rules, then quote or summarize where the plan comment engages each one.
   Treat issue-comment and pull-request requirements according to their stated
   scope.

## Check execution

1. Execute checks in this order: `diagnosis-grounded`, `scope-bounded`,
   `approach-executable`, `test-decisive`, `uncertainty-honest`,
   `thread-aligned`, `conventions-respected`, then preferred checks. This order
   prevents a detailed approach from rescuing a plan aimed at the wrong cause
   or an unbounded change.
2. For each check, use only the ledger entries named by that rubric row and
   apply its pass condition literally. Do not reward headings, length,
   confidence, or the number of files.
3. Grade `pass` only when the evidence satisfies the full condition. Grade
   `fail` when present evidence contradicts the condition or demonstrates the
   prohibited outcome. Grade `unclear` when the required fact is genuinely
   absent or the package cannot decide it; do not convert absence into a false
   factual finding.
4. For every grade, write one evidence line containing the decisive quoted
   phrase or a concrete comparison between package facts. If a check has
   several clauses, cite the clause that controls the grade.
5. Do not re-read the whole package when a ledger already contains the named
   evidence. Re-read the source section only when the ledger has an ambiguity,
   and update the ledger before grading.
6. In live mode, separately compare the comment with `voice-guide.md` and note
   any broken rule in the readable summary. Do not change a rubric grade unless
   a rubric check itself names the same evidence.

## Verdict assembly

1. List every check with its `pass`, `fail`, or `unclear` grade and one-line
   evidence.
2. Apply the rubric rule mechanically: accept only if all required checks pass;
   any required fail rejects; any required unclear rejects for insufficient
   evidence. Preferred grades never change the verdict.
3. In the readable summary, name the first deciding required fail, or if none,
   the first deciding required unclear. Quote the evidence that decided it and
   state whether the repair is a plan correction or missing evidence.
4. Emit the required JSON with the same check order and grades. Confirm the
   final `verdict` matches step 2, make the JSON valid, place it in a fenced
   block, and emit nothing after it.


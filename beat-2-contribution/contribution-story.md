# Contribution: `verify_password` raises `UnknownHashError` on malformed stored hashes instead of returning False

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Sheldon Aristide
**GitHub Username:** Aristide021
**Issue:** https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72
**Pull Request:** https://github.com/codepath/pathreview-ai301-fa26-s3/pull/228

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: Rebuilt the issue-scout with a three-query strategy and ran one scan**

I migrated computers and lost my original scan files, so this entry describes the scout as I rebuilt and re-ran it on 2026-11-08; the earlier runs are not recoverable. I installed the Unit 5 starter over my Unit 1 skill, wrote three `gh search issues` queries from my fit profile (Python, Rust, C++) and four ranking filters, and the scan returned 300 issues and graded the top five: four accepted, one rejected because an open PR already fixed it. What fell short: three of the top five came from one repo, and the 60-day filter dropped almost nothing. My own reinstall also pasted my fit profile into a scope.md that held two conflicting claim rules, which the scout flagged; I repaired the file and re-ran the scan.

**Unit 6: Voice guard with Upstream rules, scope patch, and a repo memory file**

I added Upstream rules (claims, questions, pings, each with a wrong/right pair) to my voice guide, installed the guard hook, and checked that it fires: it stopped `gh issue comment --help` with "no body the guard can read", which is the pass. I patched the rubric's scope check to `scope-feasible` with a difficulty note, and wrote a short `CLAUDE.local.md` for the repo, kept out of git with `.git/info/exclude`. What broke: on Windows the guard could not find the `claude` CLI (it does not resolve `claude.cmd`), so its first manual check stopped; I changed the call to use `shutil.which`, and the same check then passed my posted claim.

**Unit 7: Ship-gate and manifest, run on my real branch until it passed**

My manifest lists the unit tests, ruff and mypy, with format, integration and frontend excluded with dated reasons. The gate stopped four times on `fix/72-malformed-hash`: forward-slash `.venv/Scripts/...` paths do not run in the Windows shell, so I rewrote them with backslashes; the gate could not find the `claude` CLI, which I patched as in Unit 6; my pr-precheck then rejected the branch because my PR description cites a macOS run while my re-run evidence was from Windows, which I fixed by putting both runs in the evidence file; last, a console encoding crash, fixed with UTF-8. The next run printed `ship-gate: PASS`; its one failing preferred check was that I never explained skipping black.

**Unit 8: PR monitor and a playbook written from a repo with no merge history**

The monitor read PR #228 as quiet for 25 days, five checks passing, mergeable, no reviews or comments, and drafted a polite nudge because my 14-day threshold was met. My playbook's staleness number is a guess: the repo has no merged PRs to measure (7 closed, none merged, and no maintainer comments on #72), and I wrote that into the playbook instead of inventing data. I did not send the draft: it is my one allowed nudge and the thread has no maintainer to address, so I am holding it. The monitor needed the same Windows `claude` lookup patch.

---

## Why I Chose This Issue

#72 is one of the seeded Path Review issues for the course (labels `bug`, `good first issue`, `api`, `tier-1`), so it did not come out of my open-web scan. When I rebuilt my scout on 2026-11-08 and scanned all of GitHub, it graded five other issues (repowise-dev/repowise#5362, #5363 and #5364, adhit-r/fairmind#369, and exmergo/dex#1206) and accepted four, but I had already built my fix and opened PR #228 against #72, so I graded #72 directly. The scout rejected it on `unclaimed` alone (29 open linked PRs and over 30 claim comments), so no difficulty note was produced; I chose it anyway because the course's Path Review house rule says classmates' claims do not block a seeded issue, and I took that rule over the scout. The issue fits me because it is one function with a stated expected result ("fail closed, return `False`") and a covering test already sitting behind a strict `xfail` marker, so I could prove the fix with that test instead of arguing for it. I wanted to learn how to turn a short issue into a small fix with evidence and a clear edge.

The thread already had several students on it, so I claimed on 2026-09-29 and said my first step was a reproduction report, not a fix. Because others had claimed first, I did my own setup and reproduction and built on my own branch rather than on anyone's PR; three PRs (#78, #91, #92) were already open. Claim comment: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72#issuecomment-5894732215

The scout did what its rubric says, and that is the problem for #72. It cannot tell a classroom issue with a house rule from a stranger's issue, so it rejects the one I chose for a reason the course says to ignore. I did not weaken `unclaimed`, because it correctly caught the dex issue whose open PR only the timeline showed; the change belongs in `scope.md` as a classroom-versus-wild switch, which I have not made. On the wild search itself, three of its top five came from one repo, and I would cap results per repo.

**Toolkit**

- Tool: issue-scout (my search strategy and rubric), run as `issue-scout: run a scan` and `issue-scout: grade <#72 URL>`, re-run on 2026-11-08
- What it produced: a scan of 300 issues with 4 accepts and 1 reject among the top five, and a reject on #72 for `unclaimed` (29 open linked PRs)
- Difficulties: three of five top results from one repo; the 60-day filter dropped almost nothing; my reinstall duplicated two conflicting scopes into `scope.md`
- What changed: replaced `scope-bounded` with `scope-feasible` and added a difficulty note (unit 6 patch); repaired `scope.md`; I would cap results per repo and add a classroom-versus-wild switch to `scope.md`

---

## Understanding the Issue

**Problem Description**

`verify_password()` in `core/security.py` hands the stored hash straight to passlib's `pwd_context.verify()`. When the stored value is not a hash passlib can verify, passlib raises instead of the function answering "no". A login check against a damaged or non-bcrypt stored value should fail closed, not throw.

**Expected Behavior**

`verify_password("password", <malformed stored hash>)` returns `False`, the same answer a wrong password gets.

**Current Behavior**

On `main` at `2f4e82f` it raises. `"not_a_valid_bcrypt_hash"` and `""` raise `passlib.exc.UnknownHashError`; a truncated hash like `$2b$12$abc` raises a plain `ValueError` ("salt too small"). A valid hash with a wrong password correctly returns `False`.

**Affected Components**

`core/security.py` (`verify_password`) and `tests/unit/test_security.py` (`test_verify_with_wrong_hash_format`, strict `xfail` for manifest id H-05).

**Toolkit**

- Tool: reading the issue, its test and `core/security.py`, with Claude Code in the repo
- What it produced: the four answers above, anchored to the xfail H-05 test and the call at `core/security.py:37`
- Difficulties: the issue names only `UnknownHashError`; my probe later showed other malformed shapes raise plain `ValueError`, so a first answer would have been incomplete
- What changed: Current Behavior covers all three exception types

---

## Reproduction Process

**Environment Setup**

macOS 26.5.2 on Apple Silicon. Python 3.11.16 in a fresh venv (`python3.11 -m venv .venv`); the machine's default `python3` is 3.9.6, below the project's `>=3.11`, which was the one setup snag. passlib 1.7.4, bcrypt 4.3.0, pytest 9.1.1, installed with `pip install -e ".[dev]"` (no lockfile, so this resolves to the newest allowed versions). Code: a fresh clone of my fork at `2f4e82f52efbcfcc57d65b3fa5348672163ca088`, the current `main`, no source changes, `.env.example` copied to `.env`. I did not run Docker, migrations, or the frontend, because `verify_password` touches none of them.

**Steps to Reproduce**

1. Run the covering test with the strict xfail ignored: `.venv/bin/pytest "tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format" --runxfail -v`
2. Call the function directly with a valid-hash control: `h = hash_password('password')`, then `verify_password('wrong', h)` and `verify_password('password', 'not_a_valid_bcrypt_hash')`.
3. Observed: the test FAILS with `passlib.exc.UnknownHashError: hash could not be identified`, raised from `core/security.py:37`. The control returns `False`.

**Reproduction Evidence**

- **Commit showing reproduction:** none of my own; the bug reproduces on unmodified `main`, commit https://github.com/Aristide021/pathreview-ai301-fa26-s3/commit/2f4e82f52efbcfcc57d65b3fa5348672163ca088 in my fork.
- **Screenshots/logs:** full commands and raw output are in my reproduction comment: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72#issuecomment-5894742223
- **My findings:** reproduced. A follow-up probe in the same checkout showed the exception type varies: `UnknownHashError` for `"not_a_valid_bcrypt_hash"` and `""`, plain `ValueError` for a truncated hash and for one with an invalid checksum, and `PasswordSizeError` for an overlong plaintext. So a fix that catches only `UnknownHashError` would miss two malformed shapes. I also saw an unrelated passlib/bcrypt 4.3.0 "trapped" `__about__` message on stderr; it is not part of this bug.

**Toolkit**

- Tool: repro-check, run on 2026-11-08 as `repro-check: grade my claim comment and my repro report` against my posted claim and reproduction comments (saved as claim-draft.md and repro-draft.md)
- What it produced: verdict accept, all nine checks pass including the control run, with one wording fix (a claim about step 1's stderr I could not check from the excerpt) and a warning that rahulkumargmu had already posted a near-identical reproduction on 2026-09-24
- Difficulties: the real-repo scope treats a duplicate report as a problem, while Path Review's house rule treats shared issues as normal
- What changed: nothing in the posted comments; I record the duplicate here. I would link the earlier report and name what mine adds

---

## Solution Approach

**Analysis**

`verify_password()` calls `pwd_context.verify(plain_password, hashed_password)` at `core/security.py:37` with no error handling. passlib identifies the scheme from the stored hash first; when it cannot (`UnknownHashError`, a `ValueError` subclass) or when bcrypt rejects the salt or checksum (`ValueError`), the exception escapes to the caller. An overlong plaintext raises `PasswordSizeError`, a `PasswordValueError` and so also a `ValueError`, from the same call.

**Proposed Solution**

Wrap that one call. Re-raise `PasswordValueError` so an invalid plaintext is not silently treated as a wrong password, and return `False` for any other `ValueError`, which covers both malformed-hash shapes. Not changing: `hash_password()`, the login route, dependency versions, logging, or handling of non-string stored values.

**Implementation Plan**

1. In `core/security.py`, wrap the `verify` call: re-raise `PasswordValueError`, return `False` on other `ValueError`; add the import.
2. In `tests/unit/test_security.py`, remove the H-05 `xfail` and parameterize `test_verify_with_wrong_hash_format` over four stored values (`not_a_valid_bcrypt_hash`, empty string, `$2b$12$abc`, a bcrypt-shaped string with an invalid checksum).
3. Add a control test that an overlong plaintext still raises.
4. Run the reproduction again, then `make test-unit`, `make lint`, `make typecheck`.

Deviation recorded during the build: the plan said the control asserts `PasswordValueError`; the test asserts `PasswordSizeError`, the subclass passlib actually raises, which pins the same boundary more narrowly. My plan comment on the issue: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72#issuecomment-6031288054

**Toolkit**

- Tool: plan-check, run on 2026-11-08 as `plan-check: grade my plan in plan.md and draft comment in plan-comment.md`, using my posted plan comment as the draft
- What it produced: verdict reject, seven of eight checks pass; it failed `thread-aligned` because AdrianOlm1 and PRs #78, #91 and #92 had posted plans earlier and my comment cited the Path Review house rule, which the installed real-repo scope says does not travel
- Difficulties: the scope for real repos and the classroom house rule conflict, and the procedure does not say which wins; a first rewrite of my comment also failed for dropping the acknowledgement of other PRs
- What changed: I restored the posted comment and added the expected results to `plan.md`; I did not change the scope, so the reject stands and I record it here

---

## Testing Strategy

Before: `pytest ...::test_verify_with_wrong_hash_format --runxfail` on `2f4e82f` fails with `UnknownHashError` (reproduction above). After, on branch `fix/72-malformed-hash` (commit `1f52e98`): the same command passes for all four malformed stored values, and the direct call that used to raise prints `target: False`, while the valid-hash control still returns `False` for a wrong password. The new control test confirms an overlong plaintext still raises `PasswordSizeError`. Repo checks on the branch: `make test-unit` 380 passed, 52 xfailed; `make lint` passed; `make typecheck` no issues in 76 source files; `make test-integration` ran no tests because the project has none yet. CI on PR #228: all five jobs (lint, typecheck, test-unit, test-integration, frontend) passed.

**Toolkit**

- Tool: pytest, ruff and mypy from the project's `make` targets, run again by the ship-gate from my manifest on 2026-11-08
- What it produced: on Windows, the H-05 test fails before with `UnknownHashError` and 4 pass after; the unit suite gives 380 passed and 52 xfailed, ruff is clean, mypy is clean on 76 files, matching the numbers in my PR
- Difficulties: my PR description carries macOS output and the re-run is Windows, so warning counts and timings differ
- What changed: `test_evidence.md` holds both runs, labelled

---

## Implementation Notes

**2026-09-29**

Claimed #72 and posted the reproduction on `2f4e82f`. I tested only one malformed string and said so in the report.

**2026-10-07**

Ran a wider probe (empty string, truncated hash, invalid checksum, overlong plaintext) because one value was not enough to choose what to catch. `UnknownHashError` and the bcrypt errors are all `ValueError`s, but so is `PasswordSizeError`, which is why a bare `except ValueError` was not safe. Posted the plan comment and committed the fix on `fix/72-malformed-hash` (`1f52e98`).

**2026-10-14**

Opened PR #228 against `codepath/pathreview-ai301-fa26-s3`. All five CI jobs passed.

**2026-11-08**

I migrated computers and lost my local files, so I rebuilt this story from the issue thread, my PR and my fork. I reinstalled the tools, re-ran them against the same branch, and wrote the Toolkit sections from those runs, not from memory. Everything dated 2026-11-08 in the Toolkit is a re-run; the originals are gone. The re-runs did turn up problems. The ship-gate rejected my branch until my evidence file matched my PR description's platform. Plan-check rejected my plan comment because the real-repo scope disagrees with the classroom rule. I patched three tools so they could find the `claude` CLI on Windows.

**Toolkit**

- Tool: git, the GitHub issue and PR threads, and my dated notes
- What it produced: the dated entries above
- Difficulties: my original local notes are lost, so the first three entries come from the issue thread and PR timestamps
- What changed: nothing; I would keep working notes committed in my course repo

---

## Pull Request

**PR Link:** https://github.com/codepath/pathreview-ai301-fa26-s3/pull/228

**PR Description:** Wraps the `pwd_context.verify()` call in `verify_password()` so malformed stored hashes return `False`, re-raises `PasswordValueError` so an overlong plaintext still raises, removes the H-05 `xfail`, parameterizes the malformed-hash test over four values, and adds an overlong-plaintext control test. The description carries before/after output, the passing repo checks, `Closes #72`, a scope statement, an open question for maintainers about whether overlong plaintext should also return `False`, and a disclosure of how AI assistants helped me.

**Maintainer Feedback:**

- 2026-11-08: none yet.

**Status:** Awaiting review

**Toolkit**

- Tool: ship-gate (my manifest of unit tests, ruff and mypy, then pr-precheck), run by hand on 2026-11-08 on `fix/72-malformed-hash`
- What it produced: `ship-gate: PASS` after four stops, with pr-precheck accepting; its one failing preferred check was that I never reported skipping `make format`, and its voice notes said my AI-disclosure bullet was not required by this repo
- Difficulties: Windows path separators, the `claude` lookup, a mismatch between my PR's macOS output and my Windows evidence, and a console encoding crash
- What changed: manifest paths, an evidence file with both runs, and local patches to the gate; PR #228 was already open and I did not edit its description

---

## Learnings and Reflections

**Technical Skills Gained**

How passlib's exceptions are layered (`UnknownHashError`, bcrypt's `ValueError`s, and `PasswordSizeError` all share `ValueError`), why a broad catch needs an explicit boundary test, and how a strict `xfail` plus `--runxfail` exposes the real failure.

**Challenges Overcome**

Several students had claimed the same seeded issue and three PRs already existed, so I kept to my own reproduction, plan, and branch, and said so in the plan comment. My first report tested one malformed string; the follow-up probe showed that was not enough and changed the fix from catching `UnknownHashError` to a bounded `ValueError` catch.

**What I'd Do Differently Next Time**

Probe more than one failing input before writing the plan, and keep my tool outputs (scans, plans, evidence) in a repo instead of only on one laptop. I would also settle which scope rule governs a classroom issue before running three tools that each read it differently.

---

## Resources Used

- `docs/SETUP.md` in the Path Review repo, for environment setup.
- Issue #72's thread (https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72), where other students' reproduction reports let me check mine against theirs.
- passlib 1.7.4 `context.py` (the traceback frames in my reproduction), for how the scheme is identified before verification.

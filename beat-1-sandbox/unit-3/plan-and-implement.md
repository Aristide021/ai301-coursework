# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

---

## Posted upstream

**GitHub username**

Aristide021

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72#issuecomment-6031288054

````markdown
Plan for #72, built from my reproduction above at `2f4e82f`.

The H-05 run under `--runxfail` reached the unguarded call in
`core/security.py:37` and raised:

```text
passlib.exc.UnknownHashError: hash could not be identified
```

The valid-hash/wrong-password control returned `False`, so the reproduced fault
is specific to a stored value passlib cannot verify. I ran a follow-up probe in
the same clean `2f4e82f` checkout with Python 3.11.16, passlib 1.7.4, and bcrypt
4.3.0 because my posted report covered only one malformed value. It produced
`UnknownHashError` for `"not_a_valid_bcrypt_hash"` and `""`, but plain
`ValueError` for a truncated bcrypt hash and one with an invalid checksum. An
overlong plaintext instead raised `PasswordSizeError`, which is a
`PasswordValueError` and also a `ValueError`.

My plan is one bounded change in `verify_password()`: preserve and re-raise
`PasswordValueError`, then return `False` for other `ValueError` instances from
the verify call. That covers both malformed stored-hash shapes I observed
without silently changing the current overlong-plaintext behavior.

I will remove the strict H-05 `xfail`, parameterize the malformed-hash test over
the four values I ran, and add the overlong-plaintext control. I am not changing
`hash_password()`, the login route, dependency versions, logging, or handling
for non-string stored values.

For verification, I will re-run the posted H-05 command and the direct probe.
After the change, all four malformed stored values must return `False`; the
valid-hash controls must remain `True`/`False`; the overlong plaintext must
still raise `PasswordSizeError`. Then I will run the complete security test
module, `make check`, and `make test-unit`.

PRs #78, #91, and #92 are already open for this seeded issue. Per the course
house rule, this is my own plan from my own reproduction, and I will build it on
my own branch rather than piggyback another student's work.
````

---

## Your branch

**Branch**

`fix/72-malformed-hash`

**Evidence**

The Unit 2 reproduction steps (https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72#issuecomment-5894742223), re-run against the built change. Branch `fix/72-malformed-hash`, commit `1f52e98`, on top of `2f4e82f52efbcfcc57d65b3fa5348672163ca088`. Same machine and venv as the reproduction: macOS 26.5.2, Python 3.11.16, passlib 1.7.4, bcrypt 4.3.0, pytest 9.1.1.

*Before* (`2f4e82f`, copied from the posted reproduction comment):

Step 1 (excerpt, lines I trimmed are marked `...`):

```
tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format FAILED [100%]
...
tests/unit/test_security.py:227: 
core/security.py:37: in verify_password
    return bool(pwd_context.verify(plain_password, hashed_password))
...
.venv/lib/python3.11/site-packages/passlib/context.py:1132: UnknownHashError
E           passlib.exc.UnknownHashError: hash could not be identified
======================== 1 failed, 2 warnings in 2.01s =========================
```

Step 2, in full (stdout and stderr together, unbuffered):

```
(trapped) error reading bcrypt version
Traceback (most recent call last):
  File ".../passlib/handlers/bcrypt.py", line 620, in _load_backend_mixin
    version = _bcrypt.__about__.__version__
              ^^^^^^^^^^^^^^^^^
AttributeError: module 'bcrypt' has no attribute '__about__'
control, valid hash + wrong password: False
Traceback (most recent call last):
  File "<string>", line 5, in <module>
  File ".../core/security.py", line 37, in verify_password
    return bool(pwd_context.verify(plain_password, hashed_password))
  ...
  File ".../passlib/context.py", line 1132, in identify_record
    raise exc.UnknownHashError("hash could not be identified")
passlib.exc.UnknownHashError: hash could not be identified
```

*After* (the branch). Step 1, the same H-05 command, with the session header trimmed. The test now covers four stored values:

```
$ .venv/bin/pytest "tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format" --runxfail -v
platform darwin -- Python 3.11.16, pytest-9.1.1, pluggy-1.6.0 -- /Users/sheldon/Documents/GitHub/pathreview-ai301-fa26-s3/.venv/bin/python
plugins: hypothesis-6.168.3, cov-7.1.0, asyncio-1.4.0, benchmark-5.3.0, pytest_httpserver-1.1.5, platformdirs-4.12.1, anyio-4.15.1
collecting ... collected 4 items
tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format[not_a_valid_bcrypt_hash] PASSED [ 25%]
tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format[] PASSED [ 50%]
tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format[$2b$12$abc] PASSED [ 75%]
tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format[$2b$12$!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!] PASSED [100%]
  /Users/sheldon/Documents/GitHub/pathreview-ai301-fa26-s3/core/config.py:7: PydanticDeprecatedSince20: Support for class-based `config` is deprecated, use ConfigDict instead. Deprecated in Pydantic V2.0 to be removed in V3.0. See Pydantic V2 Migration Guide at https://errors.pydantic.dev/2.13/migration/
======================== 4 passed, 2 warnings in 0.27s =========================
```

Step 2, the same direct call with the control:

```
(trapped) error reading bcrypt version
Traceback (most recent call last):
  File "/Users/sheldon/Documents/GitHub/pathreview-ai301-fa26-s3/.venv/lib/python3.11/site-packages/passlib/handlers/bcrypt.py", line 620, in _load_backend_mixin
    version = _bcrypt.__about__.__version__
              ^^^^^^^^^^^^^^^^^
AttributeError: module 'bcrypt' has no attribute '__about__'
control, valid hash + wrong password: False
target: False
```

The `(trapped) error reading bcrypt version` block is the same unrelated passlib/bcrypt message noted in the Unit 2 report. It does not affect the result.

The extended probe from the plan, with all four stored values and the overlong-plaintext control:

```
(trapped) error reading bcrypt version
Traceback (most recent call last):
  File "/Users/sheldon/Documents/GitHub/pathreview-ai301-fa26-s3/.venv/lib/python3.11/site-packages/passlib/handlers/bcrypt.py", line 620, in _load_backend_mixin
    version = _bcrypt.__about__.__version__
              ^^^^^^^^^^^^^^^^^
AttributeError: module 'bcrypt' has no attribute '__about__'
control, right password -> True
control, wrong password -> False
'not_a_valid_bcrypt_hash'                                      -> False
''                                                             -> False
'$2b$12$abc'                                                   -> False
'$2b$12$!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!' -> False
long plaintext -> passlib.exc.PasswordSizeError: password exceeds maximum allowed size
```

The wider checks the plan listed:

```
$ .venv/bin/pytest tests/unit/test_security.py -q

-- Docs: https://docs.pytest.org/en/stable/how-to/capture-warnings.html
29 passed, 2 warnings in 6.99s

$ make lint
.venv/bin/ruff check .
All checks passed!

$ .venv/bin/black --check .
All done! ✨ 🍰 ✨
110 files would be left unchanged.

$ make typecheck
.venv/bin/mypy api/ core/ ingestion/ rag/ agent/ safety/
Success: no issues found in 76 source files

$ make test-unit
-- Docs: https://docs.pytest.org/en/stable/how-to/capture-warnings.html
================= 380 passed, 52 xfailed, 4 warnings in 8.09s ==================
```

Result: all four malformed stored values return `False`, the valid-hash controls are unchanged (`True` for the right password, `False` for the wrong one), and an overlong plaintext still raises `PasswordSizeError` instead of returning `False`.

## Eval iterations

**Run history**

One run:

1. Full run, 20 packages: **20/20**, bar PASS, all five categories matched (`clear-accept 7/7`, `scope-creep 4/4`, `thread-convention 2/2`, `unbuildable 3/3`, `wrong-cause 4/4`). This is the run in `eval-run.txt`.

There was no `--only` loop and no revision to the rubric, evidence guide or procedure before or after it. The skill and this run came out of a single working session, so I have no calibration history to report. After the run I regraded two packages (`pkg-20` and `pkg-04`) with `--out` to get per-check grades, using the same installed files. That was a partial run and is not part of the history above. It is where the Package analysis below gets its per-check detail.

**Package analysis**

`pkg-20` (ghostty-org/ghostty#11261, category `thread-convention`). My rubric decided `reject` and the gold label is `reject`. The gold note reads: "excellent bounded plan that follows the thread's direction, but the comment contains no AI-use disclosure and ghostty's stated policy requires disclosing all AI usage; every package here is treated as AI-assisted work"

The committed run does not show why, because the harness prints failed checks only for disagreements and all 20 agreed. The regrade does. It graded `pkg-20` `reject` again, with seven of the eight checks passing and `conventions-respected` failing. The evidence it cited for that failure: Repo facts require 'All AI usage in any form must be disclosed, stating the tool used and the extent of the assistance' for 'AI-assisted issues and comments'; the candidate plan comment contains no disclosure statement.

That fits the repo-facts line for the package: "All AI usage in any form must be disclosed, stating the tool used and the extent of the assistance," with "AI-assisted issues and comments" named. The policy reaches comments and asks for an affirmative statement, and the plan comment has none. The plan itself is sound on diagnosis, scope, approach, test, uncertainty and thread alignment, so nothing but the conventions check can reject it.

The same check let two other packages through, and the verdict rule shows why that is a real test of it. `pkg-09` (sharkdp/fd#2067) has a policy that requires stating the tool and extent "in the pull request" and no disclosure ask for issue comments. `pkg-03` (BurntSushi/ripgrep#3222) asks only that comments be "written by humans in their own words." Both are gold accepts in repos with AI policies, and both were accepted in the full run. `conventions-respected` is required, so an accept means it passed them. A rule that rejected every undisclosed comment in a repo with an AI policy would have caught `pkg-20` and failed both.

`pkg-04`, the other `thread-convention` package, is rejected in a completely different way. It fails five required checks (diagnosis-grounded, scope-bounded, test-decisive, uncertainty-honest, thread-aligned) and passes `conventions-respected`. The category is therefore two different failures. One is a good plan with a policy miss, the other a plan that ignores the maintainer's in-flight code fix.

**Check rationale**

From the `rubric.md` uploaded to `tools/plan-check/`, the `conventions-respected` pass condition as it now reads:

> The comment satisfies requirements the repository states for issue comments. Read scope before content: a disclosure duty limited to pull requests does not govern an issue comment, while a policy covering all AI use or AI-assisted comments requires the stated disclosure here. A human-voice-only rule is satisfied by a specific comment written in the contributor's own words. Silence in repo facts imposes no extra requirement. Fail when a stated comment-level disclosure or participation rule is unmet.

I did not revise this check in this unit. Its wording carries over the scope-first reading I arrived at in Unit 2, where the same check passed `pkg-20` by accident because it made the duty depend on whether AI had been used, a fact a grader cannot see in the package. I rejected that conditional wording again here. The check asks what the policy requires of an issue comment and whether the package contains it, so a pull-request-only duty does not govern a comment, a human-voice-only rule is met by a human-voiced comment, and silence imposes nothing.

**Trade-offs**

What the check gives up: it reads the comment against the policy text in the repo-facts block and nothing else. A rule stated somewhere the block does not quote, or a comment written by an assistant in a repo that asks only for a human voice, passes. The check can see what a comment contains. It cannot see who wrote it.

What I cannot say about it: I ran the full set once. In Unit 2 the same kind of rubric graded one package differently on two runs with no edit in between (`pkg-09`), so one 20/20 is a single measurement and says nothing about grader variance. Because nothing was revised, I had no changed check to protect with canaries, and I ran none.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.

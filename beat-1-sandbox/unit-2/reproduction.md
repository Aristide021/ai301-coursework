# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

---

## Your identity upstream

**GitHub username**

Aristide021

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72#issuecomment-5894732215

````markdown
Claiming #72. I've started a local run of the H-05 test against `2f4e82f` and will post the environment, the exact commands and the raw output as a separate comment, whatever the result is.

After that I'll look at where `UnknownHashError` leaves `core/security.py`. I'm not assuming a fix before I've read that path.
````

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72#issuecomment-5894742223

````markdown
Reproduction report for #72. **Result: reproduced** on current `main`.

## Environment

- Code: fresh clone of my fork at `2f4e82f52efbcfcc57d65b3fa5348672163ca088` (current `main`), no local source changes. I copied `.env.example` to `.env` unchanged.
- macOS 26.5.2 (Apple Silicon)
- Python 3.11.16 in a fresh venv (`python3.11 -m venv .venv`; the machine's default `python3` is 3.9.6, below the project's `>=3.11`)
- passlib 1.7.4, bcrypt 4.3.0, pytest 9.1.1
- Installed with `pip install -e ".[dev]"`, which resolves to the newest versions the project's ranges allow. There is no lockfile.

## Steps

1. Run the test the issue points at. It is marked `xfail(strict=True)` against H-05, so I passed `--runxfail` to see the real outcome:

```
.venv/bin/pytest "tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format" --runxfail -v
```

2. Call the function directly, with a control (a valid bcrypt hash and the wrong password):

```
.venv/bin/python -u -c "
from core.security import verify_password, hash_password
h = hash_password('password')
print('control, valid hash + wrong password:', verify_password('wrong', h))
print('target:', verify_password('password', 'not_a_valid_bcrypt_hash'))
"
```

## Observed

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

## Expected vs actual

Expected (per the issue and the test's own comment): `verify_password("password", "not_a_valid_bcrypt_hash")` returns `False`.

Actual: it raises `passlib.exc.UnknownHashError` out of `core/security.py:37`. The control returns `False`, so the valid-hash path behaves as expected.

## One unrelated message

The `(trapped) error reading bcrypt version` block in step 2 is not the bug. Observed: it is logged once, before the control line, and execution continues past it; it does not appear in the step 1 run. My guess, which I did not test, is that it comes from passlib 1.7.4 looking for `bcrypt.__about__` on bcrypt 4.3.0, and that step 2 shows it because `hash_password` is the first call to load the bcrypt backend.

## What I did not do

- I did not run `make setup`, Docker, the migrations or the frontend. `verify_password` doesn't touch any of them, and I ran only the one test plus the direct call.
- I tested one malformed string. I did not try an empty string, `None`, or a valid hash from a different scheme.
- I have not traced where the exception goes after it leaves `verify_password`, so I'm not saying anything about the right place to catch it.
````

## Eval iterations

**Run history**

Four runs, in order:

1. Full run, 20 packages: **15/20**, below the bar. The category floor was met (`disclosure 1/1`). Every reject category was perfect (`wrong-target 4/4`, `no-evidence 4/4`, `unfollowable-comms 3/3`), so the whole deficit was `clear-accept 3/8`: five gold accepts (`pkg-03`, `pkg-05`, `pkg-09`, `pkg-10`, `pkg-12`) graded `reject`, and no error in the other direction.
2. Full run, 20 packages: **16/20**, after loosening `steps-followable`, `behavior-matches` and `claims-backed`. `pkg-05` and `pkg-12` flipped to agree and `clear-accept` reached 5/8. `pkg-20` went the other way, from `reject` to `accept`, and the floor went unmet (`disclosure 0/1`). Package analysis covers it.
3. `--only pkg-03,pkg-09,pkg-10,pkg-20,pkg-05,pkg-07,pkg-12,pkg-04,pkg-13,pkg-02`: **10/10**. Before this run I tightened `conventions-respected` and loosened `claims-backed` and `behavior-matches` a second time. Four targets (`pkg-03`, `pkg-09`, `pkg-10`, `pkg-20`) and six canaries: `pkg-05`, `pkg-07` and `pkg-12`, clear-accepts in repos with AI policies, for the tightened check; `pkg-04` and `pkg-13` for `no-evidence`; `pkg-02` for `wrong-target`. This is the only run whose per-check grades I saved (`--out results.json`). A partial run can't write `eval-run.txt`, so the committed file was untouched.
4. Full run, 20 packages: **19/20**, bar PASS, all five categories matched. The rubric had not been edited since before run 3. This is the run in `eval-run.txt`.

**Package analysis**

`pkg-20` (ghostty-org/ghostty#13604). The rubric in this submission grades it `reject` and the gold label is `reject`. That was not always true, and the sequence is why I picked it.

Run 1 rejected `pkg-20` and reported `disclosure 1/1`, so I took the floor as met. Between runs 1 and 2 I loosened `steps-followable`, `behavior-matches` and `claims-backed`. On run 2 `pkg-20` flipped to `accept` and `disclosure` fell to 0/1. The harness prints failed checks only where a verdict disagrees with gold, and I saved no per-check output for runs 1 or 2, so the record does not show which check rejected `pkg-20` on run 1. The flip shows the rejection depended on something I changed between the runs. My guess, which I did not verify, is that the loosened proof checks were doing it, since the gold note calls the package an "excellent repro on every proof check."

`conventions-respected` is required and its wording was identical in runs 1 and 2. Run 2 accepted `pkg-20`, so the check passed under that wording. It most likely passed on run 1 as well, though `pkg-09` later showed the grader can vary between runs. If so, `disclosure 1/1` on run 1 was a match by accident.

The old wording made the duty conditional on AI having been used, and nothing in the package says whether it was. The model reasonably found no duty triggered. The eval set separates repos by what each policy asks of a comment:

- `pkg-20` (gold reject): "All AI usage in any form must be disclosed... AI-assisted issues and comments must be reviewed and edited by a human." It reaches comments.
- `pkg-09` (gold accept): "must state the tool and the extent of its use in the pull request... the policy states no disclosure ask for issue comments." It reaches pull requests only.
- `pkg-03` (gold accept): "comments to maintainers must be written by humans in their own words." It asks for a human voice, not disclosure.

After the revision, run 3's saved per-check grades show `pkg-20` failing `conventions-respected` and passing the other eight checks. The rubric was not edited between runs 3 and 4, and run 4 returned `reject` and `disclosure 1/1`. I did not save run 4's per-check grades, so run 4 alone does not show which check rejected `pkg-20`. I am relying on the run 3 output for that.

**Check rationale**

From the `rubric.md` uploaded to `tools/repro-check/`, the `conventions-respected` pass condition as it now reads:

> The comments satisfy the requirements the repo states **of comments**. Read the policy for
> its scope before its content: a disclosure duty written for pull requests ("state the tool
> and extent in the pull request") says nothing about an issue comment, and a policy asking
> only that comments be written by a human in their own words is satisfied by a human-voiced
> comment that never mentions tooling. Where the policy does reach these artifacts -- "all AI
> usage in any form must be disclosed", "AI-assisted issues and comments" -- it demands an
> affirmative statement, and comments containing no such statement do not satisfy it. The
> question is not whether you can tell an assistant was used; it is whether the package
> contains the disclosure the repo asks for. If the repo's bug-report template names required
> facts, the report supplies the ones this issue's behavior depends on. Silence passes: a repo
> that states no policy imposes no requirement. Fail when a stated requirement goes unmet,
> however good the proof is -- a report that cannot be accepted on the repo's own terms is not
> ready to post.

It previously read "If the contribution policy requires disclosing AI assistance, the comments disclose it." That made the duty depend on whether an assistant was used, which a grader cannot see in the package. Run 2 accepted `pkg-20` under that wording.

I rejected the obvious repair: fail any undisclosed comment in a repo that has an AI policy. That rule catches `pkg-20` and also fails four gold accepts, `pkg-03`, `pkg-05`, `pkg-09` and `pkg-12`, whose repos have AI policies and whose comments never mention AI. (`pkg-07` also has a policy, but its comment discloses, so the rule would not touch it.) The check now reads what the policy asks of a comment first. Where the policy demands a statement, the missing statement is the failure. That is visible in the package. The contributor's tooling is not.

**Trade-offs**

The weakest check is `claims-backed`, and the run history shows where it gives out. Its clause says "A repeated or varied run needs its own artifact only when it carries weight the shown artifact does not." `pkg-09` sits on that line. Its report says "I ran this 5 times and also re-ran with the second command's arguments padded," and pastes one `order.log`. Was the padded re-run a repeat of the shown outcome, or an attempt to produce a different one?

The same rubric answered both ways. Run 3's saved output shows `claims-backed` passing `pkg-09` as "corroborating detail no stronger than the shown result." Run 4, with no rubric edit in between, failed `pkg-09` on `claims-backed` and `control-run`, and that is the whole 19/20. Nine of the ten packages runs 3 and 4 share kept their verdict, and `pkg-09` is the exception. That shows the grader is unstable on this package, and one package is all I can say it about. I did not save run 4's per-check grades, so I can't say whether the other checks held steady.

What the check gives up: a report can claim a variation it never shows, and whether my rubric catches it depends on how the claim is phrased. I accept that instead of tightening, because a strict `claims-backed` caused the run 1 misses. It failed three gold accepts (`pkg-03`, `pkg-09`, `pkg-10`). The loosening does not rescue calib-03's "I ran the command ten times with identical results." I graded that package with the final rubric (`--include-calibration --only calib-03`, never scored): `reject`, matching gold. `claims-backed` still fails it, because the ten runs and the run on the reporter's version have no artifacts and the second version is a configuration that carries real weight. `behavior-matches`, `input-matches`, `outcome-honest` and `claim-grounded` fail it too.

What did not change elsewhere: after run 2 I tightened `conventions-respected` and loosened `claims-backed` and `behavior-matches` again, so run 3 carried a canary for each. `pkg-04` and `pkg-13` covered `no-evidence`, `pkg-02` covered `wrong-target`, and `pkg-05`, `pkg-07` and `pkg-12` covered clear-accepts in repos with AI policies, which the tightened check could newly have failed. All six held and all four targets flipped to agree. `steps-followable` was not touched after run 2. The final full run kept `no-evidence 4/4`, `wrong-target 4/4`, `unfollowable-comms 3/3` and `disclosure 1/1`.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.

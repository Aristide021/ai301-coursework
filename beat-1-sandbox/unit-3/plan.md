# Plan for issue #72

Issue: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72

## Diagnosis

`verify_password()` delegates directly to `pwd_context.verify()` in
`core/security.py`. The posted reproduction ran the issue's H-05 test with
`--runxfail` and recorded the exception crossing that boundary:

```text
core/security.py:37: in verify_password
    return bool(pwd_context.verify(plain_password, hashed_password))
...
.venv/lib/python3.11/site-packages/passlib/context.py:1132: UnknownHashError
E           passlib.exc.UnknownHashError: hash could not be identified
```

The same report's control, a wrong password against a valid bcrypt hash,
returned `False`. That isolates the reproduced failure to a stored value
passlib cannot verify, rather than the ordinary password-mismatch path.

I ran a follow-up probe in the same checkout and environment because the
posted report tested only one malformed string. It showed two exception shapes:

```text
'not_a_valid_bcrypt_hash' -> passlib.exc.UnknownHashError: hash could not be identified
'' -> passlib.exc.UnknownHashError: hash could not be identified
'$2b$12$abc' -> builtins.ValueError: salt too small (bcrypt requires exactly 22 chars)
'$2b$12$!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!' -> builtins.ValueError: invalid characters in bcrypt checksum
long-plain -> passlib.exc.PasswordSizeError: password exceeds maximum allowed size
```

`UnknownHashError` is a `ValueError`, but `PasswordSizeError` is also a
`ValueError` through `PasswordValueError`. Catching every `ValueError` without
distinction would therefore make an overlong plaintext return `False` as an
unintended side effect. The bounded fix is to preserve `PasswordValueError`
while failing closed on other `ValueError` instances raised while parsing or
verifying the stored hash.

## Scope

In scope:

- Add exception handling around the one `pwd_context.verify()` call in
  `core/security.py`.
- Remove the strict H-05 `xfail` marker from the existing malformed-hash test.
- Extend that test to cover the independently observed unrecognized, empty,
  truncated-bcrypt, and invalid-bcrypt-checksum stored values.
- Add a control asserting that an overlong plaintext still raises
  `PasswordValueError`, which pins the boundary of the broader `ValueError`
  catch.

Out of scope:

- `hash_password()`, JWT handling, and the login route.
- Dependency changes for the unrelated passlib/bcrypt version-probe warning.
- Non-string stored values; the function and database model type this value as
  `str`, and the reproduction did not test a type violation.
- Logging corrupted stored hashes or changing authentication error messages.

Three classmate PRs (#78, #91, and #92) are open for this seeded issue. Under
the Path Review house rule, I will build my own branch from my own reproduction
rather than copy or race their implementations.

## Files

- `core/security.py`: import the passlib password-value exception type and
  make malformed stored hashes fail closed.
- `tests/unit/test_security.py`: remove the H-05 `xfail`, add malformed stored
  hash cases, and preserve the overlong-plaintext control.

## Approach

1. Import `PasswordValueError` from `passlib.exc`.
2. Wrap `pwd_context.verify()` in `verify_password()`:
   - re-raise `PasswordValueError`, preserving the existing response to invalid
     plaintext inputs;
   - return `False` for any other `ValueError`, covering both the observed
     `UnknownHashError` and malformed bcrypt strings that reach the handler but
     fail validation;
   - leave non-`ValueError` exceptions untouched so infrastructure or coding
     faults are not converted into an authentication mismatch.
3. Remove the strict H-05 marker. Parameterize the existing malformed-hash
   assertion with all four observed stored strings.
4. Add the overlong-plaintext control so future changes cannot accidentally
   broaden the fail-closed boundary without an explicit test change.

## Test plan

Before the fix, preserve the posted reproduction output: the H-05 test under
`--runxfail` fails with `UnknownHashError`; the direct control returns `False`.
Also save the follow-up four-value probe above, where two values raise
`UnknownHashError` and two raise plain `ValueError`.

After the fix:

1. Run the H-05 test without `--runxfail`. Expect every parameterized malformed
   stored hash to return `False` and the test to pass with no xfail marker.
2. Re-run it with `--runxfail` as a direct comparison with the posted command.
   Expect the same pass and no exception at `core/security.py`.
3. Re-run the direct probe. Expect `False` for all four malformed stored values;
   keep the valid-hash controls at `True` for the correct password and `False`
   for the wrong password; expect the overlong plaintext to continue raising
   `PasswordSizeError`.
4. Run the complete security unit-test module.
5. Run the repository-required checks: `make check` and `make test-unit`.

## Risks and unknowns

- Passlib may expose other `ValueError` subclasses for malformed stored hashes;
  the implementation intentionally catches them unless they are
  `PasswordValueError`. The tests pin only the four values actually run.
- `PasswordValueError` is the boundary chosen to preserve current plaintext
  validation behavior. If maintainers want every unverifiable attempt,
  including an overlong plaintext, to return `False`, they can ask for the
  simpler broad catch; this plan does not assume that policy change.
- The malformed-hash path is security-sensitive. The change fails closed, but
  it deliberately does not hide exception families other than `ValueError`.

## Deviations

The build followed the posted plan. The exception boundary, four malformed
stored-hash cases, overlong-plaintext control, and verification commands were
implemented and run as described; no files or behavior were added to scope.

One difference of detail: Scope says the control asserts `PasswordValueError`.
The test asserts `PasswordSizeError`, the subclass passlib actually raises for
an overlong plaintext. It pins the same boundary more narrowly. The first
version of this section said there were no differences at all; that was
imprecise.

# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72

**Verdict output**

Live-mode run over three Path Review candidates, verbatim, produced by the same installed
skill whose files are in `tools/issue-select/`. All three are accepted. The chosen issue,
#72, records `"verdict": "accept"`.

The read-out ranks #69 first on domain fit and #72 second. I chose #72 anyway; the reasoning
is in the Selection rationale below. The ranking is advice from the fit profile, not part of
the verdict -- `scope.md` orders accepted candidates and never changes an accept or reject.

## Summary

**Repo-level facts** (apply to all three): not archived, last commit 2026-09-16 (4 days ago, human-authored by collaborator Aburke225) → repo-alive passes. No `CONTRIBUTING.md`/`AI_POLICY.md`/`AGENTS.md` statement restricting AI-assisted work (silence) → ai-policy-permits passes. All three issues were filed by `Aburke225`, whose `author_association` is `COLLABORATOR`, with specific file pointers and diagnosed root causes → maintainer-engaged (preferred) passes for all. All three carry `good first issue` + `tier-1` labels → newcomer-signposted (preferred) passes for all.

All three issues **accept**: each is a single, bounded, collaborator-diagnosed defect with named files and an effort estimate, no assignee, no linked/mentioned PR. Issue #69 has a same-day claim comment from a fellow student (`Yina-Mu`, association `NONE`) — under this repo's house rule, classmate claim comments don't block, so it still passes `unclaimed`.

**Ranked by your fit profile** (systems/Python debugging, AI/agent tooling, prefer real logic work over copy-only edits):

1. **#69** — output parser crashes on a top-level JSON array (`AttributeError` on `.items()`) in `rag/generator/output_parser.py`. Real debugging in the agent/RAG stack, closest to your stated AI-tooling interest, 2–4h.
2. **#72** — `verify_password` lets `UnknownHashError` escape instead of failing closed. Genuine control-flow/exception-handling bug in `core/security.py`, 1–2h.
3. **#73** — README and `.env.example` disagree on an env var name. Valid and accepted, but it's a two-file documentation/config consistency edit with no code logic — closest to the "cosmetic or copy-only" pattern you said you'd rather avoid.

```json
[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/69",
    "checks": [
      {"name": "repo-alive", "grade": "pass", "evidence": "Not archived; most recent default-branch commit 2026-09-16 by human collaborator Aburke225"},
      {"name": "issue-grounded", "grade": "pass", "evidence": "Reports an AttributeError against a stated expected behavior ('fallback path should handle array responses'), filed by COLLABORATOR Aburke225"},
      {"name": "scope-bounded", "grade": "pass", "evidence": "Single defect, 2 named files, estimated 2-4 hours, no sub-items or umbrella structure"},
      {"name": "unclaimed", "grade": "pass", "evidence": "No assignees, no linked PRs; one same-day claim comment from student Yina-Mu (association NONE) — house rule: classmate claims don't block"},
      {"name": "ai-policy-permits", "grade": "pass", "evidence": "No CONTRIBUTING.md/AI_POLICY.md/AGENTS.md statement restricting AI-assisted contributions"},
      {"name": "maintainer-engaged", "grade": "pass", "evidence": "Filed by COLLABORATOR Aburke225 with diagnosed root cause and exact crash trace"},
      {"name": "newcomer-signposted", "grade": "pass", "evidence": "Labeled 'good first issue' and 'tier-1'"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72",
    "checks": [
      {"name": "repo-alive", "grade": "pass", "evidence": "Not archived; most recent default-branch commit 2026-09-16 by human collaborator Aburke225"},
      {"name": "issue-grounded", "grade": "pass", "evidence": "Reports that verify_password should fail closed rather than raise, an observable defect against stated correct behavior, filed by COLLABORATOR Aburke225"},
      {"name": "scope-bounded", "grade": "pass", "evidence": "Single defect, 2 named files, estimated 1-2 hours, no sub-items or umbrella structure"},
      {"name": "unclaimed", "grade": "pass", "evidence": "No assignees, no comments, no linked PRs"},
      {"name": "ai-policy-permits", "grade": "pass", "evidence": "No CONTRIBUTING.md/AI_POLICY.md/AGENTS.md statement restricting AI-assisted contributions"},
      {"name": "maintainer-engaged", "grade": "pass", "evidence": "Filed by COLLABORATOR Aburke225 with diagnosed root cause and specific fix direction"},
      {"name": "newcomer-signposted", "grade": "pass", "evidence": "Labeled 'good first issue' and 'tier-1'"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/73",
    "checks": [
      {"name": "repo-alive", "grade": "pass", "evidence": "Not archived; most recent default-branch commit 2026-09-16 by human collaborator Aburke225"},
      {"name": "issue-grounded", "grade": "pass", "evidence": "Reports that README and .env.example disagree with core/config.py, an internal inconsistency in the project's own docs, filed by COLLABORATOR Aburke225"},
      {"name": "scope-bounded", "grade": "pass", "evidence": "Single defect, 2 named files, estimated 1-2 hours, no sub-items or umbrella structure"},
      {"name": "unclaimed", "grade": "pass", "evidence": "No assignees, no comments, no linked PRs"},
      {"name": "ai-policy-permits", "grade": "pass", "evidence": "No CONTRIBUTING.md/AI_POLICY.md/AGENTS.md statement restricting AI-assisted contributions"},
      {"name": "maintainer-engaged", "grade": "pass", "evidence": "Filed by COLLABORATOR Aburke225 with diagnosed inconsistency and exact file/field pointers"},
      {"name": "newcomer-signposted", "grade": "pass", "evidence": "Labeled 'good first issue' and 'tier-1'"}
    ],
    "verdict": "accept"
  }
]
```

---

## Eval iterations

**Run history**

Six runs, in order:

1. `--only issue-01,issue-02,issue-03,issue-12,issue-20` — **4/5**. Partial, diagnostic:
   the two items I most wanted to test were the new policy check and the restored
   groundedness check. Both graded correctly. issue-01 came back a wrong reject.
2. `--only issue-01,issue-05,issue-10` — **3/3**, after rewording `scope-bounded`. The
   umbrella items were the regression check, to prove the reword had not merely
   loosened the check until issue-01 passed.
3. Full run, 20 items — **18/20 PASS**, category floor met. Disagreements: issue-15
   (graded accept, gold reject) and issue-19 (graded reject, gold accept).
4. `--only issue-15,issue-19,issue-09,issue-05,issue-10,issue-01` — **6/6**, after two
   `scope-bounded` fixes. issue-09 was in the set as a canary, not a target.
5. `--only issue-15,issue-09,issue-19,issue-01,issue-14` — **5/5**, after replacing the
   fix for issue-15 with a different one (see Trade-offs).
6. Full run, 20 items — **20/20 PASS**, all five categories matched. This is the run in
   `eval-run.txt`.

**Issue analysis**

`issue-15` (zulip/zulip#19589). **The rubric in this submission grades it `reject`, and the
gold label is `reject` — they agree.** An earlier version of the rubric graded it `accept`;
that run is the third entry in Run history above, and the disagreement is what produced the
check quoted below. This field describes the reasoning of the rubric as submitted.

The reason it is worth analyzing is that the earlier accept was not sloppy — every check
passed honestly. `repo-alive` passed on
human commits two days before capture. `issue-grounded` passed because a maintainer had
confirmed the gap: `eeshangarg (MEMBER)` pointed at Slack's own docs for "the updated
format for how the slash commands are supposed to be structured." `unclaimed` passed
because both linked PRs (#20840, #23123) are closed and unmerged — correctly, since
neither is an active claim. And `scope-bounded` passed because the design was settled and
the ask was a single deliverable.

What none of those checks looked at was the thread's shape, and that is the gap the
submitted rubric closes. Its `scope-bounded` check now also fails an issue whose history
shows serial abandonment, and that is the clause issue-15 fails on. In the 40 comments the
bundle shows, there are 15 `@zulipbot claim` comments and 3 auto-unassignment notices — a
different contributor about every few months from 2021 to 2024, and not one completed
implementation. LoganNiswander claimed it in November 2021 as a first open-source
contribution and was auto-unassigned ten days later. The spec was never the obstacle. The
`good first issue` label was describing what the maintainers hoped the work cost, and a
three-year record says otherwise. That is the reasoning behind the submitted rubric's
`reject`: the spec is settled, so nothing in the issue text objects, but the observed
contribution history contradicts the tractability the label advertises.

**Check rationale**

From the `rubric.md` uploaded to `tools/issue-select/`, quoted as currently written:

| maintainer-engaged | Repo facts: "maintainer first-response sample"; the Comments section (`author_association` per comment); the issue opener's association | Substantive stewardship is visible, not merely a fast clock: an OWNER / MEMBER / COLLABORATOR filed this issue, diagnosed it, answered a contributor in this thread, or settled a question in it. A response-time figure with no accompanying substance does not satisfy this check. Grade `unclear` when the bundle reports only latency rows and the thread has no maintainer voice — the sample is a five-issue sketch, and its silence is thin evidence, not proof of neglect. | preferred |

Two things in that wording are deliberate. First, the pass condition refuses a
response-time figure on its own. A maintainer who replies "thanks, we'll look into it"
within an hour and never engages again is a fast clock, not stewardship, and latency
cannot tell those apart. Second, the weight is `preferred`, and that is a demotion I made
against my own preference. Traced across the eight `clear-accept` items, a required
version fails three of them — issue-01, issue-14, issue-16 — because each was opened by a
`CONTRIBUTOR` (someone with a merged commit, not authority), has no maintainer voice in
its thread, and has a response sample that is mostly the line "no maintainer comment in
thread." Three wrong rejects caps the run at 17/20, under the bar.

It also buys nothing as a gate: all four dead-repo items fail `repo-alive` first.
issue-14 and calib-03 have *identical* response evidence — a single sample row reading
"no maintainer comment in thread" — and what separates the living repo from the dead one
is that one committed on the capture date and the other stopped shipping 19 months
earlier.

So the construct survives and the veto does not. The check reports what it sees,
including `unclear`, and the verdict rule is where the authority lives.

**Trade-offs**

As `preferred` this check cannot reject anything, so my rubric will accept a repo whose
maintainers are visibly present but never substantive — the exact failure mode the check
was written to name. I am accepting that because the snapshot's evidence for it is
unreliable: it graded `unclear` on issue-01 and issue-20 in run 1, and had it held a veto
those would have been two more wrong verdicts.

The canary is the same three items. On the full run at step 6, issue-01, issue-14, and
issue-16 all score `accept` and agree with gold, and the per-check output shows
`maintainer-engaged` at `unclear` on issue-01 while the verdict stands — the demotion
working as intended rather than silently.

One more trade-off worth recording, in `scope-bounded`. My first fix for issue-15 was a
threshold: two or more closed unmerged linked PRs rejects. It produced the right label,
and it separated issue-15 (two closed PRs) from issue-09 (one closed PR, gold accept). It
was also shallow — it measured the two contributors who got far enough to open a PR and
missed the thirteen who never did. Reading the thread showed the real pattern, and I
replaced the count with it. I kept the earlier run file, `eval-run-18of20-PASS.txt`, as
the baseline: a number that scores correctly for the wrong reason is worth keeping a
record of.

---

## Selection rationale

**1. Fit to my interests and the time available.**

The bug is in `core/security.py`. When a stored password hash is malformed, passlib raises
`UnknownHashError` and the exception escapes, instead of verification returning `False`. I
write Python most days, and error handling on a security path in someone else's codebase is
what I wanted practice on. I also know when I am done: the test covering it is marked
`xfail` against manifest id H-05, so the fix is finished when I take that marker off and
the test passes. The issue estimates 1 to 2 hours. Unit 2 starts immediately, and I would
rather spend that time learning the repo than fighting something bigger.

**2. What the verdict got right, and what I weighed separately.**

The skill got the eligibility right and showed the evidence for it: the repo is not
archived, the last commit was four days ago by a human, nothing in the contributing docs
restricts AI-assisted work, there is no assignee, and a collaborator filed the issue with
the file and the expected behavior written out.

What the required checks could not do was separate the candidates. All three passed all
five, so the ordering came entirely from the fit profile in `scope.md`, and that put #69
first because it sits in the RAG stack, closer to the agent tooling I have worked on.

I took #72 anyway, on three things the rubric does not look at. The finish line is more
obvious. One to two hours beats two to four with Unit 2 starting now. And someone else
asked for #69 yesterday, so taking it would mean two of us writing the same fix.

**3. The anticipated difficulty in claiming it.**

Claiming should be easy. #72 has no comments, so I am not stepping on anyone. The harder
part is the work itself: I need to read enough of `core/security.py` to know what failing
closed should mean there, and reproduce the raise before I try to fix it.

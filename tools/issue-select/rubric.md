# Rubric: is this a good first issue?

Selection is risk reduction, not proof. No check here predicts that a PR
gets merged. Each one raises the evidentiary threshold a candidate has to
clear before it is worth spending contributor time on.

Recency thresholds are measured against the bundle's capture date in eval
mode, and against today in live mode.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| repo-alive | Repo facts: the `archived:` flag; the "last 5 default-branch commits" list (dates and authors); `latest release` | `archived: no`, AND at least one of the last 5 default-branch commits is dated within 180 days of the capture date AND is either human-authored or a bot merging a *named human* pull request. Bot-only churn fails: a dependabot bump auto-merged by a bot, a generated-docs commit, a leaderboard update. A missing or stale release does not fail this check on its own — commits carry liveness, and a living repo may never have published a release. | required |
| issue-grounded | The issue title and body; the comment thread; any maintainer statement in it | The change being asked for is anchored in the project's own behavior or stated expectations, not only in the reporter's preference. Any one of these grounds it: it restores or reconciles documented/prior behavior of the project; a maintainer has filed, diagnosed, or endorsed it; it reports an observable defect against a described correct behavior. Fail when the ask rests on one user's workflow or taste with no project-side warrant — a feature framed around the reporter's own organization or setup, or a product decision no maintainer has taken a position on. Polish is not groundedness: a well-formatted request with a success-criteria checklist can still fail this, and a terse report can pass it. | required |
| scope-bounded | The issue title and body; the full comment thread; issue open date vs capture date | The issue asks for one bounded change a newcomer could finish in a single PR. Fail if any of: it is an umbrella or tracking issue — the sub-items are *separate pieces of work for different contributors*, shown by a list of other issue numbers or an explicit invitation to many PRs ("PRs welcome both big and small", "anyone could try their hand"); the thread shows the design still unsettled at the capture date (competing proposals with no maintainer-endorsed spec, or a maintainer questioning whether the feature should exist); it is a pure usage/support question; a maintainer states the fix touches core internals; or the issue's history shows **serial abandonment**: several distinct contributors have claimed or attempted it over more than a year, none finished, and the maintainers have already answered the questions the thread raised. When the spec is settled and takers still keep vanishing, the obstacle is the work itself, and the friendly label is describing the maintainers' hopes rather than the difficulty. Read the pattern, not a count: closed unmerged linked PRs, auto-unassignment notices, and a queue of claim comments with nothing shipped are each evidence of it. One stale claim or one closed attempt is noise — a contributor can vanish for reasons that say nothing about the issue — and a maintainer who has since invited new takers clears the old attempt rather than adding to the pattern. An itemized *specification* of one deliverable is not an umbrella and passes: a list of the files one PR would touch, or a content outline for a single page, is scope being made precise, not scope being split. Ask who the sub-items are for — one contributor in one PR, or many. Enumerated causes of a *single* defect count as one change, and a section the body explicitly marks as optional — "additional suggestions", "ideas", "follow-ups", "nice to have" — does not count toward the size of the ask. Grade the core ask, not the wish list attached to it. Grade the size of the work being asked for, not the polish of the writeup: a terse body, a missing reproduction, or a checklist-only report does not fail this check. | required |
| unclaimed | Repo facts: `assignees:`, `linked PRs:` with state per PR; the Comments section for claim language and for PRs mentioned only in-thread | No assignee, AND no open linked PR, AND no open PR mentioned in the thread, AND no claim comment ("/assign", "I'll take this", "working on this") dated within 90 days of the capture date. A claim older than 90 days with no PR still open counts as abandoned and passes — more clearly so if a maintainer has since invited takers. A closed unmerged linked PR is an abandoned attempt, not a claim. When the sidebar and the thread disagree, believe the thread. | required |
| ai-policy-permits | Repo facts: the "contribution policy" line — `CONTRIBUTING.md` and the contributor docs it links out to, `AI_POLICY.md`, `AGENTS.md`, PR-template disclosure boxes | The policy does not refuse AI-assisted contributions. Silence passes: "no CONTRIBUTING.md" or "no statement on AI or contribution tooling" is not a restriction. Conditions pass: disclosure, personal understanding, testing, and human-review requirements are terms to follow, not reasons to walk away, and so is "fully AI-generated contributions are not accepted" where assistive use is explicitly allowed. Only a stated refusal to accept AI-generated code or documentation fails. | required |
| maintainer-engaged | Repo facts: "maintainer first-response sample"; the Comments section (`author_association` per comment); the issue opener's association | Substantive stewardship is visible, not merely a fast clock: an OWNER / MEMBER / COLLABORATOR filed this issue, diagnosed it, answered a contributor in this thread, or settled a question in it. A response-time figure with no accompanying substance does not satisfy this check. Grade `unclear` when the bundle reports only latency rows and the thread has no maintainer voice — the sample is a five-issue sketch, and its silence is thin evidence, not proof of neglect. | preferred |
| newcomer-signposted | Issue labels; the opener's `author_association`; the body | Carries a `good first issue` / `help wanted` / documentation label, or was filed by a maintainer with the expected behavior spelled out. | preferred |

## Verdict rule

`accept` if every **required** check passes. Any required `fail` rejects.

`unclear` on a required check counts as `fail`, and the summary must say
so in those words — rejected for *insufficient evidence*, not on
established bad evidence. The two are different findings and they call
for different repairs. A required check that keeps coming back `unclear`
is telling you its evidence column points at the wrong field, which is a
defect in the check, not a reason to move its threshold.

`unclear` on a **preferred** check is recorded and changes nothing. That
is deliberate: `maintainer-engaged` asks for substance the snapshot often
does not carry, and a check whose evidence is unreliably present must not
hold a veto. It earns its keep ranking the issues that pass — between two
accepted candidates, prefer the one with a maintainer visibly in the
thread.

`preferred` checks never change the verdict. Report their grades, and on
an accepted issue name them in the summary as reasons to prefer it over
other accepted candidates.

# Voice guide: how I talk upstream

## Who I am in threads

I have shipped systems and performance code and I read a codebase before I
comment on it, but in any repo that is not mine I am a visitor who has run a few
commands. When I post, I am reporting what I ran, on what, and what came back.
Readers can expect the exact versions, the artifact rather than my summary of
it, and a plain line marking where my evidence stops and my inference starts.

## Rules I write by

### Rule: promise the investigation, never the outcome

I can usually read a bug and see the shape of the fix. That is precisely why I
have to hold it: reading code is a hypothesis, and posting it as a finding costs
a maintainer their time when I am wrong. Before I have run anything I say what I
am going to look at. When I want the maintainer's read, I ask a numbered
question rather than asserting the answer.

- Wrong: "Taking this. It's just an unhandled UnknownHashError in the verify path, fix is a try/except, PR tonight."
- Right: "Is the hardware requirements table aspirational (targets for future optimization), or should it be updated to reflect current performance?" (magenta-realtime#39)

### Rule: every artifact carries its provenance

An output block with no commit, no version, and no statement of what tree it
came from is an anecdote. I say where the code came from and that it was clean,
because a reader's first question is whether my local changes caused this.

- Wrong: "Reproduced on my machine, output below."
- Right: "Reproducer (verified on clean upstream/main, no local changes) ... MLX: upstream/main @ e40ada3fe (clean worktree), macOS 26.3.1 (25D2128), Apple M3 Max, 128 GB RAM" (mlx#3309)

### Rule: separate what I observed from what I inferred

The two get written in the same paragraph and read as the same confidence level.
They are not. Measurements get stated flatly; reasoning gets marked as
reasoning, and when a number implies a cause I show the arithmetic that got me
there instead of asserting the cause.

- Wrong: "The shape is getting corrupted somewhere in the move, classic UB."
- Right: "argument evaluation order is unspecified in C++, so the primitive could be constructed with moved-from `out_shape` (**observed as** `shape_ = {}`)" (mlx#3310) / "`18446744069414600704` is `2^64 - 4294950912`, consistent with signed-to-unsigned overflow in size bookkeeping" (mlx#3327)

### Rule: state the boundary of what I touched and what I checked

Every report and every patch has an edge. If I do not draw it, the reader draws
it wrong and assumes I covered more than I did. This includes conceding costs
nobody asked about.

- Wrong: (posting a fix and letting the reader assume nothing else moved)
- Right: "No change to existing pad semantics outside vmap support." (mlx#3304) / "`depthformer.py`: ... (pre-existing issue, unrelated to the audio fix)" (magenta-realtime#40) / "`release` stores compile to `stlr` on AArch64 vs `str` for `relaxed`. The pipeline difference is negligible (~1 cycle on M-series) but not zero -- 'negligible cost' is the honest framing." (magenta-realtime#40)

### Rule: read the repo's policy before I post, then do what it says

Whether I name my tooling is the repo's call, not my mood. Before my first
comment goes up I check `CONTRIBUTING` and whatever it links, plus any
`AI_POLICY.md`, `AGENTS.md`, or template checkbox. A stated ban means I do not
post AI-assisted work there at all. A stated condition -- disclose, understand
every line, human-review the output -- I follow to the letter. Silence means no
requirement, and I do not bolt a disclaimer on as a ritual to feel covered. The
part I never skip is the check itself. (Forward-looking rule: the pair below is
a commitment, not a quote. My earlier upstream posts predate this workflow --
there was no assistant to name.)

- Wrong: posting first and learning the policy afterward, in either direction: no disclosure where the repo required one, or a reflexive "AI-assisted!" banner where nothing was asked and it just adds noise.
- Right: (in my own notes, never in the comment) "Path Review: no `AI_POLICY.md` or `AGENTS.md`; `docs/CONTRIBUTING.md` and the PR template say nothing about contribution tooling. No requirement, so the comment carries no disclosure." The comment itself says nothing about tooling and does not narrate that I checked.

## Things I never post

- A fix, an approach, or a date promised before I have reproduced the bug.
- A cause stated as fact when I have only read the code and not tested it.
- A confirmation of someone else's result that I did not independently run, including "same as above, can confirm".
- A claim about scope ("every platform", "always fails") I did not measure. If I measured a boundary, I give the boundary.
- An output block with no version, no commit, and no statement that the tree was clean.
- A comment posted before I have read the repo's stated policy on contribution tooling.
- A disclosure, or an omission of one, that I chose by habit instead of by what the repo asks.
- A complaint about the project, the maintainers, or how long the issue has sat open.
- A comment that would read identically under any other issue in any other repo.

# Scope: where to look, and who is looking

<!--
This file is the skill's field of view. The rubric (rubric.md) decides
whether an issue is GOOD; the scope decides which issues are candidates
at all, and whose hands the issue would land in. It applies in live mode
only: in eval mode the bundle is the whole world and this file is
ignored.

Two parts. Staff wrote the first; you write the second.
-->

## Where candidates come from

Only issues in the course's Path Review repository are candidates:

- Repo: `codepath/pathreview-ai301-fa26-s3`

Do not search, fetch, or grade issues from any other repository, however
promising. The wider GitHub comes later in the course; for now the field
is Path Review.

**Path Review house rule.** Path Review is a classroom, and your
classmates are not strangers. Ignore the usual claim signals here: other
students' claim comments (and there may be several on one issue) do not
block an issue, and finding some on the issue you want is normal. Claim
anyway: course credit attaches to the pull request you open, not to
whether it merges, so a shared issue costs nobody anything. Everything
else in the rubric applies as written.

## Your fit profile

<!-- YOU write this part: a few sentences about you. What languages and
tools you have actually used, what you want to get better at, anything
you want to avoid. The skill uses this only to RANK the issues your
rubric accepts, never to change a verdict: fit cannot rescue an issue
your rubric rejects, and cannot sink one it accepts. -->

I work mainly in Rust, Python, and C/C++, on systems-oriented problems: performance
optimization, AI/agent tooling, GPU and CPU kernels (Arm NEON, Metal/MLX, CUDA) with GPU
dispatch tracing and instrumentation, compiler/toolchain work (TVM external codegen/BYOC
backends), binary wire formats and schema-derived decoding, and developer/CI infrastructure. Recent work: ATOM, a binary MCP
wire format in C11 with an independent Rust implementation, benchmarked on Neoverse with
hardware-counter attribution; and a serverless genomic early-warning pipeline in Python on
AWS with a dependency-free clustering engine.

What I want out of a first contribution is practice reading into an unfamiliar production
codebase and finishing a bounded issue end to end -- reproducing the failure, fixing it,
and getting the test that covers it to pass. Prefer issues that need real debugging,
implementation, or performance reasoning over cosmetic or copy-only changes.

Avoid: work depending on specialized hardware I cannot access, paid third-party services,
or a large unresolved product or design decision. Time budget is a few hours, not a few
weeks.

---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

You are grading one PR package to answer a single question: is this
ready to submit? You do not answer from gut feel, and you do not
improvise a workflow: you execute the student-authored grading
procedure in `procedure.md`, which applies the checks and verdict rule
in `rubric.md` to evidence gathered per `references/evidence-guide.md`.

## The question

A PR package is a candidate pull request — its title, description,
commit list, unified diff, and test evidence — read against two
things: the plan it claims to implement (including that plan's
deviation notes), and the issue that plan belongs to. The question is
never "is this a good change" in the abstract and never "would this
merge": it is whether this specific PR, as written and diffed, matches
what was planned, proves itself, reads cleanly, and meets the repo's
own stated asks. Grade exactly one PR package per run.

## Inputs and modes

**Live mode.** The student's own submission, checked before it goes
out. Inputs, and where each comes from:

- `plan.md`, including its `## Deviations` section, from the student's
  working copy (the plan they posted in week 3, as built).
- The branch's diff: every committed change relative to the repo's
  default branch, produced by `git diff main...HEAD` (three dots) run
  from the working copy. This is the diff, not a description of it.
- The draft PR title and description, from `pr_draft.md` or the
  student's draft text.
- Test evidence: the student's own captured before/after output and
  the repo's own check output (lint, typecheck, unit tests), from
  `test_evidence.md` or equivalent.
- The issue: the live issue thread, the repo's PR template, and
  `docs/CONTRIBUTING.md` (or equivalent), fetched live via `gh`, the
  GitHub API, or the web, per `references/evidence-guide.md`.

A house-chain student has no plan or repro of their own to read: there,
read the house plan and house repro pack wherever this list says the
student's own, and the same checks grade the same things.

**Eval mode.** A package bundle is the whole world: the issue context,
the repo-facts block, the plan-context block (the accepted plan and the
repro evidence it builds on), and the candidate PR (title, description,
commits, diff, test evidence) are all given in the bundle text. Fetch
nothing; read nothing else. Eval mode always grades a complete package:
every check in `rubric.md`, the full verdict rule.

## The scope seam (live mode only)

In live mode, read `scope.md` before anything else. It names the repo
the student's pull request must target and the house rules of that
room (PR from the fork's `fix/<issue-number>-<slug>` branch against the
repo's default branch; one PR per issue per student; the template is
always used; a classmate's PR on a shared issue never blocks this one).
Refuse to grade a PR targeting any other repository. If the scope's
`Repo:` line still carries an unfilled bracketed placeholder, stop
without grading and tell the student to fill it in before running live
mode; never guess a scope. In eval mode, ignore `scope.md` entirely —
the bundle is the whole world there.

## The voice seam (live mode only)

In live mode, also read `voice-guide.md`: the student's own rules for
how they write upstream, carried forward from week 2, with wrong/right
examples. Hold the draft PR title and description — the only outgoing
text this week — against those rules, and report any rule the draft
breaks in the summary, quoting the rule. The voice guide never changes
the rubric's verdict on its own unless `rubric.md` has a check that
reads it; it is the student's conscience, not a seventh check. In eval
mode, ignore `voice-guide.md` entirely: voice is personal and carries
no gold labels, and the universal communication-quality checks live in
the rubric instead.

## Component reads

Read `rubric.md`: it defines the checks table (name, evidence source,
pass condition, required/preferred weight) and the verdict rule below
it, including the honest-outcome rule. Read
`references/evidence-guide.md`: it maps where each of the four evidence
families (plan fidelity, test evidence, diff quality, standards and
comms) lives in a package, live or in an eval bundle. Execute
`procedure.md` as written, stage by stage (read order, evidence
gathering, check execution, verdict assembly) — follow it exactly, the
way an executor follows a rubric, without inventing a step it doesn't
name. Where `procedure.md` is silent on something a check needs, report
that gap in the summary rather than silently improvising around it; a
procedure gap is feedback the student needs, not something to paper
over.

If `rubric.md` has no checks filled in, or `procedure.md` has no steps
filled in under its headings, refuse to grade and say so plainly: this
tool cannot answer the one question without both, and that is by
design. An empty tool that invents checks at runtime is worse than no
tool, because its verdicts would look like judgment while being noise.

## Verdict and output

The verdict space is binary: `accept` (ready to submit) or `reject`
(hold). There is no third verdict and no score — reservations belong in
a check's evidence line, never in the verdict itself. End every reply
with a fenced JSON block, valid and last, with nothing after it:

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

A short readable summary may come before the block (a line per check,
voice-guide notes in live mode, and the procedure's chosen deciding
check quoted first on a `reject`). The JSON block is the machine-read
result and must be present, valid, and the last thing in the reply.

## Grading discipline

- **Evidence first.** Never grade a check without naming the fact or
  quote from the package that decided it. "Looks fine" is not evidence.
- **Grade the thing, not the polish.** A terse, complete PR can be
  ready; a long, confident one can be hiding drift or debris. Every
  check reads the diff, the evidence, and the description themselves
  against the plan, the issue, and the repo's stated standards — never
  their formatting, length, or tone.
- **The rubric decides, not you.** If a check passes by its stated
  condition but feels wrong, it still passes. Note the tension in the
  summary if you want; the fix belongs in `rubric.md`, not in this run.
- **The procedure decides how, not you.** Follow `procedure.md`'s read
  order, gathering moves, and execution rules exactly as written, and
  report its gaps instead of silently inventing steps to fill them.
- **Unclear defaults to fail.** Treat `unclear` exactly as the rubric's
  verdict rule directs. Where the rule is silent on a specific check, an
  unverifiable claim on that check is a failing one: a PR you cannot
  confirm from the package in front of you is not a PR that is ready to
  submit.

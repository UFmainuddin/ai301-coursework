# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/61

**Verdict output**

Live-mode run graded three candidates from the Path Review repo together
(`issues/61`, `issues/62`, `issues/60`); full output below, issue 61 ranked first
and accepted:

```
All three candidates are in scope (codepath/pathreview-ai301-fa26-s3). Grading each
independently against rubric.md, applying the Path Review house rule (student claim
comments don't count as claims):

Issue #61 — Health check DB probe passes a raw SQL string, fails under SQLAlchemy 2.x
- maintainer-alive: pass — last 5 default-branch commits all within the last 4 weeks (most recent 2026-09-16)
- repo-in-use: pass — not archived; last push 2026-09-16, 5 days before today
- unclaimed: pass — no assignee, 0 comments, no linked PR
- scope-fits-newcomer: pass — one file (api/routes/health.py), one root cause (raw string needs sqlalchemy.text()), exact reproducing error message given
- ai-contribution-policy: pass — docs/CONTRIBUTING.md is silent on AI-assisted contributions
- clear-repro-or-spec (preferred): pass — exact ArgumentError message quoted

Issue #62 — Health check references settings.redis_host, which does not exist on Settings
- maintainer-alive: pass · repo-in-use: pass (same repo facts)
- unclaimed: pass — no assignee; one student comment ("would like to work on this issue") — house rule: does not block
- scope-fits-newcomer: pass — one file, one root cause (wrong attribute name), clear repro steps
- ai-contribution-policy: pass
- clear-repro-or-spec (preferred): pass

Issue #60 — Faithfulness checker crashes when a context chunk has text: None
- maintainer-alive: pass · repo-in-use: pass
- unclaimed: pass — no assignee; one student comment ("I'll take this") — house rule: does not block; no open linked PR (one stale, unresolvable commit reference found, not a real linked PR)
- scope-fits-newcomer: pass — one file (rag/evaluator/faithfulness_checker.py), one root cause (dict.get doesn't override an explicit None), repro includes a code snippet and names the exact failing unit test
- ai-contribution-policy: pass
- clear-repro-or-spec (preferred): pass

Ranked read-out (all three accepted):
1. #61 — best fit: zero competing claims, the single most mechanical and well-known fix pattern (wrap literal SQL in sqlalchemy.text()), lowest ambiguity for a first PR.
2. #62 — same file/subsystem as #61, one student already expressed interest (non-blocking under the house rule), and CONTRIBUTING.md separately flags this issue by number as also needing a pyproject.toml lint-suppression removed alongside the code fix — slightly more surface area.
3. #60 — different subsystem (RAG evaluator, less familiar than a REST health endpoint), one student already claimed intent to work on it (non-blocking), still very well-scoped with an exact reproducing snippet.

[
  {"item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/61", "checks": [
    {"name": "maintainer-alive", "grade": "pass", "evidence": "last 5 default-branch commits dated 2026-08-24 to 2026-09-16, all within 90 days"},
    {"name": "repo-in-use", "grade": "pass", "evidence": "not archived; last push 2026-09-16, 5 days before capture"},
    {"name": "unclaimed", "grade": "pass", "evidence": "assignees: none; 0 comments; no linked PR"},
    {"name": "scope-fits-newcomer", "grade": "pass", "evidence": "one file (api/routes/health.py), one root cause: raw 'SELECT 1' string not wrapped in sqlalchemy.text()"},
    {"name": "ai-contribution-policy", "grade": "pass", "evidence": "docs/CONTRIBUTING.md states no policy on AI-generated contributions"},
    {"name": "clear-repro-or-spec", "grade": "pass", "evidence": "issue quotes the exact ArgumentError message raised"}
  ], "verdict": "accept"},
  {"item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/62", "checks": [
    {"name": "maintainer-alive", "grade": "pass", "evidence": "last 5 default-branch commits dated 2026-08-24 to 2026-09-16, all within 90 days"},
    {"name": "repo-in-use", "grade": "pass", "evidence": "not archived; last push 2026-09-16, 5 days before capture"},
    {"name": "unclaimed", "grade": "pass", "evidence": "assignees: none; one student comment ('would like to work on this issue') is not a blocking claim under the Path Review house rule; no linked PR"},
    {"name": "scope-fits-newcomer", "grade": "pass", "evidence": "one file (api/routes/health.py), one root cause: settings.redis_host does not exist, should use settings.redis_url"},
    {"name": "ai-contribution-policy", "grade": "pass", "evidence": "docs/CONTRIBUTING.md states no policy on AI-generated contributions"},
    {"name": "clear-repro-or-spec", "grade": "pass", "evidence": "issue gives exact call (GET /health) and exact log signature (AttributeError for redis_host)"}
  ], "verdict": "accept"},
  {"item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/60", "checks": [
    {"name": "maintainer-alive", "grade": "pass", "evidence": "last 5 default-branch commits dated 2026-08-24 to 2026-09-16, all within 90 days"},
    {"name": "repo-in-use", "grade": "pass", "evidence": "not archived; last push 2026-09-16, 5 days before capture"},
    {"name": "unclaimed", "grade": "pass", "evidence": "assignees: none; one student comment ('I'll take this') is not a blocking claim under the Path Review house rule; timeline shows one unresolvable stale commit reference, not a real linked PR"},
    {"name": "scope-fits-newcomer", "grade": "pass", "evidence": "one file (rag/evaluator/faithfulness_checker.py), one root cause: dict.get default does not override an explicit None value"},
    {"name": "ai-contribution-policy", "grade": "pass", "evidence": "docs/CONTRIBUTING.md states no policy on AI-generated contributions"},
    {"name": "clear-repro-or-spec", "grade": "pass", "evidence": "issue includes a runnable code snippet, the exact TypeError, and names the failing unit test test_none_context_chunk_text"}
  ], "verdict": "accept"}
]
```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

1. Smoke run, `--limit 2` (partial, 2 items): 1/2 agreement. Caught a false reject on
   `issue-01`: my first `scope-fits-newcomer` wording treated any issue that listed
   several sub-steps as an "umbrella issue," which wrongly rejected a single-outcome
   documentation task with multiple file edits.
2. Smoke run, `--limit 8` (partial, 8 items): 8/8 agreement. Confirmed the reworded
   `scope-fits-newcomer` ("single coherent outcome" instead of "no sub-items") fixed
   `issue-01` without breaking the other 7.
3. Full run, 20 items: **16/20** — below the 18/20 bar. Category floor held (at least
   one match in every category), but `scope` was 2/4 and `clear-accept` was 6/8.
   `issue-01` and `issue-19` failed on `scope-fits-newcomer`; `issue-15` and `issue-20`
   passed every check yet gold said reject, meaning my rubric had no check that could
   even see what was wrong with them.
4. Targeted re-run, `--only issue-01,issue-15,issue-19,issue-20,issue-04,issue-11,issue-14,issue-16`
   (partial, 8 items): 7/8. Confirmed the `unclaimed` and `scope-fits-newcomer` rewrites
   fixed `issue-15`, `issue-19`, and `issue-20`, with the 4 known-good issues
   (`issue-04`, `issue-11`, `issue-14`, `issue-16`) still passing as a regression check.
   `issue-01` still disagreed on this run.
5. Full run, 20 items: **20/20** — bar met, category floor clean across all 5
   categories (`claimed 4/4`, `clear-accept 8/8`, `dead-repo 3/3`, `policy 1/1`,
   `scope 4/4`).
6. Full confirming run with `--save-run eval-run.txt`, 20 items: **20/20** — this is
   the run committed in `eval-run.txt`.

**Issue analysis**

`issue-15` (zulip/zulip#19589, evidence bundle only — the "unclaimed" category).
My rubric's final verdict: **reject**. Gold label: **reject**. The issue has no current
assignee and no currently-open linked PR, so a naive unclaimed check would pass it
straight through to accept. But the comment thread shows a three-year pattern: at least
ten different contributors posted a claim comment (several via `@zulipbot claim`), got
auto-assigned, then went silent and were auto-unassigned after 14 days, and the issue
carries two separate closed-and-unmerged linked PRs from earlier attempts. My `unclaimed`
check's "repeated abandonment" clause (fewer than 2 closed/unmerged linked PRs AND fewer
than 3 abandoned claimants, both required to pass) treats that pattern as a required-check
failure, because a "good first issue"-labeled task that many people have already tried and
dropped is telling you something about its real difficulty that the label doesn't show.

**Check rationale**

Quoted verbatim from the checks table in `rubric.md`:

```
| unclaimed | Repo-facts block: this issue's "assignees" and "linked PRs" fields, plus the Comments section | No assignee is set on this issue, no linked PR against this issue is currently open, AND the thread does not show a pattern of repeated abandonment: fewer than 2 closed/unmerged linked PRs against this issue, and fewer than 3 distinct people who claimed it (via an explicit claim comment or bot-assignment) and were later unassigned or went silent. A single closed/unmerged PR, or one abandoned claimant, does not by itself fail this check; a repeated pattern does, because it signals the issue is harder or more contested than its label implies | required |
```

My first draft of this check only looked at the *current* assignee and *currently open*
linked PRs — a snapshot of right-now claim state. `issue-15` in the eval run showed that
snapshot alone misses a slow-motion history: nobody is claiming it *this second*, but
plenty of people have tried and failed, which is exactly the kind of thing
`evidence-guide.md`'s family 4 section flags ("a closed unmerged [PR] is an abandoned
attempt"). I widened the check from a point-in-time fact to a pattern threshold so it can
catch a "quietly cursed" issue that a purely current-state check would wave through.

**Trade-offs**

The threshold (fewer than 2 closed PRs, fewer than 3 abandoned claimants) is deliberately
loose on a single failed attempt, because one person claiming and going quiet is normal
and common, not a signal the issue itself is bad. I re-ran `--only issue-04,issue-11,issue-14,issue-16`
as a canary alongside the `issue-15`/`issue-19`/`issue-20` fixes specifically to confirm this:
all four still passed, so the widened check isn't newly punishing issues with one ordinary
abandoned claim. What it still misses: a claim or a failed attempt that happened somewhere
the bundle can't see (a linked pull request from a fork that GitHub never formally
associated with the issue, or a claim made in a project Slack/Discord instead of the
thread) won't be counted toward the pattern, so a genuinely contested issue could still slip
through if its history isn't visible in the issue thread itself. That's an accepted gap: the
check can only grade the evidence the bundle (or, in live mode, the GitHub thread) actually
surfaces.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. **Fit to my interests and the time available.** My fit profile in `scope.md` says
   I want to practice reading unfamiliar code and shipping a clean, well-scoped PR
   without needing deep domain knowledge on day one, and to avoid frontend/UI work.
   Issue #61 is exactly that: a single Python backend file, one root cause I can name
   precisely (a raw SQL string needs `sqlalchemy.text()` under SQLAlchemy 2.x), and a
   fix that's a known, teachable idiom rather than a judgment call. Given this is also
   the unit where I'm still learning the repo's whole contribution mechanics (branch
   naming, conventional commits, the five-job CI suite, the PR template), a small,
   unambiguous bug is the right size to spend my time on the *process* instead of
   fighting the *problem*.
2. **What the verdict got right, and what I weighed beyond it.** The rubric correctly
   confirmed the mechanical facts: the repo is active, the issue is genuinely
   unclaimed (no assignee, no linked PR), the fix is bounded to one file, and the
   repo's `docs/CONTRIBUTING.md` has no AI-contribution ban. What the rubric can't see,
   because it isn't a rubric-scored fact, is competitive pressure in a shared classroom
   repo: #62 and #60 each already have a student comment expressing interest, while #61
   has zero. The rubric correctly says the house rule makes those comments
   non-blocking, but "technically unclaimed" and "nobody else is currently circling it"
   are different things, and I weighed the second one in #61's favor since it's a
   shared repo I'll be claiming this issue in for real.
3. **Anticipated difficulty in claiming it.** The code fix itself should be low
   difficulty — wrap the literal `"SELECT 1"` in `sqlalchemy.text()` and confirm the
   probe passes. The harder part is likely process, not code: `docs/CONTRIBUTING.md`
   warns that a first PR from a new contributor can sit at "waiting for approval to run
   workflows" until a maintainer releases CI, which is outside my control. I also don't
   yet know whether issue #61 has a seeded `xfail`-marked test or a `pyproject.toml`
   lint suppression tied to it the way #62 is explicitly called out as having one — I'll
   need to check for both when I reproduce the issue in Unit 2, since removing them (if
   present) is part of actually closing the issue, not just fixing the visible bug.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.

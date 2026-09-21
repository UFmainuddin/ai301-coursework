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

[The individual Path Review issue page. A link to the repository or the issue list
does not satisfy this field.]

**Verdict output**

[Your skill's live-mode output for this issue, pasted verbatim and ending with the
fenced JSON verdict block. A summary does not satisfy this field.]

**The verdict must record `accept` for this issue.** Choose an issue your own skill
accepts. If your skill rejects every candidate you try, that is a signal about your
rubric rather than about the issues: revise it and re-run — retries are unlimited and a
partial re-run costs about $0.20 — or run the skill on different candidates. Output
recording `reject` for the issue you chose earns no credit for this field.

```
paste the output here, including the closing JSON block
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

[Answer all three:

1. The issue's fit to your interests and to the time available.
2. What the verdict identified correctly, and what you weighed that the rubric could
   not.
3. The anticipated difficulty in claiming it.]

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.

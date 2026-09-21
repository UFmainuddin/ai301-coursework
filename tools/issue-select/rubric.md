# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| maintainer-alive | Repo-facts block: "last 5 default-branch commits" dates | At least 2 of the last 5 default-branch commits are dated within 90 days of the capture date | required |
| repo-in-use | Repo-facts block: "archived:" flag, "latest release" date, "last push to any branch" date | The repo is not archived, AND (the latest release is dated within 365 days of the capture date OR the last push to any branch is within 90 days of the capture date) | required |
| unclaimed | Repo-facts block: this issue's "assignees" and "linked PRs" fields, plus the Comments section | No assignee is set on this issue, no linked PR against this issue is currently open, AND the thread does not show a pattern of repeated abandonment: fewer than 2 closed/unmerged linked PRs against this issue, and fewer than 3 distinct people who claimed it (via an explicit claim comment or bot-assignment) and were later unassigned or went silent. A single closed/unmerged PR, or one abandoned claimant, does not by itself fail this check; a repeated pattern does, because it signals the issue is harder or more contested than its label implies | required |
| scope-fits-newcomer | The issue body and comment thread | The issue asks for one bounded, describable piece of work serving a single coherent outcome (even if that means editing several files, or listing several optional/"additional" ideas alongside the required fix — grade the scope of the required fix only, not of suggestions the reporter marks as optional or nice-to-have). It is not an umbrella/tracking issue whose sub-items are separate features meant to be split into independent PRs by different contributors; its design is not still under open debate with no maintainer decision; no maintainer states the fix requires deep changes to core internals; it is not a pure usage/support question with no code change implied; and if the issue is itself a new-feature request (not a bug report or a docs/maintenance task), a maintainer (Owner/Member/Collaborator association) must have opened it, commented on it, or attached a triage/priority/type label — an unreviewed feature request with zero maintainer engagement fails, because nothing confirms the maintainers actually want it built | required |
| ai-contribution-policy | Repo-facts block: "contribution policy" line | The stated policy does not contain an outright ban on AI-generated or AI-assisted contributions (silence, or a policy that only sets conditions like disclosure, required testing, or human review, both pass) | required |
| clear-repro-or-spec | The issue body | The issue includes either concrete reproduction steps (for a bug) or a concrete acceptance-criteria / description of the desired end state (for a feature or docs task), so the scope is actionable without asking the reporter follow-up questions | preferred |

## Verdict rule

Accept only if every `required` check grades `pass`. A single `fail` or
`unclear` on any required check rejects the issue. `preferred` checks are
never counted toward the verdict; they exist only to rank issues that are
already accepted, with the issue passing more preferred checks (or the one
with the clearer preferred-check evidence) ranked higher when comparing
multiple accepted candidates. `unclear` is treated as `fail` everywhere
(required and preferred), because a first issue whose evidence is genuinely
ambiguous is not one a newcomer can act on with confidence.

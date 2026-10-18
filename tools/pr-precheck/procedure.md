# Procedure: how this tool grades a PR package

## Read order

1. (Live mode only) Read `scope.md` first. Confirm the PR targets the scoped repo; note house rules (PR from the fork's `fix/<issue>-<slug>` branch, one PR per issue, the template is always used).
2. Read `rubric.md` in full: list every check's name, evidence source, pass condition, and weight, and read the verdict rule (including the honest-outcome rule).
3. Read `references/evidence-guide.md` in full: note where each of the four evidence families lives.
4. Read the plan-context block first (eval mode) or `plan.md` with its `## Deviations` section (live mode), including the original repro evidence it builds on. Write down: the plan's in-scope file list, its stated boundary, any deviation notes, and every distinct outcome its test plan names. This is read before the diff on purpose — the diff and evidence are judged against this list, never the other way around.
5. Read the candidate PR's diff and commit list next (eval mode's PR section, or live mode's `git diff main...HEAD` and `git log`). List every changed file and hunk.
6. Read the candidate PR's test-evidence section (eval mode) or your own captured before/after and check output (live mode).
7. Read the candidate PR's title and description last, alongside the repo-facts block's template/policy asks (eval mode) or the repo's actual PR template, `CONTRIBUTING.md`, and the live thread highlights (live mode).

## Evidence gathering

For each check, the concrete pull:

- **plan-fidelity**: from step 4's file list and step 5's diff, line up every diff hunk against the plan's in-scope list one at a time. Any hunk with no match gets checked against the deviation notes next; only if neither covers it does it count against the check. Separately, read the description's own fidelity claims (step 7) sentence by sentence against the diff (step 5) — a claim is evidence for this check, not just narration.
- **test-evidence**: from step 4's list of named test-plan outcomes, check off each one against step 6's evidence one at a time. An outcome with no matching before/after, or one whose "after" is shown on a path the diff didn't change, is unmet. Separately confirm the repo's own stated checks (lint/typecheck/unit tests) appear in step 6 with real output, not just a claim.
- **diff-quality**: scan every hunk from step 5 for a debris tell (commented-out code, debug print/log statements, dead-code markers, pure-formatting hunks, an import or rename not required by the fix). Note the commit messages from step 5 as a secondary signal, never the sole basis for a grade.
- **standards-and-comms**: from step 7, check each template section the repo asks for against the description's actual content, check for any repo-specific required item (linked issue, checklist, changelog entry) in the description or diff, check for AI-disclosure if the repo's policy requires it, and check whether any explicit maintainer direction from the thread highlights is engaged in the description.

## Check execution

1. Execute the checks in the order listed in the rubric's table: `plan-fidelity`, `test-evidence`, `diff-quality`, `standards-and-comms`.
2. Grade each check `pass`, `fail`, or `unclear` using only the evidence pulled for that specific check. A strong grade on one check never excuses or softens a different check — a correct, well-evidenced fix still fails `diff-quality` if debris rides along, and still fails `plan-fidelity` if the diff does more than the plan says.
3. Apply the honest-outcome rule before failing `plan-fidelity` or `test-evidence` on a shortfall: if the gap is a deferral named in the plan's own note and restated in the description, grade that specific aspect `pass`, not `fail`.
4. If the evidence a check names is genuinely absent from the package, grade that check `unclear` and say exactly what's missing. Never infer what missing evidence would probably have shown.
5. Once a check's evidence has been pulled during gathering, it may be graded without re-reading the whole package again.

## Verdict assembly

1. Apply the rubric's verdict rule exactly: `accept` only if all four required checks grade `pass`.
2. Treat `unclear` as `fail` on every check, per the verdict rule.
3. When the verdict is `reject`, quote the evidence for the first-failing check in rubric-table order (`plan-fidelity` first, then `test-evidence`, then `diff-quality`, then `standards-and-comms`) as the lead of the readable summary, even if more than one check failed. Still report every check's grade in the output, not only the lead.
4. The same set of check grades must always produce the same verdict: the accept/reject mapping never depends on which particular check happened to fail, only on whether any required check failed.

# Procedure: how this skill grades a plan package

## Read order

1. (Live mode only) Read `scope.md` first. Confirm the issue is inside the scoped repo; note any house rule that changes how evidence is read here (a classmate's plan doesn't block this one; branch naming; never piggyback).
2. Read `rubric.md` in full. List every check's name, evidence source, pass condition, and weight, and read the verdict rule below the table.
3. Read `references/evidence-guide.md` in full. Note where each evidence family (diagnosis, scope, executability, test plan, honesty, comms) lives in a package.
4. Read the issue context first (title, body, thread highlights, and the repo-facts block, or the live issue page). Fix what the issue actually asks for before reading anything else — this is what "scope" and "thread-aware" are judged against.
5. Read the repro-evidence block next (eval mode) or the student's own posted unit-2 repro comment (live mode; the house repro pack if there is no posted comment of their own). Write down what behavior or cause this evidence actually establishes, and what it rules out, before reading the plan. The plan is judged against this note, never the other way around.
6. Read the candidate plan in full: diagnosis, scope statement, files/approach, test plan, risks/unknowns, and any `## Deviations` section.
7. Read the candidate plan comment last, next to the thread highlights and the repo-facts block's stated conventions and AI-disclosure policy.

## Evidence gathering

For each check, the concrete pull:

- **cause-grounded-in-evidence**: pull the plan's stated cause (it should be stateable in one sentence). List every artifact in the repro evidence one at a time (each control run, each before/after step, each timing or output comparison). Check the stated cause against each artifact individually; one contradicting artifact is enough to fail the check.
- **bounded-scope**: pull the plan's in-scope/not-in-scope statement and its full list of named files or areas. Pull the issue's own described ask from the issue context. Line up the two lists side by side.
- **executable-plan**: pull the plan's named files/locations and its stated approach. Scan for a hedge phrase that signals an undecided question ("or", "whichever is easier", "somewhere", "upstream or vendored", "maybe also"); any such hedge on a real implementation decision is enough to fail the check.
- **decisive-test-plan**: pull the plan's test-plan section and the repro evidence's own steps/artifacts. Check whether the test plan names a specific result tied to those exact steps, not just "run the suite" or a subjective feeling.
- **honest-uncertainty**: pull the plan's risks/unknowns/deviations section (if present) and compare its hedging against how certain the diagnosis and scope sections read elsewhere in the same plan. A flagged deferral with a reason is not a certainty claim.
- **thread-and-convention-aware**: pull the thread highlights (or the repo-facts block's note of explicit maintainer direction) and the repo's stated AI-disclosure policy. Compare both against the plan comment's actual text.

## Check execution

1. Execute the checks in the order listed in the rubric's table: `cause-grounded-in-evidence`, `bounded-scope`, `executable-plan`, `decisive-test-plan`, `honest-uncertainty`, `thread-and-convention-aware`. A wrong cause makes the rest largely moot, so it is graded first and its evidence quote leads the summary when it fails.
2. Grade each check `pass`, `fail`, or `unclear` using only the evidence pulled for that specific check in the step above. A strong grade on one check never softens or excuses a different check.
3. If the evidence a check names is genuinely absent from the package — not merely thin, but actually not present — grade that check `unclear` and say exactly what evidence is missing. Never infer what missing evidence would probably have shown.
4. Once a check's evidence has been pulled during gathering, it may be graded without re-reading the whole package again.

## Verdict assembly

1. Apply the rubric's verdict rule exactly as written: `accept` only if all six required checks grade `pass`.
2. Treat `unclear` as `fail` on every check, per the verdict rule — there is no required check this rubric lets `unclear` pass.
3. In the output, quote the one piece of evidence that decided each check (the fact named by that check's Evidence column). When the verdict is `reject`, lead the readable summary with the first-failing check in execution order and its quoted evidence.
4. The same set of check grades must always produce the same verdict: the accept/reject mapping never depends on which particular check happened to fail, only on whether any required check failed.

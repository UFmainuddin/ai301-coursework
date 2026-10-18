# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s3/pull/248

**Branch**

fix/61-wrap-select1-text

## Eval iterations

**Run history**

1. Smoke run, `--limit 4` (partial, 4 items): 4/4 agreement.
2. Full run, 20 items: **15/20** — below the bar (18/20). Five disagreements, all `clear-accept` packages (`pkg-02`, `pkg-05`, `pkg-13`, `pkg-16`, `pkg-19`) coming back `reject`, all failing `test-evidence`.
3. Targeted re-run, `--only pkg-02,pkg-05,pkg-13,pkg-16,pkg-19,pkg-04,pkg-07,pkg-10,pkg-14` with `--include-calibration` (partial, 9 items): **8/9**. Confirmed a loosened `test-evidence` (secondary test-plan outcomes need only be concretely addressed, not each given their own full transcript) fixed `pkg-02`, `pkg-05`, `pkg-13`, and `pkg-19` without breaking the `not-tested` canaries (`pkg-04`, `pkg-07`, `pkg-10`, `pkg-14`). `pkg-16` still disagreed.
4. Second targeted fix and re-run, `--only pkg-16,pkg-04,pkg-07,pkg-10,pkg-14` (partial, 5 items): **5/5**. Loosened the repo's-own-check clause of `test-evidence` to accept a named command with a stated passing result (rather than requiring a full output dump or test count). Fixed `pkg-16` without breaking the `not-tested` canaries.
5. Full run, 20 items: **19/20** — bar met, category floor clean, but a new disagreement: `pkg-20` (`standards-wall`) came back `accept`. Re-graded alone with `--only pkg-20`, it correctly `reject`ed (failed both `plan-fidelity` and `standards-and-comms`, matching gold) — confirming the full-run disagreement was model sampling noise on that run, not a rubric gap.
6. Full confirming run with `--save-run eval-run.txt`, 20 items: **19/20** — bar met (18/20), category floor clean across all 5 categories (`clear-accept 6/7`, `not-tested 4/4`, `silent-drift 4/4`, `standards-wall 2/2`, `unreviewable 3/3`). One disagreement on this run: `pkg-08` (`clear-accept`) came back `reject` on `standards-and-comms`. Re-graded alone with `--only pkg-08`, it passed cleanly on every check (matching gold `accept`) — the same sampling-noise pattern as step 5, on a different package this time. This is the run committed in `eval-run.txt`.

**Package analysis**

`pkg-16` (kubernetes/minikube#23471, category `clear-accept`). My rubric's verdict on the first full run: **reject** (failed `test-evidence`). Gold label: **accept**. The PR's test evidence shows a full before/after transcript for both named test-plan outcomes (the corrupt-tarball failure now exits 1 with a printed error, and a valid-tarball control still loads and exits 0), but names the repo's own check only as `` `go test ./pkg/minikube/machine/... ./cmd/...` passes `` — no test count, no pasted output, just the named command and a one-word result. My first-draft check required the repo's own checks to be "shown actually run, with real output," and the grader read "passes" with zero output as too close to an unverifiable claim, the same shape as the genuinely not-tested packages' vague assertions. But every other `clear-accept` package in the set happens to include a parenthetical count (`1462 passed, 12 skipped`, `312 tests`, `4108 tests`) — gold doesn't actually require that detail, it requires that a *specific* command was named and a result stated, as opposed to a *generic* claim like "tests pass" with no command named at all. `pkg-16` names its command precisely; it just doesn't also give a count, which turned out not to be the bar.

**Check rationale**

Quoted verbatim from the checks table in `rubric.md`:

```
| test-evidence | The PR's test-evidence section, read against every distinct observable outcome the plan's test plan names, and against the repo's own stated checks | The plan's primary repro/outcome is shown with a specific, observable before/after tied to the exact failing path the issue and plan name — not a different or unchanged code path, and not merely asserted ("tests pass", "verified working"). Every other outcome the plan's test plan names is at least concretely addressed — a quoted assertion, a named control's stated result, or its own transcript — rather than left completely unmentioned. The repo's own stated checks are named specifically (the exact command, e.g. `go test ./pkg/x/...`, not a vague "tests pass") with their result stated; a full output dump or test count strengthens this but is not required on its own — a named command plus a stated passing result is sufficient. A secondary outcome the test plan named but the evidence never mentions at all fails this check, even if the primary outcome is shown well. | required |
```

My first draft conflated two different things under one bar: showing a result for a named check, and showing *detailed output* for it. `pkg-16` (full transcripts for both named scenarios, but a bare "passes" for the test command) and the five clear-accept packages I fixed earlier in the same run (secondary outcomes referenced by a quoted assertion rather than their own transcript) both showed me the same lesson from two angles: the eval set's real bar is specificity (a named command, a named scenario, a quoted assertion) rather than volume (a full dump, a separate transcript for every clause). I rewrote the check to key on "named and stated" rather than "shown in full," while still keeping the hard line that a test-plan outcome mentioned nowhere at all (the actual `calib-04` and `not-tested` failure shape) still fails.

**Trade-offs**

Loosening `test-evidence` to accept a named command plus a stated result, instead of requiring full output, trades strictness for recognizing that terse-but-specific evidence is still decisive evidence — which is most of what real PRs look like. I re-ran the four `not-tested` packages (`pkg-04`, `pkg-07`, `pkg-10`, `pkg-14`) as canaries specifically because this is the check most likely to flip them back in: all four still fail, because each one's problem is the *primary* repro (missing entirely, or run on the wrong/unchanged path), which the loosened clause never touches — it only loosened how the *secondary* repo-check mention is judged. What this still misses: a PR that names a specific command and simply lies about the result ("`go test ./...` passes" when it doesn't) would read as compliant, since nothing in this rubric re-runs the command itself — the check can only grade what the package claims, not verify it independently. That's an accepted gap shared with every other check in this rubric: the tool trusts the artifact in front of it, the same as a human reviewer reading a PR description would.

Separately, two full runs each produced exactly one spurious disagreement (`pkg-20` on the first confirming run, `pkg-08` on the second) that disappeared when the same package was re-graded alone with identical rubric files — model sampling noise on borderline multi-check grading, not a rubric defect. I'm noting this rather than chasing a 20/20 that the grader's own variance makes unlikely to be stable from run to run; the bar (18/20 plus category floor) is designed to tolerate exactly this kind of noise, and both full runs after my fixes cleared it.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.

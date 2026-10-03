# Evidence guide: where proof lives in a reproduction package

## Environment

**Where it lives.** In an eval bundle: the repro report's own opening statement (often labeled "Environment" or "Setup") naming software version, OS, and install method, read against the issue's own stated version/OS in the "Issue" section and the repo-facts block's latest-release line. In live mode: the equivalent statement in the student's repro-report draft, read against the issue thread's reported version and the repo's current default-branch state (or latest release) on GitHub.

**What good looks like.** The report names the actual version and OS it ran, not just "the latest version." If that version differs from what the issue reports, the report says so in the same breath ("issue filed against 4.53.2; I tested 4.53.3") rather than letting the reader assume a match. Silence on environment, or a silent version mismatch the reader would have to notice on their own, is not good enough — the build or platform can change whether the bug even exists.

## Steps

**Where it lives.** In an eval bundle: the repro report's "Preparation"/"Execution" steps — the exact commands, input files, or config shown. In live mode: the equivalent section of the student's draft, read against whether every referenced file, config, or resource is something the draft itself shows or that is public on the issue's repo.

**What good looks like.** Every step is a concrete action a stranger could reconstruct exactly: an exact command line, and an input either shown inline or described with its exact field names and values (e.g., "a `dependencies:` list plus a `category:` section" is reconstructable precisely; "some extra config" is not). A stranger with nothing but the comment and a checkout of the repo could perform every step in order and land in the same state. A step that says "set up the project" with no commands, or that depends on a private repo, an unshared config file, or local state only the author has, cannot be followed by anyone else and fails this regardless of how convincing the surrounding prose is.

## Behavior shown

**Where it lives.** In an eval bundle: the artifact in the repro report's "Execution"/"Actual" section (the literal output, error text, exit behavior, or log excerpt), read side-by-side with the issue's own quoted error/behavior in the "Issue" section. In live mode: the artifact in the student's draft, read against the issue thread's description or the student's own earlier observation.

**What good looks like.** Read the artifact's actual text, not the report's framing of it. A pass means the artifact's error class, exit behavior, or symptom is the same one the issue names — a panic is a panic, not a graceful validation error with a different exit code; the specific named behavior (e.g., "Invalid path expression") is present, not a different error produced by an altered command. Formatting, length, and confidence are not evidence: a short report with a matching artifact passes, and a long, polished report whose own artifact shows something else (a different error, a process still alive instead of crashed, a test run on the wrong version) fails. An honest "I could not reproduce this" also counts as behavior correctly shown, provided the next section (Honesty) confirms it's stated as such rather than spun as success.

## Honesty

**Where it lives.** In an eval bundle: the repro report's stated conclusion ("Analysis"/"Expected vs Actual"/closing lines), read against what the Behavior-shown artifact actually contains. In live mode: the same, in the draft, read against its own artifact.

**What good looks like.** The words match the evidence exactly. "I reproduced it" is earned only when the artifact shows the issue's behavior; "I could not reproduce it" is a complete, fully honest answer when the report says what was tried and what came out differently instead (a different artifact, a different environment) — that is not a weaker result, it's the correct one to report. What fails: a confident diagnosis, root cause, or "verified" claim stated with no artifact backing it at all, or a conclusion that contradicts the artifact shown just above it (claiming a crash when the shown output is a normal exit, or stating "expected" and "actual" backwards from what the transcript shows).

## Comms

**Where it lives.** Two separate things, both read against the repo-facts block (eval) or the live repo's actual templates/CONTRIBUTING doc (live mode):

- The claim comment's own text, read for whether it names something specific to this issue versus being interchangeable with any issue.
- The repo-facts block's (or CONTRIBUTING.md's) stated AI-contribution/disclosure policy, read against whether the claim and repro comments disclose AI assistance where that policy requires it.

**What good looks like.** A claim comment names the specific symptom, file, or mechanism involved, and promises only an investigation and a report — never a fix, a guaranteed timeline, or a date. "I'd like to work on this" or a bare "+1"/self-assignment with no stated intent is boilerplate and fails, regardless of how good the repro report attached to it is. Separately: if the repo's stated policy requires disclosing AI-assisted work, a comment that is otherwise excellent but omits that disclosure still fails this check — disclosure is a hard requirement, not something good proof elsewhere can offset. If the policy is silent or explicitly permissive with no disclosure requirement, there is nothing to disclose and this passes without comment.

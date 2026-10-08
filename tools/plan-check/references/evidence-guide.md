# Evidence guide: where evidence lives in a plan package

## Diagnosis and grounding

**Where it lives.** In an eval bundle: the plan's "Diagnosis"/"Cause" section states what it believes is wrong, read against every artifact in the package's repro-evidence block — control runs, before/after output, timing steps, anything the reproduction actually showed. In live mode: the equivalent section of `plan.md`, read against the student's own posted repro comment from unit 2 (or the house repro pack).

**What good looks like.** The stated cause is consistent with the repro evidence — never something only the issue thread's discussion believes. A diagnosis is allowed to be an inference (a plausible mechanism explaining a version regression, a control result, or a cache/environment delta); it does not need the evidence to directly observe the mechanism itself, only for nothing in the evidence to rule it out. Read the evidence's own artifacts line by line against the claimed cause: if a control run already isolates a different cause (the behavior persists with the named mechanism held constant, or disappears with it unchanged), the diagnosis is wrong no matter how polished or thread-aligned it reads. A diagnosis that explicitly cites which evidence step supports it is strong; one that never engages the evidence at all, or that quietly steps around a contradicting step, is not grounded.

## Scope

**Where it lives.** In an eval bundle: the plan's explicit in-scope/not-in-scope statement and the specific files or areas it names, read against the issue's own described ask in the issue context. In live mode: `plan.md`'s scope section, read against the live issue thread.

**What good looks like.** The named changes map one-to-one onto what the issue asks for. A plan that narrows itself on purpose — defers related work, gates a fix to the one platform the issue reports, splits an adjacent symptom into its own issue — and says so with a reason is bounded, not creeping; that is what a mature scope statement looks like. A plan is scope creep when it adds work the issue didn't ask for: a rewrite, a migration, a new configuration option, a cross-cutting refactor, or a "redesign while I'm here," even when the actual one-line fix inside it is correct. The tell is in the list of named files/areas: one isolated fix location is bounded; a sprawl of unrelated modules, new abstractions, or a CI matrix is not.

## Executability

**Where it lives.** In an eval bundle: the plan's stated files/areas and its approach/order-of-work language. In live mode: the same, in `plan.md`.

**What good looks like.** A stranger reading only the plan could open an editor and start, because the real decisions have already been made: which subsystem or file area, and which single approach among the possible ones. Naming an exact file and line is the clearest form of this, but it isn't the only acceptable one: a plan that names the specific area and says precisely how the exact spot will be pinned down (for example, debug output that already shows where the problem originates) is still executable, because no real choice is left open — only a mechanical narrowing step. What fails is an unresolved choice: "fix the recover() call somewhere in the middleware" or "patch it upstream or in the vendored copy, whichever is easier" leaves the actual decision a plan exists to make to whoever builds it.

## Test plan

**Where it lives.** In an eval bundle: the plan's test plan section, read against the repro-evidence block's own steps and artifacts. In live mode: the same, read against the student's unit 2 repro steps.

**What good looks like.** A decisive test plan re-runs (or clearly references) the exact repro steps and names the specific, observable thing that should be different afterward — an exit code, a returned value, a log line, a response body field. "Run the full test suite" with no fix-specific outcome named, or a subjective outcome like "should feel fast" or "nothing else should feel broken," is not decisive even when the rest of the plan is solid — there's nothing in it a reviewer could check pass/fail.

## Honesty

**Where it lives.** In an eval bundle: the plan's stated risks, unknowns, and any deferred items, read against the certainty of its diagnosis and scope claims. In live mode: the same, plus any `## Deviations` section added after a build.

**What good looks like.** The plan says plainly what it hasn't verified and what it's choosing not to handle, and why. An honest "I'm deferring the Windows case because I can't test it here" is a pass, not a weakness. What fails is a claim stated as settled fact — "this fully resolves the issue," "no other code paths are affected" — that the repro evidence never actually established, with no hedge or flag that it's an assumption.

## Comms

**Where it lives.** Two things, both read against the repo-facts block (eval) or the live repo/thread (live mode):

- The plan comment's own text, read against the issue thread's highlights — explicit direction a maintainer already gave, or existing related PRs/discussion already in flight.
- The repo-facts block's (or `docs/CONTRIBUTING.md`'s) stated AI-contribution/disclosure policy, read against whether the plan comment discloses AI assistance where required.

**What good looks like.** A thread-aware comment engages with what's already there: it references the maintainer's stated direction, or the existing PR it isn't trying to race, rather than posting as if the thread were empty. A plan that is otherwise excellent but silently contradicts or ignores explicit maintainer direction already visible in the thread fails this regardless of its other merits. Separately, if the repo's stated policy requires disclosing AI-assisted work, the comment must disclose it — an excellent plan that skips a required disclosure still fails this check.

# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

---

## Posted upstream

**GitHub username**

UFmainuddin

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/61#issuecomment-6050634774

> My plan for this issue, building on the reproduction I posted above.
>
> **Diagnosis.** `api/routes/health.py:32` passes a raw string, `await db.execute("SELECT 1")`, which SQLAlchemy 2.x's textual-SQL coercion rejects before any database I/O happens. My repro's control run already showed the same session and database returning `1` cleanly once the identical SQL is wrapped in `text()`, so the database being reachable is independently confirmed in the same run that shows the probe failing.
>
> **Scope, one bounded change:** wrap the raw string in `sqlalchemy.text(...)` at `health.py:32`, add a regression test (`tests/unit/test_health.py` — none exists yet for this route), and narrow the `api.routes.health` mypy suppression in `pyproject.toml` by removing only the `call-overload` code its own comment attributes to this issue (leaving `attr-defined`, which is #62's, untouched). I checked with `grep -rn "\.execute(" api/ core/`: this is the only bare-string `execute()` call in either package, so it's a single-site fix.
>
> **Not in scope:** the separate `redis_health_check_failed` failure in the same response (`AttributeError` on `settings.redis_host`) is #62, and I'm leaving it alone.
>
> **Test plan:** re-run my repro's control script and `GET /health` against the same Docker Postgres service, before and after. I expect `dependencies.postgres` to flip from `"unhealthy"` to `"healthy"` with no `ArgumentError` in the log. One thing I'm flagging now rather than after the fact: the endpoint's overall `status`/HTTP code will likely still read `"unhealthy"`/503 after my fix, because the unrelated #62 Redis failure will still trip the route's overall verdict — that's expected and not a sign the fix didn't work.
>
> I know PR #82 already exists for this issue; I'll still open my own PR from my own branch per the house rules, built from my own reproduction and this plan.
>
> This plan was put together with Claude Code's help; I'm running and verifying every step myself before it goes up.

---

## Your branch

**Branch**

fix/61-wrap-select1-text

**Evidence**

Before (from my unit-2 reproduction, same Docker Postgres service):

```
$ curl -i http://127.0.0.1:8000/health
HTTP/1.1 503 Service Unavailable
...
{"detail":{"status":"unhealthy","dependencies":{"postgres":"unhealthy","redis":"unhealthy","vector_db":"healthy"},"safety_events_last_hour":0,"timestamp":"2026-10-03T03:12:24.985949"}}
```
Server log for that request:
```
2026-10-02 23:12:24 [error    ] postgres_health_check_failed   error="Textual SQL expression 'SELECT 1' should be explicitly declared as text('SELECT 1')" request_id=7c4bd9c4-8cdd-4cfd-b5f6-98806d1ec840
```

After (same Docker Postgres service, branch `fix/61-wrap-select1-text` checked out):

```
$ curl -i http://127.0.0.1:8000/health
HTTP/1.1 503 Service Unavailable
date: Thu, 08 Oct 2026 02:07:58 GMT
server: uvicorn
content-length: 182
content-type: application/json
vary: Origin
x-request-id: 6ee19be2-14b9-4311-a662-02642fb1eda3

{"detail":{"status":"unhealthy","dependencies":{"postgres":"healthy","redis":"unhealthy","vector_db":"healthy"},"safety_events_last_hour":0,"timestamp":"2026-10-08T02:07:59.275536"}}
```
Server log for that request:
```
2026-10-07 22:07:59 [debug    ] postgres_health_check_passed   request_id=6ee19be2-14b9-4311-a662-02642fb1eda3
2026-10-07 22:07:59 [error    ] redis_health_check_failed      error="'Settings' object has no attribute 'redis_host'" request_id=6ee19be2-14b9-4311-a662-02642fb1eda3
2026-10-07 22:07:59 [debug    ] vector_db_health_check_passed  request_id=6ee19be2-14b9-4311-a662-02642fb1eda3
```

`dependencies.postgres` flips from `"unhealthy"` to `"healthy"` and `postgres_health_check_passed` replaces `postgres_health_check_failed`, exactly as the plan's test plan predicted. The overall response is still `503`/`"unhealthy"` solely because of the separate, out-of-scope #62 Redis bug, also as predicted.

New regression test, before the fix (fails as expected — asserts a `text()` construct, pre-fix code passes a raw string):

```
$ .venv/Scripts/pytest tests/unit/test_health.py -v -m unit
tests/unit/test_health.py::TestHealthCheck::test_postgres_probe_uses_text_construct FAILED
tests/unit/test_health.py::TestHealthCheck::test_postgres_probe_reports_healthy_when_reachable PASSED
1 failed, 1 passed
```

Same test, after the fix:

```
$ .venv/Scripts/pytest tests/unit/test_health.py -v -m unit
tests/unit/test_health.py::TestHealthCheck::test_postgres_probe_uses_text_construct PASSED
tests/unit/test_health.py::TestHealthCheck::test_postgres_probe_reports_healthy_when_reachable PASSED
2 passed
```

Full checks, on the branch after the fix:

```
$ make test-unit
================ 377 passed, 53 xfailed, 3 warnings in 11.09s =================

$ make test-integration
collecting ... collected 0 items
============================ no tests ran in 0.18s ============================
make: *** [test-integration] Error 5

$ make lint
.venv/Scripts/ruff check .
All checks passed!

$ make typecheck
.venv/Scripts/mypy api/ core/ ingestion/ rag/ agent/ safety/
Success: no issues found in 76 source files
```

(`make test-integration`'s "no tests ran" is expected — Path Review has no integration tests yet, per the Unit 4 instructions.)

## Eval iterations

**Run history**

1. Smoke run, `--limit 4` (partial, 4 items): 4/4 agreement.
2. Full run, 20 items: **19/20** — bar met (18/20), category floor clean, but one disagreement: `pkg-14` (gold `accept`) came back `reject`, failing `cause-grounded-in-evidence` and `executable-plan`.
3. Targeted re-run, `--only pkg-14,pkg-01,pkg-07,pkg-11,pkg-16,pkg-10,pkg-17,pkg-18,pkg-09` (partial, 9 items): **9/9**. Confirmed the loosened wording on both checks fixed `pkg-14` (now `accept`) without flipping the `wrong-cause` canaries (`pkg-01`, `pkg-07`, `pkg-11`, `pkg-16`), the `unbuildable` canaries (`pkg-10`, `pkg-17`, `pkg-18`), or the `clear-accept` canary (`pkg-09`).
4. Full confirming run with `--save-run eval-run.txt`, 20 items: **20/20** — bar met, category floor clean across all 5 categories (`clear-accept 7/7`, `scope-creep 4/4`, `thread-convention 2/2`, `unbuildable 3/3`, `wrong-cause 4/4`). This is the run committed in `eval-run.txt`.

**Package analysis**

`pkg-14` (zellij-org/zellij#5174, category `clear-accept`). My rubric's verdict on the first full run: **reject** (failed `cause-grounded-in-evidence` and `executable-plan`). Gold label: **accept**. The plan's diagnosis (OSC color-query responses arriving as input during the reattach handshake, before being consumed) is an *inference* that explains the regression window (clean on 0.44.1, leaking from 0.44.2; clean cache gives one clean attach) — nothing in the repro evidence directly observes the internal mechanism, but nothing contradicts it either. My first-draft `cause-grounded-in-evidence` check read "evidence doesn't directly show this" as a reason to fail, when the eval set actually wants diagnoses to be judged by whether the evidence *rules them out*, not by whether it *proves* them outright — a diagnosis is always somewhat inferential at plan stage. Separately, the plan names the exact subsystem (the reattach handshake in `zellij-server`'s client connection handling) and says the exact function will be pinned down using a method it has already demonstrated working (`zellij --debug` output already shows the leak's origin); my first-draft `executable-plan` check read "exact function TBD" as an unresolved choice, the same shape as the genuinely unbuildable packages' hedges, without distinguishing a narrowing step already backed by a working method from an open decision between competing approaches.

**Check rationale**

Quoted verbatim from the checks table in `rubric.md`:

```
| cause-grounded-in-evidence | The plan's stated cause/diagnosis, read directly against every artifact in the package's repro evidence (control runs, before/after steps, timing or output comparisons) — not against what the issue thread believes or how confidently the plan is written | No artifact in the repro evidence contradicts or rules out the stated cause. A diagnosis is an inference explaining the evidence (what regressed, what a control isolated, what a cache/version delta showed) and does not need to be directly observed to pass — it only needs nothing in the evidence to rule it out. If a control run or comparison in the evidence actually isolates a different mechanism (the behavior persists with the blamed mechanism held constant, or is absent with it unchanged), this fails regardless of confidence or thread consensus. | required |
```

My first draft said a cause had to be consistent with the evidence and not "fail to support" it — vague enough that the grader treated an unproven-but-plausible inference as insufficiently supported. `pkg-14` showed that this punishes exactly the kind of diagnosis a plan is supposed to make: an explanation that accounts for a regression window and a control result, without the luxury of a smoking-gun artifact. I rewrote the check to key on *ruling out*, not *proving*: it fails only when a control or comparison in the evidence actually isolates a different cause, which is what every genuine wrong-cause package in this set (`pkg-01`, `pkg-07`, `pkg-11`, `pkg-16`, and the worksheet trap `calib-03`) actually does.

**Trade-offs**

Widening `cause-grounded-in-evidence` to "not ruled out" instead of "affirmatively proven" trades strictness for the ability to accept a reasonable inferential diagnosis — which is most diagnoses, at plan stage, before a fix is even written. I re-ran the four scored `wrong-cause` packages (`pkg-01`, `pkg-07`, `pkg-11`, `pkg-16`) as canaries specifically to confirm this didn't let a genuinely wrong diagnosis back in: all four still fail, because each one has a control run that actively contradicts the stated cause, which the widened check still catches. What it now accepts and will keep accepting: a diagnosis that is actually wrong but happens to have no directly contradicting artifact in the package — an untested hypothesis rather than a disproven one. That's an accepted gap: the check can only read the evidence the package actually contains, and a wrong guess the evidence is silent on looks the same to it as a right one. The same trade shows up in `executable-plan`: accepting a named subsystem plus an already-demonstrated pinning method (rather than requiring the exact function up front) means a plan could claim a debug method "already works" without it actually narrowing anything — nothing in this eval set tests that specific overclaim, so it's a gap I'm aware of rather than one I've verified against.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.

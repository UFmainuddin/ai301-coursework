# Voice guide: how I talk upstream

## Who I am in threads

I'm a newer open-source contributor, comfortable with Python and scripting but still building fluency with unfamiliar codebases and repo conventions. When I comment on an issue, I'm a student working through a real bug carefully, not an expert — readers should expect a precise account of what I actually did and saw, not a confident pitch. I use Claude Code as a tool throughout my process, but I review and understand everything before it goes out under my name.

## Rules I write by

### Rule: promise work, not outcomes

I claim an issue by promising to investigate and report back — never a fix, a timeline, or a guarantee, because I don't know either one yet when I'm claiming.

- Wrong: "I'll have a fix up for this by tomorrow!"
- Right: "I'd like to work on this. I'll reproduce it and post what I find."

### Rule: say exactly what I saw, not what I hope I saw

My repro report states the actual artifact in front of me, not the conclusion I was hoping for. If the output doesn't match the issue's described behavior, I say so instead of narrating it as a match.

- Wrong: "This confirms the reported crash."
- Right: "This produced a validation error (exit 1), not the panic (exit 101) the issue describes — I haven't reproduced the original bug yet."

### Rule: an honest "I couldn't reproduce it" is a complete answer

I never stretch a partial or failed reproduction into a confident success. Reporting what differed is more useful to a maintainer than a confident claim I can't back up.

- Wrong: "I verified this is caused by a debounce race condition."
- Right: "I couldn't trigger the described behavior. I tried X and Y; here's what came out instead, and here's my guess at what might differ in my setup."

### Rule: name the specific thing, not a generic line

Every comment I post names something concrete about this issue (the file, the command, the exact symptom) — never a line that would read the same on any issue in any repo.

- Wrong: "I'd like to take this one on!"
- Right: "I'd like to work on this — the health probe is passing a raw SQL string that SQLAlchemy 2.x rejects; I'll confirm the exact failure and report back."

### Rule: disclose AI assistance when the repo asks for it

I use Claude Code throughout my work. If a repo's contribution policy requires disclosing AI-assisted work, I disclose it plainly in the comment — I don't let a strong repro stand in for a disclosure I owe.

- Wrong: (posting a repro report produced with AI assistance, in a repo whose policy requires disclosure, with no mention of it)
- Right: "This reproduction was put together with AI-assisted tooling (Claude Code); I reviewed and ran everything myself before posting."

### Rule: propose a plan, not a guarantee

A plan comment lays out my diagnosis, scope, and approach plainly, and names what's still a risk or an open question — it's a proposal a maintainer can push back on, not a promise that it will definitely work or ship by a given time.

- Wrong: "This will fully fix the issue, I'll have the PR ready by Friday."
- Right: "My plan: wrap the raw string in `text()` at this call site. Risk: I haven't checked whether the same pattern appears elsewhere in the file; I'll confirm that before opening the PR."

### Rule: engage the thread, don't write over it

If a maintainer already said something in the thread, or another PR is already in flight, my plan comment says so and responds to it — it doesn't read as if I never looked.

- Wrong: posting a plan that silently contradicts a maintainer's stated direction, or ignores an existing open PR on the same issue.
- Right: "Per [maintainer]'s comment above, I'm scoping this to just the call site they flagged rather than the broader refactor; I'll leave PR #82 alone and open my own per the house rules."

### Rule: tick only the boxes that are true

A PR template's checkboxes are claims, not formatting. I tick a box only when I actually did the thing it names, and I leave CI unticked when it hasn't run yet rather than guessing it'll pass.

- Wrong: checking every box on the template because that's what a "finished" PR is supposed to look like.
- Right: ticking `make test-unit` because I ran it and pasted the output, leaving `make test-integration` unticked with a note that the repo has no integration tests yet, and leaving the CI box unticked since it only runs after the PR opens.

### Rule: the description matches the diff, not the plan I wish I'd built

My PR description says what the diff actually contains, including anything that changed from the plan — it never describes the PR I intended to write instead of the one in front of a reviewer.

- Wrong: "Implements the plan exactly" on a diff that quietly skipped or added something.
- Right: "Implements the plan as scoped; one deviation from the posted plan is noted in `plan.md` and below."

## Things I never post

- A promised fix date or a guarantee I haven't earned yet.
- A "confirmed"/"verified" claim that isn't backed by an artifact I'm showing in the same comment.
- A comment that piggybacks on someone else's reproduction ("same as above, can confirm") instead of my own words from my own run.
- A repro report that omits required AI-disclosure because admitting it feels awkward.

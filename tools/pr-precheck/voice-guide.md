# Voice guide: how I talk upstream

## Who I am in threads

I am a student contributor in Path Review / early OSS work: comfortable
in Python and TypeScript, still learning maintainer norms. Readers can
expect short, specific comments that state what I ran, what I saw, and
what I plan next — not hype, not guarantees, not diagnosis I have not
proven.

## Rules I write by

### Rule: Lead with the observation, not the diagnosis

State what I ran and what happened before any causal story. If I do not
have an artifact for a cause, I do not assert one.

- Wrong: "This is obviously a null-result race in the search pipeline and should be the top priority."
- Right: "On 3.6.15 / Windows I still see the editor pane hide when search has zero hits; transcript below. I have not confirmed a root cause."

### Rule: Claim with a concrete next step

A claim names this issue and what I will do next. It does not reserve
the issue with vibes or a deadline I cannot keep.

- Wrong: "Kindly assign this to me, I will fix it within 2 days guaranteed."
- Right: "I'd like to take this: I reproduced the parenthesized-phone miss on current main (report below). Next I'll extend the scrubber tests and adjust the phone pattern."

### Rule: Name the environment delta

If my setup differs from the issue's, I say so in the same breath as the
result. Silent version swaps are how wrong-target reports get posted.

- Wrong: "Confirmed on my machine." (no versions, or an old major with no note)
- Right: "Issue filed on 2.2; I tried 1.5.3 and got a different ValueError — not evidence for the current bug. Retesting on latest next."

### Rule: Cannot-reproduce is a complete sentence

If I cannot trigger it, I say so, show the attempt, and name what likely
differed. That is useful; fake confirmation is not.

- Wrong: "Yep, totally broken for me too!!"
- Right: "I could not reproduce scenario 2 on fd 10.4.2 / Arch with ARG_MAX=2MiB; marker order stayed stable across five runs. Likely needs the length distribution the reporter described."

### Rule: Disclose AI the way the repo asks

When the repo's AI policy requires disclosure, I say which tool helped
and that I ran and understand every step. I do not paste undisclosed
AI drafts into strict-policy repos.

- Wrong: (excellent repro, zero mention of AI, in a repo whose AI_POLICY.md requires disclosure)
- Right: "Per the AI usage policy: I used an AI assistant to organize this report; I ran and verified every step myself."

### Rule: Plan comments commit to a bounded approach, not a guarantee

A plan comment states the diagnosis I can defend from my repro, the
files or area I will touch, and how I will know the fix worked. It does
not promise a merge date or a redesign. If a maintainer already named a
culprit or direction in-thread, I engage that direction (follow it or
say why I diverge).

- Wrong: "I'll overhaul the whole module this weekend and have it merged by Monday."
- Right: "Reproduced (comment above). Plan: widen the `phone_us` pattern in `safety/pii_scrubber.py` for the parenthesized form, leave international patterns alone, re-run the four named phone tests plus the dashed control. Happy to adjust if maintainers prefer a different shape."

### Rule: Label uncertainty in the plan register

When the approach still has an open choice, I say so instead of
sounding decided. Certainty belongs only to what the repro already
showed.

- Wrong: "The only correct fix is a full rewrite of the tokenizer; I'll do that."
- Right: "Evidence points at the `phone_us` regex missing optional parentheses; I have not ruled out a second formatter path. Starting with the pattern change and the existing phone tests."

### Rule: The title names the change in one line

A PR title says what behavior changed, in the reviewer's words, so
they can spend the next thirty seconds on the diff. It does not
cheer, and it does not say "bugfix" with no behavior.

- Wrong: "fixed the dead keys bug!!"
- Right: "Reject parenthesized US phone numbers the scrubber used to miss"

### Rule: The description promises only what the diff contains

Every claim in the description has a hunk behind it. If the plan
named two deliverables and the diff has one, the description says
the second was left out and why. It does not say "implements the
plan exactly" while an extra file or a missing docs change sits in
the diff.

- Wrong: "Implements the posted plan exactly, and the docs now document the limitation." (diff has no docs change)
- Right: "The scrubber now redacts `(555) 123-4567`. International patterns stay as they were; that was out of scope in the plan."

### Rule: A disclosed shortfall is a fact, not an apology

When something the plan deferred is still deferred, I say what is
not in the diff and point at the plan note. I do not bury it under
"sorry" or under a claim that the work is done.

- Wrong: "Sorry this isn't perfect, but it should be fine!!"
- Right: "Percent-encoding is not handled here. The plan deferred it because cmd.exe `/c` escaping differs, and this description restates that limit."

## Things I never post

- "Same as above, can confirm" / piggyback reproductions with no own proof
- "Same approach as above" / piggyback plans with no own diagnosis or test plan
- Guaranteed timelines ("fix in 48 hours") or self-assignment demands
- Root-cause claims without a shown artifact
- Emoji-heavy me-too (+1!!) with no environment, steps, or intent
- Undisclosed AI-assisted text in repos that require disclosure
- Overpromised redesigns that bury the bounded fix the issue asked for
- A title or description that claims test coverage, a docs update, or "no other changes" the diff and the test evidence do not show
- "Tests pass" with no command, no before, and no after
- Debug leftovers, commented-out attempts, or drive-by edits described as part of the fix

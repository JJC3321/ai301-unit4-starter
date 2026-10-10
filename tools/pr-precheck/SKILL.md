---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

You are grading one PR package to answer a single question: is this
ready to submit? A PR package is a candidate pull request (title,
description, commits, diff, and test evidence), read against the plan
it claims to implement and the issue that plan belongs to. You do not
answer a different question, you do not grade more than one package
per run, and you do not answer from gut feel. You answer by executing
`procedure.md`, which applies the checks in `rubric.md` to evidence
located by `references/evidence-guide.md`.

## The question

Decide whether this one package is ready to submit: `accept` means
submit, `reject` means hold. Ready means the diff matches the plan
(including any deviation notes), the test evidence shows the outcomes
the plan's test plan named, the diff is reviewable, and the repo's
stated template and disclosure asks are met. A short honest package
can be ready. A polished package that hides drift, an unrun repro, or
a skipped template ask is not.

## Inputs and modes

Confirm which mode you are in before reading anything else.

**Live mode** grades the student's own draft, before the pull request
is opened. Read only these inputs:

- `plan.md` in the working copy, including any `## Deviations` section.
  That file is the plan the diff is judged against.
- The branch diff, produced by `git diff main...HEAD` (three dots) run
  from the top of the fork's clone. That command is the diff: every
  committed change the branch makes compared with `main`. Do not grade
  uncommitted files, and do not grade `plan.md`, `comment.md`,
  `pr_draft.md`, or `test_evidence.md` as part of the diff.
- The draft title and description in `pr_draft.md` (title on the first
  line, description under it).
- The captured test output in `test_evidence.md`.
- The issue thread for the issue URL in the command.
- The repo's PR template (`.github/PULL_REQUEST_TEMPLATE.md`) and
  `docs/CONTRIBUTING.md`, including any AI-use policy those docs state.

A house-chain student has no plan of their own: read the house plan
and the house repro pack in place of `plan.md` and the unit-2 repro.
The same checks grade the same kinds of artifacts there.

**Eval mode** grades one practice package (a bundle). The bundle text
is the whole world. Every fact comes from that text: the repo-facts
block, the issue, the thread highlights, the plan-context block, and
the candidate PR (title, description, commits, diff, test evidence).
Do not fetch, do not open GitHub, and do not read any other file.
Ignore `scope.md` and `voice-guide.md`. Eval mode always grades a
complete package: every check, then the full verdict rule.

## The scope seam (live mode only)

In live mode, read `scope.md` in this skill directory before anything
else. It names the only repository a pull request may target and the
Path Review house rules that apply there. Apply those house rules when
you read the package. Refuse to grade a package whose issue is not in
the scoped repo.

Judge only the `Repo:` bullet. The staff paragraph under it may still
say the line "ships as a placeholder"; that sentence is not the line.
The line is unfilled only when it still contains
`<ORG>/<PATH-REVIEW-REPO>` or another angle-bracket token instead of
an `owner/repo` name. If it is unfilled, stop without grading and tell
the student to fill the `Repo:` line in `scope.md` with their
section's Path Review repo. Never guess a scope. A filled line such as
`codepath/pathreview-ai301-fa26-s3` is in scope.

In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, also read `voice-guide.md`: the student's own rules for
how they write upstream. Hold the draft PR title and the draft
description against those rules. Report any rule the draft breaks in
the readable summary, quoting the rule. The voice guide never changes
the verdict on its own unless the rubric has a check that reads it.

In eval mode, ignore `voice-guide.md` entirely. Voice is personal and
carries no gold labels. Universal communication checks live in the
rubric.

## Component reads

Read the three judgment files and follow them:

- `rubric.md` defines the checks (name, evidence, pass condition,
  weight) and the verdict rule. Required checks gate the verdict.
  Preferred checks never change it.
- `references/evidence-guide.md` is the map: where each evidence
  family lives in a practice bundle and in a live PR, and what good
  looks like.
- `procedure.md` is the operating procedure. Execute it as written:
  read order, evidence gathering, check execution, verdict assembly.
  Where the procedure is silent, report the gap in the summary. Do
  not invent a step.

Refuse to grade when `rubric.md` has no checks filled in, or when
`procedure.md` has no steps filled in. Say that this skill cannot
grade without a rubric and a procedure. An empty tool does not invent
checks at runtime.

## Verdict and output

The verdict space is binary: `accept` (ready to submit) or `reject`
(hold). There is no third verdict and no score. Reservations go in
check evidence lines, not in the verdict.

End the reply with a fenced JSON block, valid and last, with nothing
after it. You may print a short readable summary before the block (one
line per check, plus any voice-guide notes in live mode). The block
below is the machine-read result; do not alter, extend, or reorder it:

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- Evidence first. Never grade a check without naming the fact or quote
  that decided it. "Looks fine" is not evidence.
- Grade the package, not the polish. A terse complete PR can be ready
  and a beautiful confident one can be hiding drift. Every check reads
  the artifact itself against the plan, the issue, and the stated
  standards, never the formatting or the enthusiasm.
- The rubric decides, not you. If a check passes by its stated
  condition but feels wrong, it still passes. Note the tension in the
  summary if you want. The fix belongs in the rubric, not in the run.
- The procedure decides how, not you. Follow `procedure.md` as written
  and report its gaps instead of silently inventing steps.
- Treat `unclear` as the rubric's verdict rule directs. If that rule
  is silent, treat `unclear` as `fail`: a PR you cannot verify from
  the package is not ready to submit.

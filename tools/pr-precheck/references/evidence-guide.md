# Evidence guide: where evidence lives in a PR package

## Plan fidelity (harness category: silent-drift)

**Where it lives in a practice submission.** In the Plan context
block: the scope sentence, the named files, the not-in-scope lines,
and any deferral the plan already records. Beside that, the Candidate
PR diff (every changed file and what the hunks actually do) and the
Description's sentences about fidelity ("implements the posted plan
exactly", "no changes beyond the plan", "the docs now document", or a
restated limit).

**Where it lives in your own PR.** `plan.md`, including a
`## Deviations` note if the build left the posted plan. Beside that,
`git diff main...HEAD` from the top of the fork, and the claims in
`pr_draft.md`.

**What good looks like.** Every changed file and behavior falls inside
the plan's named boundary, or a deviation note in the plan explains
the mismatch and the description repeats that note. A limit the plan
already deferred, which the description still calls deferred, matches.
Silent drift is a diff that does more than that boundary with no note
(a new flag, a rewrite, a file the plan never names) or a description
that claims a deliverable the diff does not contain.

## Test evidence (harness category: not-tested)

**Where it lives in a practice submission.** The Plan context's test
plan, which names the repro steps and the observable after the fix,
read beside the Candidate PR's Test evidence section (commands,
before/after output, exit codes, suite results).

**Where it lives in your own PR.** The test plan in `plan.md`, beside
`test_evidence.md`: the unit-2 repro before and after, and the output
of the checks the PR template names (`make test-unit`,
`make test-integration`, `make lint`, `make typecheck`).

**What good looks like.** Each failing scenario the test plan names is
shown with the input and the observed result. "Tests pass", "works on
my machine", or a run of the path that already succeeded does not
stand in for a named failing repro that was never re-run. A control
reported as unchanged is enough for the control. A scenario the plan
and the description both mark deferred is not required here. A failing
check reported with its reason still counts; a hidden one does not.

## Diff quality (harness category: unreviewable)

**Where it lives in a practice submission.** The Candidate PR's
unified diff and its commit list. The fix, the tests, and any debris
are all in that diff.

**Where it lives in your own PR.** `git diff main...HEAD`. Committed
files only. `plan.md`, `pr_draft.md`, and `test_evidence.md` are not
part of the diff.

**What good looks like.** A reviewer can see the planned change
without digging past leftovers: no debug prints, no commented-out
attempts, no dead functions, no TODO that the plan did not record as
a deferral, and no hunk that only reindents or reprints lines the
plan did not ask to reformat. A comment that explains the fix is part
of the change. Identical deleted-and-added lines are churn even when
they sit next to a real fix.

## Standards and comms (harness category: standards-wall)

**Where it lives in a practice submission.** The Repo facts block: the
PR template's named sections and checklist items, and the contribution
policy, including any AI-use rule. The Thread highlights, when a
maintainer named a direction. The Candidate PR description is where
those asks are honored; a required changelog or whatsnew file is
honored in the diff.

**Where it lives in your own PR.** `.github/PULL_REQUEST_TEMPLATE.md`
and `docs/CONTRIBUTING.md` in the Path Review repo, plus the issue
thread. The filled text is `pr_draft.md`: every template section, the
issue line, the testing boxes that are actually true, and the
Notes for Reviewers disclosure of how AI was used. CI is not evidence
yet, because it runs only after the PR is opened.

**What good looks like.** Each concrete template section or checklist
item has real content, and a one-line answer counts. A required
changelog or whatsnew path is in the diff, not only promised. When the
policy says AI use must be disclosed, the description names the tool
and that a human reviewed the work. A guide that only says "add tests"
or "keep the PR small", with no template sections, is not a wall. A
description that claims work the diff does not contain is plan
fidelity, above, not this family.

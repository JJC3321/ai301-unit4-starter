# Procedure: how this tool grades a PR package

## Read order

1. Confirm mode. In live mode, read `scope.md` first. If the `Repo:`
   line is still an unfilled placeholder, stop without grading and
   tell the student to fill it. If the issue is outside that repo,
   refuse. Note the Path Review house rules. In eval mode, skip
   `scope.md` and `voice-guide.md` and treat the bundle as the whole
   world.
2. Read `rubric.md`. List every check name, its evidence, its pass
   condition, its weight, and the verdict rule, including how
   `unclear` is treated.
3. Read `references/evidence-guide.md` so each family has a known home
   before gathering starts.
4. Read the package in this order, and note one concrete fact from
   each part before grading anything:
   1. Issue context: the behavior the PR claims to fix.
   2. Thread highlights: any maintainer direction, culprit, or open
      question. Note "none" when there is none.
   3. Repo facts: the PR template's named sections and checklist
      items, and whether the contribution policy requires AI
      disclosure.
   4. Plan context, before the diff: in-scope behavior, named files,
      not-in-scope lines, deferral or deviation notes, and the test
      plan's failing scenarios. In live mode this is `plan.md`,
      including `## Deviations`.
   5. Candidate PR title and description. Note what the description
      claims was done, what it says was left out, and which template
      sections it fills.
   6. Diff and commits, after the plan is already noted, so the
      comparison is plan-then-diff and not polish-then-guess.
   7. Test evidence, after the test plan is already noted.
5. Why this order: the load-bearing checks are side-by-side. The plan
   has to be in hand before the diff, the test plan before the
   evidence, and the template asks before the description. Reading
   the description first invites grading confidence instead of the
   artifacts.

## Evidence gathering

For each family, pull the fact from the place the evidence guide
names and record a short quote or concrete fact.

1. **Plan fidelity** — From the plan, list the named files, the
   in-scope change, and every not-in-scope or deferral line. From the
   diff, list each changed file and any behavior that is not that
   change (a new flag, a rename pass, a second file, a docs claim).
   From the description, quote the fidelity sentence ("exactly the
   plan", "docs now document", or a restated deferral). Record whether
   each mismatch has a plan note the description repeats.
2. **Test evidence** — From the plan's test plan, list each distinct
   failing scenario and the observable it names. From the test-evidence
   section, record the command or input and the observed result for
   each, or note that the section only says tests passed, only shows
   a control, or skips a named scenario. A deferred scenario the plan
   and the description both mark out of scope is recorded as not
   required.
3. **Diff quality** — From the diff, record any debug print,
   commented-out code, dead function, stray TODO, or hunk whose
   deleted and added lines are the same text. Also note when the only
   extra lines are comments that explain the fix. Do not treat commit
   subjects alone as debris; the diff is the evidence.
4. **Standards and comms** — From repo facts (live: the PR template
   and `docs/CONTRIBUTING.md`), list the concrete sections, checklist
   items, and any required disclosure. From the description, record
   which of those are filled with real words and which are absent.
   If a named artifact (changelog, whatsnew) is required, record
   whether that path is in the diff. From the thread, record whether
   a maintainer direction exists and whether the description engages
   it.

Live mode only: the diff is `git diff main...HEAD`. The title and
description are `pr_draft.md`. The outcomes are `test_evidence.md`.
The plan is `plan.md`. The thread, the PR template, and
`docs/CONTRIBUTING.md` come from the scoped repo. Then read
`voice-guide.md` and note any rule the title or description breaks.
Those notes go in the summary only.

Eval mode only: quote from the bundle sections. Do not fetch.

## Check execution

1. Grade checks in this fixed order so two executors match:
   `plan-fidelity`, `test-decisive`, `diff-reviewable`,
   `standards-met`, `thread-engaged`.
2. For each check, use only the gathered evidence that check's
   Evidence column names. Apply the pass condition to the outcome.
   Do not grade length, tone, or how many commits there are.
3. Assign exactly one grade:
   - `pass` — the pass condition is met by a named fact or quote.
   - `fail` — the pass condition is not met; quote the contradicting
     fact.
   - `unclear` — the evidence the check needs is genuinely absent
     from the package, not that you skipped looking.
4. If the plan and the diff are both present, `plan-fidelity` is
   `pass` or `fail`, never `unclear`. If a test-evidence section
   exists but omits a named failing scenario, `test-decisive` is
   `fail`, not `unclear`.
5. You may grade from the gathered notes without re-reading the whole
   package unless a note lacks the deciding quote. Then re-open only
   that section.
6. Write one evidence line per check: the fact or quote that decided
   the grade.

## Verdict assembly

1. Apply the rubric's verdict rule exactly. Accept only if every
   required check is `pass`. Any required `fail` or `unclear` yields
   `reject`. Preferred checks never change the verdict.
2. Treat `unclear` on a required check as `fail` for the verdict.
3. The deciding check is the first required check in rubric order
   that is `fail` or `unclear`. When none failed, there is no
   deciding failure. The readable summary names that check when the
   verdict is `reject`. Every check still gets its own evidence line
   in the JSON.
4. In live mode, add any broken voice-guide rules to the summary,
   quoting the rule. Voice notes do not change the verdict unless a
   rubric check reads them.
5. Emit the short summary (one line per check, plus voice notes in
   live mode), then the fenced JSON block with `item`, `checks`, and
   `verdict`, and nothing after that block.

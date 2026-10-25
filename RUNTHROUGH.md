# gstack full run-through in the public lab (Spec Driven Build)

Goal: build, with the Spec Driven Build skill, a lean reference for slim instruction files (a slim AGENTS.md template and a slim skills.md template), and cover ALL 57 gstack skills while doing it. Tier A and B run for real, Tier C is skipped with a reason. Every skill gets a line in `RESULTS.md` (skill, tier, worked / partly / failed / skipped, why, who did it, who critiqued it). Nothing is dropped silently.

## Setup (replaces the toy repo)
- L = `~/orca/projects/agent-setup-lab`: a real PUBLIC repo with a real remote, the wiki's rules and lint copied unchanged (no loosening). The session starts in L. The old toy folder `~/orca/projects/gstack-trial` is retired; read its `index.html`, `greet.py`, `test_greet.py` only as fixtures to copy.
- W = `~/orca/projects/wiki-lean-build` (worktree of the curated wiki, branch `omarchygit/lean-agents-skills`): the only place the two deliverable files are built. Obey the wiki `AGENTS.md`. Never touch the main clone.
- Step 0 is done: `uv` 0.13.5, `specify` 1.2.3, `/careful` on (check `specify version`).

## How work happens in L
- One branch per stage: `run/<stage>-<skill>`. Never commit on `main`. Stage explicit paths only (rules 6 and 7: no `add -A`, `stash`, `reset --hard`, `checkout .`, `clean`, `--no-verify`).
- Run `python3 -I scripts/lint.py --root .` and `python3 -I scripts/lint_diff.py --base $(git merge-base origin/main HEAD) --root .` before every push.
- Public means permanent. Before EVERY push run the redaction scans (secrets, then `/home/`, IPs, names, emails) and fix every real hit. Push only `run/*` branches, then open a PR; Sir merges. Never push `main`.
- Every collision between a tool and a rule, every failure and every surprise becomes a GitHub Issue in L (`gh issue create`, label `finding`): what ran, what the rule said, what happened, severity. Do not weaken a rule to get past it; record it and stop.
- If a command would be destructive and `/careful` warns, do not proceed; log it.
- Read-only skills may point at W by absolute path and never write there: office-hours, spec, plan-ceo-review, plan-eng-review, plan-devex-review, autoplan, review, cso, test-audit, health, devex-review, diagram, learn, retro, context-save, context-restore, landing-report, careful, freeze, unfreeze, guard.
- Skills that commit, push, merge or edit code (ship, document-generate, document-release, qa, investigate, deslop-shared-libs) run only in L on a `run/*` branch. Plant bugs only in L.
- Spec Kit files (`.specify/`, speckit skills) live in L on branch `run/speckit-init`; they never go to W. Whether L's lint accepts them is itself a finding.
- No pushing or PR to the wiki from this run. The two wiki PRs (templates first, then `skills/git-spec-kit/`) are written by hand later with Sir's OK, citing the lab's issues.

## Stop after every stage
After EVERY stage (each skill, one at a time) STOP. Say what it produced and where (paths), write its line in `RESULTS.md`, and ask: "Continue here, or hand this to opencode or Pi?" Wait. If handed over, give an exact prompt (do the stage or only critique, file paths, read `~/.local/share/gstack-parked/gstack/<skill>/SKILL.md`). Afterwards read their output, fix or rebut each point, show Sir both lists, record "did it" and "critiqued by", and only then ask about the next stage. Never apply a critique silently. Final summary: columns "did it" and "critiqued by", one line per agent on better or worse.

## Stage order
0 done -> 1 constitution (Spec Kit, in L) -> 2 specify: office-hours, plan-ceo-review, Spec Kit specify, gstack spec only to compare -> 3 plan: Spec Kit plan, then plan-eng-review, plan-devex-review, plan-design-review, autoplan -> 4 tasks (test-audit read-only) -> 5 implement the two template files in W; test = wiki lint + checklist -> 6 converge -> 7 review, cso, health, devex-review (on W), qa-only, qa (on L) -> 8 ship (on L, up to but not including merge) and a manual PR description for W. Tier B after.

## Tier A (real runs)
1. Idea and spec: office-hours, spec (no issue filed), plan-ceo-review, plan-eng-review, plan-design-review, plan-devex-review, autoplan (list its "taste" decisions)
2. Build + test: the two template files in W; investigate (planted bug in L), test-audit, deslop-shared-libs
3. Check: review, qa-only, qa (planted bug in L), cso, health, devex-review
4. Docs and context: document-generate, document-release, diagram, make-pdf, context-save, context-restore, retro, learn, landing-report
5. Design (fixture `index.html` copied into L): design-consultation, design-html, design-review
6. Safety: careful, freeze, unfreeze, guard (show one blocked action each, then off)
7. Ship: ship, on L, read first, stop before merge

## Tier B (real runs after A)
benchmark, benchmark-models, canary (against a local page served from L), connect-chrome, open-gstack-browser, design-shotgun, eval-plan, plan-tune, skillify.
Model rule: any compare/outside-voice step uses Claude models only (Sonnet 5.5, Haiku 5.5 as subagents, Opus 5.5 if a third arm is wanted); no cost stop. Still ask first for any non-Claude provider, new API key, new sign-in or separate paid account; record a skip or substitution.

## Tier C (skip, record the reason)
land-and-deploy, setup-deploy (need a deploy target); ios-clean, ios-design-review, ios-fix, ios-qa, ios-sync (need an iOS app and device); setup-gbrain, sync-gbrain (install a CLI and database); pair-agent; scrape, setup-browser-cookies (real sessions and cookies); codex (OpenAI CLI and account); gstack-upgrade (changes the install); gstack's own browse (not registered, name clash); `_gstack-command` (extra, router only).

## End
Summarize `RESULTS.md` by tier, then list: failed skills and why; every file written outside L; anything that asked for sign-in, a key or payment; the count and list of Issues opened.

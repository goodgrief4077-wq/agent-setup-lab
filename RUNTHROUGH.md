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

## Extra shelf skills folded in (read from `~/.agents/skill-shelf/<name>/SKILL.md`; shelf skills, not gstack)
- `receiving-code-review`: governs EVERY critique from opencode or Pi. Read the whole critique first, restate each point, verify it against the files, then fix or rebut with reasons; one item at a time, test each; if any item is unclear, stop and ask before implementing any. No performative agreement ("you're absolutely right"). This is the mechanism behind "never apply a critique silently".
- `context-audit`: at Step 1 (constitution) and again before the W template PR, run its checklist only for the AGENTS.md items: no duplicated tables, command hierarchy current, no dead tools, kept lean. Its other files (MEMORY, TOOLS, USER, HEARTBEAT, SOUL) are private-setup specific; skip them and say so.
- `dogfood`: run beside `qa-only` in Stage 7 as a second QA method (5 phases: plan, explore, document, report) on the fixture `index.html`. It names Hermes browser tools (`browser_navigate` and so on) that this setup does not have; use the existing `browse` skill (agent-browser) instead and record the substitution as a finding. Output goes in a `dogfood-output/` folder in L; screenshots must not hold personal data.
- `bmad`: compare only. Its own gotcha says to use ONE planning method per project, so do not run it as a second planner; at Stage 3 write a short comparison (BMad vs Spec Kit vs gstack plan reviews) from its SKILL.md and the upstream README.
- Not on this machine, so not used: Context7 (library-docs MCP). Check first and ask before installing anything (pnpm only, never npx).

## Ranked open-source candidates (found 2026-10-25 with `pnpm dlx skills find "<query>"`, skills 1.7.2; installs as reported by skills.sh)
Use: for each step, run the gstack skill AND read the open-source candidate's SKILL.md from its GitHub repo (read, do not install), then record in `RESULTS.md` which was better and why. Always pnpm, never npx. Installing any of these needs Sir's OK first (check what is already on the machine and say what you found). Treat install counts as a popularity hint, not proof of quality; verify the source repo before trusting a skill.

| Step | Candidate (owner/repo@skill) | Installs | Compare with |
|---|---|---|---|
| Spec and plan | addyosmani/agent-skills@spec-driven-development | 64.1K | Spec Kit specify, plan |
| Plan challenge | mattpocock/skills@grill-me | 1.4M | plan-ceo-review, office-hours |
| Plan challenge | addyosmani/agent-skills@doubt-driven-development | 56K | autoplan |
| Code review | mattpocock/skills@code-review | 791.6K | review |
| UI review | vercel-labs/agent-skills@web-design-guidelines | 780.4K | design-review |
| Browser QA | vercel-labs/agent-browser@agent-browser | 1.2M | qa-only, qa, dogfood |
| UI flow test | software-mansion/argent@argent-test-ui-flow | 26.2K | qa |
| Security | addyosmani/agent-skills@security-and-hardening | 69.2K | cso |
| Security | getsentry/skills@security-review | 21.1K | cso |
| AGENTS.md | mattpocock/skills@writing-for-agents | 468.6K | slim-agent-instructions, context-audit |
| Library docs | upstash/context7@find-docs (OFFICIAL Context7 repo, MIT) | 9.4K | Spec Kit plan research step |
| Retro | mattpocock/skills@retro | 277.5K | retro |
| Retro | bmad-code-org/bmad-method@bmad-retrospective | 9.8K | retro |
Security queries "security audit" found nothing; "security review" returned off-topic cloud and chain skills (azure, solana, golang); "owasp" gave the two rows above. Changelog and docs queries gave only weak, unrelated hits, so document-generate and document-release have no ranked rival.

Context7 (official, github.com/upstash/context7): the MCP server and the `find-docs` skill. Its setup (`pnpm dlx ctx7 setup --claude`, Node 22.13+) opens an OAuth sign-in and creates an API key. That is a new sign-in, so ask Sir first; if he agrees, log the whole setup as a `finding` Issue (what it changed, what it stores, how to remove it with `ctx7 remove`). The README has no privacy policy and says its docs are community-contributed and unverified. A third-party `intellectronica/agent-skills@context7` (11.2K) exists; prefer the official one.

## End
Summarize `RESULTS.md` by tier, then list: failed skills and why; every file written outside L; anything that asked for sign-in, a key or payment; the count and list of Issues opened.

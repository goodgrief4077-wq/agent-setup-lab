# agent-setup-lab

A public lab for testing a lean, secure setup for AI coding agents, in the open.

The rules are copied unchanged from [ai-agents-wiki-second-brain](https://github.com/goodgrief4077-wq/ai-agents-wiki-second-brain) (`AGENTS.md`, `scripts/`). Agents here run real tools (the gstack skill pack, GitHub Spec Kit) against a real repo with a real remote. Nothing is loosened, so every collision between a tool and a rule is a finding.

## How it works
- Agents work on branches and open pull requests. The owner reviews and merges. Nothing is pushed to `main` by an agent.
- Each problem becomes a GitHub Issue (for example, "a skill tried `git add -A`, blocked by rule 6").
- `RESULTS.md` is the running table of what was run, what worked, and what failed.
- Findings worth keeping go to the curated wiki as a normal pull request that cites the issues here.

## Safety
Public means permanent. Secret scanning and push protection are on, `main` is protected, and every file is scanned for secrets and personal data before it is pushed.

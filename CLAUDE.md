# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repository is

This is not an application codebase. There is no build, lint, or test tooling. The directory holds three agent skills under `.agents/skills/`:

- `agent-factory-content-strategy`: written here. A content strategy skill for the Panaversity team's book *The AI Agent Factory* (agentfactory.panaversity.org).
- `content-strategy`: vendored from the GitHub repo `coreyhaines31/marketingskills` (path `skills/content-strategy/SKILL.md`).
- `skill-creator`: vendored from the GitHub repo `freestylefly/canghe-skills` (path `skills/skill-creator/SKILL.md`).

The two vendored installs are tracked in `skills-lock.json`. The repo is pushed to `github.com/Techfemme123/Techfemme-s-skills`.

## Layout

- `skills-lock.json`: lock file from the skills installer. It records each vendored skill's source repo, path, and a `computedHash` of the installed content. If you edit vendored skill files, the hash will no longer match upstream, and a reinstall or update may overwrite your changes. `agent-factory-content-strategy` is not in the lock file, so the installer never touches it.
- `.agents/skills/<name>/SKILL.md`: each skill. YAML frontmatter (`name`, `description`, and sometimes `metadata.version`) followed by the instructions. The `description` field decides when the skill triggers, so keep its trigger phrases intact when editing.
- `.agents/skills/<name>/references/`: longer material that `SKILL.md` links to and loads on demand. Keep detail here and leave `SKILL.md` as the main spine.
- `.claude/skills/<name>`: Windows directory junctions to `.agents/skills/<name>`, so Claude Code loads the skills. Edit the files under `.agents/`. The junctions pick up changes on their own. They are gitignored, so after a fresh clone, recreate each one in PowerShell from the repo root: `New-Item -ItemType Junction -Path .claude\skills\<name> -Target .agents\skills\<name>`.
- `.agents/skills/*-workspace/`: test-run output from skill-creator (generated outputs, `grading.json`, `benchmark.json`, `review.html`, `feedback.json`), organized as `iteration-N/<case-name>/<with_skill|without_skill>/`. `agent-factory-content-strategy-workspace/` is committed so its test results are on GitHub. Other workspaces, such as `content-strategy-workspace/`, are gitignored and kept local.

## The agent-factory-content-strategy skill

- `SKILL.md` covers grounding, goals and the learner journey, pillars, formats, language choice, claims hygiene, and the output format. It builds on the generic `content-strategy` skill's frameworks and takes precedence over it for anything about the book.
- `references/book-map.md`: a dated snapshot (28 October 2026, corpus generation 82) of the book's sections and crash courses with URLs, mapped to pillars. The book changes often. Refresh this file from the Agent Factory System of Record tools (`outline_agent_factory`) or the live site rather than from memory, and update the date line when you do.
- `references/bilingual.md`: English / Roman Urdu / Urdu guidance. Its Urdu examples show register only; a fluent Urdu writer should review any Urdu copy.
- `references/claims.md`: facts that need checking before publishing, such as certification rules, fees, market figures, and live counters. Each fact carries the date it was checked. Keep those dates and the reported-versus-announced distinctions whenever you touch the figures.
- `evals/<case-name>/`: test cases in the `claude plugin eval` folder format (`prompt.md`, `case.yaml`, `graders/NN-*.md`). There are three cases: `pcao-f-certification-campaign`, `roman-urdu-foundations-series`, and `thesis-linkedin-thought-leadership`. When you change the skill's behavior, update the matching graders so they still describe what the skill should do.

## Conventions inside content-strategy

- The skill looks for product context in `.agents/product-marketing.md`, falling back to `.claude/product-marketing.md` and then the legacy `product-marketing-context.md`, before it asks the user any questions.
- Bold names in `SKILL.md` such as **copywriting**, **seo-audit**, **social**, **launch**, **link-building**, **ai-seo**, and **programmatic-seo** point to sibling skills in the upstream marketingskills repo. Those skills are not installed here.
- Some numbers in the skill come from dated sources and are labeled as directional, for example the Foundation Inc. backlink study from March 2026. Keep the attribution and caveat whenever you touch those figures.
- `evals/evals.json` holds 8 eval cases in the older format (`prompt`, `expected_output`, `assertions`, `files`). When you change the skill's behavior, update the matching assertions.

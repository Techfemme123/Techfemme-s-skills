# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repository is

This is not an application codebase. There is no build, lint, or test tooling. The directory holds one installed agent skill, `content-strategy`, vendored from the GitHub repo `coreyhaines31/marketingskills` (path `skills/content-strategy/SKILL.md`). The install is tracked in `skills-lock.json`.

## Layout

- `skills-lock.json`: lock file from the skills installer. It records each skill's source repo, path, and a `computedHash` of the installed content. If you edit vendored skill files, the hash will no longer match upstream, and a reinstall or update may overwrite your changes.
- `.agents/skills/content-strategy/SKILL.md`: the skill itself. YAML frontmatter (`name`, `description`, `metadata.version`) followed by the instructions. The `description` field decides when the skill triggers, so keep its trigger phrases intact when editing.
- `.agents/skills/content-strategy/references/`: longer material that `SKILL.md` links to and loads on demand (`content-distribution.md`, `headless-cms.md`). Keep detail here and leave `SKILL.md` as the main spine.
- `.claude/skills/content-strategy`: a Windows directory junction to `.agents/skills/content-strategy`, so Claude Code loads the skill. Edit the files under `.agents/`. The junction picks up changes on its own.
- `.agents/skills/content-strategy/evals/evals.json`: 8 eval cases. Each has `prompt`, `expected_output`, `assertions`, and `files`. When you change the skill's behavior, update the matching assertions so they still describe what the skill should do.

## Conventions inside the skill

- The skill looks for product context in `.agents/product-marketing.md`, falling back to `.claude/product-marketing.md` and then the legacy `product-marketing-context.md`, before it asks the user any questions.
- Bold names in `SKILL.md` such as **copywriting**, **seo-audit**, **social**, **launch**, **link-building**, **ai-seo**, and **programmatic-seo** point to sibling skills in the upstream marketingskills repo. Those skills are not installed here.
- Some numbers in the skill come from dated sources and are labeled as directional, for example the Foundation Inc. backlink study from March 2026. Keep the attribution and caveat whenever you touch those figures.

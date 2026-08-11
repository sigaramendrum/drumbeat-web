# CLAUDE.md — `drumbeat-web` (public landing page)

*Auto-loaded at the start of every session in this repo. `AGENTS.md` should be a symlink to this
file. **This repo is public.** Everything committed here is world-readable, permanently, including
anything you delete in a later commit.*

## What this repo is

A minimal public "coming soon" landing page. Plain HTML, served by GitHub Pages, no framework, no
build step. That is the design, not a stage it is growing out of.

## The rule that matters most

**Nothing strategic, proprietary, or customer-identifying goes in this repo.** Not in HTML, not in a
comment, not in a commit message, not in a filename, not in an issue.

Specifically prohibited:

- roadmap, pricing model, or GTM strategy
- pilot customer names, logos, quotes, or counts
- architecture, stack choices, vendor names, or anything from `drumbeat-app`
- competitive analysis, or any framing of a competitor
- internal metrics, ARR figures, targets, or forecasts

All of that lives in the private `drumbeat` repo. When in doubt, it does not go here. A leak is not
recoverable by a follow-up commit: git history is public and mirrors exist within seconds.

## Before you commit

1. `git diff --cached` and read every line as a stranger would.
2. Ask what the diff reveals about the business even if each line looks innocuous. A page section
   titled "For RevOps teams at $10-100M ARR" leaks the segment. A commented-out testimonial leaks a
   pilot.
3. Commit messages are public too. "fix copy per Arun's pricing call" leaks a pricing call.

## Guards

Secret scanning is armed (`gitleaks`, plus a pre-commit hook). Treat a finding as a stop, not a
warning to tune away. If the hook is not installed in your checkout, install it before your first
commit — a guard that is present in the repo but not running is not a guard.

Adding a dependency, a build step, or a framework is a decision, not a chore: it introduces
third-party code into a public artefact. Record it as an ADR in the `drumbeat` repo first.

## Keep it boring

No analytics that sets cookies without a banner. No third-party script tags. No fonts fetched from a
CDN that logs visitor IPs. Each of those is a privacy commitment made on the company's behalf, and
this is a pre-launch page for an EU-residency product.

## Session-start protocol

1. `git fetch --prune origin && git checkout main && git pull`.
2. `git rev-list --count HEAD..origin/main` should return 0.
3. Confirm you are in `drumbeat-web` and not `drumbeat` before writing anything with substance in it.

## Source of truth

| Question | Authority |
|---|---|
| What may appear publicly | this file, and the private `drumbeat` repo for anything strategic |
| Brand, copy, positioning | `20_strategy/` in the private `drumbeat` repo |
| Whether a dependency may be added | an ADR in `drumbeat/30_build/decisions/` |
| What the product does | `drumbeat-app` — and almost none of it belongs on this page |

This repo is a **publication surface, not a source**. Nothing originates here except markup.

## Design principles

1. **Public by default means careful by default.** Assume every commit is captured on push.
2. **No dependencies.** Zero third-party code in a public artefact is a feature. Each addition is a
   supply-chain and privacy decision.
3. **No data collection without a decision.** Analytics, fonts and embeds all disclose visitor IPs to
   a third party. This product sells EU residency; the page should not contradict it.
4. **Boring and static beats clever.** No build step to break, no framework to patch.
5. **Say less than you could.** The page's job is credibility and a contact route, not persuasion.

## Definition of Ready (before making a change)

- The exact copy is approved and sourced from the private repo, not drafted here.
- The change adds no dependency, script tag, or remote asset — or has an ADR that permits it.
- You have confirmed you are in `drumbeat-web`, not `drumbeat`.

## Definition of Done (before pushing)

- `git diff --cached` read line by line as a stranger would read it.
- Nothing prohibited by the disclosure list above, including in the commit message.
- Secret scanning green, and the pre-commit hook actually installed in this checkout.
- The page renders over HTTPS with no console errors and no external network requests.
- No new dependency, and `README.md` still describes the repo accurately.

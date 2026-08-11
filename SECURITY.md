# SECURITY.md — `drumbeat-web` (public site)

*This repository is public and served to the internet. Engineering controls live in
`drumbeat-app/SECURITY.md`; this document covers what is specific to a public artefact.*

## Reporting a vulnerability

Email **sarvesh.sathish@sigaramendrum.com** with a description and a reproduction. Do not open a public
issue. Acknowledgement within 2 business days; a fix or documented mitigation within 30 days for
anything exploitable.

**In scope:** this repository and the site it serves — content injection, a hostile dependency, a
leaked artefact in git history, or a privacy problem in what the page loads. **Out of scope:** missing
headers with no demonstrated impact, and scanner output without a working example.

## The primary risk is disclosure, not exploitation

The page is static HTML with no backend, no authentication, and no user input. There is very little to
exploit. What there is to lose is **information**, and git history is permanent and mirrored within
seconds of a push.

Treated as a security incident, not a content mistake:

- pilot customer names, logos, quotes or counts
- pricing, roadmap, or GTM strategy
- anything about the architecture, stack, or vendors of `drumbeat-app`
- internal metrics, ARR figures, targets or forecasts
- a commit message that reveals any of the above

A later commit does not undo this. Assume anything pushed is captured.

## Controls

**Secret scanning** is armed, with a pre-commit hook and a CI job. A finding is a stop. If the hook is
not installed in your checkout, it is not protecting you — install it before your first commit.

**Dependencies.** The page has none, deliberately. Adding one puts third-party code into a public
artefact and needs an ADR in the `drumbeat` repo first. No CDN script tags, no remotely-hosted fonts,
no analytics that sets a cookie without a banner — each of those is a privacy commitment made on the
company's behalf, for a product that sells EU data residency.

**Content review before push.** Read `git diff --cached` as a stranger. Ask what the diff reveals
about the business even when each line looks harmless: a section headed for a named segment discloses
the segment, and a commented-out testimonial discloses a pilot.

**Transport and headers.** The site is HTTPS-only. Where the host allows it, set a
`Content-Security-Policy` that permits only self-hosted assets, plus `X-Content-Type-Options`,
`Referrer-Policy` and `X-Frame-Options`. With no scripts and no third-party origins these cost
nothing and remove whole classes of finding.

## If something leaks

1. Email sarvesh.sathish@sigaramendrum.com — do not discuss it in an issue or a public commit.
2. Do not force-push to "remove" it. Assume it is already captured, and preserve the history for the
   assessment.
3. The decision about disclosure or customer notification belongs to Sarvesh and Arun, and gets a
   `DECISION` stub in the `drumbeat` repo.

# Security policy

## Reporting a vulnerability

Email **sarvesh.sathish@sigaramendrum.com** with a description and, where relevant, a reproduction.
Please do not open a public issue, and please do not include live credentials in the report.

We aim to acknowledge within 2 business days and to fix or document a mitigation for anything
exploitable within 30 days. These are targets, not a contractual SLA: this is a small pre-launch team
with a single reporting mailbox and no on-call rota. We would rather state that plainly than publish a
commitment we cannot yet demonstrate.

We do not run a paid bounty. We will credit you unless you ask us not to.

**In scope:** this repository and, once it is published, the site it serves — content injection, a
hostile dependency, a secret or artefact exposed in git history, or a privacy problem in what the page
loads.

**Out of scope:** missing HTTP headers with no demonstrated impact, scanner output without a working
example, and denial of service by volume.

## Current state

The site is **not deployed**. GitHub Pages is not enabled for this repository, so there is no live
origin to test. The repository contains static HTML with no backend, no authentication, no user input
and no dependencies.

## What we are actually protecting

Because there is no server and no user data, the realistic risk here is **disclosure, not
exploitation**. The repository is public and git history is permanent and widely mirrored, so anything
committed should be assumed captured on push.

We treat the following as a security matter rather than a content mistake: customer or pilot
identities, pricing, roadmap, internal metrics, and details of the product's architecture or vendors —
including in commit messages and filenames.

## Controls, and their limits

**Secret scanning** runs in CI and in a pre-commit hook. We are explicit that this is a speed bump
rather than a control: the scanner's configuration lives in the repository, so a change can weaken it
in the same commit being scanned. This was verified, not assumed. GitHub push protection is the control
that does not travel in the diff.

The pre-commit hook only runs in a clone that has `core.hooksPath` set, so it protects a checkout only
after it has been installed there.

**Dependencies.** The page has none, deliberately. Adding one introduces third-party code into a public
artefact and requires a recorded decision first. There are no CDN script tags, no remotely-hosted fonts
and no analytics.

**Transport and headers.** When the site is published it will be HTTPS-only. A restrictive
`Content-Security-Policy` permitting only self-hosted assets, plus `X-Content-Type-Options`,
`Referrer-Policy` and `X-Frame-Options`, costs nothing here given there are no scripts and no
third-party origins.

## If something is exposed

1. Email the address above. Do not discuss it in an issue or a public commit message.
2. Do not force-push to remove it. Assume it is already captured, and preserve the history so the
   exposure can be assessed accurately.
3. Rotate anything credential-shaped immediately, whether or not it looks live.

The decision about disclosure or customer notification is made by the repository owners, not by
whoever finds it.

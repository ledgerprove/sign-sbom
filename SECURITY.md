# Security Policy

LedgerProve takes security seriously. This document explains how to report
vulnerabilities, what we consider in scope, and what to expect when you do.

## Reporting a vulnerability

**Please do NOT open a public GitHub issue for security problems.**

Email: **security@ledgerprove.com**

PGP key available on request — reply to a confirmation message and we'll send
ours so you can re-encrypt sensitive details.

When reporting, include if possible:

- A clear description of the issue
- Steps to reproduce (proof-of-concept code, request payloads, screenshots)
- Affected component (action source, action runtime, the LedgerProve API,
  the verification site, the docs site)
- Your assessment of impact (what an attacker could do)
- Affected versions (action tag/SHA, browser, OS)

You can submit anonymously. We will not pursue legal action against good-faith
researchers who follow this policy.

## What we'll do

- **Acknowledge your report within 72 hours** (usually within one business day).
- Triage and confirm the issue, with you in the loop.
- For valid issues, we'll agree on a coordinated disclosure timeline. Default
  is 90 days from triage; we'll publish sooner if a fix is ready and 90 days
  is not needed for downstream patching, and we'll request more time only if
  remediation genuinely requires it.
- Credit you in the advisory if you'd like (or keep you anonymous if you
  prefer).
- For exploitable vulnerabilities in the action itself (i.e. that affect
  customers' CI), we will issue a GitHub Security Advisory and a patched
  release before public disclosure.

## Scope

### In scope

- This repository (`ledgerprove/sign-sbom`) — the action source, the bundled
  `dist/`, the action.yml manifest, release artifacts and tags.
- The LedgerProve API at `https://api.ledgerprove.com` and the verification
  endpoints under `/verify/*` and `/.well-known/public-key.pem`.
- The web app at `https://ledgerprove.com`.
- The docs site at `https://docs.ledgerprove.com`.
- The GitHub App "LedgerProve" (its permission model and the secret-writing
  flow).

### Specifically interesting attack vectors

- **Tag hijacking** — anything that would let a non-owner re-point `v1`
  or any release tag.
- **Action input handling** — argument injection via crafted `repo-id`,
  `commit-hash`, `sbom-file` paths.
- **Supply chain** — Syft install integrity, transitive npm dependencies in
  `dist/`, anything that would let a malicious actor inject code into a
  customer's CI by compromising our distribution path.
- **Signature forgery / chain manipulation** — anything that would let
  someone produce a record that verifies against our public key without
  having gone through our signing flow, OR that would let someone modify
  an existing record without breaking verification.
- **Credential exfiltration** — anything that exposes a customer's
  `LEDGERPROVE_API_KEY` or our internal AWS keys.
- **SSRF / outbound webhook abuse** — using our outbound webhook feature to
  reach private/cloud-metadata addresses (we have a guard but report any
  bypass).
- **Authentication / impersonation** — flaws that let one user act as
  another, or that escalate a non-admin to admin.

### Out of scope

- Denial of service from raw flood (we rate limit at the edge).
- Self-XSS that requires the user to paste hostile content into their own
  console.
- Missing security headers on the marketing site that don't change the
  exploit surface (we accept reports but won't issue a bounty for these).
- Vulnerabilities in third-party services we depend on (Stripe, GitHub, AWS,
  Cloudflare) — please report these to the relevant vendor first.
- Vulnerabilities in dependencies that are already known and unpatched
  upstream — we'll track them and upgrade when an upstream fix lands.
- Issues requiring physical access to a user's device, MITM on a network
  we don't operate, or social-engineering of LedgerProve staff.

## Bounty

We do not currently operate a paid bounty programme. We do publicly credit
researchers, ship fast, and keep the disclosure window honest. As we grow we
intend to formalise a programme — if you'd like to be notified when we do,
mention it in your initial report.

## Hall of fame

We list researchers who report valid vulnerabilities here, with their
permission. (Empty for now — be the first.)

## Encryption keys

If you need to send us cryptographically sensitive material, our security
team will respond to your initial email with a current PGP public key. We
do not pre-publish a key here so we can rotate it without breaking
this document.

## Other contacts

- General security questions, not vulnerabilities: `hello@ledgerprove.com`
- Privacy / GDPR / data subject requests: `privacy@ledgerprove.com`

— LedgerProve Ltd, registered in England and Wales

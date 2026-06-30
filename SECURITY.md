# Security Policy

## Reporting a vulnerability

**Do not open a public GitHub issue for security problems.**

If you find a security issue in this starter (or in the Buildbase platform it deploys), report it privately:

- Email **support@buildbase.app** with details and reproduction steps.
- Alternatively, use GitHub's **[Report a vulnerability](../../security/advisories/new)** (private advisory) on this repo.

We'll acknowledge your report and keep you updated on the fix.

## A note on leaked secrets

This repo is intentionally secret-free — only `.env.selfhost.example` (placeholders) is tracked, and `.env.selfhost` is gitignored. If you ever notice a real credential committed here (an `INSTALLATION_API_KEY`, password, or key), please report it via email above so it can be rotated and purged. **Any exposed `INSTALLATION_API_KEY` should be rotated in the dashboard immediately** — treat it as compromised.

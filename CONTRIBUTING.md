# Contributing to Buildbase Self-Hosted Starter

Thanks for helping improve the starter! This repo is open to contributions from anyone via the standard **fork → branch → pull request** flow.

## Ground rules

- **Never commit secrets.** No real `INSTALLATION_API_KEY`, passwords, or `.env.selfhost`. Only `.env.selfhost.example` (placeholders) belongs in git. Push protection is enabled and will block obvious secrets, but you are the first line of defense.
- **Keep it doc-grounded.** The compose file, env vars, ports, and nginx config mirror the official self-hosting docs (https://docs.buildbase.app/self-hosted/overview). Don't invent config the platform doesn't support — if the docs don't cover it, say so in the PR.
- **One focused change per PR.** Small, reviewable diffs merge faster.

## How to contribute

1. **Fork** this repo to your account.
2. **Clone** your fork and create a branch:
   ```bash
   git clone git@github.com:<you>/self-hosted-starter.git
   cd self-hosted-starter
   git checkout -b fix/short-description
   ```
3. **Make your change.** If you touched the compose file, validate it:
   ```bash
   docker compose -f docker-compose.selfhost.yml config >/dev/null && echo "compose OK"
   ```
4. **Test it actually boots** (when relevant) using the quick start in the [README](./README.md).
5. **Commit** with a clear message and **push** to your fork.
6. **Open a Pull Request** against `main`. Fill in the PR template.

## What happens next

- `main` is protected: PRs require **1 approving review**, passing conversation resolution, and **linear history**.
- We merge with **squash** (or rebase) to keep history clean — your PR becomes one tidy commit.
- A maintainer will review. Be ready for follow-up questions or change requests.

## Good first contributions

- Fixing typos or unclear steps in the README
- Improving comments in `docker-compose.selfhost.yml` / `nginx-lb.conf`
- Adding a troubleshooting tip you hit during your own deploy
- Updating image tags when a new Buildbase release ships (note the version in the PR)

## Reporting bugs & asking questions

- **Bug in the starter?** Open an [issue](../../issues/new/choose).
- **Self-hosting *usage* question?** Check the [docs](https://docs.buildbase.app/self-hosted/overview) first; for help, contact **support@buildbase.app**.
- **Security issue?** Do **not** open a public issue — see [SECURITY.md](./SECURITY.md).

## License

By contributing, you agree your contributions are licensed under the repo's [MIT License](./LICENSE).

# Contributing to QueryProxy

Thanks for thinking about contributing. This document is the org-wide default; individual repositories may override it with their own `CONTRIBUTING.md` that adds project-specific guidance (build commands, repo layout, architecture notes). Where both exist, both apply — see [`QueryProxy/QueryProxy/CONTRIBUTING.md`](https://github.com/QueryProxy/QueryProxy/blob/main/CONTRIBUTING.md) for the portal.

## TL;DR

1. Open an **issue first** for anything non-trivial. We would rather discuss before you spend time.
2. Read the [Code of Conduct](CODE_OF_CONDUCT.md).
3. Fork, branch, code, test, open a pull request.
4. Be patient with reviews — this is a side-project for the maintainers.

## How we accept contributions

QueryProxy uses the **inbound = outbound** licensing model. By opening a pull request you agree that your contribution is licensed to the project under the **AGPL-3.0-or-later** license that governs the repository. There is no separate Contributor License Agreement (CLA) and no Developer Certificate of Origin (DCO) sign-off is required.

If your employer or another party may claim rights over your contribution, please confirm with them before submitting.

## Issues

Before opening an issue:

- Search existing **open and closed** issues — you may not be the first to hit it.
- For bugs, prefer the **bug report** template and fill in every field. The exact SQL (redacted), the deployment method, the target database engine and the QueryProxy version save us hours.
- For features, prefer the **feature request** template and describe the motivation first, the proposal second.
- For "how do I…" or "is this supported?" questions, start with the [documentation](https://queryproxy.com/docs/). If it does not answer, open an issue with a `question:` prefix in the title.

**Never open a public issue for a security problem.** See [SECURITY.md](SECURITY.md).

## Pull requests

- Keep PRs **focused**. One logical change per PR. Refactors that touch many files should be split into preparatory PRs and the substantive change.
- Write the PR description with a **summary** and a **test plan**. The PR template captures both.
- Reference related issues with `Fixes #N` or `Refs #N`.
- Run the project's test and lint commands locally before pushing. CI will run the same checks.
- Update the `CHANGELOG.md` entry under *Unreleased* when the change is user-visible.

## Security-sensitive changes

Some code in QueryProxy decides whether a statement reaches a production database or whether PII reaches a browser. Changes to any of these need **an explicit note in the PR description explaining the threat-model impact**, plus tests that demonstrate the new behavior:

- the SQL guards (`SqlInspector`) — statement classification, `WHERE` enforcement, `LIMIT` clamping, forbidden statements,
- authorization — roles, policies, team isolation, connection grants, self-approval prevention,
- webhook verification — Slack signatures, the Teams HMAC endpoint, replay windows,
- masking (`Masker`) — rule matching and the point at which results are written,
- credential storage and anything that formats a connection, an exception or an audit record.

Treat the existing test suites for these paths as a contract: add cases, do not relax them. A PR that loosens a guard to make a test pass will be sent back.

## Coding style

- **PHP:** run `./vendor/bin/pint` before committing. CI rejects unformatted code.
- **Tests:** every behavior change comes with a Pest test (`php artisan test`). Aim for the smallest test that demonstrates the issue.
- **Structure:** class-based Livewire components in `app/Livewire`, domain logic in `app/Services/<Area>/`, enums in `app/Enums`, one migration per feature.
- **Comments:** explain *why*, not *what*. Code should be readable on its own.
- **Commit messages:** written in English, imperative mood, explaining the reason for the change. Keep them free of tool signatures and attribution trailers.

## Product scope

The product scope is defined by the PRD and the ADRs in the SSOT repository, and summarized publicly on [queryproxy.com](https://queryproxy.com). Some deliberate non-goals — a hosted version, an analytics gateway, replacing network-level controls — are decisions rather than gaps. If your proposal moves one of those lines, say so explicitly in the issue; it is a conversation worth having, but not one to discover halfway through a review.

## Questions

Open an issue in the relevant repository with a `question:` prefix, or email **info@queryproxy.com** for anything that needs to be off-channel.

Thank you for helping us improve QueryProxy.

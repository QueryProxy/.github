# Governance

QueryProxy is a small, maintainer-driven open source project. This document says plainly who decides what, so contributors know what to expect before they invest time.

## Roles

| Role | Who | What they do |
|------|-----|--------------|
| **Maintainer** | [@muhammetsafak](https://github.com/muhammetsafak) | Final say on scope, architecture, releases and security triage. Reviews and merges pull requests. |
| **Contributor** | Anyone who opens an issue or a pull request | Proposes changes, reports bugs, improves documentation. No formal process to join — the first accepted PR makes you one. |

There is currently a single maintainer. If that changes, this file changes with it.

## How decisions are made

- **Bug fixes** — merged on technical review. If the behavior was wrong, the fix stands on its own.
- **Features** — discussed in an issue before implementation. A feature is accepted when it fits the product scope defined in the PRD and ADRs, does not weaken the security model, and does not add an external dependency that a self-hosted deployment cannot avoid.
- **Architecture** — captured as an Architecture Decision Record. Existing ADRs (the Laravel monolith, the database-backed queue, cursor-based streaming, HMAC webhook verification, AGPLv3) are decisions, not defaults; reopening one means arguing against its recorded context, which is a fair thing to do in an issue.
- **Security** — the maintainer triages privately and decides the disclosure timeline together with the reporter, following [SECURITY.md](SECURITY.md).

Disagreement is resolved by discussion in the open. When no consensus emerges, the maintainer decides and records the reasoning in the issue.

## Deliberate non-goals

These are settled decisions, not unimplemented features:

- **No hosted version.** A system that sees every production query is not one to hand to a third party.
- **Not a network access control.** Bastion hosts and VPNs control who can reach the database; QueryProxy reads the statement. It composes with them rather than replacing them.
- **Not an analytics gateway.** Reporting workloads belong on a read replica, not in an approval queue.
- **No mandatory external services.** A default installation must work with SQLite and a database-backed queue, with no Redis, no message broker and no outbound network calls.

A proposal that crosses one of these lines is not forbidden — it just needs to argue the trade-off explicitly.

## Releases

Releases are cut by the maintainer, tagged `vX.Y.Z`, and documented in each project's `CHANGELOG.md` following [Keep a Changelog](https://keepachangelog.com/). Versions follow [SemVer](https://semver.org/), with the pre-1.0 caveat that the `0.x` line may change the configuration and API surface between minor versions. Security fixes ship on the latest release line only.

## Licensing

All projects are licensed **AGPL-3.0-or-later**, on an inbound = outbound basis: contributions are licensed under the same terms as the repository, with no CLA and no DCO sign-off. See [CONTRIBUTING.md](CONTRIBUTING.md).

Relicensing would require the agreement of every copyright holder. There is no contributor agreement that would allow the maintainer to relicense contributions unilaterally, and that is intentional.

## Code of conduct

Participation in every QueryProxy space is governed by the [Code of Conduct](CODE_OF_CONDUCT.md). The maintainer is responsible for enforcement; reports go to **info@queryproxy.com**.

## Changing this document

Open a pull request. Governance changes are merged after discussion in the open, never silently.

# `.github` — Organization defaults

This repository holds the community health files and templates that apply to **every repository under the [QueryProxy](https://github.com/QueryProxy) organization** by default, plus the organization profile page itself.

> If a file in this repository conflicts with a file at the same path in a specific repository, **the file in that repository wins**. This repository provides defaults; individual repositories can override.

## What is here

| File / directory | Purpose |
|------------------|---------|
| [`profile/README.md`](profile/README.md) | The organization profile page shown at <https://github.com/QueryProxy>. |
| [`CODE_OF_CONDUCT.md`](CODE_OF_CONDUCT.md) | Code of conduct (Contributor Covenant v2.1) applied across all QueryProxy projects. |
| [`CONTRIBUTING.md`](CONTRIBUTING.md) | Default contribution guidelines: licensing, PR conventions, coding style. |
| [`SECURITY.md`](SECURITY.md) | How to report a vulnerability privately, and what counts as one in a security tool. |
| [`SUPPORT.md`](SUPPORT.md) | Where to ask questions, report bugs, and request features. |
| [`GOVERNANCE.md`](GOVERNANCE.md) | Who decides what, and how product scope is set. |
| [`FUNDING.yml`](FUNDING.yml) | GitHub Sponsors configuration. |
| [`ISSUE_TEMPLATE/`](ISSUE_TEMPLATE/) | Default issue templates (bug report, feature request, documentation) and the chooser config. |
| [`PULL_REQUEST_TEMPLATE.md`](PULL_REQUEST_TEMPLATE.md) | Default pull request template. |
| [`LICENSE`](LICENSE) | AGPL-3.0-or-later, the license these files are published under. |

## How overrides work

GitHub looks for a file in this order when a repository event needs one:

1. `repo/.github/<file>` — repository-specific override.
2. `repo/<file>` — at the repository root.
3. `org/.github/.github/<file>` — this repository (with a `.github/` prefix).
4. `org/.github/<file>` — this repository (for `CONTRIBUTING.md`, `SECURITY.md`, etc.).

In practice: **add a file to a specific repository to override the default here**. `QueryProxy/QueryProxy`, for example, ships its own `CONTRIBUTING.md` and `SECURITY.md` with project-specific detail (build commands, guard-bypass scope) — those take precedence, and the files here cover everything they do not say.

The profile page is the one exception: `profile/README.md` only works from this repository, and only from the default branch.

## Editing the profile page

[`profile/README.md`](profile/README.md) is public marketing copy, not documentation. Two blocks go stale fastest and are marked with comments in the file:

- the **release badge and announcement block** — update on every release of `QueryProxy/QueryProxy`,
- the **Projects table** — update when a repository is added, renamed, or changes status.

Documentation links point at <https://queryproxy.com/docs/>; check them after a site restructure.

## License

The content of this repository is released under the **AGPL-3.0-or-later** license, matching the rest of the QueryProxy organization.

## Maintainers

If you find an outdated link, typo, or unclear policy in any of these files, please open an issue or a pull request.

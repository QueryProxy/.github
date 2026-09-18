# Getting Support

Different questions belong in different places. Picking the right channel gets you a faster answer.

## Start with the documentation

Most setup and configuration questions are answered at **<https://queryproxy.com/docs/>**:

- [Quick start](https://queryproxy.com/docs/quickstart/) — Docker in about two minutes.
- [Manual installation](https://queryproxy.com/docs/manual-installation/) — PHP 8.3+ without Docker.
- [Configuration](https://queryproxy.com/docs/configuration/) — every environment variable.
- [SQL guards](https://queryproxy.com/docs/sql-guards/) — what gets rejected, clamped or blocked, and why.
- [Approval workflow](https://queryproxy.com/docs/approval-workflow/) — roles, decisions, and what is recorded.
- [Data masking](https://queryproxy.com/docs/data-masking/) — rule types and strategies.
- [Slack](https://queryproxy.com/docs/slack-integration/) and [Teams](https://queryproxy.com/docs/teams-integration/) integration.

Wondering how QueryProxy relates to a tool you already run? The [comparison pages](https://queryproxy.com/compare/) cover Bytebase, Teleport, CloudBeaver and a plain bastion host.

## "How do I…?" / general questions

If the documentation does not answer it, open an issue in the relevant repository with a **`question:` prefix** in the title. (Discussions are not enabled yet; when they are, this section will point there instead.)

Examples:
- "Can a DBA approve for a team they are not a member of?"
- "How do I point the result store at S3 instead of the local disk?"
- "Which masking strategy should I use for a column that is partially needed?"

A question that turns out to be a documentation gap becomes a docs issue — those are welcome and easy to fix.

## Bug reports

Use the **bug report** issue template in the affected repository. Include:

- QueryProxy version and how you deployed it (Docker image, Compose, manual PHP).
- The target database engine and version, if the bug involves a query.
- The exact SQL that triggered it, **with literal values redacted**.
- What you expected vs. what happened, with the relevant log output.

Never paste connection credentials, API keys, Slack signing secrets or real production data into an issue.

## Feature requests and proposals

Use the **feature request** issue template. Substantive proposals are most useful when they describe:

1. **Motivation** — what is hard or impossible today.
2. **Proposal** — how it could work, even if rough.
3. **Alternatives** — other ways the same problem could be solved, including "do nothing".

Some non-goals are deliberate — there is no hosted version, QueryProxy is not an analytics gateway, and it does not replace network-level access controls. Proposals that cross those lines are still worth raising; just name the trade-off up front.

## Security vulnerabilities

Do **not** open a public issue. Follow [SECURITY.md](SECURITY.md) and email **info@queryproxy.com**. Guard bypasses, masking leaks and authorization holes belong in that channel, not in the tracker.

## Direct contact

For anything that does not fit the above — commercial use, integrations, sponsorship questions, anything off-channel — email **info@queryproxy.com**. Please keep public technical questions in public channels so others can benefit from the answer.

## What we cannot help with

- Tuning, debugging or recovering **your** database. QueryProxy sits in front of it; it does not administer it.
- Generic Laravel, PHP or Docker questions unrelated to QueryProxy.
- Slack or Microsoft Teams administration and app-approval processes inside your own tenant.
- Compliance certification. QueryProxy produces audit evidence; it does not make anyone compliant on its own.

## Response times

This is a maintainer-driven open source project. We respond as we can:

- **Security reports:** see [SECURITY.md](SECURITY.md) for target windows.
- **Bug reports with a reproduction:** usually within a week.
- **Feature requests:** when we have time and energy.
- **Pull requests:** depends on size and reviewability. Smaller, well-scoped PRs land faster.

If you have not heard back in two weeks and the issue is real, a polite bump on the thread is welcome.

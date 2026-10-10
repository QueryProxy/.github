# Security Policy

This document applies to **all repositories under the [QueryProxy](https://github.com/QueryProxy) organization**. Individual repositories may publish a project-specific `SECURITY.md` that supplements this document — [`QueryProxy/QueryProxy`](https://github.com/QueryProxy/QueryProxy/blob/main/SECURITY.md) does, with the scope details for the portal itself.

QueryProxy is a security tool. A defect here is not only a bug in an application — it can be an unreviewed statement reaching a production database, or unmasked PII reaching a browser. We take reports seriously and we would rather hear about a false alarm than miss a real one.

## Supported Versions

QueryProxy is on a pre-1.0 `0.x` line. Security patches are issued for the latest released version only; there is no backporting to older minor versions yet.

| Version | Supported |
|---|---|
| Latest release | Yes |
| Older versions | No |

Please upgrade to the latest release before reporting, and expect the fix to ship as the next release rather than as a patch to an older line.

## Reporting a Vulnerability

Do not report security vulnerabilities through public issues, pull requests or discussions.

Report vulnerabilities privately through GitHub Private Vulnerability Reporting: open a private security advisory for the affected repository at `https://github.com/QueryProxy/<REPO>/security/advisories/new`, where `<REPO>` is the name of the repository. See GitHub's guide to [privately reporting a security vulnerability](https://docs.github.com/en/code-security/security-advisories/guidance-on-reporting-and-writing-information-about-vulnerabilities/privately-reporting-a-security-vulnerability).

If you cannot use GitHub, send an email to `info@muhammetsafak.com` with the subject line `[SECURITY] <REPO>`.

Please include:

- the affected version or versions, the deployment method (Docker image, Compose, manual PHP) and the target database engine and version where relevant,
- steps to reproduce the issue, including a minimal proof-of-concept if possible — for a guard bypass, the exact SQL string is the most useful thing you can send,
- the impact of the vulnerability, including your assessment of severity (low / medium / high / critical) — feel free to disagree with our final triage,
- a suggested fix, if you have one.

## Response Process

- You will receive a response within 7 days of your report.
- The report is assessed and you are informed of the result.
- After a fix is released, a GitHub Security Advisory is published, describing the issue and noting the affected versions and the fix.
- If you wish, you are credited for the report in the advisory and the release notes. Let us know your preferred name and, optionally, a contact URL.

We follow coordinated disclosure: we will not publish details before a fix is available, and we ask you to do the same.

This is a maintainer-driven open source project, not a commercial product with an SLA. Beyond the 7-day first response, we will make a genuine best effort but cannot promise fixed timelines for fixes.

## Scope

### Especially sensitive areas

Findings in these paths are treated as security issues by default, and a working proof-of-concept is welcome:

- **SQL guard bypasses** (`SqlInspector`) — an `UPDATE` / `DELETE` without a `WHERE` that passes inspection, `LIMIT`-clamp evasion, forbidden-statement evasion, comment or dialect tricks that change what the parser sees versus what the server executes.
- **Authorization** — crossing team isolation, reading or acting on a connection that was never granted, self-approval of one's own request, or a role acting outside its permissions (Admin / DBA / Developer / Auditor).
- **Webhook authentication** — Slack signature verification, the Teams HMAC action endpoint, replay windows, and anything that lets a party who is not the mapped approver produce an approval.
- **Masking** — any path where unmasked PII reaches the result store, the result viewer, a CSV export, or a chat notification.
- **Credential storage** — connection secrets appearing in logs, audit metadata, error messages, exception traces or debug output.
- **Audit integrity** — an action that executes without a corresponding immutable audit record, or a record that can be altered after the fact.

### Known trust assumptions

These are documented design limits rather than vulnerabilities. Reports that describe a way around them are still welcome; reports that restate them are not new findings.

- A DBA can approve any request on a connection their team owns. QueryProxy limits and records that power; it does not remove it.
- Anyone holding the Teams action secret can produce an approval on behalf of a mapped user. This is why the Teams action endpoint is treated as a lower-trust channel than Slack, where identity comes from Slack's own signed payload.
- Whoever controls the host controls the instance — the encryption key, the result store and the audit database all live there.

### Out of scope

The following are generally not considered security issues for the purposes of this policy:

- Vulnerabilities in third-party dependencies (report them to their maintainers).
- Vulnerabilities in the target database engines themselves (report to the vendor or upstream project).
- Vulnerabilities in Slack or Microsoft Teams as platforms (report to them directly).
- Issues that require an already-compromised host, an already-compromised administrator account, or physical access.
- Findings that assume an instance deliberately deployed without TLS, or exposed to the public internet against the deployment documentation.
- Missing hardening headers or best-practice suggestions with no demonstrated impact — these are welcome as normal issues.
- Automated scanner output pasted without a reproduction or an explanation of impact.
- Theoretical timing or side-channel attacks without a demonstrated practical impact.

Edge cases will be evaluated on a case-by-case basis. When in doubt, send the report and we will decide together.

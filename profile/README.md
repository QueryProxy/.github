<!--
  Bu dosya GitHub'da https://github.com/QueryProxy sayfasında görünür.
  Pazarlama odaklı: pitch + neden + nasıl çalışır + ürünler + nasıl başlanır.
  Her release'de güncellenecek yerler: sürüm rozeti ve aşağıdaki duyuru bloğu.
-->

# QueryProxy

> **Production data access, without production credentials.**

QueryProxy is a **self-hosted database access control and query approval portal**. Instead of handing out production credentials, developers submit SQL through a guarded editor; a DBA approves or rejects it from the web interface or straight from Slack or Teams; the approved query runs asynchronously and the result comes back **limited, masked and fully audited**. Website: **[queryproxy.com](https://queryproxy.com)** · **[Documentation](https://queryproxy.com/docs/)**

[![Latest release](https://img.shields.io/github/v/release/QueryProxy/QueryProxy?sort=semver&label=latest&color=2ea043)](https://github.com/QueryProxy/QueryProxy/releases/latest) [![License](https://img.shields.io/badge/license-AGPL--3.0--or--later-blue)](https://github.com/QueryProxy/QueryProxy/blob/main/LICENSE) [![Docker image](https://img.shields.io/badge/docker-queryproxy%2Fqueryproxy-2496ed)](https://hub.docker.com/r/queryproxy/queryproxy) [![Databases](https://img.shields.io/badge/databases-PostgreSQL%20%7C%20MySQL%20%7C%20MariaDB%20%7C%20SQL%20Server%20%7C%20SQLite-informational)](https://queryproxy.com/docs/)

<!-- Duyuru bloğu: her yeni release'de sürüm + öne çıkan değişiklik güncellenir. -->
> [!NOTE]
> 🚀 **`v0.1.3` is out.** One container is now a complete instance — nginx, php-fpm, the queue worker and the scheduler run together, so `docker run -p 7432:7432` gives you a working portal with a single volume. Images ship for `linux/amd64` and `linux/arm64` on both Docker Hub (`queryproxy/queryproxy`) and GHCR (`ghcr.io/queryproxy/queryproxy`). → [Release notes](https://github.com/QueryProxy/QueryProxy/releases/latest)

> [!IMPORTANT]
> QueryProxy is **pre-1.0 software on a `0.x` line**. The workflow, the guards, the masking pipeline and the audit log are implemented and covered by tests, but the API and configuration surface can still change between minor versions, and there is no long-term support line yet. Read the [security policy](https://github.com/QueryProxy/.github/blob/main/SECURITY.md) before you put it in front of a production database.

---

## Why?

Developers legitimately need production data, and every way they get it today is bad:

- **A shared password** — nobody can say afterwards who ran what.
- **A personal account** with more rights than anyone remembers granting.
- **A tunnel opened for one incident** and never closed.

The credential outlives the reason it was issued. Network controls decide *who can reach* the database; they say nothing about *what was actually run*.

QueryProxy puts a workflow where the credential used to be. The unit of access stops being a session and becomes **a single, reviewed statement**.

---

## How it works

1. **Submit.** A developer picks a connection they were explicitly granted and writes SQL in a guarded editor.
2. **Guard.** A server-side AST inspection runs before a human ever looks at it: `UPDATE` / `DELETE` without `WHERE` are rejected, `SELECT` without `LIMIT` gets `LIMIT 1000` injected (hard cap `10000`), multi-statement work requires an explicit `BEGIN; … COMMIT;`, and administrative statements (`GRANT`, `DROP DATABASE`, `SET GLOBAL`, …) are blocked outright.
3. **Approve.** A DBA approves or rejects — in the web UI, or from a Slack message with interactive buttons or a Microsoft Teams card. Self-approval is blocked; every decision records who, when and through which channel.
4. **Execute.** The approved query runs on a queue worker. Reads stream through database cursors with constant memory usage, masking rules are applied *while results are written* — so unmasked PII never reaches the result store — and everything lands in an immutable audit log.

[Read the full approval workflow →](https://queryproxy.com/docs/approval-workflow/)

---

## Get started

```sh
docker run -d --name queryproxy \
  -p 7432:7432 \
  -v queryproxy-data:/var/www/html/storage/app \
  -e QUERYPROXY_ADMIN_EMAIL=you@example.com \
  -e QUERYPROXY_ADMIN_PASSWORD='choose-a-strong-password' \
  queryproxy/queryproxy
```

Open <http://localhost:7432> and log in. One container runs the whole stack; one volume holds the SQLite database, the generated `APP_KEY` and the result files. Prefer Compose, or a plain PHP 8.3+ server? Both are documented.

- [Quick start](https://queryproxy.com/docs/quickstart/) · [Manual installation](https://queryproxy.com/docs/manual-installation/) · [Configuration](https://queryproxy.com/docs/configuration/)
- [SQL guards](https://queryproxy.com/docs/sql-guards/) · [Data masking](https://queryproxy.com/docs/data-masking/)
- [Slack integration](https://queryproxy.com/docs/slack-integration/) · [Teams integration](https://queryproxy.com/docs/teams-integration/)

---

## Projects

| Project | Description | Status |
|---------|-------------|--------|
| [**QueryProxy**](https://github.com/QueryProxy/QueryProxy) | The portal itself: a single Laravel monolith with the Query Studio, RBAC and team isolation, the connection vault, the approval workflow, ChatOps, async execution, dynamic masking and the audit log. | **Pre-release — v0.1.3** |

---

## What it is not

- **Not a bastion host, not a VPN replacement.** Those control who can reach the database. QueryProxy reads the query itself. They compose — keep the network controls and put approval where the statement is.
- **Not an analytics gateway.** Reporting workloads want a read replica, not an approval queue.
- **Not a hosted service, deliberately.** A system that sees every production query is not one to hand to a third party. [Compared with Bytebase, Teleport, CloudBeaver and a plain bastion host →](https://queryproxy.com/compare/)

---

## Principles

- **Self-hosted by design.** Being self-hosted is load-bearing, not a packaging choice. Your queries, your results and your credentials stay on your infrastructure.
- **The statement is the unit of access.** Approving a session tells you nothing afterwards; approving a statement tells you exactly what ran.
- **Guards before humans.** Structural mistakes should be rejected by a parser, not caught by a tired reviewer at 2 a.m.
- **Auditable by default.** Every login, grant, submission, decision, execution and download is recorded, and the auditor role can read it without touching anything else.
- **Zero external dependencies to start.** SQLite and a database-backed queue out of the box; swap in PostgreSQL, MySQL or Redis when you outgrow them.
- **Open source, AGPLv3.** Network-hosted modifications stay open.

---

## Contact

- **Bug reports and feature requests:** open an issue in the [relevant repository](https://github.com/QueryProxy/QueryProxy/issues).
- **Questions and setup help:** start with the [documentation](https://queryproxy.com/docs/); if it does not answer, open an issue prefixed with `question:`.
- **Security:** do **not** open a public issue — see [SECURITY.md](https://github.com/QueryProxy/.github/blob/main/SECURITY.md) or email **info@queryproxy.com**.

---

> Stop issuing credentials you will regret. Let the query be the thing you approve.

---

This project is developed by [Muhammet Şafak](https://www.muhammetsafak.com.tr/en/) under the [tignex.com](https://tignex.com) umbrella.

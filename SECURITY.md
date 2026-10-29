# Security policy

mcpbus sits between AI assistants and your databases, so we take reports about it seriously.

## Reporting a vulnerability

Report it privately, not in a public issue:

- **GitHub:** the [Security tab](https://github.com/offeconsulting/mcpbus-releases/security) of this repository → **Report a vulnerability**.
- **Email:** hello@mcpbus.sh, with "security" in the subject.

Please include the mcpbus version (`mcpbus version`), the database server and its version (SQL Server or PostgreSQL), and the steps or SQL that show the problem. We confirm receipt within five working days, keep you updated while we work on it, and credit you in the release notes if you want.

## What counts

In scope:

- SQL that does more than the database's level (`schema`, `read`, `readwrite`), its exceptions and the server switches allow.
- Writes that run without approval when the configuration asks for `strict`.
- String or binary values in results that come back unhashed although hashing is on.
- `mcpbus.exe` or `install.ps1` doing something other than what the [README](README.md) describes, or a way to make `install.ps1` install a file that is not the signed release.

Known limits, described in the [Terms](https://mcpbus.sh/terms) and [docs](https://mcpbus.sh/docs), are not vulnerabilities in themselves:

- Approval in the chat (`prompt`) is a check the assistant can bypass; `strict` is the barrier.
- Hashing hides values in results, but filters in the SQL still run on the real values. Column names, definitions, index usage and SQL Agent job details are not hashed.
- The login you give mcpbus is the outer limit, and `executeAsLogin` and `agentRead` widen access when you turn them on.

## Supported versions

Only the latest release gets security fixes. Update with `irm https://mcpbus.sh/install.ps1 | iex`.

## Verifying a release

Every `mcpbus.exe` is signed by Offe Consulting AB, and every release has a `SHA256SUMS.txt`. See [Manual install with verification](README.md#manual-install-with-verification).

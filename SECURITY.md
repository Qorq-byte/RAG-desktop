# Security Policy

## Supported versions

Security fixes are applied to the current default branch and the latest published release when practical.

| Version | Supported |
| --- | --- |
| Latest release | Yes |
| Older releases | Best effort |

## Reporting a vulnerability

Please do not open a public issue for a suspected vulnerability. Contact the maintainers through the private contact method listed in the repository profile. Include a concise description, affected version or commit, reproduction steps, impact, and a safe way to reach you.

Remove API keys, passwords, tokens, signed URLs, private documents, account identifiers, and unredacted logs before sending a report. If the report contains sensitive material, ask for a secure channel first.

We will acknowledge a report when possible, investigate it, coordinate a fix, and publish a release note when disclosure is safe. Response times depend on maintainer availability and the severity of the report.

## Security boundaries

RAGDB stores a local SQLite database, Chroma index, imported cache, conversations, and logs. Cloud embedding and chat providers receive the text required by the selected operation. Protect the data directory and system credential store, and review provider retention policies before enabling external services.

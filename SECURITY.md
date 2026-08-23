# Security policy

## Supported versions

Only the latest published QuotaNotch release receives security updates.

## Report a vulnerability

Please use [GitHub private vulnerability reporting](https://github.com/GuojianSun/QuotaNotch/security/advisories/new).
Do not open a public issue containing credentials, authentication tokens,
private paths, or exploit details.

## Release integrity

Official releases are distributed only from this repository. Release assets are
expected to be signed with Guojian Sun's Apple Developer ID, notarized by Apple,
and accompanied by a SHA-256 checksum.

QuotaNotch is not sandboxed because it launches a locally installed Codex
executable and, when explicitly enabled, reads local Codex task metadata.
Hardened Runtime remains enabled for release builds.

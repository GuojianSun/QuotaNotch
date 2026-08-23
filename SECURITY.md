# Security policy

## Supported versions

Only the latest published QuotaNotch release receives security updates.

## Report a vulnerability

Please use [GitHub private vulnerability reporting](https://github.com/GuojianSun/QuotaNotch/security/advisories/new).
Do not open a public issue containing credentials, authentication tokens,
private paths, or exploit details.

## Release integrity

When published, official preview releases will be distributed only from this
repository and accompanied by a SHA-256 checksum. The checksum helps detect a
mismatched or corrupted download, but it does not independently prove the
identity of the publisher when it is obtained from the same release page.

The first planned preview build will not be signed with an Apple Developer ID
and will not be notarized by Apple. macOS may therefore require explicit
approval on first launch. Do not disable Gatekeeper or remove quarantine
attributes globally. Developer ID signing and Apple notarization are planned
for a future release; this policy and the release notes will state when that
changes.

QuotaNotch's update design requires HTTPS and Sparkle EdDSA signatures. Before
an update is published, the production appcast and its referenced artifact must
describe the exact same release bytes. No production appcast or public update
artifact exists yet.

QuotaNotch is not sandboxed because it launches a locally installed Codex
executable and, when explicitly enabled, reads local Codex task metadata.
Hardened Runtime remains enabled for release builds.

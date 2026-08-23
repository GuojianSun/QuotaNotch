# Security policy

## Supported versions

Only the latest published QuotaNotch release receives security updates.

## Report a vulnerability

Please use [GitHub private vulnerability reporting](https://github.com/GuojianSun/QuotaNotch/security/advisories/new).
Do not open a public issue containing credentials, authentication tokens,
private paths, or exploit details.

## Release integrity

Official releases are distributed only from this repository and accompanied by
a SHA-256 checksum. The checksum helps detect a mismatched or corrupted
download, but it does not independently prove the publisher's identity when it
is obtained from the same release page.

QuotaNotch 1.0.0 is ad-hoc signed and is not notarized by Apple. macOS may
therefore require explicit approval on first launch. Do not disable Gatekeeper
or remove quarantine attributes globally; use the per-app **Open Anyway**
control in Privacy & Security after verifying the official download.

QuotaNotch's update design requires HTTPS and Sparkle EdDSA signatures. Before
an update is published, the production appcast and its referenced artifact must
describe the exact same release bytes. Release assets are immutable; a fix is
published under a higher build number instead of replacing an existing file.

QuotaNotch is not sandboxed because it launches a locally installed Codex
executable and, when explicitly enabled, reads local Codex task metadata.
Hardened Runtime remains enabled for release builds.

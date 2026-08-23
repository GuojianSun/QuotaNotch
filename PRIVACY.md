# Privacy

Last updated: 2026-08-24

QuotaNotch does not collect analytics, advertising identifiers, crash reports,
or user content.

## Usage data

To display usage, QuotaNotch starts a local Codex app-server process selected by
the user or verified as part of an OpenAI-signed application. That Codex process
uses the user's existing Codex authentication and communicates with OpenAI.
QuotaNotch does not parse, copy, or transmit the authentication token.

Quota and reset information is cached locally with private file permissions.
Optional aggregate Token activity is kept in memory for display and is not
persisted.

## Optional local collaboration history

Local collaboration tracking is off by default. When enabled, QuotaNotch reads
local Codex rollout files only to extract terminal task identifiers, completion
times, durations, terminal states, and session-source metadata.

QuotaNotch ignores prompts, responses, task titles, working directories, and
final-response content. Task identifiers and source paths are hashed before the
deduplicated ledger is saved. Subagent rollouts are excluded. The ledger can be
cleared from Settings at any time.

## Updates

Public builds use Sparkle to check an HTTPS update feed and download updates
according to the user's preferences. Sparkle system profiling is disabled.
QuotaNotch does not attach an account identifier, Codex token, usage payload,
or analytics identifier to update requests.

## Local files

QuotaNotch stores private local state under its macOS Application Support
directory, including:

- an isolated Codex home containing a symbolic link to the user's existing
  `auth.json` file;
- a last-known-good usage cache without authentication tokens or full protocol
  payloads;
- an optional collaboration ledger containing hashed identifiers and timing
  metadata, never conversation text; and
- application preferences in `UserDefaults`.

QuotaNotch never consumes a reset credit. Reset-credit actions only open the
official Codex usage page in the user's browser.

The included privacy manifest declares no tracking and no collected data.

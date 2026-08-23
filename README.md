# QuotaNotch

<p align="center">
  <img src="assets/brand/app-icon.png" width="128" alt="QuotaNotch app icon">
</p>

<p align="center">
  <strong>Codex usage, right by the notch.</strong><br>
  A native macOS companion that puts quota and optional activity summaries beside the MacBook notch.
</p>

<p align="center">
  <strong>English</strong> · <a href="README.zh-CN.md">简体中文</a> · <a href="README.zh-Hant.md">繁體中文</a>
</p>

> [!IMPORTANT]
> **Public preview status:** [GitHub Releases](https://github.com/GuojianSun/QuotaNotch/releases) currently contains no QuotaNotch binary. The first downloadable build will be identified as a public preview. It will not yet be signed with an Apple Developer ID or notarized by Apple. Please read [Safe installation](#safe-installation-of-the-public-preview) before installing it when it becomes available.

QuotaNotch opens from the physical MacBook notch and keeps the current weekly Codex limit in view while you work. A menu-bar entry provides the same access on Macs without a notch and when using an external display. The panel can stay compact or expand into configurable token, collaboration, and milestone modules.

## Interface preview

### Expanded activity view

![QuotaNotch expanded panel in English and dark appearance](assets/screenshots/en/panel-expanded-dark.png)

### Compact weekly limit

![QuotaNotch compact panel in English and light appearance](assets/screenshots/en/panel-compact-light.png)

### Configurable modules

![QuotaNotch configurable data modules in English](assets/screenshots/en/settings-data-modules.png)

<details>
<summary><strong>More settings</strong></summary>

#### Behavior

![QuotaNotch behavior settings in English](assets/screenshots/en/settings-behavior.png)

#### Verified Codex connection

![QuotaNotch Codex executable verification in English](assets/screenshots/en/settings-codex-connection.png)

The verification applies to the selected Codex executable. It does not mean that OpenAI has reviewed or endorsed QuotaNotch.

</details>

The screenshots come from the real SwiftUI/AppKit interface. Panel values are point-in-time aggregate states and will differ from each user's current account. A dash means the upstream field was unavailable when the screenshot was taken; no conversation content is shown.

## Core experience

- Open by hovering at the notch, or use the permanent menu-bar fallback.
- See the weekly limit, remaining percentage, exact reset time, last-updated status, and plan label returned by Codex.
- Expand or collapse the data area without moving the fixed top header.
- Choose compact, standard, or spacious width; system, dark, or light-gray appearance; and a custom module order.
- Move the panel away from the notch and return it to the physical notch position.
- Configure launch at login, hover delay, and whether the panel hides when the pointer leaves.
- Use the interface in English, Simplified Chinese, or Traditional Chinese.
- Respect macOS Reduce Motion and Reduce Transparency accessibility settings.

## Modules and data sources

| Module | What it shows | Source and behavior |
| --- | --- | --- |
| **Weekly limit** | Used and remaining percentage, reset timestamp, plan label, and eligible reset-credit alerts | Required module. Requested through the locally launched Codex app-server using the existing Codex login. QuotaNotch never consumes a reset credit; its button only opens the official Codex usage page. |
| **Token activity** | Seven-day heatmap, lifetime Token total, and current-week Token total | Optional display module. Uses aggregate data returned by the Codex `account/usage/read` endpoint. It can be absent or delayed depending on the installed Codex build and account-side availability. The response is held in memory, not persisted by QuotaNotch. |
| **Collaboration Today** | Aggregated duration for today and, optionally, the number of qualifying sessions | Opt-in local module. Periodically scans terminal-state and timing metadata in existing Codex rollout files. It does not use a live timer. |
| **Lifetime collaboration** | Duration and qualifying-session totals across locally recoverable history | Uses the same opt-in local ledger as Collaboration Today. Only records still represented in compatible local rollout history can be reconstructed. |
| **Usage milestones** | Current and best streak, peak Token day, and longest recorded task | Optional module derived from the aggregate summary returned by `account/usage/read`; unavailable fields remain blank rather than being estimated. |

The weekly limit always remains available in the compact panel. Other modules appear only in the expanded panel and can be enabled and reordered in Settings.

## How local collaboration is calculated

Collaboration tracking is **off by default**. When enabled, QuotaNotch scans local Codex rollout files at startup when needed, approximately every 15 minutes while running, after wake, and on manual refresh.

- It imports task identifiers, completion times, reported durations, and terminal states.
- Completed and failed tasks count as sessions. An interrupted task counts as a session only when its duration is longer than 20 minutes; its recorded duration can still contribute to time.
- A task spanning midnight contributes only the overlapping time to each day. Its session count belongs to the day on which it completed.
- Duplicate terminal events are deduplicated. Conflicting or unusable records are excluded instead of guessed.
- Subagent rollout files are excluded.
- Task identifiers and source paths are hashed before the local ledger is stored.
- Prompts, responses, task titles, final answers, and working directories are ignored and are never written to the ledger.

This is a local productivity summary, not an official OpenAI metric or a billing record. If QuotaNotch is closed, it does not scan or run a live counter; after reopening, it can import compatible history that still exists on disk.

## Requirements

- macOS 14 Sonoma or later
- Apple silicon (M1 or later); no Intel or Universal 2 build is currently planned for the first preview
- An existing Codex login
- A Codex executable supplied by an OpenAI-signed Codex or ChatGPT macOS application, or another Codex executable that you explicitly select and approve

A physical notch is optional. The notch-hover interaction requires one, while the menu-bar entry works on other supported displays.

## Current distribution status

QuotaNotch is not being submitted to the Mac App Store. The intended distribution channel is this repository's [Releases page](https://github.com/GuojianSun/QuotaNotch/releases), followed by in-app Sparkle updates.

At the current stage:

- no public DMG has been uploaded;
- the first public preview is planned without a Developer ID signature or Apple notarization, pending a dedicated and verified public-preview build path;
- macOS may therefore block its first launch or show an unidentified-developer warning; and
- Developer ID signing and notarization remain a later release-hardening step.

If installing software without a Developer ID signature and Apple notarization is outside your security policy, wait for a future signed and notarized build.

## Safe installation of the public preview

These steps apply only after a preview DMG appears on the official Releases page.

1. Download the DMG and its published SHA-256 checksum from the same GitHub Release. Do not install copies from mirrors, issue attachments, or file-sharing links.
2. Verify the downloaded file locally, substituting its actual path:

   ```bash
   shasum -a 256 "/path/to/QuotaNotch.dmg"
   ```

3. Compare the complete 64-character result with the checksum shown in that release. Stop if they differ.
4. Open the DMG, drag **QuotaNotch** to **Applications**, then try to open it once normally.
5. If macOS blocks it because the developer cannot be verified, follow [Apple's per-app override procedure](https://support.apple.com/en-ie/guide/mac-help/mh40617/mac): open **System Settings → Privacy & Security**, locate the QuotaNotch message, and choose **Open Anyway**. Confirm only after checking that the app and release source are the ones you intended to trust.

Do **not** disable Gatekeeper globally, and do not use `xattr` to strip quarantine metadata. The per-app **Open Anyway** exception preserves macOS's normal protection for every other download.

### What SHA-256 does—and does not—prove

A matching SHA-256 value shows that your DMG is byte-for-byte identical to the asset represented by the published checksum. It detects an incomplete or unexpectedly modified download. It does **not** establish the developer's legal identity, replace Apple code signing or notarization, or protect against an attacker who could replace both a release asset and its checksum. Always verify the repository owner and release URL as well.

## Update security

QuotaNotch's release pipeline is configured around [Sparkle](https://github.com/sparkle-project/Sparkle), an HTTPS appcast, and a pinned EdDSA public key. A public build will enable updates only when the feed URL and key are embedded. Sparkle then accepts an update archive only when its EdDSA signature verifies against that embedded key. System profiling is disabled for update requests.

EdDSA protects the automatic-update path against a package that was not signed with the corresponding private update key, assuming the initially installed app and its embedded public key are trusted. It is separate from SHA-256 and from Apple's Developer ID system: it does not notarize the app, identify the developer to macOS, or prevent Gatekeeper warnings for a preview without a Developer ID signature.

Until the first release and production appcast are published, the update service has nothing to install.

## Privacy

QuotaNotch has no analytics, advertising SDK, account tracking, or automatic crash-report upload.

| Data | Handling |
| --- | --- |
| Codex authentication | An isolated local Codex home links to the existing `auth.json` so the selected Codex process can use the current login. QuotaNotch does not parse, copy, or transmit the token. |
| Weekly quota and reset data | Requested from OpenAI by the locally launched Codex process and cached locally with private file permissions for last-known-good display. |
| Token activity | Aggregate account values kept in memory for display and not persisted by QuotaNotch. |
| Collaboration history | Off by default. When enabled, a local ledger stores hashed identifiers plus timing and terminal-state metadata—never conversation text. It can be cleared in Settings. |
| Updates | Sparkle requests the HTTPS feed without a Codex token, usage payload, account identifier, analytics identifier, or system profile. |

QuotaNotch itself does not upload local collaboration metadata. The Codex app-server subprocess still communicates with OpenAI as part of the user's existing Codex session. See the complete [Privacy Policy](PRIVACY.md).

## Known limitations

- The first preview will not have Developer ID signing or Apple notarization.
- Only macOS 14+ on Apple silicon is supported initially.
- Quota and Token fields depend on the locally installed Codex app-server and account-side data contracts; a Codex update or service change may temporarily make a field unavailable.
- Token activity can lag the most recent conversation and may not be returned by older Codex builds.
- Local collaboration totals are recoverable only from compatible rollout files still present on this Mac. Deleted, missing, truncated, or older incompatible history cannot be reconstructed.
- Collaboration time is a metadata-derived summary, not active-window time, keyboard time, or a measure of model compute time.
- QuotaNotch is not sandboxed because it must launch a locally installed Codex executable and, only when opted in, read local Codex task metadata. Release builds still enable Hardened Runtime.
- Moving the panel is supported, but full edge docking and edge-reveal behavior are not part of the first preview.

## FAQ

<details>
<summary><strong>Does my Mac need a notch?</strong></summary>

No. A physical notch enables hover-to-open at the top of the built-in display. The menu-bar entry remains available on supported Macs without a notch and on external displays.

</details>

<details>
<summary><strong>Why does macOS say the developer cannot be verified?</strong></summary>

The first public preview is planned without Apple Developer Program membership, so it will not carry a Developer ID signature or Apple notarization. Verify the official release URL and SHA-256 value, then use the per-app **Open Anyway** path described above. If the message differs, the checksum fails, or the file came from another source, stop and report it.

</details>

<details>
<summary><strong>Does QuotaNotch consume reset credits?</strong></summary>

No. It can show eligible or soon-to-expire reset-credit counts when the Codex response provides them. Clicking the label only opens the official Codex usage page in your browser.

</details>

<details>
<summary><strong>Why is a Token or milestone value missing?</strong></summary>

Those modules use a supplementary Codex account-usage endpoint. The endpoint or an individual field may be unavailable in an older Codex build, for a particular account, or while upstream data is still updating. QuotaNotch displays a dash instead of inventing a value.

</details>

<details>
<summary><strong>Does collaboration tracking keep running while QuotaNotch is closed?</strong></summary>

No. It is a periodic metadata scan, not a real-time background timer. When QuotaNotch reopens, it can import compatible rollout records that Codex retained while the app was closed, without double-counting known task identifiers.

</details>

<details>
<summary><strong>Is this repository open source?</strong></summary>

No. This is the public distribution, documentation, and support repository. QuotaNotch's implementation source remains private, and official binaries are provided under the proprietary [QuotaNotch Binary License](BINARY-LICENSE.md).

</details>

## Roadmap

- Publish the first Apple-silicon public-preview DMG with release notes and a SHA-256 checksum.
- Validate installation, data-source compatibility, localization, and Sparkle updates with public-preview feedback.
- Promote tested builds through separate beta and stable update channels without rebuilding the promoted artifact.
- Add Developer ID signing and Apple notarization when sustainable for the project.
- Reassess Intel/Universal 2 support and deeper edge docking based on demonstrated demand.

The roadmap is directional and has no guaranteed dates. The [Changelog](CHANGELOG.md) records shipped behavior.

## Support and reporting

- Use [GitHub Issues](https://github.com/GuojianSun/QuotaNotch/issues) for reproducible bugs, compatibility reports, and feature requests.
- Include the macOS version, Mac model, QuotaNotch version, Codex installation type, and reproduction steps.
- Never post `auth.json`, access tokens, private paths, conversation content, or private rollout files.
- Report vulnerabilities privately according to the [Security Policy](SECURITY.md).

## License and ownership

QuotaNotch is proprietary software distributed as compiled binaries. The implementation source is private and is not included in this repository. Use of official binaries is governed by the [QuotaNotch Binary License](BINARY-LICENSE.md).

Documentation, screenshots, artwork, and branding remain protected by the [Copyright notice](COPYRIGHT.md). Embedded third-party components retain their own licenses; see [Third-party notices](THIRD_PARTY_NOTICES.md).

Copyright © 2026 Guojian Sun. All rights reserved.

---

QuotaNotch is an independent project. It is **not affiliated with, sponsored by, endorsed by, or an official product of OpenAI**. OpenAI, Codex, and ChatGPT are trademarks of OpenAI.

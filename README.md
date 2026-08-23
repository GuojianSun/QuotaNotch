# QuotaNotch

<p align="center">
  <strong>Codex usage, right at the MacBook notch.</strong>
</p>

<p align="center">
  A small, native macOS companion for checking quota and optional local activity without leaving your work.
</p>

<p align="center">
  <a href="https://github.com/GuojianSun/QuotaNotch/releases">
    <img alt="Download — coming soon" src="https://img.shields.io/badge/Download-Coming_soon-8A8A8A?style=for-the-badge&logo=apple&logoColor=white">
  </a>
</p>

<p align="center">
  macOS 14 or later · Apple silicon<br>
  <a href="./README.zh-CN.md">简体中文</a>
</p>

> **Release status:** the first Developer ID-signed and Apple-notarized DMG is being prepared. The button above opens the official Releases page.

QuotaNotch opens when the pointer reaches the MacBook notch, putting weekly Codex usage a short movement away. A permanent menu-bar entry provides the same access on Macs without a notch and while using external displays.

## Preview

| Light | Dark |
| :---: | :---: |
| ![QuotaNotch expanded in light mode](assets/screenshots/light-expanded.png) | ![QuotaNotch expanded in dark mode](assets/screenshots/dark-expanded.png) |

<details>
<summary>Compact weekly-usage view</summary>

| Light | Dark |
| :---: | :---: |
| ![QuotaNotch collapsed in light mode](assets/screenshots/light-collapsed.png) | ![QuotaNotch collapsed in dark mode](assets/screenshots/dark-collapsed.png) |

</details>

## Highlights

- **One-movement access** — open from the notch, or use the menu-bar fallback anywhere.
- **Quota at a glance** — see weekly usage, remaining capacity, reset time, and freshness.
- **Expandable detail** — add optional Token activity, usage milestones, and local collaboration-time modules.
- **Made to fit** — choose a panel size, reorder modules, and use compact or expanded views.
- **Native appearance** — light and dark themes, with support for reduced motion and reduced transparency.
- **Localized interface** — English, Simplified Chinese, and Traditional Chinese.

## Requirements

- macOS 14 Sonoma or later
- A Mac with Apple silicon (M1 or later); no Intel or Universal 2 release is currently offered
- Codex supplied by an OpenAI-signed Codex or ChatGPT macOS app, or a Codex command-line executable that you explicitly select and approve
- An existing Codex login

## Install

1. Download the DMG from [GitHub Releases](https://github.com/GuojianSun/QuotaNotch/releases) once the first public build is available.
2. Open the DMG and drag **QuotaNotch** to **Applications**.
3. Launch QuotaNotch. If prompted, select the Codex installation you trust.

Official downloads are distributed only through this repository's Releases page. You should not need to bypass Gatekeeper for a signed, notarized release.

## Privacy

QuotaNotch has no analytics or advertising SDK and does not collect crash reports, authentication tokens, or conversation content. It uses your existing Codex installation and login to request usage through a local Codex process; QuotaNotch does not parse your token.

The optional **Today's collaboration** module is off by default. When enabled, it reads only the local terminal-state and timing metadata needed to calculate collaboration time and counts. Prompts, responses, task titles, and working directories are ignored and are not copied into QuotaNotch's cache or ledger.

Read the complete [Privacy Policy](PRIVACY.md) before installing.

## Source and license

QuotaNotch is proprietary software distributed as compiled binaries. Its implementation source code is private and is not included in this repository. Use of an official build is governed by the [QuotaNotch Binary License](BINARY-LICENSE.md). Public documentation, screenshots, artwork, and branding remain protected as described in the [Copyright notice](COPYRIGHT.md); third-party components remain under their respective licenses as listed in [Third-party notices](THIRD_PARTY_NOTICES.md).

## Support

Questions, bug reports, and release feedback are welcome in [GitHub Issues](https://github.com/GuojianSun/QuotaNotch/issues). When reporting a problem, include your macOS version, Mac model, QuotaNotch version, and the installation method—but never post `auth.json`, access tokens, or conversation content. Please report vulnerabilities privately as described in the [Security Policy](SECURITY.md).

---

QuotaNotch is an independent project. It is **not affiliated with, sponsored by, endorsed by, or an official product of OpenAI**. OpenAI, Codex, and ChatGPT are trademarks of OpenAI.

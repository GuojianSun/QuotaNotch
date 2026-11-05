# Changelog

## 1.1.1 (15) — 2026-11-05

[Release details](https://github.com/GuojianSun/QuotaNotch/releases/tag/v1.1.1)

- Restores verified Codex discovery after recent ChatGPT bundle-layout changes, fixing stale cached plan and quota displays.
- Supports server-reported Pro and ProLite plans in quota-module settings and automatic display selection.
- Plus and Pro-family accounts gain independently configurable 5-hour and weekly quota modules, with at least one enabled and 5-hour usage first by default.
- Automatic quota display switching is enabled by default: it prioritizes 5-hour usage and switches to weekly usage at exactly 0% remaining, showing both reset times.
- Turning automatic switching off restores module-order selection in the compact panel and shows all enabled quota rows in the expanded panel.
- Missing or ambiguous quota windows are not displayed as zero usage; other account plans retain their existing weekly quota behavior.
- New settings, quota labels, and fallback states support English, Simplified Chinese, and Traditional Chinese.

## 1.0.0 — 2026-08-24

The first public QuotaNotch release includes:

- notch-hover and menu-bar access to Codex weekly usage;
- fixed reset timestamps, quota progress, and reset-credit alerts;
- optional Token activity, local collaboration timing, and usage milestones;
- configurable data-module ordering, panel density, and light/dark appearance;
- Simplified Chinese, Traditional Chinese, and English interfaces;
- a movable panel that can return to the physical MacBook notch;
- privacy-preserving local caching and opt-in collaboration history; and
- HTTPS and Sparkle EdDSA-protected updates with separate stable and beta channels.

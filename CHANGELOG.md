# Changelog

All notable changes to Operum Desktop are documented here.

## [0.33.0] - 2026-07-09

### What's New

- **Integration health at a glance** — New indicator shows when your API credentials or integrations need attention, right in the UI
- **Abuse reports now tracked** — Report misuse with confidence: you'll get a case ID and confirmation email for your records
- **Legal pages just got reliable** — Account deletion, privacy, and terms pages now use secure server-side forms instead of email links

### Improvements

- **Agents recover automatically** — Long sessions or unexpected restarts? Agents bounce back cleanly without needing manual intervention
- **Session management is more reliable** — Better handling of expired sessions and automatic recovery when you reconnect
- **Logout actually clears your identity** — On shared machines, logging out now fully clears your GitHub credentials from the system
- **Progress tracking is rock solid** — Progress cards now persist correctly across agent restarts and UI updates
- **Schedules run on time** — Fixed reliability issues with scheduled tasks so your workflows stay consistent
- **Light theme looks crisp** — Improved contrast and visual polish in light mode
- **Chat and modals work smoothly** — Fixed layout and rendering issues in chat panels and modal dialogs

### Bug Fixes

- Fixed silent failures in team setup and role initialization
- Resolved progress card duplicates and rendering glitches
- Fixed modal chat display issues and styling inconsistencies
- Improved reliability of file uploads and attachments
- Better error messages when GitHub authentication expires

### For Developers

- Enhanced locking documentation for safer concurrent operations
- Improved MCP git tool reliability with better error handling
- Better observability for credential and auth state

## [0.24.0] - 2026-05-07

### Added

- **Sign out other devices** - New option in Settings → Security that invalidates all other active sessions while keeping the current device signed in

### Fixed

- Fixed knowledge-drift panel "Discard local" now provides proper feedback; clicking Discard when commit details can't be loaded shows an error toast instead of a silent non-response
- Fixed IPC watcher duplicate triggers - self-assessment and label-change events no longer arrive in batches of 20+ copies
- Fixed team creation with managed GitHub repo no longer fails with "temporarily unavailable" after the Quick Start onboarding path

---

## [0.23.0] - 2026-05-05

### New Features

- **Streaming activity updates** - Watch agent progress in real-time instead of waiting for long operations to complete
- **Billing plan upgrades** - Upgrade your plan directly from the billing page without leaving the app

### Improvements

- **Session reliability** - Sessions now recover stashed work more reliably when restarting agents
- **Multi-team support** - Sequential PR enforcement now works correctly across repositories in different teams
- **Desktop environment awareness** - Agents now receive context about the Operum Desktop runtime, enabling better environment-aware behavior

### Bug Fixes

- Fixed duplicate messages appearing in the chat panel
- Fixed agents not picking up new tasks on session startup
- Fixed auto-merge state synchronization and locking issues
- Fixed duplicate label-change events triggering multiple times
- Fixed git push upstream tracking fallback for branches without explicit upstream config

### For Developers

- operum-core crate improvements - `cargo test` now runs successfully on clean Windows developer machines
- Windows agent worktree isolation fixed - tests no longer fail due to platform-specific file handling
- Stash recovery system now includes runtime depth cap to prevent accumulation across session restarts

---

## [0.19.3] - 2026-04-15

### Fixed

- Fixed critical Rust dependency security advisories
- Improved Windows agent startup reliability

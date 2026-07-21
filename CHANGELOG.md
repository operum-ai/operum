# Changelog

All notable changes to Operum Desktop are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/).

## [0.36.0] - 2026-07-21

### What's New

- **Public changelog page**: Operum now has a dedicated changelog page on the website (`/changelog`) showing past releases and what's new
- **Credential security hardened**: Desktop credentials are now encrypted at rest using your OS keychain as the master key — safer by default

### Improvements

- **GitHub connection reliability**: Complete overhaul of GitHub token handling — tokens are now team-scoped, transient 403 errors no longer re-prompt Connect GitHub, and connection recovers automatically from temporary server issues
- **Account and team management**: Fixed repository visibility UI alignment, team archive/trash operations now work reliably, and onboarding copy is clearer
- **Disk usage fix**: Agent worktree builds no longer accumulate forever. Previously, teams could reach 338 GB of unused build artifacts; now build caches are shared and cleaned up automatically — no more silent disk fills from background work

### Bug Fixes

- Fixed credential migration from older keychain-only storage to the new encrypted file store
- Team-aware credential resolution now works correctly across restarts
- Resolved PGRST203 errors when moving teams to trash
- Improved startup performance with parallelized agent initialization

### For Developers

- Team-scoped credential store and diagnosis tooling for debugging auth issues
- Enhanced canonical team-log path and simplified file-tool allowlist

## [0.35.0] - 2026-07-20

### What's New

- **System template rollout**: All customer teams now get the latest Engineer agent improvements for better orchestration and reliability
- **Fully async Desktop**: Eliminated UI freezes by converting all long-running commands to async operations — the app stays responsive even during heavy background work
- **Workflow board improvements**: Shows in-progress issues in real time, better status visibility across your team, and improved navigation

### Improvements

- **Search and filtering**: Collapsible search/filter bar in Mission Control for cleaner navigation
- **Loading indicators**: Column-shaped loading skeleton for the Workflow board so you know work is coming
- **Git reliability**: Moved git operations from command-line to in-process library — no need to have git installed, faster and more reliable
- **Authentication resilience**: Young access tokens auto-refresh instead of forcing logout; GitHub temporary errors recover automatically
- **CI performance**: Lane-aware sequential gate means ci:cheap PRs don't block behind slow ci:full builds

### Bug Fixes

- Fixed GTK main-thread deadlock cycles that were causing random app freezes
- Stopped the trigger delivery flood that was causing repeated duplicate task assignments
- Fixed trigger delivery gaps — missed tasks are now back-delivered to waiting agents
- Fixed critical auth issues: 401s trigger real refresh instead of logout, missing GitHub tokens self-heal with backoff
- macOS Keychain no longer prompts repeatedly on startup
- Fixed OTP login loop and OAuth issues on landing site
- External links now open properly in your default browser
- Team creation rate-limit errors now show correct retry timing

### For Developers

- CI now forbids blocking operations in sync Tauri commands — enforced by class-guard lint
- All ~60 sync commands migrated to async with spawn_blocking where needed

## [0.34.0] - 2026-07-15

### What's New

- **Session rotation**: Agents automatically rotate sessions when context gets full, preventing mid-task cutoffs
- **Claude auth**: Support for injecting long-lived Claude auth tokens for better integration
- **CI-wait recovery**: If CI takes too long, agents auto-recover instead of hanging
- **Task tracking**: Task-level "Validating via CI" status so you can see what's happening during builds

### Improvements

- **Dynamic todos**: Progress cards now render todo items as they're created, no static fallbacks
- **Settings reorganization**: Moved Claude token configuration from Integrations to Settings for better discoverability

### Bug Fixes

- Fixed dedup of repeated trigger delivery
- Fixed QA approval label resurrection causing unexpected auto-merges
- Fixed orphan recovery preventing cross-owner leaks
- Fixed trigger re-delivery for closed/merged issues
- Fixed bounded heartbeats during inline waits
- Fixed operum-hold marker handling for better release gating
- Fixed webview freeze issues through off-lock stop and async status
- Fixed model invocation floods
- Improved agent-start race condition handling
- Fixed auth refresh loops and credential caching
- Fixed agent recovery for oversized sessions
- Exponential-backoff half-open breaker recovery
- Fixed mobile idea-carousel clipping on landing page
- Fixed support email handling and notifications

### For Developers

- Staging-first Supabase deploy gate for safer releases
- Improved CI setup for nightly testing and golden installations

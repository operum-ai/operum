# Changelog

All notable changes to Operum Desktop are documented here.

## [0.35.0] - 2026-07-20

### What's New

- Moved ~60 desktop commands off the UI thread for snappier, responsive interface
- Async agent operations prevent UI freezes
- Cross-machine issue claims tracking for team workflows
- Collapsible Mission Control search/filter bar
- Loading skeleton for Workflow board
- "Queued" lane for better task visibility
- Self-hosted runner for nightly test suite

### Improvements

- Git operations now handled in-process (no external dependency)
- Double-dispatch race eliminated through delivery mechanism
- Sequential PR gate now lane-aware (faster builds don't block on slower ones)
- Governance PRs (documentation-only) exempted from sequential gate
- Workflow board shows in-progress issues; CI status persists across navigation
- Reduced desktop lock contention with async message handling

### Bug Fixes

- **Fixed critical app freezing** — Two confirmed GTK main-thread deadlock cycles identified and guarded against. Auto-recovery if any deadlock recurs. Overnight soak-tested with zero recurrence.
- Eliminated duplicate message deliveries
- Fixed self-assessment and trigger duplicate-delivery flood that caused token-cost blowups
- Missed triggers now back-delivered to idle agents
- Trigger watcher self-heals on initialization
- **GitHub auth improvements**: Young access tokens no longer force full logout; GitHub server errors treated as transient; missing tokens use backoff instead of tight loops
- Total agent restart failure fixed across all causes
- macOS Keychain prompt storms eliminated; deep-link login fixed
- OAuth login loop and code-verifier issues resolved
- New user onboarding gate more robust
- Promise rejections hardened in billing and settings
- **External links now work properly** — Fixed to open via proper mechanisms instead of failing silently
- **Team creation errors** now show correct timing (no more "undefined seconds")
- **"OF WORK" stat** no longer goes stale after switching teams
- GitHub error HTML no longer leaks into UI
- Auto-merge pre-fetch now bounded; chronic hangs instrumented
- Git push operations bounded to prevent hung origin from wedging system
- Terminal-state guard extended to all dispatch types
- QA-approval-label resurrection and PASS/FAIL race closed
- Owner-scope enforcement tightened on CI-status triggers
- Sonnet 5 context window correctly classified
- Knowledge panel unified from repo checkout
- Agent start no longer freezes UI
- Landing page footer and LinkedIn link fixed

### For Developers

- CI linter forbids unbounded I/O held across Mutex guards
- `list_integrations` now enumerates custom credential secrets

## [0.34.0] - 2026-07-15

### What's New

- Intelligent session rotation on token budget with no work loss
- Context metrics now compaction-aware
- Long-lived Claude Code OAuth token support
- Claude credential auto-rotation with health monitoring
- CI-wait auto-recovery and non-silent stall detection
- Backend-owned respawn for common restart triggers
- Task-level "Validating via CI" status marker
- Claude token moved to Settings
- Deduplication of repeat support emails

### Improvements

- Dynamic todos now render in Progress card
- Duplicate message elimination with canonical ID envelopes
- Better message deduplication at transport level
- Task progress scoped more accurately in Workflow board
- Agent sidebar CI-wait better labeled as "Waiting for CI"
- Activity feed less noisy (reduced internal status and chat spam)
- Mission Control Activity Feed deferred to avoid blocking initial paint
- GitHub connect modal reconciled with backend token injection
- macOS deep-link post-login handoff fixed
- Smoke-test rows more reliable with improved state management

### Bug Fixes

- Fixed app webview freezing through async agent operations and bounded render churn
- Deduplication of repeated self-assessment and external-change triggers
- QA approval state invalidated on QA failure to prevent stuck auto-merge
- Orphan recovery fails safely without cross-owner leaks
- Trigger writers guarded against re-dispatching closed/merged issues
- Progress heartbeats bounded during inline waits
- Operum-hold markers honored on Architect and Tester delivery
- Model invocation floods stopped through emit-once + fingerprint gating
- Benign agent-start races demoted from error stream
- Task clock resets per-dispatch to prevent elapsed time ghosting
- Stuck refresh token loops escalate to re-auth
- Oversized session recovery forces fresh start
- Exponential backoff half-open breaker recovery with log throttle
- PM checkpoint runway restored; metric threshold corrected
- "Restart to apply" banner suppressed for system-initiated template updates
- Progress card reconciliation improved; live untagged cards preserved
- --continue replay duplicates collapsed in sidebar
- Expected NotLoggedIn errors dropped from Sentry
- Landing mobile carousel translations no longer clip
- Report-abuse and legal-contact forms now properly async
- Landing FAQ stale content fixed
- Email notifications properly awaited before sender cleanup

### For Developers

- Staging-first Supabase deploy gate implemented
- Knowledge base index maps regenerated in git workflow
- Golden install suite improvements
- Decision log maintenance automated
- Mandatory rebase-onto-latest-main before push/PR
- Client migration safety guidance ported across templates
- Claude session history bounded
- Architectural decision records caught up
- Attachment tool macOS compatibility improved
- Abuse report confirmation email samples added
- Test reliability improved

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

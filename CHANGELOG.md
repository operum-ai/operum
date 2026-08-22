# Changelog

All notable changes to Operum Desktop are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/).

## [0.43.0] - 2026-08-21

### What's New

- Quick Start is available again directly in the setup wizard.
- Mobile preview builds are available via EAS, for hands-on testing ahead of general release.

### Improvements

- **Paired mobile devices now connect directly over your local network** — the cloud relay transport has been removed, so traffic between your phone and the desktop app no longer passes through Operum's infrastructure.
- Privacy policy corrected to more accurately describe what we collect: hostname/IP address handling, OAuth token authentication, third-party services (including Sentry), and telemetry retention.
- Repositories with their own CI setup are no longer blocked from merging: the merge gate now accepts your branch protection's required status checks as evidence of passing CI, instead of expecting one specific workflow layout. (Repositories using rulesets rather than branch protection aren't covered by this yet.)
- The merge gate no longer treats a permissions error or temporary outage as if all checks had passed, and its refusal messages are clearer about what's actually missing.
- Token refresh for managed teams is more resilient: failures retry with backoff instead of giving up immediately, and refresh loops stop cleanly when a team is deactivated or deleted.
- Pricing page rewritten around the current plan tiers.
- Settings' Personal and Team tabs restyled to match the rest of the app.
- Mobile API pairing is available for opt-in testing, with the pairing code shown directly in Settings.
- Restored the "Add Custom Integration" button.

### Bug Fixes

- Switching teams now shows a loading indicator instead of appearing to hang, and the Workflow page no longer shows an empty state right after the switch.
- Workflow page errors now name what went wrong and what to do about it.
- Activity feed keeps its scroll position when you resize the window.
- Agents now pick up edited instructions on their next run instead of continuing on an out-of-date copy.
- Asking an agent about an issue no longer resets that issue's status — a QA-approved issue keeps its approval, and a paused issue stays paused.
- Pull requests are no longer closed by mistake when a closing keyword appears only in the description rather than the merge itself.
- "Fork to GitHub" now opens the correct export flow.
- Landing page text no longer overlaps when it wraps to a new line.
- Fixed a bug where agents recycled while idle could be incorrectly reported as crashed.
- An agent waiting on a CI re-run no longer gets stuck waiting indefinitely.

## [0.42.0] - 2026-08-19

*This release includes everything since v0.40.0. v0.41.0's build was never published — its changes ship here instead.*

### Improvements

- **Your email address is no longer sent to Sentry or PostHog.** We also closed an additional path where it could still reach Sentry through error-reporting breadcrumbs.
- Further security hardening across database access (session, edge-function, and service-role paths) and stronger auth-token file permissions.
- Pricing page rewritten, retiring the $149 tier.
- Trial framing corrected across the site.
- Plan-change options in Billing locked down ahead of launch.
- Removed the last remaining references to a nonexistent "Claude Code Max" product — the correct name is Claude Max.

### Bug Fixes

- Links to issues opened from chat no longer 404.
- OTP rate-limit messages now show the correct wait time.
- The setup guide no longer replays for existing users creating an additional team, and its steps no longer flash past too quickly to read.
- A new team's first message from PM is a greeting, not internal status text.

## [0.41.0] - 2026-08-18

*This version's build was never published as a release — its changes shipped together with v0.42.0.*

### What's New

- Quick Start teams now get a guided nudge sequence toward migrating to their own repository.

### Improvements

- Stalled merge approvals now surface as a To Do so they don't get missed.
- Improved keyboard accessibility: focus is now trapped correctly inside every modal dialog.

## [0.40.0] - 2026-08-16

### What's New

- **Clear Customisation** — a safe, per-team way to clear an agent's customisation, which now correctly clears the stored content rather than just the local file.
- **Affiliate program** — a self-serve affiliate dashboard with custom vanity codes, referral tracking, and commission accrual.
- Support chat now streams its answer as it's written, instead of waiting for the whole reply.
- You're now emailed when an admin extends your trial.
- Agents can now report which credential is missing instead of failing without explanation.

### Improvements

- **Plan naming: the "Team" tier is now "Unlimited"**, with the project limits it implies actually enforced. Existing subscriptions are unaffected — both names work everywhere during the transition.
- Auto-Merge and Turbo are now Premium features, gated at the point of use.
- Chat no longer surfaces internal jargon or Quick Start's GitHub implementation details.
- **Telemetry is de-identified.** Your device is now identified by a random opaque alias rather than anything derived from the machine itself; device session records no longer store your IP address or hostname, and expire after 30 days.
- Telemetry is now gated behind explicit consent.
- The online-clients list is no longer broadcast individually — only aggregate counts are shared.

### Bug Fixes

- **Agent state survives a restart.** Work in progress is no longer lost when an agent is stopped or restarted.
- **No more false "agent is idle."** An agent waiting on CI is now correctly treated as in-flight, so it's neither reported idle nor handed new work while it waits.
- Fixed a case where an agent could remain incorrectly blocked from work after the reason no longer applied.
- **Your agent template customisations can no longer be silently reverted** by a background sync.
- Removed the broken "Reset to Default" button, which reported success while doing nothing.
- A failed settings reload no longer displays as though your settings were reset to defaults.
- Integrations page no longer crashes on an unrecognised service.
- "Current Task" no longer shows another agent's issue.
- Board card indicators now reflect per-issue activity, and the board no longer freezes after a merge.
- Sign-in no longer burns a single-use code, double-sends the OTP email, or masks a login that actually completed.
- The activity feed re-anchors correctly when you change the filter.
- Clipboard-paste failures are now surfaced instead of failing silently.
- First-run orientation no longer opens after a rejected team load, and its copy matches how many teams you actually have.
- Team creation no longer shows contradictory toasts or a transient "No teams configured" gap.
- The Settings model picker now shows the correct 1M context window for Sonnet.
- Workflow and pipeline views no longer hang on an aborted background refresh.

## [0.39.0] - 2026-08-07

### What's New

- Native login now includes an Acceptable Use Policy consent step.
- Agent context-pressure now surfaces in the activity feed.

### Improvements

- Onboarding now reports per-step timing and gives an actionable error instead of hanging silently when a repository clone stalls.
- "Fork to your GitHub" now states which account it will fork into.
- The "Upgrade plan" link at your team limit now routes to the pricing page instead of checkout for one specific tier.
- Todos you added yourself, and manual-setup todos, are no longer swept away when an issue closes.
- Credentials are now redacted from the activity log before it's written to disk.
- Resolved a high-severity dependency vulnerability (`js-yaml`) plus 12 further advisories.
- The Cursor engine now runs behind a sandbox for safety.

### Bug Fixes

- **macOS:** app data moved to Application Support instead of `~/.operum`, which was triggering repeated permission prompts; existing installs are migrated automatically with nothing lost.
- **Windows:** fixed the Workflow page becoming unclickable.
- Agent greeting is now time-aware, rather than always saying "Good morning."
- The GitHub token settings link and Integrations links are now clickable, with clearer token-scope guidance.
- Header and badge counts no longer disagree with the list they describe.
- The TOKENS stat is now labelled "uncached," with cached usage explained in the tooltip.
- Trial-to-paid conversion now sends the correct email and clears the trial end date, including same-tier conversions.
- Agent messages are no longer written to the activity log twice.
- Newsletter unsubscribe works again for anonymous recipients.

### For Developers

- Supabase anon-key validation now explains what's wrong instead of failing opaquely.

## [0.38.1] - 2026-08-01

*Patch release: v0.38.0's release build failed to publish on every platform due to a packaging-only version mismatch. This release fixes that and ships what merged immediately after.*

### Bug Fixes

- Fixed "Connect Repo": a stored GitHub token wasn't being passed through to the wizard's own GitHub checks, so Validate/Create could fail with "GitHub token not configured" even after the token showed as verified.
- "Move to OS keychain" migration now previews what would be discarded and asks for confirmation, instead of getting permanently stuck on a corrupted credential entry.

## [0.38.0] - 2026-08-01

A security and reliability release.

### Improvements

- Fixed a cross-site-scripting issue in the blog's markdown rendering.
- Unsubscribe links are now cryptographically signed, closing a link-forgery issue; the old unauthenticated link format was removed.
- Added rate limiting to public sign-up endpoints, and client IPs are now hashed before being logged.
- Signing out — including "sign out other sessions" and account deletion — now revokes your session on the server, not just locally.
- Tokens used for managed-repo operations (transfer, archive, export, rollback) are now scoped to the specific repository being acted on, rather than your whole GitHub installation.
- **The context gauge now reads correctly from the start** — it used to under-report until an agent's first turn; it now shows the correct available window as soon as the model is known.
- **Multi-machine reliability, for every agent role** — same-issue collision protection, previously Engineer-only, now also covers Tester and Marketing. A configurable concurrent-PR cap helps multi-machine setups avoid stepping on each other's CI runs.
- The standalone Turbo Mode status banner was removed and the toggle re-introduced in cleaner form.
- Sidebar now shows a plan badge; Workflow cards show which machine — this device or a named other device — is actively working an item.
- "Reclaim build cache" is now a real action instead of a dead-end.
- Fixed template-diff viewer scrolling.

### Bug Fixes

- Signing out via force-takeover now revokes only the specific session, not all of them.
- An expired one-time code is now classified correctly instead of showing a generic error.

## [0.37.0] - 2026-07-29

A stability and security release.

### What's New

- **Linux credential encryption is now backed by your OS keyring (GNOME Keyring / KWallet), matching macOS and Windows.** Previously, Linux fell back to a scheme whose encryption key ships inside every release binary — anyone holding the binary could decrypt stored credentials.
  **Action required if you're on Linux:** anything you stored before this release was encrypted under that shared key and should be treated as exposed. Please rotate any tokens and API keys configured in Operum — re-encrypting them under a new key does not un-expose a value that was already readable.
- Operum now refuses to save a credential when no durable system keyring is available, instead of silently falling back to a weaker scheme. On headless machines, containers, and CI, set `OPERUM_MASTER_KEY` to a base64-encoded 32-byte key to store credentials; a GUI recovery path and a dev-safe login also cover keyring-less setups.
- Paste images directly into agent chat — they're routed to the focused agent, and the sidebar chat now persists across agent switches.
- Disk reclaim: a global build-cache size budget with oldest-first eviction, per-team disk usage with a one-click reclaim button, and automatic cleanup of idle shared build caches.
- Board cards now show the issue owner's GitHub avatar, plus a sprint highlight for backlog cards.
- Opt-in GitHub-verified commit identity for agents.
- A visual staging-mode indicator, with an environment-variable override.

### Improvements

- Signing out now reliably clears your stored GitHub token — previously, a credential lookup overlapping sign-out could leave the old token available to the next account signing in on the same machine.
- Credentials are now redacted consistently everywhere they're written, catching previously-missed formats.
- License and managed-repo token files are now encrypted at rest and written with tighter file permissions.
- Access scoping now fails closed rather than open: if session ownership can't be determined, routing blocks rather than widening access.
- The desktop app now authenticates with a device-scoped session via single-use code exchange, rather than a shared credential.

### Bug Fixes

- Accounts that cancelled but are still within their paid period keep write access until the period actually ends.
- Checkout is refused when you already have an active subscription — the pricing page now shows your current plan instead of offering a duplicate.
- Existing paying customers get an in-place upgrade path instead of a dead end.
- Plan-change direction (upgrade vs. downgrade) is now determined from your live subscription rather than a value that could go stale.
- A stray billing event can no longer un-cancel a subscription or cause your paid-through date to drift.
- Managed-repo limits corrected: Unlimited-tier teams get unlimited managed repositories (under a global safety ceiling), and BYO/connect teams are no longer capped at all.
- Trashed teams no longer resurrect on their own.
- Restoring a team from trash no longer risks a later archival deleting live data.
- Team trash view now shows the correct team name and is easier to read.

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

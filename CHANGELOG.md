# Changelog

All notable changes to Operum Desktop are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/).

## [0.50.0] - 2026-09-24

### What's New

- Windows builds are working again.
- **A card's status now shows a real priority instead of a "hold ?" badge that appeared on every card and explained nothing.** Held work is now visible directly on the board.
- The sidebar is reorganized into Run / Configure / System, and it's now resizable.
- The Project page is gone — Working Directory now lives in Settings.
- Plans are now Free / Standard / Premium — the fourth tier has been retired.
- Each team can now sign in to Claude with its own Anthropic login, alongside the setup token.
- A welcome screen replaces the Create Team pop-up that used to open on first launch.
- Board columns can be sorted by Updated or Priority.
- The agent chat panel is resizable, and the navigation snaps to set widths.
- An agent waiting on CI now has its own animated "waiting" state, and a message you send in Mission Control shows which agent it went to.
- The window is now titled "Operum - AI Agent Team", with "Your AI Team" under the logo.

### Improvements

- **Moving a Quick Start repository to your own GitHub account is no longer offered.** To take your code with you, use the zip export.
- **Claude can no longer be connected by reusing your Claude subscription login.** Use a setup token or your own API key.
- The plan screens in the app now match operum.ai/pricing, and a trial shows as PREMIUM, which is what a trial includes.
- Manual Claude setup now shows the exact `claude setup-token` command to run.
- The agent progress indicator now works for agents running on the Claude CLI.
- **Credentials are now redacted in live agent output and the activity feed**, not only in the stored journal. A token could previously appear in the place you are most likely to look while being masked in the place designed to be audited.
- **A group-facing agent does not act on privileged instructions from a public chat.** A message asking for a merge, a hold, or a dispatch is surfaced to you in the desktop rather than performed.
- Release notes are escaped before rendering, and the in-app changelog is now a link instead of a pasted copy.
- A credential that cannot be checked is no longer treated as working.
- Per-poll GitHub request accounting, so the cost of the polling loop is measurable rather than estimated.
- Build-cache write failures are now surfaced instead of silently degrading into slower builds.
- Cloning a repository is more resilient: a missing repository says so instead of blaming the network, a stalled clone can actually be retried, and a partially-damaged local checkout is detected and repaired.
- Comments an agent posts are now reliably attributed to the agent that posted them.
- Connecting GitHub is more resilient: temporary failures recover automatically instead of asking you to reconnect, and the separate connection paths no longer share state in a way that could confuse one for another.
- Custom credential setup now offers AWS as a supported pairing, and refuses a raw secret value entered where a name is expected.
- Integrations: edit a custom credential from its own card, with "Add Custom Integration" always visible.
- **A pull request could previously skip QA approval entirely by declaring it closed no issue.** That short-circuit is gone.

### Bug Fixes

- **Quick Start teams could freeze after startup.** Agents on a managed repository refused all work for a period, and the warning pointed to a GitHub setting that managed teams do not have. Both are fixed.
- **A Claude token that Anthropic had rejected could be saved as working.** The team then would not start, even though setup reported success.
- Idle agents were repeatedly flagged as stuck and restarted.
- Notifications and session summaries sent to agents could get stuck and be re-sent every 30 seconds and on every restart.
- An agent waiting on CI could be reported as stalled, or shown as stopped in the status bar.
- The GitHub counters in the status bar could stay blank on pages other than Mission Control.
- Internal status blocks could appear in the chat panel.
- Status dots and indicators were hard to read in the light theme.
- A resize handle appeared on the left navigation where it should not, and the chat panel replayed its slide-in animation.
- Archived Teams appeared outside the Team tab in Settings.
- **The workflow board could stop rendering entirely.** If the same issue arrived twice, the whole board went down rather than the one card. Duplicate entries now collapse to a single card, and a populated column no longer renders as empty depending on window size.
- **A screenshot could disappear between attaching it and sending.** The draft folder could be deleted while the message was still reading from it. Cleanup now waits until every attachment has been read; an attachment that fails to reach the agent is now reported explicitly instead of the agent proceeding as though the file were absent.
- **Error reports were attributed to nobody on most sessions.** Your account was recorded only when you signed in interactively, so an ordinary launch of an already-signed-in install reported errors anonymously. It is now restored at startup and cleared properly on sign-out.
- **A repository whose CI is shaped differently from Operum's own could not merge at all.** The coverage check required one exact job layout; it now falls back permissively and tells you what it couldn't confirm.
- An agent waiting on a slow CI run is now shown as waiting, rather than as actively working with a running timer.
- Agents were being sent to fix build failures that had never run. A job killed before it started now goes back for a re-run instead of being treated as a code fix.
- A paused issue no longer produces "this agent is idle" alerts, and an agent working through a slow CI run is no longer reported as stuck or handed new work while it waits.
- The retired Unlimited tier displayed inconsistently across screens; every screen now reads one source.
- Lifecycle emails were not being sent on schedule.
- The workflow board no longer writes a diagnostic file every time it recalculates what to display.
- The model picker's default selection is now legible instead of shown twice.
- Knowledge panels that couldn't load now say which of several reasons applies, instead of just appearing blank.
- A dropped model selection could leave an agent with no visible window and stuck on the same model; it's now recovered automatically.
- GitHub links shown in the app could render broken; they now render as intended.
- Scheduled tasks are now created idempotently, so retrying a schedule doesn't create duplicates, and a scheduled run that hasn't started yet is no longer misreported as timed out.
- PDF and other non-image attachments are now delivered directly to agents that can read files, instead of failing silently.

### For Developers

- Every agent commit now carries a DCO sign-off derived from the agent's own identity — no separate configuration needed.
- Agents can now request a re-run of a cancelled GitHub Actions run when the cancellation reason allows it.

## [0.49.0] - 2026-09-11

### What's New

- **Press `Ctrl+Shift+F12` to collapse every side panel**, and press it again to get your exact arrangement back — not a tidied default.
- **You now choose Full or Observer access when you pair a phone**, and can restrict a device's level afterwards without re-pairing. Raising it back to Full requires pairing again.
- **Mobile: a consent gate on signup and first sign-in**, so terms acceptance is recorded rather than assumed.
- **Merge-coverage enforcement is now a per-team setting**, defaulting to advisory, so a repository whose CI is shaped differently is no longer frozen out of merging.

### Improvements

- **A pull request could previously skip QA approval entirely by declaring it closed no issue.** That short-circuit is gone.
- **Phone tokens now last 30 days instead of 90.** Existing phones must be paired again — old tokens are rejected by design.
- **A credential could be read by any local user.** One alert path put a bot token in a subprocess command line, where it is readable for as long as the process runs. It now uses the same in-process transport as everything else.
- **A paid template could be self-granted** — a signed-in client could record a purchase nothing had charged for. That permission is revoked.
- **A custom integration could be named the same as a built-in credential and silently overwrite it**, including your team's GitHub access. Reserved names are now refused.
- A stored secret that could not be decoded is no longer treated as plain text.

### Bug Fixes

- **Mission Control on mobile did not work at all** — the screen errored and the chat was never mounted, on both iOS and Android.
- **A normal cold start was rendered as a catastrophe**, showing error copy and stopping agents for the couple of seconds before anything had loaded.
- The status-bar issue and pull-request counters could sit blank for a long time, looking identical to a team with no repository connected.
- An agent's Creativity setting reverted to the default on the next sync while its model and style persisted.
- **Windows helper binaries were not signed.** They are now signed before the installer is bundled, so the whole package is covered rather than just its outer layer.
- A malformed issue search reported zero results rather than an error.
- Documentation described the Windows SmartScreen warning as one-time and the app as unsigned. Both were wrong.
- A restarted app forgot which alerts it had already dismissed, so the same ones returned.
- Several alerts fired against work that was deliberately paused, reporting it as stalled.
- A message to the project manager could be delivered twice or lost.

## [0.48.0] - 2026-09-08

### Improvements

- **Secrets could be written to the runtime log.** PostHog keys with the `phs_`, `pha_` and `phr_` prefixes were not covered by log redaction. The patterns are now shared in one place rather than duplicated.
- Five dependency advisories resolved by upgrading. A sixth has no fixed version at any release, is development-only, and is not part of the signed application; it is recorded with a written rationale rather than silently ignored.
- Icon extraction no longer writes to a fixed, predictable temporary path.
- **The app now uses conditional requests on its heaviest GitHub read paths**, so a poll that finds nothing changed costs nothing against your rate limit. Repeated limit exhaustion had been taking the whole fleet read-only.
- **Agents no longer file a new issue for every observation.** Filing is now the exception rather than the default — a large reduction in automatically created tracking items.
- **The Claude session reconnect control has been removed.** It did not do what its label implied.

### Bug Fixes

- **A repeated `403` was answered by retrying at the same rate**, which is what sustains the limit that caused it. The check now backs off, and "your credential is bad" is no longer reported identically to "GitHub answered, but refused".
- **The owner-scope banner cleared as soon as the main app recovered**, while agents started during the outage were still read-only. It now distinguishes recovered from recovering and names which agents are still catching up.
- Agents could stay read-only indefinitely after an outage cleared.
- **One log line was roughly 90% of the entire runtime log**, at about a thousand records a minute, which made the log unusable as evidence. Only that line was quieted.
- A paginated fetch had no upper bound and could return a short list indistinguishable from a complete one. It now stops loudly rather than truncating quietly.
- An activity feed that could not be read presented as an empty one.
- A running internal schedule showed the empty state as though nothing were scheduled.
- A refused restore showed a generic message instead of the reason the server gave.
- Declining the Tailscale setup guide on mobile left you with no way forward.
- A failed repository clone gave up before the cause had been classified, so a fixable problem was reported as a generic failure.
- **A pull request could be treated as passing when a required check had not reported at all**, which is different from having reported a failure.
- An idle agent could block its own next assignment, and a stopped agent could be shown as busy.
- A time-limited grant could be issued to a paying customer, overriding the plan they were already paying for.

## [0.47.0] - 2026-09-04

### Improvements

- Board search now matches labels, not just issue number and title.
- A permanent auto-merge refusal is now surfaced to you rather than retried silently.
- Anthropic keys containing hyphens are now correctly scrubbed from logs.
- Settings offers to save when only the commit identity has changed.

### Bug Fixes

- The full rendered template panel no longer fails when local agent files are absent.
- The plan badge shows **Unlimited** rather than **Team** for a paying customer.
- The staleness banner is themed rather than hardcoded to dark-theme colours.
- Documentation no longer claims a mobile-app capability that does not exist.
- Architecture diagrams now all render instead of some being dropped.

## [0.46.0] - 2026-08-28

> **Known issue — macOS sign-in is NOT fixed in this release.** If you hit *"Can't sign in: no secure credential store on this machine"* on macOS, this release does not resolve it. What ships here is better diagnosis, not a fix: the keychain check previously collapsed five different conditions into one unhelpful "no keychain exists" message, and now reports which actually occurred. The workarounds in the sign-in screen still apply — unlock or start your keyring and re-check, generate a local key, or set `OPERUM_MASTER_KEY`.

### Bug Fixes

- **State from a previous team could survive a team switch**, which could suppress an alert, delay a dispatch, or excuse a stuck agent on behalf of a team you had switched away from. Clearing is now structural rather than a hand-maintained list.
- **A CI check that never ran reported the same as one that ran and passed**, so a lane that silently failed to execute looked green.
- An agent already working could be dispatched to the same work again.
- A withdrawn upstream dependency failed the security audit on every branch, including `main`, with nothing in the repository having changed.
- CI could not be pointed at a different runner pool when the default one was degraded, so an infrastructure outage had no manual escape.
- Agent instruction templates now promote correctly through their review path.

## [0.45.0] - 2026-08-26

### Bug Fixes

- **Green CI but the merge is refused.** A pull request with every required check passing could be blocked with *"Merge coverage could not be determined"*. The gate gave up before reading the required checks your repository declares. It now reads them — through classic branch protection **or** a ruleset — and accepts a check your merge policy requires, green at that commit, whatever shape your CI takes. One aggregator job counts the same as twenty separate ones.
- **QA approved the work, but the approval label never appeared.** The approval was written under a placeholder team while the reader looked for the real one, so the record existed and was invisible.
- **A pull request that could never be approved, however many times QA passed.** A retry is now idempotent.
- **A rate limiter that stopped enforcing during an outage.** When the shared limiter could not read your limit it allowed the request. Anything that authorizes, mutates or issues a credential now denies instead.
- **"Check your internet connection" when the cause was never determined.** An undetermined cause now says so rather than naming a wrong one.
- A failed credential refresh no longer stops the background task that performs it.
- Attachments are now confirmed as delivered — and when one does not reach the agent you are told explicitly, rather than the agent proceeding as though the file were absent.
- **Deleting a team now warns you before the deletion becomes permanent.**
- **Signing in on a machine whose credential store has changed** now offers to recover what it can still read, instead of proposing to wipe everything.
- Long comment threads are read to the end; previously only the first page was fetched, so an agent could act on a stale picture of the discussion.
- A GitHub server error is now reported as a server error rather than as a response-parsing failure.
- An unexpected error is recorded once rather than repeatedly.
- A scheduled sweep no longer re-proposes decisions you have already made.
- The prompt inspector no longer presents its preview as the exact text an agent received — it could differ.
- Filing feedback now tells you at the time if the issue cannot be triaged automatically.

### Improvements

- **An unassigned issue can no longer vouch for a merge.**
- **The Archives view has been removed**, along with a banner that pointed at it and no longer did anything.
- Interface wording is no longer rewritten through a substitution layer; the text you see is the text as written.
- Telegram notifications now carry sending limits and a kill switch, so a misbehaving loop cannot flood a channel.

## [0.44.0] - 2026-08-22

### What's New

- **Auto-merge is now available on the Standard $49 tier**, not only on higher tiers.
- **The trial is 14 days of full Premium**, described as such rather than as a bounded allowance.
- **Homepage restructured**, leading with the trial offer.

### Improvements

- Privacy policy and terms rewritten for accuracy — what is collected, where it is stored, and what leaves your machine.
- Account Settings call-to-action buttons updated.

### Bug Fixes

- **The Quick Start guided tour waits for you at each step** instead of advancing on a timer. On a freshly provisioned team several steps are already satisfied, and the tour previously walked itself to the end without asking for anything.
- An agent response could be dropped silently when its first line looked like bookkeeping, so completed work went unnoticed with no error anywhere.
- A refused pull request now names every way to satisfy the linked-issue requirement, including the opt-out for genuinely issue-less work.
- **An agent no longer refuses to start because a different agent has work in progress** — the busier the team, the likelier any given agent stalled on launch.
- **Merging no longer updates a pull request's branch unless your repository actually requires an up-to-date branch.** That update invalidated fresh QA approvals and restarted the entire CI run, once per merge for every other open pull request.

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

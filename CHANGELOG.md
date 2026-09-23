# Changelog

All notable changes to the South Africa privileged admin backend are recorded here.
Format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/); versioning
follows [Semantic Versioning](https://semver.org/) (MAJOR.MINOR.PATCH).

The version number shown here matches the `<meta name="app-version">` tag in
`index.html` and the `v{version}` badge in the page's footer.

## [0.7.0] — 2026-09-23

### Added
- Raw CSV export can now download **`customers`** (follow-up phone numbers, E.164) and
  **`customer_emails`** (follow-up emails) for the selected site.

### Fixed
- CSV export neutralises spreadsheet formula injection in visitor-typed text (cells starting with
  `=`, `+`, `-`, `@`), while leaving `+263…`-style phone numbers readable.

## [0.6.0] — 2026-09-23

### Added
- **Pipeline health card on Overview.** Per table, for the selected site: when the last real
  public submission arrived, counts for the last 24h / 7 days, and distinct devices (7d), with a
  🟢/🟡/🔴/⚪ freshness flag. Added because a silent backend rejection (below) looked exactly like
  "no testers yet" on this dashboard.
- `supabase-antispam-migration.sql` lives in `zwispqosp` (shared project, one migration for all six sites).

### Fixed
- A panel whose backend fetch failed (e.g. an expired login → `JWT expired`) stayed on
  "Loading…" forever because the error was only logged to the console. It now shows the real
  error in the panel, with a sign-in-again hint for auth errors.
- Root cause of missing tester results, fixed in the shared database (migration
  `add_device_id_antispam_and_open_lang_codes`): the public sites send a `device_id` column that
  didn't exist, so every public insert was rejected. Also opened `translations.lang` to any
  2–4 letter code and added the per-device rate-limit trigger the demo sites' comments promised.

## [0.5.1] — 2026-09-18

### Added
- Malawi (`mw`) added to `SITE_LABELS`/`MISSING_LANGS`/`PROVIDER_DIRECTORY` (this app is kept as a
  byte-identical copy of `zwispqosp` aside from title/version/`currentSite`). See `zwispqosp`'s
  CHANGELOG v0.6.1 entry for the full detail; the new `maispqosp` repo (v0.1.0) is Malawi's own
  copy of this same admin app.

## [0.5.0] — 2026-09-17

### Added
- **Role-based access control (RBAC), ported from zwispqosp v0.6.0.** `admins.role` is now one of
  `viewer` (read-only, the default for any newly-added admin), `country_admin` (read/write, scoped
  to the site codes in `admins.scope`), or `global_admin` (full read/write across every country,
  but only while break-glass is switched on). A "Global Admin · break-glass ON/OFF" badge now sits
  next to "Signed in as…" in the header, with a toggle for eligible global admins, logged to a new
  `admin_audit_log` table on every activation/deactivation via the `toggle_break_glass()` RPC. The
  moderation-delete, ISP-license-save, and translation approve/reject actions now check
  `canWrite(site)` client-side first; the real enforcement is server-side RLS shared across all
  five country apps (`has_write_access(site)` / `is_global_admin()`).

### Fixed
- **The site-selector dropdown/header only showed a country once it had a live row in
  `qos_reports`** — since South Africa had none yet, the dropdown rendered blank instead of
  showing "South Africa (za)". `populateSiteSelect()` now always seeds every known country from
  `SITE_LABELS` up front, so this app (and the other four) show their own country correctly from
  first load.

## [0.4.0] — 2026-09-17

### Added
- **Ported the full Drill-down Explorer from `zwispqosp` v0.5.0/v0.5.1** (this app is kept as a
  byte-identical copy of zwispqosp aside from title/version/`currentSite`): a single, condensed,
  breadcrumb-navigable Explorer replacing the previous stacked-card layouts across Provider
  analytics (City → Area → ISP → reports, now including South Africa's real, sourced areas from
  `saispqosd` v1.2.0), ISP licensing, Benchmark reports, Moderation and Translations.
- **Benchmark reports now render inline** as the leaf of the same breadcrumb Explorer, instead of
  opening in a new tab — full stat grid, 12-week QoS trend, complaint-cluster keyword tally and
  scrollable raw-comments table, with Print/Save-as-PDF via a scoped in-page print.
- Every Overview stat tile is now a clickable entry point into the relevant drill-down.

### Fixed
- Same rating-form truncation fix as `saispqosd` v1.2.0 (CSS grid `0fr → 1fr` expand, no fixed
  `max-height` cap) wherever this admin app renders the same expandable-row pattern.

### Notes
- See `zwispqosp`'s CHANGELOG v0.5.0/v0.5.1 entries for the full detail this app inherits.

## [0.1.1] — 2026-09-10

### Fixed
- Ported from zwispqosp v0.3.3: the Translations panel's "still-missing languages" note was
  hardcoded to Zimbabwe's list and was wrong here (South Africa's actual blank languages, per
  saispqosd v1.1.0, are Afrikaans, isiZulu, Sepedi, siSwati and isiNdebele — not the Zimbabwe list
  this panel was showing). Now renders the correct list for whichever site is selected via a
  `MISSING_LANGS` map, checked against each demo's real `I18N` content.
- Removed a dead, always-true conditional in the raw CSV export that looked like a bug (it wasn't
  — the site filter always applied — but the code was confusing).
- See zwispqosp's CHANGELOG v0.3.3 entry for the full detail; this app is a byte-identical copy of
  zwispqosp aside from title/version/`currentSite`, kept in sync.

## [0.1.0] — 2026-09-10

### Added
- First build for South Africa: the same admin app as zwispqosp (Supabase Auth +
  RLS, Google sign-in, passkeys, provider analytics, ISP licensing CRM, moderation,
  translation review, all six formatted exports), defaulting to `currentSite = "za"`.
- Connects to the same shared Supabase project as zwispqosp/bwispqosp (multi-tenant
  via the `site` column) — no separate database or migration for this repo; see
  zwispqosp's SETUP.md/SCHEMA.md for the backend itself.
- `PROVIDER_DIRECTORY.za` populated (this was also added to zwispqosp itself in the
  same change, so all three admin app copies stay in sync) — needed since Supabase
  only has the report tables, not a `providers` table.
- Brand footer/logo matching zwispqosd/zwispqosp from day one.

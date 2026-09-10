# Changelog

All notable changes to the South Africa privileged admin backend are recorded here.
Format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/); versioning
follows [Semantic Versioning](https://semver.org/) (MAJOR.MINOR.PATCH).

The version number shown here matches the `<meta name="app-version">` tag in
`index.html` and the `v{version}` badge in the page's footer.

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

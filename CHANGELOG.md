# Changelog

All notable changes to the South Africa privileged admin backend are recorded here.
Format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/); versioning
follows [Semantic Versioning](https://semver.org/) (MAJOR.MINOR.PATCH).

The version number shown here matches the `<meta name="app-version">` tag in
`index.html` and the `v{version}` badge in the page's footer.

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

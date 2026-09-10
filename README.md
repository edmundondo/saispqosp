# saispqosp

Private backend for the South Africa ISP Tracker (public site: saispqosd, live at
edmundondo.github.io/saispqosd).

This is the same admin app as [zwispqosp](https://github.com/edmundondo/zwispqosp),
pointed at the `za` site by default — one shared Supabase project serves Zimbabwe,
Botswana and South Africa via a `site` column, so there is no separate database or
migration to run here. See `zwispqosp`'s SCHEMA.md and SETUP.md for the full table
breakdown, RLS policy notes and backend setup steps; none of that is South
Africa-specific. Actual Supabase credentials live only in the Supabase dashboard —
never in this repo.

Contains all six formatted export options (PDF report, ISP/QoS/Status/Speed CSV,
EPUB) for whichever site is selected in the app, gated by Supabase Auth + RLS via
`is_admin()`, not a hardcoded service-role key.

See `CHANGELOG.md` for version history.

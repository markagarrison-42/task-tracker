# Task Tracker

Live at task-tracker.mg42apps.com. Part of the MG42 Apps suite. Owner: Mark (call him "bro").
Family chore and task tracker that replaced BusyKid's chore features. Mark's kids use it with their own logins.

## Stack
- Pure static HTML + JS (`index.html`), no backend, no build step
- Data: Supabase project `rwtgleklptqixhigbrzy`, tables prefixed `ck_`
- Hosted on Netlify

## Workflow (GitHub-first)
- GitHub `markagarrison-42/task-tracker` is the source of truth. Netlify deploys it straight from GitHub on push to main (no Actions workflow).
- This repo is PUBLIC. Never commit secrets, service-role keys, real emails or family data. The Supabase anon key is meant to be public; RLS is what protects the data.
- Mark sometimes uploads `index.html` through the GitHub web UI from his phone. Always pull before starting work so you don't overwrite those changes.
- Every push costs a Netlify deploy. Batch changes into one commit per feature set, one descriptive sentence.
- Mark tests on his phone before calling anything done. Never say a change is live until he confirms.

## Users and security
- Kids log in with their own Supabase accounts (plus-addressed emails). RLS locks each kid to their own tasks.
- Any schema or policy change must keep that isolation. A kid must never see or edit another kid's tasks.
- Supabase RLS: four explicit policies (SELECT, INSERT, UPDATE, DELETE), not `FOR ALL`. Upserts with `onConflict` need a real UNIQUE constraint.
- SQL schema files must be idempotent.

## Hard rules
- The app is plain JS inside `index.html` (no Babel), Supabase JS via CDN.
- Validate JS before committing and grep to confirm the edit landed.
- Dates: format with the `en-CA` locale to avoid UTC shifts.
- Supabase anon key must be the long `eyJhbGci...` JWT, never `sb_publishable_`.
- Fetch helpers must return distinct values for empty vs error.
- Before any direct Supabase data edit, tell Mark to close the app on all devices.

## Design
Dark `#0f0f0f` background, surface `#1a1a1a`, gold `#c8a96e` accent, system font.

## Debugging
- One diagnostic step before any fix.
- If save/edit/delete "doesn't work" but the code reads fine, add a temporary `alert()` probe. Safari's remote console often shows nothing.
- Slow login that eventually succeeds: check device clock skew (JWT "issued in future") first.
- iOS caches the home screen icon at install. Changing `manifest.json` alone does nothing.

# Rinkside — Prototype

Static HTML/CSS/JS prototype for Rinkside, an ice rink discovery and
booking app: a B2C side for skaters/patrons/teams and a B2B admin side for
rink operators. This is a design/UX prototype, not the production app —
there's a separate, real backend engineering track (Postgres/Express)
being built in parallel.

For the full history of what's been built, every bug found and fixed, and
the reasoning behind product decisions, see **`PROJECT-TRACKER.md`** —
it's the running decision log for this project and the most reliable
source of "why is it built this way."

## Running it

**Don't just double-click `index.html` and expect everything to work.**
Several pages (Checkout, Manage Booking, Session Detail, League
Registration, Player Stats, Create Team) load as overlays fetched via
JavaScript, and `fetch()` can't load sibling files under a plain `file://`
URL — there's no server. Some of these have a `file://` fallback built in
(the page opens directly instead of failing silently), but the safest way
to test the full prototype is with a real local server:

```bash
cd rinkside-prototype
python3 -m http.server 8000
# then open http://localhost:8000/index.html
```

Any static server works (`npx serve`, VS Code's Live Server, etc.) — the
only requirement is that it's an actual HTTP server, not a direct file
open.

**A note on previewing in the Claude apps specifically:** if you open one
of these HTML files as an attachment inside a Claude conversation, the
in-app preview is a sandboxed single-file viewer with no access to
sibling files at all. A 404 or broken link there isn't a code bug — it's
that preview's own limitation. Use a real browser (via a local server, or
opening the unzipped folder's files directly) to actually test navigation.

`index.html` at the repo root just redirects to `rinkside-signup.html` as
an entry point.

## Structure

Every `rinkside-*.html` file is broadly self-contained — its own `<style>`
block with CSS custom properties (design tokens) at the top, and its own
inline `<script>`. There's no shared stylesheet or JS bundle; a change to
shared visual language (e.g. a color token) currently has to be applied
file-by-file. This is a known, intentional prototype-stage tradeoff, not
an oversight — see `PROJECT-TRACKER.md` for when/why this came up (light
mode theming, for one).

**B2C — patron-facing**
`rinkside-welcome.html`, `rinkside-onboarding.html`, `rinkside-login.html`,
`rinkside-signup.html`, `rinkside-home.html`, `rinkside-explore.html`,
`rinkside-schedule.html`, `rinkside-saved.html`, `rinkside-checkout.html`,
`rinkside-manage-booking.html`, `rinkside-session-full.html`,
`rinkside-profile.html`, `rinkside-player-stats.html`,
`rinkside-teen-view.html`

**Rinks & associations**
`rinkside-rink-profile.html`, `rinkside-rink-profile-unaffiliated.html`,
`rinkside-association-profile.html`

**Teams & competition**
`rinkside-teams.html`, `rinkside-create-team.html`,
`rinkside-team-profile.html`, `rinkside-team-management.html`,
`rinkside-roster.html`, `rinkside-game-detail.html`,
`rinkside-tournament-detail.html`, `rinkside-standings.html`,
`rinkside-stats.html`, `rinkside-league-registration.html`

**B2B — rink operator admin**
`rinkside-admin.html`, `rinkside-admin-checkin.html`

## Known limitations

- No shared design-token file — theme changes (e.g. an eventual light
  mode) apply per-file until this gets consolidated.
- No real persistence for most flows — bookings, team creation, etc. use
  in-memory JS state that resets on page reload, seeded with sample data
  for demo purposes. A few flows (waivers, some booking state) use
  `localStorage` as a lightweight stand-in — see `PROJECT-TRACKER.md` for
  which.
- No live map — Explore's map view is an intentional static placeholder
  pending a real Maps/Places API key.
- No search bars — acknowledged as an engineering-phase item, not built
  into this static prototype.

## Related

- `PROJECT-TRACKER.md` — full chronological decision log
- `rinkside-badges-spec.md` — badge/achievement system spec, not yet wired
  into the prototype

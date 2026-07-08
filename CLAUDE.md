# CLAUDE.md — Spell Meditation (internal context)

Internal working context for Claude Code / devs. **Not part of the deployed
site** — excluded via `.assetsignore`. The public-facing front page is
`readme.md`. Read this first each session.

## What this is

Spell Meditation is a browser toy that sneaks people into meditation through the
premise of casting a spell. Press and hold to charge a spell, breathe with the
on-screen sigil that paces your breath, release to cast. The "spell" is the
meditation; the framing is the trick that gets people to sit still. Tone:
playful, not clinical.

## Status: live

Deployed at https://spellmeditation.com via Cloudflare (Workers Static Assets).
Static site — no backend, no build step, no framework; plain HTML/CSS/JS with a
canvas animation.

## Deploy & branch workflow

- `main` = production (Cloudflare deploys it to the live domain). `staging` =
  working branch; Cloudflare builds preview deploys for it.
- Work on `staging`, then fast-forward `main` when Matto says **"publish"**.
- `wrangler.jsonc` (repo root) configures Cloudflare: Worker name
  `spell-meditation`, `assets.directory: "./"` (serves the repo root).
- `.assetsignore` (repo root) lists files kept in the repo but NOT served
  (this file, `readme.md`, `styleguide.md`, `wrangler.jsonc`). Put anything
  internal here so it isn't fetchable at the domain.

## Files

- `index.html` — the whole app: canvas animation, Web Audio drone,
  feature-detected haptics + wake lock, the tip banner, and the crawlable
  landing copy below the hero.
- `privacy.html` — privacy policy, styled to match; linked from the footer.
- `styleguide.html` / `styleguide.md` — design-system reference (palette + type).
  The `.html` is `noindex` (served); the `.md` is internal (unserved).
- `favicon.svg`, `favicon.ico` (16/32/48), `apple-touch-icon.png` (180) — the
  `{7/2}` seven-pointed-star favicon set.
- `og-image.png` — 1200×630 social share image (sigil + wordmark + "Cast a Spell!").
- `robots.txt`, `sitemap.xml` — SEO; sitemap lists `/` and `/privacy.html` only
  (the styleguide stays noindex).
- `_headers` — Cloudflare headers (security + asset caching). Deliberately NO
  CSP / Permissions-Policy (would break wake lock, vibration, Google Fonts).
- `readme.md` — public GitHub front page (unserved on the site).
- Fonts load from Google Fonts CDN.

## Core loop — tuning levers (top of the script in `index.html`)

- `CHARGE_SECONDS` (27) — full hold to a castable spell.
- `BREATH_HALF` (4.5) — seconds per inhale/exhale; the breathing pace.
- `CAST_THRESHOLD` (0.7) — charge needed on release to count as cast vs. dissipate.
- `BANNER_DELAY` (4500) — ms after a cast before the tip banner slides up.
- `AUDIO_FADE` (3) — seconds the drone fades in from silence on hold.
- `SUPPORT_URL` — the Square payment link the tip buttons point at.

## Load-bearing design decisions (don't quietly reverse)

- **Animation is the primary feedback channel.** Audio + haptics are
  feature-detected progressive enhancement (they degrade on iPhone). Nothing in
  the core loop should depend on them.
- **iOS limits are real:** no Vibration API, unreliable locked-screen audio.
  Design for eyes-open, screen-on use.
- **Web-only for now;** native (Capacitor) deferred until web feedback justifies it.
- **Dark-only by design** (`color-scheme: dark`).
- `localStorage` is available on the real host (the earlier avoidance was an
  artifact-sandbox restriction). Fine to use for cast count / banner dismissal.
- The sigil star is a `{7/2}` heptagram (`STAR_STEP = 2`) — chosen over the
  hexagram (Star-of-David reading) and the sharper `{7/3}`.

## Copy & voice

- Published copy uses Matto's voice: direct, self-aware, tongue-in-cheek,
  footnotes welcome. (The privacy page is intentionally spicier than a typical
  neutral doc — that was a deliberate call.)
- Minimize em-dashes in polished copy (AI tell); rare + intentional is fine.
- Keep wellness claims soft ("a playful way into meditation"), never therapeutic.

## Naming

Settled: **Spell Meditation** (domain spellmeditation.com). Wordmark, `<title>`,
and OG/Twitter titles use it. The verb "cast a spell" stays throughout the copy,
and the in-content `h1` is the playful "Cast a Spell!" — only *proper-name* uses
are "Spell Meditation".

## How Matto likes to work

Peer-level, direct, minimal hand-holding; steelman the alternative before
recommending. Small verifiable slices — one change, show it, commit, move on.
Don't refactor broadly or add unrequested features without flagging. Commit
after each working change. Everything goes through `staging`; promote to `main`
only on "publish".

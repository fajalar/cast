# CLAUDE.md — Cast

Context for working on this project in Claude Code. Read this first each session.

## What this is

Cast is a browser toy that sneaks people into meditation through the premise of casting a spell. The user presses and holds the screen to charge a spell, breathes with an on-screen sigil that paces their breath, and releases to cast. The "spell" is the meditation. The framing is the trick that gets people to sit still.

Goal for now: ship it to the open web, free, and see if it resonates. If the feedback is good, expand it. Keep the tone playful, not clinical.

## Current status

A working, near-finished static site. Three files, no backend, no build step, no framework. Plain HTML/CSS/JS with a canvas animation. It runs by opening the HTML file in a browser.

Not yet done: a few placeholder values (see "Open placeholders"), and it is not yet an installable PWA (no manifest or service worker yet — that's a near-term step, not done).

## Files

- `index.html` — the whole app. (Currently named `spellcast-prototype.html`; rename to `index.html` for deployment.) Canvas animation, Web Audio, feature-detected haptics and wake lock, the tip banner, and the crawlable landing copy below the hero.
- `privacy.html` — privacy policy page. Serves at `/privacy.html`.
- `og-image.png` — 1200x630 social share image. Must sit at site root; the HTML references `/og-image.png`.

Fonts load from Google Fonts CDN, so there are no other local assets.

## The core loop and where to tune it

All the feel-tuning levers are named constants at the top of the script in `index.html`:

- `CHARGE_SECONDS` (27) — how long a full hold takes to reach a castable spell.
- `BREATH_HALF` (4.5) — seconds per inhale or exhale; sets the breathing pace.
- `CAST_THRESHOLD` (0.7) — how charged the spell must be on release to count as cast vs. dissipate.
- `BANNER_DELAY` (4500) — ms after a successful cast before the tip banner slides up.
- `SUPPORT_URL` — the Stripe Payment Link the tip buttons point at.

The two things most worth iterating on early: whether the charge length feels meditative vs. impatient, and whether the release lands as satisfying.

## Load-bearing design decisions (don't quietly reverse these)

- **The animation is the primary feedback channel.** Audio and haptics are enhancements layered on top and are feature-detected. Nothing in the core loop should *depend* on audio or haptics, because they are exactly what degrades on iPhone (see below). If you add feedback, add it as progressive enhancement.
- **iOS PWA limits are real.** On iPhone, the Vibration API does not exist (no haptics), and background/locked-screen audio is unreliable. Design for eyes-open, screen-on use. These gaps are what an eventual native build fixes, not something to fight in the web version.
- **Web-only for now.** Native (App Store / Play via Capacitor) is deferred until web feedback justifies it. Don't add native-build complexity yet.
- **Storage note:** the current code deliberately avoids `localStorage`/`sessionStorage` because it was first built inside the Claude.ai artifact sandbox, which blocks them. That restriction does NOT apply to the real hosted site. Once deployed you can freely use `localStorage` to persist things like cast count, streaks, or remembering that someone dismissed the tip banner across sessions. This is a good early enhancement.

## Roadmap

Immediate (to go live):
- Fill the open placeholders, rename to `index.html`, deploy (Netlify or Cloudflare Pages), point the Namecheap domain at it as a custom domain, test on a real phone (especially the audio, which is invisible on desktop).

Near-term:
- Tune the feel (constants above).
- Make it an installable PWA: web app manifest, service worker for offline, install prompt. Optionally push-notification reminders (works on iOS 16.4+ outside the EU).
- Persist cast count / simple stats with `localStorage`.

Later (only if feedback is good):
- A small library of guided casts — recorded narration layered on the same hold-to-cast mechanic. Needs an audio player and a session-list screen, plus a decision on bundling audio files vs. hosting them. Keep it a small set, not a sprawling catalog; the unique mechanic is the differentiator, guided content is depth.
- Native wrap with Capacitor for App Store + Google Play, including the native features (haptics, background audio, notifications) needed to clear Apple's "minimum functionality" review.

## Copy and voice

- When writing published copy (landing text, social posts, store descriptions), use Matto's voice: direct, self-aware, tongue-in-cheek, opinionated, footnotes welcome. Neutral/institutional docs (like privacy policy) stay plain.
- Minimize em-dashes in polished copy — they read as an AI tell. Rare and intentional is fine; Matto likes them personally, so don't eliminate them entirely.
- Keep wellness claims soft. "A playful way into meditation" is fine; anything implying it treats anxiety or improves health invites store and health-claim scrutiny. Frame it as play.

## Naming

The name is **Spell Meditation** (domain: spellmeditation.com). "Cast" was the placeholder wordmark; proper-name mentions were updated to "Spell Meditation" — the `.wordmark` div, the "Spell Meditation is a trick…" and "Spell Meditation doesn't collect…" lines, and the `<title>` / OG / Twitter titles (now "Spell Meditation: Cast a Spell!"). The verb "cast a spell" is kept throughout the copy, and the in-content `h1` is still the playful "Cast a Spell!" — only *proper-name* uses of Cast became Spell Meditation. (This file still refers to the project as "Cast" in places from its original draft; not yet swept.)

## Open placeholders

- `SUPPORT_URL` in index.html → your Stripe Payment Link (enable "customer chooses what to pay"). Still a placeholder; the tip buttons point at it.
- ~~`YOURDOMAIN.com`~~ → done: index.html now uses `spellmeditation.com` (canonical, og:url, og:image, twitter:image).
- `og-image.png` doesn't exist yet — the OG image will 404 until added.
- ~~`privacy.html`~~ → done: created, styled to match the site, linked from the footer, with contact email (matthew@studiovo.co) and operating company (Studio VO Ventures and Opportunities, Inc.) filled in.

## How I like to work

Talk to me like a peer who can read the code. Be direct, skip the hand-holding, don't over-hedge, and tell me when something is a bad idea. Steelman the alternative before landing on a recommendation.

For the build itself: work in small, verifiable slices. Make one change, let me see it, commit, move on. Don't refactor broadly or add features I didn't ask for without flagging it first. Commit after each working change so there's always a clean point to roll back to.

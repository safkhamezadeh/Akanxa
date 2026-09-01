# Akanxa Booking Demo — Project Context

## What this is

A working prototype of a rebuilt appointment-booking flow for **Akanxa Beauty &
Wellness Studio**, a real business in Amstelveen. This is unsolicited outreach
work: a demo built to show the owner what a fixed version of her booking page
could look like, as the opening move for landing her as a first freelance
client. It is not a real booking system — it does not write to any calendar
and no data leaves the browser.

The single file `akanxa-booking-demo.html` is the entire deliverable so far:
plain HTML/CSS/JS, no build step, no dependencies beyond a Google Fonts link.
Keep it that way unless there's a real reason to add tooling — it needs to be
easy to host anywhere (Netlify Drop, GitHub Pages) and easy to open on a
phone, since that's how the owner will most likely see it.

## The business

- **Website:** https://akanxa.nl/
- **Booking page:** https://akanxa.nl/appointments/
- **Location:** Kostverlorenhof 14, 1183 HE Amstelveen
- **Phone / WhatsApp:** 06 578 752 49
- **Email on site:** info@akanxa.com — note the mismatch with the `akanxa.nl`
  domain. Unverified whether this inbox is actually monitored.
- **Instagram:** instagram.com/akanxa.wellness.studio/
- **Hours:** Tuesday–Saturday, 10:00–18:00. Closed Sunday and Monday.
- **Positioning:** women-only studio. Stated explicitly on the Hair Removal
  page ("women-only hair threading services", bikini waxing done "in our
  women-only space") but **not stated anywhere on the booking page** — this
  is the root cause of the "can't book as a man" issue, not a bug.
- **Existing agency:** footer credits "Aptinnova" as site builder. Don't pitch
  a full redesign — pitch small, fixed-price fixes instead, so this doesn't
  read as trying to displace the incumbent.
- Owner is possibly named **Akanksha** (inferred from Instagram captions,
  unconfirmed).

### Likely tech stack (inferred, not confirmed)

WordPress with a booking plugin (something like Amelia, Bookly, or Salon
Booking System) rather than an embedded third-party booking service. Evidence:
a `/staff-login/` route exists on their own domain (a plugin pattern; an
embedded external service like Salonized wouldn't need this), and the
appointments page renders entirely client-side with no server HTML. **Not
independently verified** — the fastest way to confirm is View Source on
`/appointments/` and search for `wp-content/plugins/` or any `iframe` tag.

### The nail business is a separate company, not a bug to fix

"The Nailway" (https://the-nailway.salonized.com/) operates inside Akanxa's
location but runs its own Salonized booking account under its own brand. This
is a legitimate business boundary (confirmed by their own site text: "Located
inside Akanxa Beauty & Wellness Studio, Kostverlorenhof 14, Amstelveen"), not
something to merge. The fix here is just to explain the handoff, not remove
it — which is what the nail category in the demo already does.

## Confirmed issues on the live site

1. **Booking page (`/appointments/`)**
   - Navbar overlaps/covers part of the booking widget (CSS/z-index bug).
   - No way to go back a step to change an earlier choice, because the navbar covers the 'go back button' — have to restart on larger screen.
   - Selecting "male" leaves the calendar with no selectable dates and no
     explanation. Root cause: the studio is women-only, but this is never
     stated on this page (it _is_ stated on the Hair Removal page).
   - Nav menu on this page is missing items present on the homepage nav
     (Brows, Nail Care, Blog) — the two pages are out of sync.
   - Page's HTML is empty pre-JS-render — no SEO content, nothing indexable.
   - Last modified ~23 September 2025, vs. the homepage's ~27 July 2026 — this
     page has clearly not been touched in nearly a year while the rest of the
     site keeps getting edited.

2. **Homepage**
   - ~~The "Click Here!" link under Waxing points to `akanxa.nl/waxing/`,
     which 404s.~~ **Re-checked 2026-09-01: no longer true.** `/waxing/` now
     loads normally and shows the same Hair Removal content as
     `/threading-waxing/` (either fixed on their end, or it's an alias/
     redirect to that page). Confirmed by user report + WebFetch on both
     URLs. Don't include this in outreach.
   - Footer copyright reads © 2025 despite recent edits elsewhere.
   - Nail booking CTA sends visitors off-domain to `the-nailway.salonized.com`
     with no explanation of why it's a different brand/system.

3. **Instagram link**
   - The `<a>` href in the markup is correct
     (instagram.com/akanxa.wellness.studio/). User reports the link doesn't
     actually work when clicked on the live site — likely the same
     overlay/z-index issue affecting the booking widget, not a broken href.
     **Not yet independently re-verified** — confirm exact behavior before
     including this in any outreach message.

## Real service data already collected

Scraped from live service pages and used in the demo. Treat this as accurate
unless the site has changed since:

- **`/threading-waxing/`** (Hair Removal) — threading, body waxing, face
  waxing, bikini/intimate waxing. Full price list and most durations pulled
  into the demo.
- **`/facial/`** (Facials) — full list including the Hydra Facial, which is
  listed on the real site as _temporarily unavailable_ with a phone number to
  call. The demo mirrors this exact pattern (state _why_ something's
  unavailable rather than silently disabling it) for consistency with how the
  studio already communicates elsewhere.

**Not yet collected** — these four categories exist as empty placeholders in
`TREATMENTS` in the demo and need real data before this goes further:

- Brows
- Hair Care
- Spa & Massages
- Nails (intentionally external — links out to The Nailway rather than
  listing services, since that's a separate business)

**Estimated, not confirmed:** durations for face waxing and bikini/intimate
waxing items were estimated to fill out the demo. Verify against the real
site or ask the owner before this goes into anything she'd actually use.

## What the demo fixes

- Sticky header with content flowing below it — nothing gets visually
  covered.
- Persistent "chips" showing every choice made so far, each clickable to jump
  straight back and change it; a `Back` button in the header at every step
  past the first; full edit access from the final review screen without
  losing other answers.
- No gender selector at all. The women-only notice is shown up front, on the
  very first screen, using the studio's own wording from the Hair Removal
  page — so nobody invests time picking a treatment before hitting a wall.
- Closed days in the calendar are visually marked (hatched pattern + label)
  rather than just unclickable with no explanation.
- Nail category explains the handoff to The Nailway with a direct link,
  instead of silently redirecting.

## Code review notes (2026-09-01)

Full read-through of `akanxa-booking-demo.html` against this file's stated
goals. Findings not already covered by existing sections below:

- **The women-only notice is already live in the code, which raises the
  stakes on the open "does this apply studio-wide?" question below.** The
  notice at the top of step 1 ("Akanxa is a women-only studio... All
  treatments are carried out in a women-only space") is a paraphrase, not a
  verbatim quote from the Hair Removal page, and it now fires before *any*
  category is picked — including Facials, Brows, Hair Care, Spa, which are
  not confirmed to be women-only. This isn't just a future nice-to-verify
  anymore: it's a specific factual claim about the owner's business that's
  already in front of her in the demo. Confirm scope before sending, or
  narrow the wording to hair removal/waxing until confirmed.
- Minor code nit: `go()`'s scroll-to-top check (`"instant" in window`) tests
  for a property on `window` that doesn't exist, so it always evaluates
  false and silently falls back to `"auto"`. Harmless (no visible bug — page
  has no `scroll-behavior:smooth`) but dead logic; could simplify to
  `window.scrollTo(0,0)`.
- Minor content note: "Upper Lip, Chin or Forehead" appears twice under Hair
  Removal — once in the Threading group, once in Face Waxing — differing
  only by group heading and price. Likely intentional (matches how the real
  site prices it) but worth a glance in case it reads as confusing/duplicated
  to the owner.
- **Update (2026-09-01): addressed in code.** Brand name is now a link to
  `akanxa.nl` (opens same tab, deliberate exit); a small phone-icon link
  (`tel:+31657875249`) sits in the header next to it and stays visible
  through every step including the confirmation screen; `stepcount` text
  hides at ≤420px to make room, leaving the progress dots as the only
  progress indicator on very small screens. The `scrollTo` dead-code check
  was simplified to `window.scrollTo(0,0)`.
- **Update (2026-09-01): women-only notice narrowed as a stopgap.** Reworded
  from a blanket "Akanxa is a women-only studio" claim to "Our hair removal
  & waxing treatments take place in a women-only space, as stated on our
  website" — this only asserts what's actually confirmed on the Hair Removal
  page, and drops the implied claim about Facials/Brows/Hair/Spa. Still
  shown up front on step 1 before category selection. **This is a stopgap,
  not a resolution** — the open item below (confirm scope with the owner)
  still stands; revert to broader wording only once confirmed studio-wide.

## Design decision: header during booking

The full site navigation (Home / Waxing / Facial / Brows / Nail Care / Blog) should not appear on the booking flow. Once someone has committed to booking, every nav link is an exit — this is why checkout flows generally strip navigation down to logo + progress only.

The demo already does this: the sticky header shows just the brand name, a Back button, and step progress — not the full menu. Keep it that way.

Two small additions still worth making:

The brand name/logo in the header isn't currently a link. Make it click through to the homepage, so there's a deliberate way out for anyone who actually wants to leave, without it acting as a tempting nav item like the full menu would.
The phone number (06 578 752 49) is currently only in the footer. Consider surfacing it small in the header too — some people booking on mobile would rather just call, and it costs nothing to offer that next to the back button.

## Outreach plan (for context, not code)

- Approach: show her the working demo first, unprompted, then offer a
  fixed-price job to apply the fixes to the live site — "pay only if you're
  happy" to lower the barrier to yes.
- Do **not** pitch a full redesign or imply Aptinnova did bad work — frame it
  as small, specific, fixable things.
- Suggested price range: low hundreds of euros. Not free (undersells the
  work), not high (this is a first client / portfolio-building job).
- Given the mismatched domain/email and her working hours, WhatsApp
  (06 578 752 49) or an in-person visit may land better than cold email.
- Secondary/smaller lead at the same address: The Nailway (solo operator,
  ~16 hrs/week, no website at `thenailway.nl` despite using that name in her
  email and Instagram handle — separate, smaller pitch, lower priority than
  Akanxa but shares the same physical location).

## Open items / next steps

- [ ] Contact owner about the fix.
- [ ] Verify actual booking-plugin/tech stack via View Source on
      `/appointments/`.
- [ ] Re-confirm the Instagram link failure and what specifically happens.
- [ ] Collect real data for Brows, Hair Care, Spa & Massages.
- [ ] Confirm estimated durations for face/bikini waxing.
- [ ] Confirm whether "women-only" applies studio-wide or only to certain
      services, before finalizing that copy.
- [ ] Decide on and build a hosting link (Netlify Drop / GitHub Pages) so the
      demo can be shared as a URL, not just a file.

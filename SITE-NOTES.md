# Site Notes

Running log of what's been changed on this site and what's still open. Not a
changelog for every commit — a place to remember *why* things are the way
they are and what to look at next.

## Current information architecture (as of 2026-08-15)

The site's actual job right now is **one thing**: get people to sign up for
the Sentrya beta via the Google Form. It is not yet a full marketing site.

- **`index.html`** — the short landing page. Hero states what Sentrya is and
  why, one feature-tab section, one final CTA. Every CTA on this page points
  either to the beta signup Google Form or to `trial.html`.
- **`trial.html`** — the deep-dive page for people who want more before
  signing up: what the beta is, what testers get, what's expected of them,
  the not-a-medical-device disclaimer. Linked once from `index.html`'s "See
  how" button.
- **Everything else is intentionally unlinked for now**: `about.html`,
  `contact.html`, `faq.html`, `data-sharing.html`, `education-tools.html`,
  `team.html`, `migraine-log.html`, `privacy.html`, `terms.html`. No nav
  menu, no footer links to them. They exist but aren't part of the beta
  landing experience. Revisit this once the "rest of the site" work happens.
- `old-home.html` and `style-guide.html` look like Webflow-generated
  leftovers, not real pages — confirm whether to delete them or leave as-is.

Previously `index.html` was built like a general marketing homepage running
in parallel with `trial.html` as a separate gated beta page. That's why the
two pages used to duplicate a lot of the same content (two near-identical
tabbed feature sections, two different "get started" flows). That's been
collapsed down to the single-purpose structure above.

## What changed on 2026-08-15

**Whole-health rebrand pass** (index, about, trial):
- Retitled pages and rewrote meta/OG/Twitter tags and schema JSON to reflect
  migraines + sleep + glucose + pain + medications, not migraine-only.
- Removed deprecated "Sentinel Health" branding from index.html and
  trial.html schema and the about.html testimonial.
- Added the "Not a Medical Device" disclaimer section to `trial.html`
  (glucose data via Dexcom/HealthKit is historical tracking only, not
  alerts or medical advice).
- Anonymized placeholder team member names on `about.html` to "Team Member."

**Landing-page restructuring pass** (index, trial):
- Renamed every CTA from "Get started" to "Join the beta" for clarity — the
  Google Form *is* the beta signup, so the button should say so.
- Removed the second, redundant tabbed section on `index.html`
  ("Effortless tracking / Share with providers / Private by design") — it
  covered the same ground as the section above it and just added scroll
  without adding information.
- Removed three dead per-card buttons ("Try now," "View charts," "Set
  reminder") that linked to `#` and implied a live product that doesn't
  exist yet.
- Replaced the broken bottom-of-page signup block on `index.html` — it had
  an empty `<form>` with no input fields and a "Sign up Now" button linking
  to `#` — with a working final CTA section pointing at the Google Form.
- Rewrote the "Whole health, managed with clarity" subheading to lead with
  the actual problem (juggling separate apps for different conditions)
  before the solution, instead of jumping straight to feature description.
- Fixed a leftover "migraine tracking" phrase on `trial.html`
  ("Effortless migraine tracking, real results" → "whole health tracking").
- Removed the dead social-media icon rows (all linked to `#`, no real
  profiles exist yet) from both footers and replaced with a plain
  `mailto:` contact line.

## Open items for later

- **`no-reply@mysentrya.com` as the support contact address** — it's used
  as the "email us" link in both footers now, plus on `contact.html`. A
  "no-reply" address is a strange choice for something people are supposed
  to write to. Confirm whether it's actually monitored, or set up a real
  support inbox before the beta launch gets real traffic.
- **`contact.html` still has "Sentinel Health" and "Migraine Tracker app"**
  in its title, meta tags, and schema (including an `alternateName` of
  "Sentinel Health"). Wasn't touched in the rebrand pass since the page
  isn't linked from anywhere right now, but fix it before it goes live.
- **`contact.html`'s hero "Contact us" button is a dead `href="#"` link.**
  The second "Contact us" button further down the page (anchored to
  `#first-name-4`) actually works.
- **Stock imagery mismatch** — the hero and one feature card on
  `index.html` reuse an image alt-tagged "image of gaming tutorial," which
  doesn't fit the calm/health-forward tone next to the meditation and
  doctor images. Needs real product screenshots or better-matched photography.
- **`about.html` has a team card for "Melanie Brown / Lead Product
  Designer"** sitting separately from the testimonial that now also
  attributes a quote to "Melanie Brown / Sentrya Founder." That'll read as
  a duplicate/contradiction once the About page is actually linked —
  reconcile when About gets its real pass.
- **Deciding the real nav/IA** once the rest of the site is built out:
  which pages get a real menu, whether `trial.html` becomes the permanent
  home for beta info or gets folded back into `index.html` after the beta
  ends, what happens to `migraine-log.html`, `data-sharing.html`,
  `education-tools.html`, `team.html` (there's real written content in
  these already — worth reviewing before rewriting from scratch).
- **`old-home.html` / `style-guide.html`** — confirm these are safe to
  delete rather than orphaned pages someone still needs.

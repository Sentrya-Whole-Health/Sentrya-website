# Site Notes

Running log of what's been changed on this site and what's still open. Not a
changelog for every commit — a place to remember *why* things are the way
they are and what to look at next. (This file is excluded from the published
site via `_config.yml`.)

## How the site is built (as of 2026-11-04)

The site is a **Jekyll** site, built automatically by GitHub Pages from the
`main` branch (Pages "legacy" build, no Actions workflow needed). Every page
shares one layout, so the header and footer only exist in one place:

- `_layouts/default.html` — the `<head>` (title/meta/OG tags filled from
  each page's front matter), skip link, `<main>` wrapper, scripts.
- `_includes/header.html` — top bar (logo, site name, "Join the beta").
- `_includes/footer.html` — footer (logo, "Questions about the beta? Email us").
- `_includes/schema/<page>.json` — each page's JSON-LD structured data.
- `css/a11y.css` — accessibility overrides, loaded after the Webflow CSS.
- Each page (`index.html`, `trial.html`, …) is just front matter + its body:

  ```yaml
  ---
  layout: default
  title: "Page title for <title>, og:title, twitter:title"
  description: "Used for description, og:description, twitter:description"
  og_image: "https://…"
  wf_page: "Webflow page id — needed by webflow.js for that page's animations"
  schema: trial   # optional; loads _includes/schema/trial.json
  ---
  ```

To preview locally: `jekyll serve` (or `jekyll build` and serve `_site/`).
`_site/` is build output and is git-ignored.

**Webflow is no longer the source of truth.** The pages were split out of a
Webflow export. If anything is ever re-exported from Webflow, don't paste the
export over these files — copy the changed section into the matching page
body. `css/sentrya-whole-health.webflow.css` can be replaced by a fresh export
because all accessibility fixes live in `css/a11y.css` instead.

`old-home.html` and `style-guide.html` were **not** converted (no front
matter, so Jekyll publishes them untouched). They still have the old
header/footer and none of the accessibility fixes — see open items.

## Current information architecture

The site's actual job right now is **one thing**: get people to sign up for
the Sentrya beta via the Google Form. It is not yet a full marketing site.

- **`index.html`** — the short landing page. Every CTA points to the beta
  signup Google Form or to `trial.html`.
- **`trial.html`** — the deep-dive page: what the beta is, what testers get,
  what's expected, the not-a-medical-device disclaimer. Linked from
  `index.html`'s "See how" button.
- **`privacy.html`** — linked from `trial.html` ("Learn more" under
  "Your data, always in your control").
- **Everything else is unlinked for now**: `about.html`, `contact.html`,
  `faq.html`, `data-sharing.html`, `education-tools.html`, `team.html`,
  `migraine-log.html`, `terms.html`. They're still publicly reachable by URL.

## Accessibility (WCAG 2.2 AA) — audit of 2026-11-04

**How it was tested:** axe-core 4.10 (WCAG 2.0/2.1/2.2 A+AA rules plus best
practices) run in a real browser against every converted page, plus manual
checks: keyboard-only navigation and focus visibility, the tab widget's arrow
keys, reflow at 320px wide, WCAG text-spacing overrides, contrast of text
over photos, and behavior with the OS "reduce motion" setting on.

**Result:** all 11 converted pages pass axe with zero violations. The
reachable pages (index, trial, privacy) also passed the manual checks.

**Not yet done:** testing with a real screen reader (VoiceOver on Mac/iPhone,
NVDA on Windows). Automated tools catch roughly a third to half of issues;
a short VoiceOver pass through index → trial → the Google Form is the most
valuable next check.

### What was fixed

Site-wide (layout, header, footer, `css/a11y.css`):
- `lang="en"` on every page (screen readers pick the right voice/pronunciation).
- "Skip to main content" link and a `<main>` landmark on every page; this
  also fixed the duplicate "banner" landmark caused by hero `<header>`s.
- **Brand green darkened from `#3da280` to `#237256`.** White button text on
  the old green was 3.14:1 (needs 4.5:1); now 5.81:1. The green "Learn more"
  link on white had the same problem. Hover now darkens instead of lightening.
  *This is a visible brand-color change* — if you'd rather keep the lighter
  green, the alternative is bigger, bold button text (≥ 18.66px bold only
  needs 3:1), which changes the button design instead.
- Eyebrow labels (small caps above headings) raised from 60% to 80% strength.
- Feature tabs: inactive tabs were faded to 50% opacity (2.1–2.9:1). Now full
  strength, and the selected tab shows a thicker bar so it doesn't rely on color.
- Visible keyboard focus ring on everything (Webflow had removed it on tabs):
  navy on light backgrounds, white on dark ones.
- Footer text was dark-on-navy (invisible — my mistake from the 2026-08-15
  pass). Now white, with an underlined "Email us" link.
- Hero headings/subheadings in dark sections on about, contact,
  education-tools and team were navy/black on navy — invisible. Fixed.
- Signup links that open the Google Form in a new tab now say so to screen
  readers ("opens in a new tab"), and all of them consistently open in a new
  tab with `rel="noopener"`.
- Header: the logo image link (a duplicate of the "Sentrya Whole Health" text
  link next to it) is hidden from screen readers and the Tab order, so there's
  one clear home link. The mobile "hamburger" button was removed — there's no
  menu, so it opened nothing.
- All pages now show "Join the beta" in the header (some still said "Get
  started" for the same action — WCAG 3.2.4 Consistent Identification).
- **Reduced motion:** with the OS "reduce motion" setting on, scroll-driven
  parallax/scale/slide effects (trial page) are turned off. Strictly this is
  AAA (2.3.3), but motion can trigger migraines and vestibular symptoms, so
  for this audience it's treated as required.
- Images fixed at 400px on the trial page caused sideways scrolling on
  phones; they now shrink to fit.

Page-specific:
- **Alt text** (index, trial): Webflow's AI-generated alt text ("image of
  aromatherapy (for a massage therapist)", "image of gaming tutorial", …) was
  being read aloud. Mood photos are now marked decorative (`alt=""`); the hero
  phone photo is described as "Hands holding a smartphone showing the Sentrya
  Whole Health sign-in screen."
- **trial.html:** the three sub-sections under "Designed for clarity. Built
  for trust." are now `<h3>` (were `<h2>` at the same level as their parent),
  same visual size. "You may notice: …" is a real bulleted list instead of
  lines separated by `<br>`. "Learn more" is announced as "Learn more about
  privacy."
- **faq.html:** page had no `<h1>`; the title is now the `<h1>` and the
  questions are `<h2>`s. Same visual sizes.

### How to re-test after changes

Paste into the browser console on any page (local or live):

```js
const s=document.createElement('script');s.src='https://cdnjs.cloudflare.com/ajax/libs/axe-core/4.10.2/axe.min.js';s.onload=async()=>{const r=await axe.run(document,{runOnly:{type:'tag',values:['wcag2a','wcag2aa','wcag21a','wcag21aa','wcag22aa']}});console.table(r.violations.map(v=>({id:v.id,impact:v.impact,count:v.nodes.length,help:v.help})))};document.head.appendChild(s);
```

An empty table means no automated violations. Then Tab through the page
with the keyboard and confirm you can always see where focus is.

When adding content: give meaningful images real alt text and decorative
ones `alt=""`; don't skip heading levels; any new dark (`is-inverse`)
section containing a white card will need that card's headings set to a
dark color explicitly (see the note in `css/a11y.css`).

## Earlier changes (2026-08-15)

**Whole-health rebrand pass** (index, about, trial): retitled pages and
rewrote meta/OG/Twitter tags and schema to cover migraines, sleep, glucose,
pain and medications; removed deprecated "Sentinel Health" branding from
index/trial schema and the about testimonial; added the "Not a Medical
Device" section to `trial.html`; anonymized placeholder team names.

**Landing-page restructuring pass** (index, trial): renamed CTAs to "Join the
beta"; removed a redundant tabbed section and three dead `href="#"` buttons;
replaced a broken, empty signup form with a working final CTA; led the
"Whole health, managed with clarity" section with the problem being solved;
replaced dead social-media icons with a `mailto:` contact line.

## Open items for later

- **`old-home.html` / `style-guide.html`** — Webflow leftovers, still
  published, with the old header and no accessibility fixes (no `lang`, low
  contrast, etc.). Recommend deleting both; confirm nothing links to them.
- **Unlinked pages still need a content pass for accessibility before they go
  live:** they pass automated checks, but their alt text is still Webflow's
  AI prompts — about (9 images), education-tools (5), contact (2),
  data-sharing (2), team (2). Headshots on about/team need real alt text;
  mood photos should be `alt=""`.
- **`contact.html`'s form can't actually submit.** It's a Webflow form
  (`method="get"`, no action); Webflow form handling doesn't exist on GitHub
  Pages. Replace with a form service, a Google Form, or a `mailto:` link.
- **`education-tools.html` has invalid JSON-LD** (a malformed nesting around
  `"mainEntity"`, pre-existing). Search engines ignore it until fixed — it's
  in `_includes/schema/education-tools.json`.
- **`no-reply@mysentrya.com` as the support contact address** — used in the
  footer, on `contact.html`, and in privacy/terms. A "no-reply" address is a
  strange choice for something people are asked to write to. Confirm it's
  monitored, or set up a real support inbox.
- **`contact.html` still has "Sentinel Health" and "Migraine Tracker app"**
  in its title, meta tags, and schema. Fix before it's linked.
- **`contact.html`'s hero "Contact us" button is a dead `href="#"` link.**
- **Imagery** — the hero center image is now a real product photo, but the
  first feature-tab card still uses the old "gaming tutorial" stock image.
  Also, `images/Hands Tapping Sentrya Sign-In on Smartphone.png` is 1.7 MB;
  converting it to AVIF/WebP (like the other images) would speed up the page
  on slow mobile connections.
- **Footer logo** still uses the old `migraine_tracker_full_transparent.png`
  while the header uses the new `sentrya_webclip.png` — swap if intended.
- **`about.html` has a team card for "Melanie Brown / Lead Product
  Designer"** alongside a testimonial attributed to "Melanie Brown / Sentrya
  Founder." Reconcile when About gets its real pass.
- **The Google Form itself** is outside this site; Google Forms is generally
  accessible, but keep questions labeled clearly and avoid image-only questions.
- **Deciding the real nav/IA** once the rest of the site is built out.

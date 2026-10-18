# CLAUDE.md

Guidance for Claude Code (claude.ai/code) when working in this repository.

## What this is

The public web presence for **klk**, a private, invite-only photo and video app built
around small groups called *klks*. Pages: a landing page, a privacy policy,
`encryption.html` (how the end-to-end encryption works), terms of service, a support page (the App Store support URL), an invite page (`join.html`), and
`delete-account.html` — the account-deletion URL filed with Google Play, which checks it
live, so it must never move or 404. The app source lives in a
sibling repo (`../klk`) — this repo contains no app code and never should.

The `assets/` directory holds the Open Graph card (`og-image.png`, 2400×1260) that every
page links for link unfurls. It is regenerated from `assets/og-template.html`, a
standalone page rendered at 1200×630 and screenshotted — that file duplicates the design
tokens on purpose so it renders identically anywhere; keep it in sync with `styles.css`
by hand. Its header comment has the steps.

## Stack

Hand-written static HTML and CSS. **No build step, no framework, no package manager, no
dependencies.** The only external request on any page is the Google Fonts stylesheet for
Fira Sans — plus, on submit only, the landing page's request-access form, which posts to
the app's `requestAccess` Cloud Function (`../klk/functions`). Keep it that way: the whole value of this repo is that the privacy-policy URL
is stable, free, and cannot break.

Preview with `python3 -m http.server 8765`. Deploys are just a push to `main` —
GitHub Pages serves it at **klkapp.com**, via the `CNAME` file. Never delete that file;
losing the custom domain would break the privacy-policy URL already filed with Apple.

## Design

The stylesheet mirrors the app's design tokens from `../klk/constants/theme.ts`. When the
app's palette or type scale changes, port the change here rather than inventing a new
value.

- Warm bases, never neutral — `#FAF8F3` paper and `#191612` ink in light, `#14120F` and
  `#EFEAE1` in dark. Both themes are supported via `prefers-color-scheme`.
- Fira Sans 600 for headings, the system face for body and UI.
- Hairlines and generous whitespace carry the design; shadows are barely there.

**The one rule that holds it together: color only ever means "which klk."** The six klk
hues (pine, clay, indigo, plum, ochre, teal) appear only where they identify a klk — the
moment ring, the klk label on a post, the bar in the klks list. Nothing is colored for
decoration. Pine doubles as the interactive accent.

### The app mockups

The phones on the landing page are **stylized HTML/CSS renderings, not screenshots** —
deliberately, so small UI changes in the app do not force a reshoot. They should stay
faithful to the real screens' layout, wording, and hue usage, but they are an illustration
and are allowed to simplify (the post photo is pinned square so a whole post fits the
frame, for instance).

## Editing the legal pages

These are the actual App Store submission blocker, so accuracy matters more than polish.

- **Name what is end-to-end encrypted; never say "everything."** Photos, videos,
  thumbnails, captions, comments and profile photos are end-to-end encrypted, so "not even
  we can see it" is true of *those*. Emails, display names, klk names and colours,
  membership, and who posted, liked, viewed or commented when are still visible to the
  server — the "What we
  can still see" section of the privacy policy is the canonical list, and every other page
  links to it rather than restating it differently. If the app starts or stops encrypting
  something, that list changes first.
- Before claiming what the app does or does not collect, **verify it against the app repo**
  rather than assuming. The current text reflects Firebase Auth, Firestore (including
  profile photos, encrypted inline on the profile), Cloud Storage (only encrypted files from
  posts made before the move to R2), Cloudflare R2 (the
  encrypted photo and video files), Expo push (content-free), Sentry crash reports (which
  carry uid, email and display name, and no decrypted content), and on-device media
  preparation that strips photo EXIF and video metadata before encryption.
- Bump the `Last updated` date whenever the substance changes.

## The encryption page

`encryption.html` explains the end-to-end encryption twice: in plain words with
illustrations for anyone, then under "Geek stuff" precisely enough for an expert to
check (algorithms, Argon2id parameters, the key hierarchy, a sample sealed post, and
what it does *not* protect against). Every number and claim on it comes from the app
repo (`classes/crypto/`, `classes/klkKeys.ts`, `classes/mediaUpload.ts`,
`functions/index.js`, `firestore.rules`), so when any of those change, so does this
page. It links to the privacy policy's "What we can still see" rather than keeping its
own list. Keep the "doesn't protect against" list honest: it is what makes the rest
credible to the people who can tell. In its figures, a key or lock takes the hue of the
klk it belongs to (Family is pine, Cabin trip is clay), and nothing else is coloured.

## Conventions

- Two-space indent, double-quoted attributes, prose wrapped near 90 characters.
- Prefer a comment explaining *why* a non-obvious CSS or SVG choice was made — the shutter
  clip-path and the mockup photo ratio both have one.
- `privacy@klkapp.com` for data-rights requests; `support@klkapp.com` for everything else.

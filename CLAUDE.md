# CLAUDE.md

Guidance for Claude Code (claude.ai/code) when working in this repository.

## What this is

The public web presence for **klk**, a private, invite-only photo and video app built
around small groups called *klks*. Four pages: a landing page, a privacy policy, terms
of service, and a support page (the App Store support URL). The app source lives in a
sibling repo (`../klk`) — this repo contains no app code and never should.

The `assets/` directory holds the Open Graph card (`og-image.png`, 2400×1260) that every
page links for link unfurls. It is regenerated from `assets/og-template.html`, a
standalone page rendered at 1200×630 and screenshotted — that file duplicates the design
tokens on purpose so it renders identically anywhere; keep it in sync with `styles.css`
by hand. Its header comment has the steps.

## Stack

Hand-written static HTML and CSS. **No build step, no framework, no package manager, no
dependencies.** The only external request on any page is the Google Fonts stylesheet for
Fira Sans. Keep it that way: the whole value of this repo is that the privacy-policy URL
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

- **Never describe klk as end-to-end encrypted.** It is not, today. Content is protected by
  server-side security rules, which means the operator retains technical access. The
  honest claim is "only your klk can see it," not "not even we can see it," and the callout
  at the top of the privacy policy says so on purpose. When E2E ships, that callout is the
  first thing to change.
- Before claiming what the app does or does not collect, **verify it against the app repo**
  rather than assuming. The current text reflects Firebase Auth, Firestore, Cloud Storage,
  Expo push, and Sentry crash reports (which carry uid, email and display name).
- Bump the `Last updated` date whenever the substance changes.

## Conventions

- Two-space indent, double-quoted attributes, prose wrapped near 90 characters.
- Prefer a comment explaining *why* a non-obvious CSS or SVG choice was made — the shutter
  clip-path and the mockup photo ratio both have one.
- `privacy@klkapp.com` for data-rights requests; `support@klkapp.com` for everything else.

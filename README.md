# classifido-site

The static site at [classifido.com](https://classifido.com). No build step, no
framework, no dependencies, no JavaScript: GitHub Pages serves these files as
they are.

## One stylesheet

`site.css` is the whole site's answer to how it looks: the two faces, the
ground, the ink, the muted ink, the accent, the variable block, the two
breakpoints, the link face and the document type scale. Every page links it
and declares nothing of its own that it already answers.

It exists because that block was in three copies -- `index.html` and the two
placeholders -- the moment the second page appeared, and three copies of one
answer are three answers waiting to drift. **Nothing is inline in any page
now.** The lockup was the last thing that was, on the argument that only one
page had an element for it; `404.html` gave it a second, so it moved here too.

## The lockup is a copy, not an implementation

**The ClassıFido lockup on `index.html` — its markup, the mortarboard cap's
placement and its three `em` steps, the type scale, the spacing variables at
all three breakpoints, the ground and the mark — is a COPY of the output of
`src/render.py` in the product repo, taken from what `render.sign_in_page()`
actually returned.** If the sign-in page's lockup or scale changes there, this
copy goes stale silently and must be re-copied from that file — nothing in
either repo will tell you.

### Why a copy rather than a third drawing of it

The cap's optical scale is already a deliberate duplication. `web/src/
Wordmark.jsx` and `src/render.py` both hold it because Python cannot import
JSX, and `tests/test_wordmark_cap.py` holds the pair together with a shared
ten-input golden table. A third hand-drawn copy here would be a third answer
to the same question with no test behind it, so this repo takes the rendered
artifact instead and draws nothing of its own.

### Re-copying it

From a checkout of the product repo:

```bash
python -c "import sys; sys.path.insert(0,'src'); import render; print(render.sign_in_page('/start'))" > signin.html
```

Everything to lift lands in **`site.css`**, which is the whole of the answer
now: the `@font-face` rules for Source Sans 3, the reset, the `body` ground
and ink, the variable block and its two `@media` blocks, the link face, and --
in its own marked block near the top -- the `.wordmark` / `.i` / `.cap` /
`.fido` rules with the three `.cap` size rules that
`render.sign_in_cap_css()` appends. Replace that block rather than editing it
line by line; it is a rendered artifact, not code.

The `<div class="wordmark">` run and the `data:` URIs for the mark and the cap
are in the markup of the two pages that draw the lockup, `index.html` and
`404.html`. The faded dog is in `index.html`'s footer and `404.html`'s.

The rules that positioned the OLD landing page -- `.frame`, `.column`,
`.type`, `.tagline` -- are gone with it and are not in this list any more.

`fonts/SourceSans3-*.woff2` are the same files the product serves, from
`assets/fonts/`. `fonts/Caveat-SemiBold.woff2` is not from the product: it is
the Latin subset Google Fonts serves for weight 600, saved here because the
homepage's three hand-written notes are set in it and the privacy policy
promises no third-party request of any kind. Licences for both are in
`fonts/LICENSE.md`.

## What this repo deliberately does not match

- **Links are 600, where the sign-in page's `.secondary` is not.** That control
  is a de-emphasised way out from under a white button; these are the only
  controls on the page, and `web/src/link.css` states that an action link takes
  the accent and 600. The underline face itself is identical.
- **The bottom pad allows for a phone's home indicator, and the link rows
  carry negative margins.** `env(safe-area-inset-bottom)` is added to the
  bottom padding of `.page` and of the homepage's footer -- it was on the old
  landing page's `.frame`, which is gone. The negative margins buy 44px touch
  targets while giving the copied gaps back to the layout. Neither moves
  anything the sign-in page positions.

## Pages

- `/` — the landing page.
- `/privacy`, `/terms` — the privacy policy and the terms of service. **The
  prose in both is approved copy and is reproduced exactly**; the only things
  this repo added are the markup, the two `mailto:` links, the cross-reference
  from the terms' "Ending it" to `/privacy`, and the footer row. Anything that
  changes a word belongs upstream of this repo, not in it.
- `/help/class-link` — one heading and one screen recording, linked from the
  modal in the product that carries the steps in words.
- `404.html` — served by the host for any address it does not have. The bar
  and the footer are the homepage's, and every address on it is absolute,
  because the host answers a missing `/a/b/c` with this document while
  leaving `/a/b/c` in the address bar.
- `robots.txt`, `sitemap.xml` — everything here is public, so the rules allow
  all of it. The sitemap names the four real pages with the trailing slashes
  the host answers 200 on, and deliberately does not name `404.html`.

### No lone last words

`site.css` sets `text-wrap: pretty` on document paragraphs and list items and
`balance` on their headings, as the homepage has always had. Measured at
fourteen widths from 320 to 1920, the two documents carry seventy-two
paragraphs ending on a single word without those two lines and none with
them.

**Measure this character by character, not word by word.** A `Range` over a
whole word that the browser split at a hyphen returns one box spanning both
lines, whose top is the first line's -- so the word is credited to the line it
started on and whatever follows it looks stranded. Two "widows" were reported
here that way and a screenshot disproved both.

### A document page

Both are the same three-part shape: `.page` (the pad and the ground),
`.doc` (the measure), and a `.footer` holding the way back. The measure is
`58ch` written on `.doc` itself and deliberately NOT hoisted into a custom
property -- `ch` resolves where it is declared, so on `body`, which has no
font-size of its own, it would quietly mean 58 characters of the browser's
16px default and stop tracking the type scale at every breakpoint.

Running text is left-ranged, which the landing page is not. Three centred
lines under a mark are a lockup; nine pages of centred prose are unreadable.

## DNS

The apex is four `A` records at Porkbun pointing at GitHub Pages
(185.199.108–111.153), with `www` a `CNAME` to `classifido.github.io`.
`CNAME` in this repo is what binds the domain to the site.

The four apex addresses are GitHub's own and are the same whoever owns the
repository, so they did not move when this repository went from
`bradley-duitlabs` to the `classifido` organisation. The `www` record did:
a `CNAME` names the OWNER's Pages host, so it is `classifido.github.io`
now. The custom domain and its certificate survived the transfer intact --
`classifido.com` and `www.classifido.com` are both on the certificate and
HTTPS is still enforced -- so nothing here had to be re-entered.

## Analytics — the policy is written, the code is not

`/privacy` carries an `Analytics` section, a provider in the sharing list and
an effective date of **12 October 2026**. Nothing is built. The page describes
what the build has to be, so these are requirements and not preferences:

- Events go **browser → our server → PostHog**. The browser never talks to
  PostHog.
- **PostHog US region**, which is what "ClassiFido and our analytics are
  hosted in the United States" commits to.
- **No PostHog script on any page.** `/privacy` says "No third-party scripts
  on our pages" and that line was deliberately left standing.
- **No cookie and no device storage for the ID.** `/privacy` says the only
  cookies are the ones that keep you signed in.
- The ID is **derived per month on our server**, and is never sent with a
  name, an email, a school or an IP address.
- **GeoIP off.** `/privacy` says "No device or GPS location", and the
  Analytics section's own list of what is recorded is exhaustive.
- **Retention one year or less**, which is what "we keep them for no more
  than a year" commits to.

The policy also promises an email before a material change takes effect. The
product repo has no code that sends one, and its terms-agreement row is
recorded once per address and never re-checked against a version.

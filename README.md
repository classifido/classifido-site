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
answer are three answers waiting to drift. Only rules with no element on any
other page stay inline, which today is the lockup in `index.html`.

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

The values to lift land in two files now.

Into **`site.css`**: the two `@font-face` rules, the reset, the `body` ground
and ink, the variable block and its two `@media` blocks, and the link face.

Into **`index.html`**: the `.frame` / `.watermark` / `.column` / `.mark` /
`.type` / `.wordmark` / `.i` / `.cap` / `.fido` / `.tagline` rules, the three
`.cap` size rules that `render.sign_in_cap_css()` appends, the
`<div class="wordmark">` run, and the three `data:` URIs on the watermark,
mark and cap images.

The fonts in `fonts/` are the same files the product serves, from
`assets/fonts/`.

## What this repo deliberately does not match

- **Links are 600, where the sign-in page's `.secondary` is not.** That control
  is a de-emphasised way out from under a white button; these are the only
  controls on the page, and `web/src/link.css` states that an action link takes
  the accent and 600. The underline face itself is identical.
- **There is a `padding-bottom` on `.frame` and negative margins on the link
  rows.** The first clears a phone's home indicator; the second buys 44px touch
  targets while giving the copied gaps back to the layout. Neither moves
  anything the sign-in page positions.

## Pages

- `/` — the landing page.
- `/privacy`, `/terms` — the privacy policy and the terms of service. **The
  prose in both is approved copy and is reproduced exactly**; the only things
  this repo added are the markup, the two `mailto:` links, the cross-reference
  from the terms' "Ending it" to `/privacy`, and the footer row. Anything that
  changes a word belongs upstream of this repo, not in it.

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

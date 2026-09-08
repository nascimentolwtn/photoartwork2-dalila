# Feature Plans

Planning notes for upcoming site changes. No code changes are made until a plan here is confirmed and explicitly greenlit.

## Homepage redesign

**Status:** direction chosen (2026-09-05) — **Editorial Dark**, out of the three sketched in the "Dalila Homepage Directions" design canvas (Gallery White / Editorial Dark / Asymmetric Grid). A modernized version of the current dark theme: warm charcoal background, editorial serif display type (Bodoni Moda) + Work Sans body, a deep amber accent replacing the dated flat green, and an Instagram icon in the nav and footer.

**In progress:** migrating this direction into real HTML/CSS in `preview/editorial-dark/` — a folder not linked from the live site's navigation, so it can be reviewed (this weekend, with Dalila) without touching the live pages. All six pages (Home, Sobre, Portfólio, Design, Links, Contato) are migrated and cross-linked, using real photos and content ported from the live site. Published via GitHub Pages on this branch for review.

**Feedback round 1 (2026-09-05):**
- Portfolio thumbnails were too low-res (they used the old tiny `_thumb.jpg` files, some only ~50×75px). Fixed: the gallery now uses the full-size images directly, and swaps in the higher-resolution originals found in `images/portfolio_dalila/originais/` where available.
- The homepage's single static hero image was a step down from the live site's auto-advancing image carousel. Fixed: rebuilt the hero as a real crossfading carousel (vanilla JS, autoplay + dot navigation) using real photos.
- Requested a more modern feel: full-screen, alternating image/content sections that reveal as you scroll (common on contemporary portfolio/agency sites). Added three of these below the carousel, each pairing a real photo with a short description and a link further into the site, using IntersectionObserver + a CSS fade/slide-in (falls back to fully visible if JS doesn't run).
- Caught and fixed two content-accuracy issues while doing this: `home_2.jpg`/`home_3.jpg` turned out to be leftover stock photos (tulips, stone ruins) from the original template, not Dalila's work — removed from the carousel. `código de barras Dalila.jpg` (despite its filename) is actually a photo of a mirrored floor lamp, not the "Poluição das Águas" piece — recaptioned correctly and swapped the Poluição section over to the correct photo (`poluicao-verso.jpg`).

**Known follow-up (not urgent):** a couple of the higher-resolution originals are quite large as files (e.g. the "Árvore da Vida" photo is ~2.5MB); fine for this weekend's review, but worth compressing/resizing before this becomes the real live site.

**Feedback round 2 (2026-09-05) — two mobile/Chrome Android bugs reported:**
- Scroll-reveal wasn't triggering. Root cause: a CSS specificity mistake — the rule that hides each section by default happened to be *more* specific than the rule meant to reveal it once scrolled into view, so the "reveal" could never win even though the scripting was working correctly. Fixed by rebalancing the selectors so the reveal rule always wins. Also reduced how much empty space each section reserved on narrow screens, which was making the page feel stuck while scrolling toward the next section.
- Portfolio thumbnails opening in an overlay "broke totally" on phone. Rather than patch the overlay (fixed-position overlays are a common source of mobile browser bugs, especially around locking background scroll), replaced it with the same pattern the current live site already uses: a large "now viewing" image above the thumbnail grid — clicking a thumbnail swaps that image in place and scrolls up to it, no overlay, no scroll-locking, nothing fixed-position.

**Feedback round 3 (2026-09-05):**
- Scroll-reveal requested on every page, not just Home. Added the same fade/slide-in-on-scroll treatment to Sobre (bio block), Portfólio (viewer + thumbnail grid), Design (each object card, staggered), Links (each list entry, staggered) and Contato (the info/form block) — same mechanism as the homepage (falls back to fully visible without JS).
- Dalila found the background too dark; she likes the warm/brown tone and the fonts as they are. Lightened the whole palette (background, panel, and border tones moved from near-black to a warm mid-brown) while keeping the same amber accent color and typography. Colors are now defined as CSS variables at the top of `style.css` so further tone adjustments are a one-line change instead of hunting through the file.

**Feedback round 4 (2026-09-05) — restructured as a one-page site:**
- The reveal effect still wasn't reading as "working" on the secondary pages — turned out the real cause was that Sobre/Design/Links/Contato had so little content that each section was already visible on load, so it faded in almost instantly instead of on an actual scroll.
- Referenced two examples: a template with a strong scroll-reveal feel (scroll-revealing.webflow.io), and a one-page site where the nav smooth-scrolls to sections instead of loading new pages (webflow.com/made-in-webflow/website/flaechenverbauung). Also asked for full-width banner photos between sections, echoing the very first ask in this whole thread (interlaced full-screen photos and revealed content).
- All three point at the same fix: **`index.html` is now the entire one-page site** — Home, Sobre, Portfólio, Design, Links and Contato are sections on one continuously-scrolling page, joined by full-width banner photos, with a sticky nav that smooth-scrolls to each section (`#sobre`, `#portfolio`, ...) and highlights the current section as you scroll past it. This gives every section real scroll distance, so the reveal effect reads properly everywhere, not just Home.
- The previous separate `sobre.html` / `portfolio.html` / `design.html` / `links.html` / `contato.html` files are left in place but are no longer linked from the nav — kept only in case we want to compare against the one-page approach; they won't be kept in sync with further changes unless asked.

**Feedback round 5 (2026-09-05):** liked the hide-again-on-scroll-up behavior. Five more requests:
1. Swipe left/right on the home carousel — added a shared touch-swipe helper (drag past ~40px, mostly horizontal) and wired it into the carousel alongside the existing dots/autoplay.
2. Portfolio "still broken" on Chrome Android. Likely real cause: the thumbnails were `<a href="image.jpg">` elements with a JS `preventDefault()` — if that handler ever lost the race on a real device, the tap would fall through to a plain navigation to a bare image file, leaving the whole site. Fixed properly rather than patched again: thumbnails are now `<button>` elements with no `href` at all, so there is nothing for the browser to navigate to even if JS were to fail. The main viewer is also now swipeable (same helper as the carousel) with prev/next buttons, so there's no separate "enlarge" step to break — the big image *is* the viewer.
3. Home headline changed from an invented line to one grounded in the current live site's own content: the Leonardo da Vinci–inspired quote ("A arte diz o impronunciável...") that was already used as a homepage slide caption on the live site, with its attribution shown under the headline.
4. The "Natureza e a Luz" section's image read as too similar to the "Sobre" portrait next to it (both fairly neutral, domestic-toned photos). Swapped in `portifolio_6.jpg` ("Iluminação Étnica Interna") — an actual lamp piece, illuminated from inside — not used anywhere else on the page, and rewrote that section's heading/text to describe it honestly instead of the previous invented title.
5. Asked how the Contato "Enviar" button works: **it doesn't send anything itself.** The form uses the same old-school trick as the current live site (`action="MAILTO:..."`), which asks the visitor's browser to open their computer/phone's default email app with a pre-filled draft — nothing is transmitted unless the visitor's device has an email app configured and they hit send there. It's unreliable on modern browsers and especially on phones with no default mail client. Added a one-line note under the button explaining this so it's not mistaken for a real submit. A real fix needs either a plain `mailto:` link (same limitation, simpler) or a form backend (e.g. Formspree/Netlify Forms) that actually delivers the message — worth deciding before this goes live.

**Feedback round 6 (2026-09-05):** swipe confirmed working well. One follow-up on Portfólio, seen on Chrome Android: a huge blank gap scrolling from Portfólio into Design. Two real causes, both fixed:
- The 20-thumbnail grid below the viewer was rendering (visible in "desktop site" mode) but effectively dead weight now that the viewer itself is swipeable — on a phone it's ~20 stacked 300px-tall rows of mostly-empty-feeling scroll before reaching Design. Hidden the thumbnail grid entirely under 900px width; swipe (plus the new prev/next buttons) is the only navigation on phones. Added a small "3 / 20" position counter next to the swipe hint so there's still a sense of place without the grid.
- Separately, the viewer image's *box* was sized by each photo's own aspect ratio (`max-height` on the `<img>`), so the page's content height changed slightly every time you moved to a differently-shaped image — a layout "swing" while swiping, worst on desktop where it's most visible. Fixed by giving the viewer frame a fixed height and letting the image letterbox inside it (`object-fit: contain` on a box whose size no longer depends on the image), so moving between photos never resizes the page.

**Known follow-up (not urgent):** the one-page version loads all of Dalila's portfolio images on a single page load; added `loading="lazy"` to everything below the fold, but a real launch should still resize/compress the largest source images (see round 1's note).

## Instagram post links (replaces Notion articles plan)

**Status:** planning — not implemented yet.

**Context:** the original idea was for Dalila to write new articles in Notion, which would then be turned into new blog-style pages on the site (like `blog_autvis.html`, `blog_feitio_oracao.html`, etc.), linked from the "Links" page (`blog_2004.html`).

**Change of plans (2026-09-05):** she won't write new articles in Notion. Instead, new entries on the "Links" page will link directly out to her existing Instagram posts, rather than to a full article page on this site.

**Proposed approach: manually curated entries** (chosen over oEmbed, third-party feed widgets, or the Instagram API — see rationale below):
- Add new entries to `blog_2004.html`, following the same pattern as existing entries (`<h4>` title + `<p>` description with an `<a href="..." target="_blank">` link), but pointing at Instagram post URLs instead of a local blog page.
- Optionally save a screenshot/thumbnail of the post locally (like the other images in `images/`) to show alongside the entry, same as the existing `right_content` image on that page.
- No Notion integration, no article-writing workflow, no third-party script or API dependency needed — just a hand-added entry each time there's a new post to feature.

**Why this approach:** compared for a static, hand-maintained HTML/CSS site with no backend:
- *Official oEmbed embed* (Instagram's `<blockquote>` + `embed.js`) renders the live post inline, but pulls a script from Instagram on every page load and can break if Instagram changes the embed API.
- *Third-party feed widgets* (SnapWidget, LightWidget, Elfsight) auto-update a grid of latest posts, but add a paid/rate-limited third-party dependency for something that can just be a link.
- *Manually curated entries* match the curated-entry style already used on the Links page, have zero external dependencies, and won't break — tradeoff is someone has to add an entry (and optionally a screenshot) by hand each time.

**Open questions / inputs needed before implementing:**
- Instagram post URL(s) to link (for the Links page entries).
- Short title/caption to use as the link text for each post.

## Contact form — actually deliver messages (email / WhatsApp)

**Status:** planning — options gathered, not decided. See [ADR 0005](adr/0005-contact-form-delivery.md) (Proposed).

**Problem:** `protos/editorial-dark/contato.html` (and the live site) use
`action="MAILTO:dalila.nascimento@gmail.com"` on the form. That transmits
nothing — it only asks the visitor's device to open a mail app with a draft.
Unreliable on desktop, mostly broken on phones with no configured mail client
(see homepage redesign feedback round 5, item 5). Constraints: pure static
HTML on the orgfree.com free tier, FTP-only deploy, no PHP / backend
([ADR 0002](adr/0002-hosting-stay-on-orgfree.md)), ideally no secrets in the
page source.

Contact details in the prototype: Tel. (11) 2959-0226 · Cel. (11) 97282-2004 ·
`dalila.nascimento@gmail.com` · Instagram `@dalilahsnas`. WhatsApp E.164 for
the mobile: `5511972822004`.

### WhatsApp options

- **A. Click-to-chat link/button** — `https://wa.me/5511972822004?text=<pre-filled>`.
  Zero dependencies, most reliable on mobile (opens the app; WhatsApp
  Web/desktop otherwise). Add as a CTA next to the form.
- **B. Form that hands off to WhatsApp** — keep the Nome/Email/Mensagem
  fields; on submit, `preventDefault()` and
  `window.open('https://wa.me/5511972822004?text=' + encodeURIComponent(templated body))`.
  Visitor reviews in WhatsApp and sends. Message comes from *their* number and
  requires they have WhatsApp; Dalila gets their WhatsApp contact, not
  necessarily an email.

### Email options

- **C. Third-party form backend (recommended for email).** Form POSTs to their
  endpoint, they forward to Gmail. No backend, no build step, free tiers.
  - *Web3Forms* — one access key in a hidden field, no account; honeypot +
    optional hCaptcha; unlimited free tier.
  - *FormSubmit.co* — `action="https://formsubmit.co/<email or token>"`, no
    account; first submission triggers a one-time activation email.
  - *Formspree* — account + form ID; 50 submissions/mo free.
  - *Getform / Basin* — account + endpoint; similar.
  - Optionally `fetch()` on submit to stay on the page with an inline
    "mensagem enviada" confirmation instead of redirecting.
- **D. EmailJS** — client-side JS sends via a connected Gmail account, free
  200/mo. Downside: public key sits in the page (scrapeable for spam), needs
  their JS SDK loaded.
- **E. Embedded Google Form** — replace the custom form with an embedded
  Google Form; delivers to a Sheet + email notification, free, no backend.
  Costs design control (iframe) — or just link out to it.
- **F. Honest `mailto:` link** (not a form) —
  `<a href="mailto:dalila.nascimento@gmail.com?subject=Contato%20site">`. Same
  limitation as today but no fake "Enviar" button. Keep the `tel:` links too.

### Ruled out

- Serverless functions (Netlify/Cloudflare) and Netlify Forms — would mean
  leaving orgfree, which [ADR 0002](adr/0002-hosting-stay-on-orgfree.md)
  rules out.

### Leaning toward (for the ADR to confirm)

Do both channels — they serve different visitors:
1. **WhatsApp click-to-chat button** (A) as the primary CTA — what Dalila
   actually checks, nothing to maintain, never breaks.
2. **Web3Forms or FormSubmit** (C) behind the existing form for people who
   prefer email — one-time setup, delivers to Gmail; add the honeypot for spam.
3. Keep the visible `mailto:` and `tel:` links as fallback.

**Open questions:** confirm the WhatsApp number is the right one to publish;
pick Web3Forms vs FormSubmit; decide whether email delivery is even needed if
WhatsApp + visible email/phone links are enough.

## General Instagram follow link in the footer

**Status:** planning — not implemented yet.

**Decided (2026-09-05):** add a general "follow on Instagram" link in the site footer (the commented-out social icons row present on `index.html`, `about.html`, `contact.html`, `blog.html`, `blog_2004.html`), pointing to https://www.instagram.com/dalilahsnas

**Proposed approach:**
- Uncomment/adapt the existing footer social icons block and add an Instagram icon/link alongside (or replacing) the current twitter/facebook/rss placeholders, since there's no `images/instagram.png` asset yet.
- Apply consistently across all pages that carry that footer block.

**Update (2026-09-05):** the Instagram link should also be added on the (new) contact page — currently `contact.html`, which lists phone/email/contact form and links to the old site. Note: it's not yet clear if "new contact page" means a planned redesign of `contact.html` or just adding the link to the existing one; needs confirming before implementing.

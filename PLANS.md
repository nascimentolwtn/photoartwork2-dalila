# Feature Plans

Planning notes for upcoming site changes. No code changes are made until a plan here is confirmed and explicitly greenlit.

## Instagram post links (replaces Notion articles plan)

**Status:** planning — not implemented yet.

**Context:** the original idea was for Dalila to write new articles in Notion, which would then be turned into new blog-style pages on the site (like `blog_autvis.html`, `blog_feitio_oracao.html`, etc.), linked from the "Links" page (`blog_2004.html`).

**Change of plans (2026-09-05):** she won't write new articles in Notion. Instead, new entries on the "Links" page will link directly out to her existing Instagram posts, rather than to a full article page on this site.

**Proposed approach:**
- Add new entries to `blog_2004.html`, following the same pattern as existing entries (`<h4>` title + `<p>` description with an `<a href="..." target="_blank">` link), but pointing at Instagram post URLs instead of a local blog page.
- No Notion integration, no article-writing workflow needed.

**Open questions / inputs needed before implementing:**
- Instagram post URL(s) to link (for the Links page entries).
- Short title/caption to use as the link text for each post.

## General Instagram follow link in the footer

**Status:** planning — not implemented yet.

**Decided (2026-09-05):** add a general "follow on Instagram" link in the site footer (the commented-out social icons row present on `index.html`, `about.html`, `contact.html`, `blog.html`, `blog_2004.html`), pointing to https://www.instagram.com/dalilahsnas

**Proposed approach:**
- Uncomment/adapt the existing footer social icons block and add an Instagram icon/link alongside (or replacing) the current twitter/facebook/rss placeholders, since there's no `images/instagram.png` asset yet.
- Apply consistently across all pages that carry that footer block.

**Update (2026-09-05):** the Instagram link should also be added on the (new) contact page — currently `contact.html`, which lists phone/email/contact form and links to the old site. Note: it's not yet clear if "new contact page" means a planned redesign of `contact.html` or just adding the link to the existing one; needs confirming before implementing.

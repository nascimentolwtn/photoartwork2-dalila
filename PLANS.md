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
- Instagram post URL(s) to link.
- Short title/caption to use as the link text for each post.
- Whether a general "follow on Instagram" link (profile-level, not post-specific) is also wanted, e.g. in the footer.

# Random Tuesday — company website

Static marketing and legal site for Random Tuesday, the studio behind
[PadPing](https://randomtuesday.app/padping). Plain HTML and one stylesheet — no
framework, no build step, no dependencies.

The site exists for three reasons:

1. Apple requires a **publicly available, functional website on a domain associated
   with the organization** before it will approve an organization enrollment in the
   Apple Developer Program. A parked domain or a social link is explicitly rejected.
2. Both stores require a **hosted privacy policy URL**.
3. Both stores require a **support URL** that a real person monitors.

## Structure

```
index.html      Company homepage
padping.html    Product page
support.html    Support + FAQ  (use as the App Store / Play "Support URL")
privacy.html    Privacy policy (use as the "Privacy Policy URL")
terms.html      Terms of use
404.html        Not-found page
styles.css      All styles, light and dark
netlify.toml    Publish config, security headers, .html -> clean-URL redirects
robots.txt      Allows everything, points at the sitemap
sitemap.xml     Five URLs
favicon.svg     Wordmark mark
```

## Before this goes live — required edits

Every placeholder is in `[SQUARE BRACKETS]`. Find them all with:

```bash
grep -rn "\[[A-Z ,]*\]" --include="*.html" .
```

| Placeholder | Replace with | Must match |
|---|---|---|
| `[LEGAL ENTITY NAME]` | Registered legal name of the entity | Your D&B record, **exactly** — including the suffix and punctuation |
| `[STREET ADDRESS]` | Registered business street address | Your D&B record |
| `[CITY, STATE ZIP]` | City, state, ZIP | Your D&B record |
| `[STATE]` / `[COUNTY, STATE]` | Governing law and venue in `terms.html` | Where the entity is registered |

This matters more than it looks: Apple and Google both validate organization
details against Dun & Bradstreet, and the address on your D-U-N-S record is
published on your App Store product page under EU DSA trader rules. If the footer
here says something different from what you type into the consoles, it invites a
manual review on both platforms.

### Domain

The site is written for **randomtuesday.app**. `randomtuesday.com` was already
registered (Namecheap, parked, since 2014). To use a different domain:

```bash
grep -rl "randomtuesday.app" . | xargs sed -i '' 's/randomtuesday\.app/YOURDOMAIN/g'   # macOS
```

Then update `sitemap.xml` and the `og:url` / `canonical` tags, which the same
replacement covers.

### Email addresses referenced

These need to exist and be monitored before you point a store listing at them:

- `hello@` — general and beta requests
- `support@` — App Store / Play support URL destination
- `privacy@` — privacy policy contact
- `security@` — vulnerability reports

A single Google Workspace mailbox with aliases covers all four.

## Deploying to Netlify

1. Log in to Netlify → **Add new site → Import an existing project → GitHub**.
2. Pick this repository.
3. Build settings: **build command empty**, **publish directory `.`**.
   `netlify.toml` already sets this, so the defaults should be correct.
4. Deploy. You get a `*.netlify.app` URL immediately.
5. **Domain management → Add a domain** → enter your domain.
6. At your registrar (Cloudflare Registrar), point the domain at Netlify — either by
   using Netlify DNS nameservers, or by adding Netlify's `A` / `CNAME` records.
   Cloudflare users: set the records to **DNS only** (grey cloud), not proxied, or
   Netlify's certificate provisioning will fail.
7. Wait for the Let's Encrypt certificate, then confirm `https://` works and that
   `http://` and the `www.` variant both redirect to it. Apple's check follows
   redirects, but a certificate error will fail it.

Netlify auto-deploys on every push to the default branch.

## After deploying, before Apple enrollment

- [ ] Homepage loads over HTTPS with no certificate warning
- [ ] `/privacy` and `/support` load and are linked from the homepage footer
- [ ] All four email addresses accept mail
- [ ] Legal entity name and address on the site match the D&B record exactly
- [ ] The site is on the same domain as the work email used to enroll

## Keeping the privacy policy true

The policy currently states that PadPing collects nothing and transmits nothing.
That is accurate for the build as it stands. **If analytics ship in v1** — decision
D1/D2 in `claude/launch-plan.md` in the PadPing project — this policy must be
updated before submission, and the App Store privacy labels and Play Data safety
form must match it. A privacy policy that undersells what the app actually does is
a rejection risk on both stores and a trust problem that outlives the rejection.

## Licence

© 2026 [LEGAL ENTITY NAME]. All rights reserved. Not open source; published here for
deployment convenience.

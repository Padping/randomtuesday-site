# randomtuesday-site

The Random Tuesday company site, https://randomtuesday.app. Static HTML and
one stylesheet, no build step. Netlify deploys it straight from `main`. Apple's
organization enrollment required it, and `support.html` is the Support URL
for both store listings. `privacy.html` covers the website and email only;
the app's policy is padping.co/privacy, in `padping-landing`.

## Rules
- Never merge a PR. I merge. Merging to `main` deploys the live site.
- No em dashes in anything a user reads.
- The privacy page, the data inventory (`padping/docs/data-inventory.md`) and
  the store forms must agree. Change one, check the other two.
- Repo notes (`README.md`, `AUDIT.md`, this file) sit in the publish root.
  Each one needs a 404 rule in `netlify.toml` so it is never served.

The full rules live in `padping/CLAUDE.md`, in the `padping` repo beside this
one. Read them before changing anything here.

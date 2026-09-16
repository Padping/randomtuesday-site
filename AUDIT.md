# Repository Alignment Audit: randomtuesday-site

**Date:** 2026-09-16
**Branch:** `audit/full-recursive-2026-09-16` (cut from `main` at `476276f`)
**Target branch:** `main`
**Passes run:** 5 of 5
**Final state:** Clean for this repo. Pass 5 found nothing new here; the remaining low items are in the other two reports. Not served publicly: `netlify.toml` returns 404 for `/AUDIT.md`.

This replaces the report-only audit written earlier today. Companion reports: `padping/AUDIT.md` (app) and `padping-landing/AUDIT.md`.

## Summary

Findings by severity: Critical 0 | High 4 | Medium 4 | Low 6
Fixed: 14. Open items: 1 (shared with padping-landing: O-05). Regressions: 0.
Files changed: 10 (+110 / -178). No build step and no test suite. Every page was re-read in full in each pass.

randomtuesday.app/privacy was an older, unmaintained copy of the PadPing policy. It was dated 14 Sep, missed the sixth event and the device fields, and claimed a reminders measurement that doesn't exist. You ruled that padping.co/privacy is the only app policy. This page now covers the company website and email, and links there. The terms page no longer shows a public "have a lawyer review this" note. Support and terms point to the export in Settings and describe phone backups correctly. The PadPing page sends beta signups to the padping.co waitlist. There are no em dashes on any page (21 removed, including a CSS bullet that rendered three more). The README no longer describes a privacy position from before D2.

## Parameters audited against

1. "No em dashes in anything a user reads." (project instructions)
2. "The privacy page, the data inventory and the store forms must agree." (project instructions, D2)
3. Every statement about PadPing matches `padping/docs/data-inventory.md` and the app; nothing claims that nothing is collected. (D2P §3)
4. Plain HTML, one stylesheet, no build, no dependencies; file structure and five sitemap URLs as the README lists. (README)
5. Legal name and address identical everywhere and matching D&B. (README)
6. Mailboxes hello@, support@, privacy@, security@. (README)
7. randomtuesday.app/privacy covers the website and email; PadPing's policy is padping.co/privacy. (your ruling R1)
8. No public lawyer note. (your ruling R7)
9. Repo notes are not served. (your ruling R8)
10. Beta emails: two (beta and launch). (your ruling R11)

## Contradictions resolved

- **Which privacy URL goes to the stores:** padping.co/privacy (launch log; you confirmed). This README said this site's page; it now says padping.co.
- **Backups:** this site's support page was the accurate one. padping.co now agrees with it.

## Findings: Critical

None. The earlier report rated the under-disclosing policy Critical on the assumption that the stores might get this URL. The launch log already said they get padping.co, so it is High here.

## Findings: High

- **R-A01** The privacy page was an incomplete second app policy: no `task_instance_added`, no device fields or identifiers, "whether reminders get acted on", and "nothing else" with a suggestion. Reduced to the website and email, with a PadPing section linking to padping.co/privacy (your ruling). "Your rights" no longer says we hold nothing; pass 4 added early-access email addresses to what we hold. (`ba4c593`, `0d2b751`)
- **R-A02** The terms page showed a public note: "Have a lawyer review them before you rely on them." Removed; the review is queued in the app repo's handoff notes. (`3a59945`)
- **R-A03** The README said "The policy currently states that PadPing collects nothing and transmits nothing. That is accurate for the build as it stands." Rewritten to point at D2 and the inventory. (`e11754c`)
- **R-A04** The PadPing page asked people to email hello@ for the beta and promised "that one message"; padping.co runs the waitlist and promises two. It now links to padping.co/#waitlist and promises two. (`3a59945`)

## Findings: Medium

- **R-A05** Support and terms didn't mention Settings > Export my data. Added, with "not encrypted" on support. (`3a59945`)
- **R-A06** 21 em dashes on the pages, including titles and the `.meta-list` bullet (a CSS `content` em dash rendered three times). Replaced; the bullet is now a middle dot. (`aeca363`)
- **R-A08** `publish = "."` served `/README.md`. 404 rules added for it and `/AUDIT.md`. (`e11754c`)
- **R-A09** README: placeholder instructions for strings that are filled, "© 2026 [LEGAL ENTITY NAME]", the privacy URL line, no mention of `feedback@padping.co`. Fixed; the reasons list now says where PadPing's policy lives (pass 2). (`e11754c`, `cf2792e`)

## Findings: Low

- **R-A07** The 404 page had no skip link. Added. (`aff54d1`)
- **R-A10** Sitemap dates updated to 16 Sep. (`e11754c`)
- The README's domain-swap command would also rewrite README and AUDIT files, and uses macOS-only `sed`. Narrowed to page files, with a Linux note. (`e11754c`)
- The homepage bullet "Your home details and tasks stay on your device" now says details and notes, matching the in-app line. (`aeca363`)
- The PadPing card now says only anonymous usage is measured, plus a suggestion if sent (pass 2). (`cf2792e`)
- Privacy "Information you send us": beta emails now match the two-email promise (pass 3). (`8242a8a`)
- The "Children" h3 under "Reminders" went away with the app sections.

**Not changed, flagged:**
- British spellings ("Licence", "enquiries"). The site may intend them.
- `theme-color` meta is set on padping.co but not here. No rule requires it.
- The terms contact is `hello@randomtuesday.app`. It is one of the four listed mailboxes; say if you want support@ instead.

## Documentation-side fixes

| Doc | Said | Now |
|---|---|---|
| README | Policy says PadPing collects nothing | Company policy covers the website and email; PadPing's is padping.co/privacy, per D2 and the inventory |
| README | "Before this goes live: required edits" with placeholders | The filled strings, and why they must match D&B |
| README | privacy.html is the stores' Privacy Policy URL | The stores use padping.co/privacy |
| README | Four mailboxes | Plus `feedback@padping.co` used by padping.co |
| README | Licence line with a placeholder | RANDOM TUESDAY LLC |

## Open items: need your decision

### O-05 Forced pretty-URL redirects may loop
- **File:** `netlify.toml:8-30`
- **Recommendation:** After pushing, load `/privacy.html`, `/terms.html`, `/support.html` and `/padping.html` once each and confirm a single redirect. Confirm `/README.md` and `/AUDIT.md` return 404. (Queued in the app repo's handoff notes.)

## Regressions and non-convergence

None.

## Coverage

- **Files in scope:** 13 tracked files.
- **Files reviewed:** 13 of 13, read in full in every pass.
- **Excluded:** `.git/`, `.DS_Store`, and an empty `.claude/` folder (not tracked, not deployed).
- **Not fully verified:**
  - Live deploy behaviour. There is no network from here, and nothing is pushed.
  - Whether the four mailboxes accept mail.
  - The D&B record match.

## Changes by pass

- **Pass 1:** 12 findings confirmed or new. 12 fixed.
- **Pass 2:** 2 findings. 2 fixed.
- **Pass 3:** 1 finding. Fixed.
- **Pass 4:** 1 finding. Fixed.
- **Pass 5:** none.

## Commits on this branch

```
ba4c593 fix(privacy): company policy covers the website and email; PadPing links to padping.co/privacy (R-A01)
3a59945 fix(copy): PadPing page, support and terms match the app; lawyer note removed (R-A02 to R-A05)
aeca363 fix(copy): no em dashes on the company site (R-A06)
aff54d1 fix(a11y): skip link on the 404 page like every other page (R-A07)
e11754c chore: stop serving repo notes; README matches the site; sitemap dates (R-A08 to R-A10)
cf2792e fix(copy): PadPing card names the suggestion exception; README says where the app policy lives (pass 2)
8242a8a fix(privacy): beta emails match the two-email promise (pass 3)
0d2b751 fix(privacy): 'what we hold' includes early-access email addresses (pass 4)
```

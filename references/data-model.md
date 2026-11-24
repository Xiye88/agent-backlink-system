# Data model

The system keeps four kinds of records. Keep them separate. Mixing them is how counts drift.

## Master channel library (one table, coordinator writes only)

One row per domain. Suggested fields:

- Site name and site URL
- Grade: P1, P2 or P3
- Status: core, pending, removed (removed only for rejected or paid; dead sites are deleted)
- Entry type: direct submit page, registration required, email only, contact form, and similar
- Free or paid, and the price when paid
- Submitted projects: which products have submitted here
- Per-project result summary: one short line per product with its outcome
- Notes: category restrictions, quirks, correct submit page when the obvious one is wrong
- Last verified date

## Per-product record table (one per product)

One row per site times target page actually submitted:

- Site name, site URL
- Submission date
- Type (directory, listing platform, blog, community, and similar)
- Status: submitted, in review, listed, rejected, pending, paid
- Listing page: the public URL once listed, or the exact page URL submitted
- Link attribute: dofollow or nofollow, measured on the live page
- Registered account: the product mailbox used
- Notes

## Sender ledger and inbox files (one set per product)

- The ledger is the product's running narrative record, owned by its sender bot.
- The inbox file is the machine-readable daily output the coordinator imports. One line per site:

```
Site name (domain) | result | listing URL | rel | correct submit page | product | note with date
```

- Result words stay inside a fixed vocabulary the importer understands: found entry, unavailable, rejected, paid, listed, passed, in review, note. Map local wording to this vocabulary before writing the file, or the import fails.

## Submit kit (one per product)

See submit-kit-template.md. The kit is input data for submissions, not a record of them.

## Fallback batch list (one shared file)

Sites the sender browsers could not finish, one line each, with the reason:

- Needs the dedicated login account (Google or GitHub only)
- Needs a password typed by the owner (some device channels block agents from password fields)
- Blocked by a challenge the bot could not pass after trying
- Waiting on an SMS code, with the trigger protocol from SKILL.md

The coordinator works this list in the EGO browser on the owner's machine, in small batches, and reports each outcome back to the owning sender.

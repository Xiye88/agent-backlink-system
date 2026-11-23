# Agent Backlink System

A multi-agent system that earns directory and listing backlinks for one or more products, continuously and at near-zero marginal cost. One coordinator owns the shared channel library and all reporting. One sender bot per product does the daily submissions. One mining bot keeps the channel library growing by reverse-engineering where similar products already got listed.

The stack is Muse agents plus the EGO browser. Muse runs the coordinator, the senders and the miner as scheduled agents. EGO is the persistent browser on the owner's machine, where mailbox and dedicated-account login sessions survive between sessions. The method itself comes from a production run across 4 products and 12,000+ catalogued channels.

## Decide the configuration first

- Decide how many products and sender bots to run. One sender bot per product is the default. A single product still uses the coordinator plus mining bot split.
- Decide the table backend. Any shared table the coordinator alone writes works. The reference run used Feishu Bitable, one master table plus one sub-table per product. A spreadsheet or a small SQLite database is fine.

- Decide how the mining bot obtains referring-domain data. Public sources work out of the box, including content-platform APIs, public crawl indexes and search results, with varying confidence. Leave the choice to whoever operates the system.

- Decide the accounts before the first run. Each product gets its own mailbox. All senders share one unified password, stored in a vault or a restricted file and never written into reports or ledgers. Set up one dedicated Google and GitHub account pair, kept logged in inside the EGO browser on the owner's machine, for sites that only accept social login.

- Agree the escalation rules with the owner up front. Small submission-level decisions belong to the coordinator. Spending money, using the owner's personal accounts, and public posting under the owner's own name always need the owner.

## Run the three roles

- Run one coordinator- Keep one record per site times target page in the product sub-tables. Submitting a second, different page of the same site to the same product is a new record, not a duplicate.

- Reconcile after every import. The table total and the status distribution in the backend must match the local copy exactly before the daily report goes out. See references/data-model.md for the full schema and the one-line feedback format.

## Grow the library with intersection mining

- Bootstrap with public directory lists and hand-picked channels, then switch the growth engine to intersection mining. Static lists exhaust. One product in the reference run burned through a 379-candidate pool in three weeks.

- Mine in three steps. Pull outbound links from fresh content sources such as the dev.to API. Clean them down to real independent product sites in the product's category and keep 20 to 40 as seeds. Then find the domains that link to at least 3 of those seeds, and capture the exact landing page URL where each peer got listed.
- Treat a channel found this way as high-certainty. Peers already listed there prove the site accepts submissions in this category.

- Refill on a schedule, daily by default. Write new targets to an inbox file for the coordinator to import, after deduplicating against the master library by domain. See references/mining.md for the full procedure and data sources.

## Prepare a submit kit per product before sending anything

- Build one kit per product and reuse it for every submission. The kit holds the brand name, the canonical URLs, the logo, a short and a long description, pricing, category picks, and the real privacy and terms page URLs, all verified against the live site.

- Store screenshots of the product's own pages in the kit, the home page hero, the pricing page and the core feature pages, one clean full-page shot each. Sites that demand a screenshot then take seconds, not a detour.

- Re-check the product site weekly. When the site changed and the new wording is certain, update the kit and log the change. When it is uncertain, list the question for the owner instead of guessing.

- Record honestly what does not exist. A product with no public pricing page is Freemium in the kit, with no invented prices. See references/submit-kit-template.md.## Submit with the sender bots

- Pick targets in this order. Same-category P1 first, then general P1, then P2, then P3. Skip sites already submitted for this product, removed sites, and sites whose notes say they only accept another category.

- Submit 5 to 10 per product per day, with a hard floor of 2 to 3 successful submissions. When the pool runs dry, widen the search or lean on the mining bot instead of lowering quality.

- Stay free only. Skip paid placements, badge requirements, reciprocal backlinks and review-for-listing exchanges on sight, and record them as removed or noted, never as pending decisions.

- Fill minor missing fields plausibly and submit, such as a contact name, a phone number the owner designated for forms, or a company size that fits the product. The owner sets this latitude in advance. Money, badges and personal accounts stay outside it.

- Register with the product mailbox and the unified password. Prefer scripted or API submission where a site allows it, and a browser otherwise.
- Try captchas and agreement checkboxes yourself first, in the sender's own browser. Only park a site in the human-browser fallback batch when the challenge truly blocks you. Run the fallback batch in the EGO browser on the owner's machine, where login sessions survive.

- Never trigger an SMS code on your own. Tell the coordinator first, let the owner confirm they are online, then trigger it and use the code immediately. Codes expire in minutes and die when sent too early.

- Record the actual page URL you submitted, not the home page by default. Verify the success message on the page before writing the result. Measure the link attribute on the live page later instead of trusting the site's promise.

- Retry a failed site once, the next day. If it fails again, close it with a clear note and move on. Keep the note detailed, with the reason and any candidate entry points, so a future stronger pass can reuse it.## Use the dedicated login accounts for walled sites

- Route sites that only offer Google or GitHub login to the fallback batch, flagged for the dedicated account. Do the submission inside the EGO browser, where that account is already logged in.

- Never touch the owner's main Google or GitHub account. The dedicated pair exists precisely so the owner's identity stays out of the system.

- Keep collecting verification codes and confirmation links from the product mailboxes. The dedicated account is an identity for login, not a mailbox replacement.

## Track the listing lifecycle

- Treat submission and listing as different events. Free review queues take days to weeks. A submission is a pending review until the public page is verified.

- Inspect every 14 days. Each sender re-checks its submissions that are at least 7 days old and still pending, and reports listed with the public URL, rejected with the reason, or still pending.

- Treat email as the fast channel and the 14-day inspection as the backstop. Sites list products without writing, and mail gets missed.

- Click confirmation emails as soon as they arrive, through the EGO browser session. Do not let them age in an inbox, because confirmation tokens expire within hours to days.

## Report daily, with verified numbers
- Open the daily report with three headline numbers, new submissions, new listings and an anomaly summary. Then report per product, targets checked, submissions made, each site with its result, and where the blockers cluster.

- Add the mining bot's daily output, the fallback batch backlog, and any parked items such as drafted pull requests, each labelled with its product and whether it waits on a reviewer or on the owner.

- Recompute every number from the ledgers before reporting. A report assembled from other bots' summaries without checking the ledgers will eventually be wrong, and the owner will find it.

- List decisions the owner must make separately from items already submitted and waiting on a reviewer. Never merge the two into one pending count.

## Available connectors

- Use `muse.connector_search` to find other connectors and `muse.skill_search` to find related skills.

- If nothing here fits, look on the web for a maintained API or MCP server and set it up with [Custom Connectors and MCPs](/opt/muse/skills/custom-connector-setup/SKILL.md), or build a custom connector.

## Websites

- [dev.to API](https://dev.to/api/articles) for fresh outbound-link mining. Pull article bodies, extract outbound domains, and clean them into seed sites per references/mining.md.

- [Common Crawl index](https://index.commoncrawl.org/) for public referring-page lookups when mining intersections. Verify each candidate domain against the master library before importing it.

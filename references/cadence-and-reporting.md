# Cadence and reporting

The reference schedule. Shift the clock times, keep the rhythm.

## Daily

- Early morning: the mining bot runs one mining round and writes its inbox file.
- Morning: each sender bot runs its submission batch, 5 to 10 targets with a floor of 2 to 3 successes, then reports to its own chat and to the coordinator.
- Mid-morning: the coordinator imports all inbox files, syncs the tables, reconciles totals, and publishes the daily report.
- Later morning: the coordinator works the fallback batch in the persistent browser, a small batch at a time, confirmation emails first.

## Weekly

- Once a week, the coordinator dispatches a site check to every sender. Each sender compares its product's live site against its submit kit, updates the kit where the change is certain, and lists uncertain changes for the owner.

## Every 14 days

- The coordinator dispatches a listing inspection to every sender. Each sender re-checks pending submissions at least 7 days old and reports listed, rejected or still pending, with public URLs.
- The coordinator imports the results, syncs the tables, and reports the newly listed and newly rejected sites by name.

## Daily report shape

Open with the three headline numbers. Then, in order:

1. Per product: targets checked, submissions made, each site with its result (listed, in review, rejected, paid), and the blocker pattern of the day
2. Mining output: new targets by product
3. Backlog: fallback batch size, parked pull requests by product
4. Decisions for the owner, each with its product and what exactly waits
5. Bookkeeping result: rows imported, master total, backend and local copies reconciled or not, sub-table rows added per product

Keep "submitted, waiting on the reviewer" and "not submitted, waiting on the owner" as two visibly different lists. A single merged pending count is the reporting failure this system is designed to avoid.

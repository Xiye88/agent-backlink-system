# Intersection mining

The discovery engine. Instead of copying public directory lists, find where similar products already earned links, then submit to the same places.

## Step 1: harvest outbound links from fresh content

- Pull recent articles from the dev.to public API, a few hundred to start, and fetch each article body.
- Extract every outbound link with its domain and anchor text.
- Log them all in a raw outbound table before filtering. Raw data is cheap and re-filtering later is common.

## Step 2: clean down to seed sites

- Remove social giants (X, GitHub, YouTube, LinkedIn, Reddit), pure directories, personal blogs, documentation subdomains and the products' own domains.
- Keep real, independent product sites in the target category. Aim for 20 to 40 seeds per category.
- Hand-add known peers and competitors to the seed pool whenever the owner names one. Seeds improve every later step.

## Step 3: intersect referring domains

- For the seed set, find domains that link to at least 3 different seeds.
- Capture, for each hit, the domain, how many seeds it links to, and the exact landing page URL where a peer is listed. The landing page is the evidence and usually sits one click away from the submit entry.
- Use public sources for the link data, such as the Common Crawl index, search-engine link queries or free backlink checkers. Mark the source and the confidence in the note, because public data is noisier than the intersection threshold assumes.
- Optionally, the operator may plug in a paid backlink data source that returns full referring-domain lists per seed: the data is fresher and also covers small seeds that public crawls miss, so intersections computed from it carry higher confidence. Keep the public sources as the fallback when the paid source is unavailable or out of budget, and track the per-round spend in the mining report.

## Step 4: dedupe and hand over

- Deduplicate candidates against the master library by domain. Never re-report a channel the library already holds.
- Write survivors to the mining inbox file in the one-line format from data-model.md, with the intersection count and a peer landing page as the note.
- Report daily: seeds per category, new targets found per product, and the data source used.

## Why the threshold is 3

One seed linking somewhere proves little. Three independent peers listed on the same domain almost always means a public, category-relevant submission entry. Lower the threshold to 2 only when a category is starved, and expect more noise.

## Cadence

Run daily. In the reference design the miner refills whenever the pending pool runs low, so senders never starve. Discovery is the cheapest part of the system and the first thing to automate harder when volume matters.

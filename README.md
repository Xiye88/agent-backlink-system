# Agent Backlink System

A multi-agent playbook for earning directory and listing backlinks for SaaS and indie products, continuously, at near-zero marginal cost.

It is a method, not a script pack. It runs on Muse agents plus the EGO browser: Muse runs the coordinator, the senders and the miner as scheduled agents, and EGO is the persistent browser where mailbox and account sessions survive. Hand the `SKILL.md` to a Muse agent and it can stand the whole operation up: a coordinator that owns a shared channel library, one sender bot per product doing daily submissions, and a mining bot that keeps finding new channels by reverse-engineering where similar products already got listed.

## Why it works

- **High-friction links are the valuable ones.** Listings behind real logins, long structured forms and human review queues are exactly the links scripts cannot mass-produce, and exactly the ones search engines still count. The system is built to clear that friction with agents instead of avoiding it.
- **Submission is a funnel.** Hundreds of targets turn into dozens of live listings. Volume plus persistence wins, and agents supply both.
- **Discovery compounds.** Intersection mining (find the domains that link to 3 or more of your peers) produces targets that arrive with proof they accept products like yours, instead of another stale directory list.

## What is inside

- `SKILL.md` — the operating playbook: roles, configuration decisions, submission rules, account strategy, lifecycle tracking, reporting discipline.
- `references/data-model.md` — the master library, per-product records, ledgers and the one-line feedback format.
- `references/mining.md` — the three-step intersection mining procedure.
- `references/submit-kit-template.md` — the per-product kit every sender reuses.
- `references/cadence-and-reporting.md` — the daily, weekly and 14-day rhythm, and the daily report shape.
- `examples/` — a sample inbox file and a sample daily report.

## Choices left to the operator

- How many products and sender bots to run.
- The table backend (the reference run used Feishu Bitable; a spreadsheet or SQLite works).
- How the mining bot obtains referring-domain data: public sources such as content-platform APIs, public crawl indexes and search results.
- The escalation latitude: what senders may fill in on their own, and what always goes back to the owner.

No accounts, credentials or private data are included; every deployment supplies its own.

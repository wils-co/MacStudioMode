# Synnex Marketing Dashboard

An automated, self-updating dashboard that turns a noisy work inbox into a clean, sales-ready view of every live vendor marketing campaign — built and run entirely through conversational AI (Claude, in Cowork mode), with zero hand-written code from me.

🔗 **Live site:** deployed via Vercel from this repo
🤖 **Refreshes itself:** every day at 10:00am, hands-free

---

## The problem

As a Synnex Australia account manager, my Gmail receives a constant stream of forwarded work email: pricelists, stock-on-hand reports, daily allocation/inventory reports — and, mixed in among them, the actual **marketing campaigns** (vendor promos, EOFY deals, product launches, cloud offers) that I want in front of customers.

The signal I care about is buried in the noise. Manually it meant scrolling, opening each EDM, working out which ones still mattered, and trying to remember the offer when a customer called.

## What this project achieves (workflow perspective)

This is an end-to-end automation that runs unattended. Each morning the pipeline:

1. **Reads the inbox** — searches Gmail for mail from the Synnex domain in the last 7 days.
2. **Separates signal from noise** — keeps only genuine marketing material from *Team Synnex* (`marketing@`) and *Synnex Cloud* (`cloud@`), and discards every pricelist, SOH report and allocation/inventory report.
3. **Extracts the campaign** — pulls vendor, headline, offer details, validity dates and the EDM hero images and tracking links straight out of each email's HTML.
4. **Rebuilds the dashboard** — drops new campaigns into a fun, filterable HTML dashboard, ages out anything older than 14 days, and refreshes the "last updated" date.
5. **Makes it actionable** — every card opens the full original email *with working links* (so "Learn more", "View Guide", etc. still work), and a one-click **One Line Sales Pitch** that gives a conversation-opener, the business use case, and a why-buy reason for each product.
6. **Publishes itself** — commits the updated dashboard into this repo and pushes to GitHub, which triggers an automatic Vercel deploy. The live page is current before the work day starts.

The result: a static problem (a flooded inbox) became a living product (a self-publishing web app) without me writing or maintaining any code.

## How it was built

- **Claude in Cowork mode** — the whole thing was specified in plain English, iteratively: *"pull the marketing emails"*, *"make it fun"*, *"I don't like dark"*, *"let me click the deal and have the links work"*, *"add a one-line sales pitch"*, *"push to GitHub daily"*.
- **Gmail connector** — read-only access to search and read the forwarded emails.
- **Scheduled task** — a single recurring job (10:00am daily) runs the full pull → build → publish pipeline with no human present.
- **Plain HTML/CSS/JS** — the dashboard is one self-contained file. No build step, no framework, no dependencies.
- **GitHub + Vercel** — Git for versioning, Vercel for zero-config hosting and auto-deploy on every push.

## Features

- **Light, filterable card grid** — filter by category (Storage, Cloud, AV & Collab, Devices, EOFY Deals, Incentive).
- **At-a-glance stats** — active campaigns, how many carry an offer, vendor count, and what's new today.
- **Open email** — view the full EDM in a popup with the original, working campaign links.
- **One Line Sales Pitch** — an instant opener + business use case + why-buy for each product.
- **Auto-ageing** — campaigns older than 14 days drop off automatically.

## What's in this folder

| File | Purpose |
|------|---------|
| `index.html` | The self-contained marketing dashboard |
| `README.md` | This file |

## Notes

- The dashboard only ever contains **marketing material**. Pricelists, stock reports, customer lists and account credentials live in a separate private workspace and are never published here.
- Campaign links are the original Synnex marketing tracking links, exactly as they arrived in the inbox.

---

*Part of [MacStudioMode](https://github.com/wils-co/MacStudioMode) — a running record of what I build with AI.*

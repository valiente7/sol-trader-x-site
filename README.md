# Sol-Trader-X: Public Verification Layer

**AI trading that proves itself in public.**

**https://sol-trader-x.com**

This repository holds the public website, live track record and verification layer for the Sol-Trader-X research pilot.

It is not the trading engine. The private Sol-Trader-X engine generates and evaluates the decisions. This repository publishes the evidence.

## What Sol-Trader-X is testing

Sol-Trader-X is a live-market research pilot. It studies how an autonomous AI agent behaves inside a defined trading environment, using real Solana market data. Once a day the agent (Jev AI, TypeSafe System One) chooses:

| Action | Meaning |
|---|---|
| **BUY** | Open a long SOL position. |
| **SELL** | Open a short SOL position. |
| **HOLD** | Remain in cash and take no position. |

The decision is generated before the outcome is known. The result comes later.

```
Live market data
      ↓
Defined decision environment
      ↓
AI decision
      ↓
Public pre-commitment
      ↓
Market outcome
      ↓
Scoring
      ↓
Public track record
```

The purpose is not just to display predictions. It is to create evidence of what the agent knew, what it decided, how confident it was, when it decided, whether it chose to take risk, and what happened afterward.

**This is a paper-trading pilot. No capital is deployed and no trades are made.**

## Why a public record?

AI trading systems are often judged by backtests, screenshots, private performance claims and selected winning periods. Sol-Trader-X tries a different approach: publish the decision first and preserve the evidence.

The record is meant to make each decision increasingly inspectable: the action, the decision time, the entry price, the confidence and probabilities, the price source, the prompt and context version, the outcome and the scoring.

## A research pilot, not a proven strategy

Sol-Trader-X is not presented as a proven trading strategy. The core question is broader:

> Can an autonomous financial agent make well-calibrated decisions about when to take risk, when to abstain, and how confident it should be, under a defined market environment?

BUY, SELL and HOLD are all valid outcomes. The system is not designed to force trading activity.

## Current decision environment

From 2026-10-03, the methodology version is recorded with each decision.

- **Until 2026-10-02 (`v1-plain`):** Jev was asked a bare question, "Should we buy, sell, or hold SOL right now?", with the price and, from 2026-09-30, basic market context (the earliest days sent the price alone).
- **From 2026-10-03 (`v3-full`):** Jev is told that the portfolio starts flat, that BUY opens a long position, SELL opens a short position and HOLD stays in cash, and that the horizon is 24 hours. It is told not to favor any action by default and to use only the information supplied.

The market context for `v3-full` (context version `c2-vol-range`) is:

- current SOL price
- 7-day and 30-day return
- 20-day and 50-day moving averages
- RSI(14)
- last full-day volume relative to its 30-day average
- 14-day realized volatility
- position within the 30-day range, with the 30-day low and high

Jev's API returns structured output: its choice, a confidence and the BUY, SELL and HOLD probabilities. It does not return a written rationale, so none is published.

## What testing has shown so far

These are retrospective tests on 90 days of past prices, run by the project. They are not the live record, and they are not proof of anything.

- Prompt wording changes Jev's behavior a great deal. Across the wordings tested, Jev traded on anywhere from about 10% to 81% of days.
- Under the most explicit and neutral wording (`v3-full`), Jev was highly selective: **9 BUY, 0 SELL, 81 HOLD** over the 90 days. The 9 directional trades did not demonstrate a statistically meaningful edge.
- Adding more market context made Jev more cautious, not more aggressive.
- No tested configuration showed a measurable trading edge.

Caveats: Jev may have seen this price history in training, which can flatter backtests; the entry price is approximated; fees and slippage are not included; and the number of trades is small.

## HOLD is a decision

In Sol-Trader-X, HOLD is an explicit decision to abstain: the agent does not see enough directional opportunity to justify taking risk. That raises a better question than "did it predict correctly?":

> When Jev does take risk, is its confidence actually informative?

With enough live days, the public record can be used to look at participation rate, performance when it trades, BUY versus SELL behavior, abstention, confidence calibration and benchmark-relative results.

## Decision pre-commitment

The decision is published before the evaluation period ends. A pending row can exist before its outcome is known (illustrative):

```
SOL
Decision: BUY
Entry: $118.48
Decided: 2026-10-03T07:21:44.512Z
Status: PENDING
```

Only after the evaluation period does the system add the outcome and the score. The prediction should be observable before anyone knows whether it was right.

Pre-commitment started on 2026-10-01. The first scored day, 2026-09-30, was published after its outcome; that is disclosed on the record.

## Version-controlled evidence

The public record is stored in Git. The `main` branch is protected against force-pushes and deletion, and the protection applies to administrators. Every change to the record is visible in the repository history.

That makes the record version-controlled and tamper-evident. It is not absolutely immutable. Future versions may add stronger cryptographic or external timestamping.

## Jev response fingerprints

Jev's raw answers are fingerprinted with SHA-256, and the fingerprint is published with the decision. It does not prove Jev was right. It identifies which model response the system used.

Be clear about the limit: the raw answers and the exact text sent to Jev are private for now, so outsiders cannot yet recompute a fingerprint themselves. Making that checkable is a goal, not a current feature.

## Data sources

Live market data comes from **Kraken**, with **OKX** as a backup. A published market observation must come from valid exchange data. Internal fallback values are never published as real observations.

## What is published for each decision

Each row in `history.json` can include: the date and decision time (millisecond UTC), the action, Jev's confidence and its BUY/SELL/HOLD probabilities, the entry price and price source, the market indicators supplied to Jev, the model version, the response fingerprint, and (once scored) the exit price, the same-day and 24-hour results and the scoring time.

From 2026-10-03, rows also carry `promptVersion`, `contextVersion` and `experimentSha256`, a hash of the whole decision environment (prompt text, question, options, context fields and scoring rules). Rows before that date predate this versioning.

## Early pilot records

The earliest records were created before all current safeguards existed. They are preserved, not rewritten, and where provenance is incomplete the record says so. A verification system should document weaknesses in its history rather than erase them.

## Repository boundary

| This repository (public) | Private Sol-Trader-X repository |
|---|---|
| Website, live status, track record, published evidence, provenance notes | Market-data collection, Jev integration, prompt and context code, scoring, backtesting, automation, safeguards, credentials |

The public layer shows the evidence. The operating infrastructure stays private.

## Contents

| Path | Purpose |
|---|---|
| `index.html` | The landing page (HTML and CSS, no build step). |
| `track-record.html` | The track record page: renders every day's decision and scored result from `history.json`. |
| `history.json` | The track record data, one row per day. |
| `status.json` | The latest result shown on the landing page. |
| `assets/` | Logo, favicon, share image, and self-hosted fonts (with their license texts). |
| `robots.txt`, `sitemap.xml` | Crawler hints. |
| `CNAME` | Custom domain for the site. |

## Technology

- **This repository:** HTML, CSS and JavaScript (no framework, no build step), JSON, GitHub Pages hosting, a Content Security Policy, self-hosted WOFF2 fonts, millisecond UTC timestamps, SHA-256 fingerprints.
- **Private engine:** Jev AI (TypeSafe System One), Node.js, the Kraken and OKX APIs, JSONL logs, GitHub Actions.

## Status and limits

Sol-Trader-X is a live paper-trading research pilot. No capital is deployed. Results are not proof of future performance, and the system has not demonstrated a persistent trading edge. That is not hidden; it is part of what is being tested.

Not claimed: trading alpha, outperformance, superiority to buy-and-hold, that more trading is better, or that Jev's confidence is already proven to be calibrated.

## Why this matters

The important question is not "can an AI produce a trading signal?" It is: can we tell whether an agent deserves to be trusted with capital? That requires knowing what it knew, what environment it was in, what options it had, how confident it was, when it decided, whether it chose to participate, and what happened next. Sol-Trader-X is built to make those questions answerable.

## Where it may go

Today it evaluates one agent. The larger idea is a framework for evaluating many autonomous financial agents against evidence rather than claims. Possible future work, each to be versioned before it affects the live pilot: confidence calibration, benchmark comparisons, market-regime analysis, transaction-cost and slippage modeling, external timestamping, publishing the methodology and raw answers for independent verification, and multi-agent comparison.

## Research principle

Sol-Trader-X does not optimize for the number of trades and does not assume more activity means better performance. It asks when an agent should take risk, when it should abstain, whether its confidence deserves trust, whether richer context improves its decisions, and whether its behavior stays consistent going forward. The goal is not more trades; it is better evidence.

## License

**Track record data** (`history.json` and `status.json`) is licensed under [Creative Commons Attribution 4.0 (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/). You may copy, share and build on it, including commercially, as long as you credit "Sol-Trader-X (sol-trader-x.com)" and say if you changed it.

The fonts in `assets/fonts/` (Sora and JetBrains Mono) are under the SIL Open Font License 1.1; the license texts are alongside them.

Everything else in this repository (the website code, text and logo) is all rights reserved.

## Disclaimer

Sol-Trader-X is experimental software and a paper-trading research pilot. Nothing published by this project is investment advice or a guarantee of future performance.

# Sol-Trader-X

AI paper-trading pilot for Solana.

**https://sol-trader-x.com**

This public repo holds the Sol-Trader-X website and live track record: one AI decision per day from a paper-trading pilot, published with its entry price, Jev model version and an answer fingerprint, then scored against actual exchange prices. The main branch is protected against rewrites, so every change is visible in the history. The pilot engine, credentials and raw logs live in a separate private repo and are not published here. Early pilot days are labeled where provenance is incomplete.

## Contents

| Path | Purpose |
|---|---|
| `index.html` | The landing page (HTML and CSS, no build step). |
| `track-record.html` | The track record page: every day's decision and its scored results. |
| `history.json` | The track record data, one row per day. |
| `status.json` | The latest result shown on the landing page. |
| `assets/` | Logo. |
| `CNAME` | Custom domain for the site. |

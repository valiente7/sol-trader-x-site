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

## License

**Track record data** (`history.json` and `status.json`) is licensed under [Creative Commons Attribution 4.0 (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/). You may copy, share and build on it, including commercially, as long as you credit "Sol-Trader-X (sol-trader-x.com)" and say if you changed it.

Everything else in this repository (the website code, text and logo) is all rights reserved.

The track record is published for information only. It is a paper-trading pilot and not investment advice.

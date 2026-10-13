---
name: sitecheck
title: "SiteCheck"
description: "52 pay-per-call tools for agents in USDC: web page to Markdown, PDF, SEO and link checks, DNS/mail, OCR, SEC filings and financials, macro rates, Solana and EVM token, wallet, gas and swap quotes, KYB checks, website audits, AI models."
use_case: "Use to read a web page or PDF, check a site (SEO, links, headers, DNS) or a company (SEC, VAT, LEI, UK registry), get macro rates, OCR an image, or check Solana and EVM tokens, wallets, gas and swap quotes."
category: data
service_url: https://api.sitecheck-api.workers.dev
openapi:
  path: openapi.json
---

SiteCheck is a live pay-per-call API for AI agents. Every call is paid on its own with x402 in USDC on Solana, Base or Arc: no account, no API key. A payment settles only when the call succeeds, so a failed call is never charged. The same tools are served as a remote MCP server at `/mcp` (x402 MCP transport; official MCP Registry: `io.github.bck-stack/sitecheck`).

Web and documents
- `GET /api/read?url=` ($0.002): one web page as clean Markdown or text (main content only) with title, description, author, date, language and canonical URL; robots.txt respected.
- `POST /api/pdf` ($0.003): PDF to text by URL or base64, page by page, with metadata (text PDFs only).
- `GET /api/sitemap?url=` ($0.002): every URL in a site's XML sitemaps with lastmod.
- `GET /api/tech?url=` ($0.003): website technology detection with version, confidence and evidence.
- `GET /api/meta`, `/api/links` (broken links), `/api/redirects`, `/api/headers` (security grade), `/api/dns` (SPF/DMARC/DKIM grade): $0.001-0.003 each.
- `POST /api/ocr` ($0.003): text from an image. `GET /api/exif` ($0.002), `/api/qr` ($0.001), `/api/lang` ($0.001), `/api/validate` (ISBN/EAN/VIN/IMEI/ISIN, $0.001).
- `GET /api/audit?url=` ($0.02): WCAG 2.2 accessibility issues with fixes, SEO basics, security headers. `GET /api/contacts?url=` ($0.01): company e-mails, phones, socials.

Data checks
- `GET /api/email-check` ($0.001): syntax, mail DNS, disposable, role account, typo suggestion; up to 50 per call.
- `GET /api/domain` ($0.002): RDAP registration data and DNS.
- `GET /api/fx` ($0.001): ECB exchange rates, conversion and time series. `POST /api/translate` ($0.002).
- KYB: `/api/vat` (EU VIES, $0.002), `/api/iban` ($0.001), `/api/lei` (GLEIF, $0.005), `/api/recalls` (FDA/CPSC, $0.003), `/api/violations` (OSHA/EPA, $0.005), `/api/uk-insolvency` (The Gazette, $0.003).

Company and public data
- `GET /api/sec/filings` ($0.002) and `/api/sec/financials` ($0.003): SEC EDGAR filings and XBRL key figures.
- `/api/uk-company` (Companies House, $0.003), `/api/congress` (US bills, $0.002), `/api/macro` (Fed, ECB and Eurostat rates and inflation, $0.001), `/api/github/repo` ($0.002), `/api/hn` ($0.001), `/api/wiki` ($0.001).

Crypto (information only, not financial advice)
- `GET /api/gas` ($0.001): fees on Base, Ethereum, Arbitrum, Optimism, Polygon and Solana. `/api/solana/quote` ($0.002): Jupiter swap quote. `/api/evm/token` ($0.003): ERC-20 proxy, owner and risky-function flags. `/api/evm/wallet` ($0.003), `/api/ens` ($0.001).
- `GET /api/solana/token?address=` ($0.002): price, liquidity, holders, organic score, top-holder share, on-chain mint and freeze authority, red flags.
- `GET /api/solana/trending` ($0.003): trending, new, top-traded or top-organic tokens with the same flags.
- `GET /api/solana/wallet?address=` ($0.003): SOL and token balances with USD values.
- `GET /api/markets`, `/api/markets/brief`, `POST /api/markets/quote`, `POST /api/markets/build-buy`: Panta prediction markets (search, odds, quote, unsigned buy transaction the agent signs itself).

AI models: `POST /api/image`, `/api/tts`, `/api/transcribe`, `/api/embed`, `/api/chat`.

Code (MIT): https://github.com/bck-stack/sitecheck-x402

## Spend-aware usage

- Read a page with `/api/read` before paying for heavier tools; use `maxChars` to cap the answer.
- `/api/email-check` and `/api/iban` take up to 50 items per call: batch them.
- For Solana, call `/api/solana/trending` once and `/api/solana/token` only for the tokens you will act on.
- Call `/api/audit` once per URL and reuse the result; it already includes SEO, security headers and tech stack.
- Prices are listed per route in the 402 challenge and in `/openapi.json`; nothing is charged if the call fails.

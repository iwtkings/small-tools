# AGENTS.md

Static "small tools" collection. Pure HTML/CSS/JS — **no build system, package manager, dependencies, tests, lint, or CI.** Do not look for or invent `npm`/build commands.

## Run / verify
- Open a file directly, or serve the repo root: `python3 -m http.server` (some tools call cross-origin APIs, which can fail under `file://` — prefer serving over HTTP).
- Verify changes by loading the page in a browser and exercising the UI manually; there is no automated test suite.

## Layout
- Root `index.html` is the **landing list page**: a mobile-friendly card list linking to each tool.
- Each tool lives in its own directory (e.g. `au-calculate/`) as one or more **self-contained** HTML files.
- To add a tool: create a directory with a self-contained HTML file, then add a card/link in root `index.html` (relative path).

## Conventions
- UI text and code comments are **Chinese (zh-CN)** — keep new content consistent.
- HTML files are fully self-contained: inline `<style>`/`<script>`, no external assets, CDNs, or bundlers. Only external calls are runtime data APIs via `fetch`.
- Mobile pages (and root `index.html`) use `viewport-fit=cover` + `env(safe-area-inset-*)` padding and ~44px touch targets. Follow this for any new mobile-facing page.

## au-calculate gotchas
- `gold-au999-calculator.html` (desktop, wide grid) and `gold-au999-calculator-mobile.html` are **near-duplicates**: the `<script>` logic is effectively identical (only a table cell `class="num"` differs), while CSS/layout diverge.
- **Mirror any change to calculation logic, formula, API endpoints, or defaults in both files.** Formula: `国内金价(元/克) = 国际金价(美元/盎司) ÷ 31.1035 × 美元兑人民币汇率` (`OZ_PER_GRAM = 31.1035`).
- Live data comes from two external APIs at runtime: `https://api.frankfurter.dev/v1/latest?base=USD&symbols=CNY` (USD→CNY) and `https://api.gold-api.com/price/XAU` (gold price). Both require network; failures are expected to degrade gracefully to manual input (the code already handles this).

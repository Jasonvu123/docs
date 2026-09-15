# Documentation project instructions

## About this project

- This is the public documentation site for **updown**, built on [Mintlify](https://mintlify.com)
- Pages are MDX files with YAML frontmatter
- Configuration lives in `docs.json`
- Run `mint dev` to preview locally
- Run `mint broken-links` to check links
- `docs-rebuild-request.md` and `docs-rebuild-response.md` are working files from the September 2026 rebuild (product pivot from Theta Labs to updown). They are ignored by Mintlify. `docs-audit-*.md` are from the earlier July 2026 refresh.

## Terminology

- The product name is **updown**, always lowercase, even at the start of a sentence.
- Use **contract**, not "share". The order book's "Shares" column is the one exception; mention it as a column label only.
- Use **window** for one round of a market, **to beat** for the opening value, **Up** and **Down** for the two sides.
- The two hubs are **Live** (15-minute windows) and **Trade** (daily / weekly / monthly windows). Use those exact labels.
- The trading balance is **pUSD**. Deposits and withdrawals are **USDC on Polygon**.
- **Cash** and **Portfolio** are the top-bar chip cells. **Available**, **In position**, and **Total traded** are the Balance card cells. **Buying power** appears only in Live Positions.
- The one-time approval is **Allow trading** (the panel is headed "Enable trading").
- Do not use "buying power" as a general term, "options", "strike", "Yes Call", "spot", "redeem", or "shares". Those belong to the discontinued Theta Labs product.

## Style preferences

- Use active voice and second person ("you")
- Keep sentences concise — one idea per sentence
- Use sentence case for headings
- Bold for UI elements: Click **Settings**
- Quote in-app copy verbatim when describing what the user sees
- Code formatting for file names, commands, paths, and code references

## Content boundaries

- Document only what ships in the Live / Trade product. Do not document the archived options, spot, narratives, social, arena, collect, compute, survey, or API-docs routes, even though some remain reachable by URL.
- Do not document the admin dashboard.
- Do not document the Ctrl/⌘+K palette as a feature; it is a legacy search that leads to an archived page.
- Do not name data vendors for price feeds unless the team confirms public wording.
- State "no trading fee today" rather than promising no fees permanently.
- Do not list blocked countries; describe the in-app region indicator and point to support.

# Docs Update Request — updown: US equity options added, interval markets narrowed

> **To the agent receiving this file:** You are working inside the **updown** production codebase. This file was generated from the public documentation site (Mintlify), which was rebuilt on 2026-09-15 from an audit of `main @ 5248006` and is accurate **as of that commit**. Since then the product has changed in two ways the docs team knows about:
>
> 1. **US equity options were added** as a new product line.
> 2. **Interval markets (Live / Trade up-or-down windows) were kept, but only for stocks and crypto.** Sports and prediction-market outcomes are believed to be gone.
>
> The docs team knows nothing else. Your job is to (A) describe the US equity options product in full so a writer with **no codebase access** can document it from scratch, (B) verify every existing interval-market claim against current code and report what changed, and (C) report every global change (navigation, balances, funding, landing copy) that touches shared pages. Return a single file, `docs-update-response.md`, in the format specified in Section 6.

---

## 1. Your mission and rules of engagement

**What to do, in priority order:**

1. Answer the **US equity options questionnaire** (Section 3). This is the new product and the writer cannot start without it.
2. Go through the **claim inventory** (Section 4) and give every claim a verdict. These are the facts the docs assert today about interval markets and shared surfaces.
3. Answer the **open questions** (Section 5), including the items the team never confirmed after the last round.
4. Save the response as `docs-update-response.md` using the template in Section 6.

**Verdicts for Section 4:**

- `CONFIRMED` — still true as written.
- `OUTDATED` — the feature exists but details changed (labels, numbers, flows, names). Describe what it is **now**.
- `REMOVED` — the feature no longer exists.
- `UNVERIFIED` — the code does not settle it. Say what you looked for.

**Rules:**

- **Verify by reading code, not by assumption.** Route trees, nav components, tickets, constants, settlement jobs, feature flags, and in-app copy strings are your sources of truth. Cite a file path (and symbol where useful) for every fact and every non-CONFIRMED verdict.
- **Report what ships today.** If something is built but flagged off, unreleased, or reachable only by direct URL, say so explicitly.
- **Exact values matter.** Fees, minimums, maximums, multipliers, expiries, cutoff times, refresh intervals, caps, prize amounts.
- **Exact UI labels matter.** Report button, tab, page, modal, column, and menu labels **verbatim** with their casing.
- **Quote in-app copy.** Explainer sections, tooltips, empty states, warnings, and error strings are the writer's best source of vocabulary. Quote them.
- **Write for a docs audience.** Describe user-facing behavior. Implementation detail belongs in the evidence column.
- **Don't guess.** `UNKNOWN` / `UNVERIFIED` with a note beats a confident wrong answer. Brand, legal, and product decisions go in Section H.
- **Options vocabulary must be precise.** For US equity options, distinguish clearly between: listed exchange-traded options routed through a broker, versus updown's own cash-settled contracts on a stock price. Distinguish calls and puts, buying and writing, American and European exercise, physical and cash settlement. Do not paper over these with the word "options".

**Good places to look:** the route tree under the app, the top-level nav and mobile nav, the landing page and metadata, every ticket component, the options chain / quote / pricing code, order routing and any broker or clearing integration, settlement and expiry jobs, the interval market registry and keeper cron, portfolio and history views, balance/exposure calculations, constants and feature flags, geo/compliance middleware, onboarding and any KYC flow, and any internal docs, ADRs, or "Claude Docs" records that postdate 2026-09-15.

---

## 2. Current docs site map

Mintlify, one tab, five groups, 16 pages. Every page is accurate as of `main @ 5248006`.

| Group | Page (path) | What it covers today |
|---|---|---|
| Get started | `introduction` | What updown is (windows, to beat, Up/Down, $1 payout), key-concepts table, Live vs Trade summary |
| Get started | `quickstart` | Start Trading → Login → onboarding → Allow trading → deposit $2+ USDC → Quick Buy → settle |
| Get started | `account-setup` | Privy (email/Google), onboarding form, Allow trading, two Polygon addresses, balance surfaces, Profile, user menu |
| Trading | `trading/how-windows-work` | The mechanic: markets, windows, cadences, tie rule, payoff, window header, settlement, stock hours, game-over rule |
| Trading | `trading/live-markets` | Live hub: Stocks / Crypto / Sports, featured panel, cards, market page anatomy, sports sessions, liquidity |
| Trading | `trading/trade-markets` | Trade hub: Outcomes / Stocks / Crypto rows, Daily / Weekly / Monthly UTC windows, expiry menu, cards |
| Trading | `trading/placing-orders` | Ticket (Quick Buy / Market / Limit-disabled), confirmation card, fills, price protection, selling back, error strings |
| Portfolio | `portfolio/overview` | Portfolio / Cash chip, Balance card cells, what moves Available, value chart, P&L, tabs |
| Portfolio | `portfolio/deposit-withdraw` | Wallet modal, $2 minimum, status stages, withdraw flow, open-position lock, supported assets |
| Portfolio | `portfolio/positions-and-history` | Interval position rows, Live Positions panel, closing early, history labels, settled-window pages |
| Platform | `platform/finding-markets` | Browsing both hubs, cadence menu, Past dropdown, new listings, Ctrl+K warning |
| Platform | `platform/leaderboard` | Weekly Trading Competition: realized interval P&L, Fri→Thu ET, prizes, columns |
| Help | `help/faq` | Trading, settlement and balances, deposits and withdrawals, account and eligibility |
| Help | `help/support` | Contact form, channels, mis-sent deposits, restricted accounts, status |
| Help | `help/glossary` | ~45 terms as the product uses them |

`docs.json`: name "updown", colors `#3ecf8e` / `#7ce3ad` / `#0b0b11`, footer socials X `@thetalabsgg` and Discord `discord.gg/d9gfrYWSAu`, favicon still the Theta Labs asset.

**What the writer expects to do with your response:** add a new Trading group of pages for US equity options; trim sports and outcomes out of the interval pages (or keep them if you say they survive); update the introduction, quickstart, portfolio, leaderboard, FAQ, and glossary wherever the new product touches them.

---

## 3. US equity options questionnaire

Answer every item. Reference the IDs in your response.

### E-1 — What it is
- One paragraph, in the product's own voice (quote landing / hub copy).
- **Which of these is it?** (a) Listed US-exchange options (OCC-cleared) routed through a broker-dealer or partner; (b) updown's own cash-settled contracts that reference a US stock price; (c) something else. Cite the code that settles the trade and the code that sources the quote.
- If (a): which broker/partner, whether the user opens a brokerage account, and what the user-visible relationship is (branding, disclosures, account approval levels).
- If (b): what is the settlement price source and rule, and how does this differ from the interval product.
- Real money? Any paper/demo mode?

### E-2 — Where it lives
- Nav label(s) and route(s), desktop and mobile. Is it a new hub beside **Live** and **Trade**, a tab inside one of them, or a mode on the stock market page?
- Default landing after login now.
- Hub page anatomy: rails/filters, rows, cards, what each card shows (verbatim labels).
- Empty states and "coming soon" states.

### E-3 — Underlying universe
- Which stocks/ETFs have options. Fixed registry, admin-added, or the full US listed universe? How does a user find them (search? list?).
- Whether index options, ETF options, or crypto options exist.

### E-4 — Contract specifications
- Calls, puts, or both. Buy-to-open only, or can users write/sell-to-open? If writing: covered, cash-secured, naked; margin/collateral rules and how they display.
- Strike ladder: how strikes are chosen/displayed, spacing, how many shown.
- Expiries: which cycles (daily/0DTE, weekly, monthly, LEAPS), how they are listed, expiry time and timezone, last trading time.
- Exercise style (American / European), contract multiplier (100 shares? 1?), quote units (per share, per contract, cents, dollars).
- Settlement: physical delivery vs cash; automatic exercise rules (ITM threshold), what happens to OTM contracts, when cash lands.
- Early exercise: allowed? how?
- Assignment (if users write): how notified, what happens.
- Corporate actions / halts / adjustments: any user-facing handling or copy.

### E-5 — Quotes, pricing, and counterparty
- Where prices come from (exchange NBBO via broker? updown's model? a market maker?) and what the user sees (bid/ask/last/mark, size, greeks, IV, volume, OI).
- Who is the counterparty. Is there an order book or a house quote?
- Fees: commissions, per-contract fees, regulatory/exchange fees (OCC, ORF, SEC/TAF), spread/markup. Where each is disclosed in the ticket.
- Greeks and analytics shown anywhere (delta/gamma/theta/vega, IV, probability, breakeven, max profit/loss, payoff chart).

### E-6 — The options chain and ticket
- Chain layout: columns (verbatim), call/put arrangement, expiry selector, strike filters, "near the money" markers.
- Ticket anatomy: every input, toggle, readout, and confirm-button label. Sizing (contracts? dollars?). Order types available (market, limit, stop; day/GTC). Multi-leg/spreads?
- Confirmation copy (verbatim). Partial fills, rejections, and every error string.
- Price protection / slippage rules.
- Minimums, maximums, per-user caps, position limits, buying-power checks.
- Phone layout.

### E-7 — Positions, closing, and history
- Position row fields (verbatim) on Portfolio and on the market page.
- How to close (sell-to-close ticket, hold-to-sell, exercise button?). At what price.
- History labels for options events (bought, sold, exercised, assigned, expired, settled) and detail strings.
- P&L: mark-to-market source, unrealized/realized display, how "In position" and "Portfolio" include options.

### E-8 — Money, balances, and funding
- Does pUSD fund options, or is there a separate balance/account (USD cash at a broker)? If separate: how the user moves money between them, minimums, timing, labels.
- Does **Cash / Available / In position / Total traded** change definition? Any new cells (e.g. buying power, margin, maintenance)?
- Does the open-position withdrawal lock apply to options? Any settlement holds (T+1)?
- Does **Total traded** count options, and how?

### E-9 — Hours and time
- Trading hours (regular session only? extended?), holidays, time zone shown, expiry-day cutoffs, when quotes go stale.

### E-10 — Eligibility, compliance, risk
- KYC / identity verification / suitability questionnaire / options approval levels. Where in the flow, what is collected, what the user sees while pending or rejected.
- US-only, or US-excluded? Geo behavior for options specifically.
- Required disclosures (e.g. the OCC "Characteristics and Risks of Standardized Options" acknowledgement), agreements, and risk copy — quote verbatim and say where it is shown.
- Terms/privacy/risk URLs that now exist.

### E-11 — Explainers
- Quote in full every "how it works", "how this settles", tooltip, and rules block for the options product.

### E-12 — Interaction with the rest of the app
- Leaderboard: do options trades count? Ranking metric and rules copy now.
- Referrals: do options fees pay referrers now? Current referral copy and constants.
- Rewards/competitions/promos tied to options.
- Notifications for expiry, assignment, exercise.
- Share cards for options positions.

---

## 4. Claim inventory — interval markets and shared surfaces

Each claim states what the docs assert today. Give each a verdict. Group identical verdicts where the correction is the same.

### Brand, landing, links — BR

- **BR-1** — Product name is `updown`, lowercase everywhere.
- **BR-2** — Landing tagline "Trade Movement on Everything"; meta description "Trade short-term directional movement on stocks, crypto, sports, outcomes, and more. Pick Up or Down for the window, hold to buy, and know your max loss before you're in."
- **BR-3** — Landing CTA **Start Trading** lands on the Live hub.
- **BR-4** — Support: `hello@thetalabs.gg`, X `@thetalabsgg`, Discord `discord.gg/d9gfrYWSAu`; docs link `docs.thetalabs.gg`; app host `app.thetalabs.gg`.
- **BR-5** — Live page description "Live 15-minute up-or-down markets on stocks, crypto, and sports — pick a side, hold to buy." Trade page description "Daily, weekly and monthly up-or-down markets on live prediction-market odds."

### Navigation and chrome — NV

- **NV-1** — Desktop top bar: mark + **updown** → `/app/live`; **Live**; **Trade**; right: **Portfolio**, geo globe icon, **Portfolio / Cash** chip (opens **Wallet**), avatar user menu (**Profile**, **Referral**, **Light mode** / **Dark mode**, **Log out**) or **Login**.
- **NV-2** — `/app` redirects to `/app/live`; after login you return to the page you came from.
- **NV-3** — Mobile: hamburger drawer (Trade: Live, Trade · Account: Portfolio · Profile: Profile, Referral, theme, Log out) and bottom tabs **Live / Trade / Portfolio**.
- **NV-4** — Bottom status bar: Online/Offline dot, **Contact us**, **Docs**, Discord, X.
- **NV-5** — Banners: maintenance strip; **Weekly competition** banner in the last 6 hours ("Weekly competition ends in 2h 14m — you're #4, $3.10 behind #3"); **Account flagged** modal.
- **NV-6** — Leaderboard is not in the nav (reachable via banner or `/app/leaderboard`). *(Team said it is returning to the nav — confirm label and position.)*
- **NV-7** — Ctrl/⌘+K opens a legacy prediction-market search routing to an archived spot page.

### Auth, onboarding, wallets — AU

- **AU-1** — Privy; login methods **email** and **Google** only.
- **AU-2** — Onboarding form **Tell us about you**: Name (required), Phone (optional), "What kind of trader are you?", "Where do you trade today?" (Polymarket / Kalshi / Both / New to prediction markets), "Experience level", "How'd you hear about us?"; **Submit**. Referral cookie 30 days.
- **AU-3** — **Enable trading** panel in the Wallet modal, button **Allow trading**, states "Approving trading access…" → "Trading enabled" → **Done**. Copy still says "Theta Labs" and "Only works with Polymarket & Kalshi". No revoke control.
- **AU-4** — Profile page: header, **Account** (Edit → Name / Phone / read-only Email → Save), **Wallet** (Balance, **Deposit wallet (Polygon)**, **Signing wallet (Polygon)**, Polygonscan links), **Sign out**.
- **AU-5** — No KYC, age, or identity step anywhere.

### Money in / out — MO

- **MO-1** — Wallet modal: **Available** line "pUSD — your tradeable balance on Polymarket."; **Deposit** section rules "Send at least **$2 of USDC** on **Polygon** to the address below — it converts automatically. Wrong tokens or networks may result in permanent loss."; card "⚠ USDC on Polygon" with QR + **Copy Address**; footer **Go to Portfolio** / **Close**.
- **MO-2** — Deposit minimum $2; below-minimum copy "We received $X, but deposits under $2 can't be converted…"; stages "Deposit detected…" → "Finalizing…" → "Converting your deposit to pUSD…"; success toast "Deposit converted to pUSD"; sweep poll 20 s, bridge poll 5 s.
- **MO-3** — Withdraw modal: "Send native USDC to any Polygon address. Gas-free, usually instant."; fields **Recipient address (Polygon)**, **Amount (USDC)** + **MAX**; **You receive**; **Withdraw**; footnote "Gas is sponsored. Withdrawals are irreversible — double-check the address."; minimum $2; success **View transaction →** / **Withdraw again** / **Done**.
- **MO-4** — Open-position lock: "Amount $X exceeds your available $Y ($Z is committed to open interval positions)."; modal Available shows the raw balance.
- **MO-5** — No fiat on-ramp; no fees; gas sponsored; restricted accounts can still withdraw.

### Balances and portfolio — PF

- **PF-1** — Nav chip cells **Portfolio** and **Cash**; tooltip "Portfolio = cash + open positions · Cash = available to trade"; refresh 30 s.
- **PF-2** — Balance card: headline **Balance**; cells **Available / In position / Total traded**; buttons **Deposit / Withdraw**.
- **PF-3** — Available = pUSD − cost basis held in open windows; In position = contracts × midpoint (cost basis while settling); Balance = sum; Total traded = $1 per contract over every fill, buys and sells.
- **PF-4** — Portfolio value chart 1D / 1W / 1M / 1Y / ALL, default 1W, "+$X (+Y%) Past week", hover cursor; snapshots every 15 min; refresh: balance 15 s, positions 10 s, history 30 s, chart 60 s.
- **PF-5** — Tabs **positions / Open orders / history**; positions section **Interval markets**; Open orders empty copy "Interval trades fill instantly, so nothing rests here yet. Limit orders for intervals arrive with the order book."; history filters **All / Interval** (legacy Spot / Options for old accounts).
- **PF-6** — Legacy sections **Options**, **Written (covered)**, **Markets** render only for accounts with old positions. *(The new US equity options product may reuse or replace these — say which.)*

### The interval mechanic — IM

- **IM-1** — Live windows on the UTC 15-minute grid, rolling continuously; Trade windows Daily (00:00–23:59 UTC), Weekly (Mon 00:00 UTC), Monthly (1st 00:00 UTC); Trade level to beat = previous period's close.
- **IM-2** — Up wins strictly above the open; unchanged settles Down; ⓘ copy "If the price is unchanged, it settles as Down. Up needs a real move."
- **IM-3** — Contracts 1¢–99¢; winner $1.00; fully collateralized; no shorting/writing.
- **IM-4** — Window header: window line; status tags **LIVE** / **UPCOMING · opens 7:30 PM** / **GAME OVER · SETTLING** / **FINAL** / "Series ended"; **To beat / Now / Time left** (amber under 50%, red under 20%).
- **IM-5** — Settlement: close vs open on the minute-bar feed; `interval-settle` cron every 5 min plus page-triggered pass (throttled 45 s); net moved on-chain once per window; "settling" / "awaiting settle"; history **Won / Lost / Settled** with details "paid $1 each" / "settled worthless" / "nothing owed either way".
- **IM-6** — Settled-window page: **Result** ("Up won" / "Rangers up" / "unchanged — settles down"), **Open**, **Close**, static chart with "Target"; "This window isn't recorded yet" / "Windows are saved shortly after they close." / **Back to the live market**.
- **IM-7** — **Settlement rules** accordion, stock variant (four paragraphs quoted in the docs, incl. "Prices come from the Robinhood tape, the live bid and ask midpoint…"); odds variant (Polymarket last-trade probability sampled once a minute; series ends when the underlying market closes).
- **IM-8** — Stock hours weekdays 4:00 AM – 8:00 PM ET; closed copy "Market closed — stock markets trade weekdays 4:00 AM – 8:00 PM ET." / "Crypto markets stay open 24/7." / **Back to Live**; stock cards leave the hub off-hours.
- **IM-9** — Sports: in-play two-team games auto-listed by the keeper cron (busiest game, odds 5–95%), windows from kickoff, game-over freeze, "Trading paused — game over" copy, "Series ended" copy, team-named sides, 3-way soccer "Up / Down / Flat", "View the underlying market →". *(Expected REMOVED — confirm, and confirm whether any sports code paths still surface anywhere.)*
- **IM-10** — Outcomes (prediction-market odds) as a Trade row "Live chance of a prediction-market outcome". *(Expected REMOVED — confirm.)*
- **IM-11** — Registry: Live stocks AAPL, NVDA, SPCX, TSLA, GOOGL, PLTR; crypto BTC, ETH, SOL, DOGE; Trade subjects admin-added. Report the current list and whether it is DB- or code-driven.
- **IM-12** — "Mentions" category paused and filtered off the hub.

### Live hub — LH

- **LH-1** — Filter rail **All / Sports / Stocks / Crypto** (chips on phones).
- **LH-2** — Featured panel: title "Bitcoin (BTC) · 15 min"; rows **Market / Pays out / Odds** ("Up · 1.96x · 51¢"); **Closes in** / **Opens in**; **To beat** / **Now**; 2-minute tape; pager "3 of 12".
- **LH-3** — Cards: image, title, "15 min", countdown, **To beat / Now**, side rows "▲ Up 51¢" with bars, **LIVE**, **Last 6** triangles with tooltip "2:00 PM window · $325.40 → $325.62 · Up".
- **LH-4** — Empty states "No live sports markets right now" / "New recurring markets roll out here as their metrics go live." / "Waiting for the first quote" / "Waiting for a price…" / "Trading opens in 12:04".
- **LH-5** — Hub registry refresh 60 s.

### Market page — MP

- **MP-1** — Chart lookbacks **Live / 5m / 15m / 1H** (Trade adds **MKT** default and **1D / 1W / 1M**).
- **MP-2** — **Past** dropdown up to 8 windows ("2:15 PM ET · Today") + last-4 chips; tooltip "Change expiry" on the cadence menu; menu items **15 Minutes / End-of-Day / End-of-Week / End-of-Month**; page title pattern "Apple (AAPL) | End-of-Day ▾ | Up or Down".
- **MP-3** — **Live Positions** panel: **Buying power**; per-side row (dot, label, "5.00 contracts", value, **Sell**), "entry 51¢ · now 54¢", "pays $5.00 if Up"; totals **Value / Unrealized / Total return**; empty "No position in this window yet."
- **MP-4** — Outcome row "Up or Down | BTC Price: $77,271.11 · 51% ▲1" with **Up 51¢ / Down 49¢** + ⓘ.
- **MP-5** — Order book: **Trade Up / Trade Down**; columns **Price / Shares / Total**; **Asks** / **Bids**; "Last 50¢ · Spread 2¢"; footnote "Preview of the treasury quote: size is capped to the per-window risk budget (~20 contracts/side) and tapered toward top-of-book, requoting as the model mid moves. An Up at p¢ and a Down at 100−p¢ lock $1 between them."
- **MP-6** — Tape refresh ~1.5 s stocks; tick-by-tick crypto.

### Trade hub — TH

- **TH-1** — H1 **All Markets**; sub "Outcomes, stocks and crypto — each market's live value and recent movement. Pick an expiry to see that window's level to beat and its Up / Down odds." / with expiry: "Call whether each market closes this window above or below where it stood at the previous period's close."
- **TH-2** — Rail **All / Daily / Weekly / Monthly**; rows **Outcomes / Stocks / Crypto** with blurbs "Live chance of a prediction-market outcome" / "Live share price off the tape" / "Live USD price, 24/7".
- **TH-3** — Cards: expiry tag **DAILY / WEEKLY / MONTHLY**; **To beat / Now** with ▲/▼; buttons **▲ Up 51¢ / ▼ Down 49¢** only with an expiry selected; footer "Expires today" (amber) / "Expires Sep 30 UTC" + countdown "18d 5:12:34"; All view "Priced per expiry · daily, weekly, monthly"; empty "No markets listed" / "Subjects are added from the admin's Markets tab."
- **TH-4** — Trade slugs end `-1d` / `-weekly` / `-monthly`; settled pages live under `/app/live/<slug>/<window>`.

### Ticket and fills — TK

- **TK-1** — Ticket top: **Buy / Sell** pill; order-type dropdown **Quick Buy / Market / Limit**.
- **TK-2** — Quick Buy: pills "▲ Up 51¢" / "▼ Down 49¢"; boxes **$1 / $5 / $10** ("$5" over "win $9.80"); "Hold a box to buy" → "Placing…"; hold ~0.7 s; tooltip "Hold to buy $5 Up"; haptic on phones.
- **TK-3** — Market: side pill; **Dollars** (default $50); **Odds** "51% chance"; **Max payout**; **Buy Up / Buy Down**, in Sell mode **Sell Up / Sell Down**.
- **TK-4** — Limit: **Contracts**, **Limit price** (¢), "Ask 51¢ · pays $1 if it wins", checkbox "Submit as resting order only", **Cost / Max payout**; button disabled "Limit orders open with the order book"; tooltip "Resting limit orders open with the order book (CLOB). Market orders trade now."
- **TK-5** — Confirmation **Trade executed**: "Bought $5.00 **Up** @ 51¢" / "Pays $9.80 if Up wins"; sells "Sold $4.90 Up @ 49¢" / "+$0.40 realized" / "· settling…"; partial "Filled $X of your $Y — book was thin here"; auto-dismiss ~2 s.
- **TK-6** — Fills against the treasury ladder: 6 levels/side, sizes 30/24/18/13/9/6% of remaining budget; `OI_CAP = 20` contracts net per side per window; `FEE_BPS = 0`; half-spread 1¢ + up to 4¢ pin-widen; min spread 2¢; inventory skew up to 3¢.
- **TK-7** — Price protection: buy cap shown × 1.3 + 3¢, sell floor shown × 0.7 − 3¢; rejections "Price moved to 55¢ (above your 40¢ limit) — order not filled." / "Price moved to 2¢ (below your 5¢ limit) — order not filled."
- **TK-8** — Errors: "Insufficient balance: need 5.00 pUSD, have 2.10." / "No liquidity available at these levels." / "You only hold N contracts on this side." / "Market is not open for trading right now." / "Trading is paused — the game has ended. This window settles at the final odds."
- **TK-9** — Phone: bottom sheet, **Quick / Market / Limit** toggle; Quick is two steps; collapsed pills **Buy Up 51¢ / Buy Down 49¢**.
- **TK-10** — Selling back: hold **Sell** in Live Positions (sells the side's whole position) or ticket Sell mode (dollars ÷ price contracts); fills at the bid ladder; realized paid immediately; no minimum.

### Positions and history — PH

- **PH-1** — Position row: image, title, "Up · 15m · closes in 12m" / "· settling"; **Contracts / Entry / Now / Pays** ("$5.00" sub "if Up"); value + unrealized (%) or "awaiting settle"; **Trade** / **View**.
- **PH-2** — History row: label chip **Bought / Sold / Won / Lost / Settled**; title + side; detail ("3.00 contracts @ 51¢"); timestamp; signed cash; P&L %; withdrawals as "Withdrawal to 0x…" labelled **Withdrew**; capped at 200 rows.
- **PH-3** — Sold-out windows show only Sold rows; hedged windows show the winning side.
- **PH-4** — Market page marks at the bid; Portfolio marks at the midpoint.

### Leaderboard — LB

- **LB-1** — "Weekly Trading Competition"; ranked by realized P&L on interval markets for windows closing inside the week, sell-to-close included, ties by volume; only successful settlements count.
- **LB-2** — Rounds Friday 12:00 AM → Thursday 11:59 PM ET (code approximates ET as UTC−4); header **This week** "Sep 11 – Sep 17 ET"; **Resets in**.
- **LB-3** — Prizes $100 / $50 / $25 / $5 × 4th–10th, "paid in pUSD straight to their wallet"; no automated payout code.
- **LB-4** — Rules copy "Ranked by **realized P&L on interval markets** this week — what your settled windows paid out minus what you staked, sell-to-close included. Volume alone doesn't rank you; only windows that close inside the week count."
- **LB-5** — Columns `#` / avatar / **Trader** / **Volume** / **P&L** / **24h**; top-3 podium; "You" tag; pinned row; refresh 10 s; empty "No trades yet this week." / "Be the first on the board."; admins/hidden excluded.

### Fees, compliance, support — FC

- **FC-1** — No platform fee (`FEE_BPS = 0`); spread only; no pass-through, deposit, withdrawal, or gas fees.
- **FC-2** — Geo: blocked list incl. US shows only the nav globe icon "Trading is unavailable in your region"; interval trades not actually blocked. *(Team said this was a gap — report whether enforcement shipped and whether the list changed.)*
- **FC-3** — Banned: **Account flagged** modal copy (quoted in docs); API error "This account has been restricted. You can still withdraw your balance. Contact support if you believe this is a mistake."
- **FC-4** — No terms, privacy, or risk-disclosure pages or copy anywhere.
- **FC-5** — Contact page: "Get in Touch" / "Contact Us" / "Questions, feedback, or partnership inquiries — we'd love to hear from you."; cards **EMAIL / X (TWITTER) / DISCORD**; form **Username** (read-only), **Email**, **Message** (5000 chars, counter), **Send Message** → "Sending…" → "Message Sent!"; signed-in only in practice.
- **FC-6** — Referral page copy pays 10% of options fees for 6 months, $5 minimum cash-out (= $0 on interval trades). *(May now be meaningful with real options fees — report current copy and constants.)*
- **FC-7** — Rewards (reveal / streak / first-deposit) only on the archived options hero; `rewards` cron still runs.

### Leftovers — LO

- **LO-1** — Archived routes reachable by URL: `/app/predictions` (+ `/app/spot`, `/app/options` and slugs), `/app/arena/*`, `/app/collect`, `/app/compute`, `/app/narratives`, `/app/social`, `/app/survey`, `/app/api-docs`. Report the current list. *(In particular: has the old Theta Labs `/app/options` route been reused, replaced, or deleted for the new US equity options product?)*
- **LO-2** — Solana embedded wallet still created and delegated but never shown.
- **LO-3** — Theta Labs strings remain: login page logo/alt, Enable-trading copy, tab titles "… | Theta Labs", OG image "thetalabs.gg / Trade Prediction Markets", share-card watermark.

---

## 5. Open questions

- **Q-1 (Scope of the pivot)** — In one paragraph: what changed between `5248006` (2026-09-15) and now, from the user's point of view. Cite any internal record (changelog, ADR, "Claude Docs" file) that describes the decision to add US equity options and narrow intervals.
- **Q-2 (Product hierarchy)** — Which product is the flagship now: US equity options or interval markets? What does the landing page lead with? What is the default landing after login? This decides the order of the docs' Trading group and what the Introduction leads with.
- **Q-3 (Interval survivors)** — Confirm exactly which interval categories and cadences remain (Live 15-min: stocks, crypto? Trade daily/weekly/monthly: stocks, crypto?). Is the **Trade** hub still a separate hub, or did it merge with Live or with the options hub?
- **Q-4 (Shared vocabulary)** — The docs use "contract" for interval units. If US equity options also use "contract", how does the UI disambiguate (e.g. "window contract" vs "option contract")? Give the labels the docs should use.
- **Q-5 (Onboarding changes)** — Did the "Where do you trade today?" dropdown, any KYC step, or any options-suitability step change the sign-up flow?
- **Q-6 (Balance model)** — Restate the full balance model with both products live: every cell/pill, its formula, and what locks funds.
- **Q-7 (New features beyond the two known changes)** — Anything else user-facing that shipped since 2026-09-15 (search, alerts, watchlists, new order types, limit orders for intervals, notifications, mobile app, API). For each: name, where in the UI, what it does, flow with exact labels.
- **Q-8 (Carried over, still unconfirmed from the last round)** — The team never resolved these; answer from code where possible and flag the rest for Section H:
  - Company/legal name and whether Theta Labs strings are being rebranded.
  - Custody wording (server-side delegation; no revoke control).
  - Geo enforcement and the official region list.
  - Terms / privacy / risk-disclosure URLs.
  - Data-vendor wording in the settlement rules.
  - Fee statement ("no fees" vs "no fees at launch").
  - Official app / marketing / docs domains.
  - Favicon source and brand colors.
  - Whether `/app/leaderboard` and `/app/referrals` are back in the nav.

---

## 6. Required response format

Return **one file**, `docs-update-response.md`, structured exactly like this. The writer applies it without codebase access, so completeness beats brevity, but keep everything user-facing.

```markdown
# Docs Update Response — generated <date> from <repo>/<branch>@<commit>

## A. What changed (Q-1, Q-2, Q-3 in one place)
One paragraph in the product's voice, then a bullet list of every user-visible change since 5248006.
State the flagship product and the default landing.

## B. US equity options
### E-1 … ### E-12
Every sub-question answered; UNKNOWN where applicable; evidence cited; in-app copy quoted.

## C. Claim verdicts
### Confirmed
Comma-separated IDs: BR-1, NV-2, …
### Corrections
| ID | Verdict | What the app does now (user-facing, specific: labels, numbers, flows) | Evidence (file / symbol) |

## D. Proposed docs navigation
The full page list with each page marked NEW / REWRITE / EDIT / KEEP / DELETE relative to Section 2,
with a reason for every NEW, DELETE, and rename. Say where the options pages sit relative to the interval pages.

## E. Page briefs
One brief per NEW or REWRITE page, and an edit list per EDIT page:
### <group> / <path> — "<Title>"
- **Purpose:**
- **Status:**
- **User flows:** numbered steps with verbatim labels
- **UI anatomy:** tables of fields/columns and meanings
- **Key numbers/limits:**
- **Warnings/caveats:**
- **In-app copy to reuse:** quoted
- **Cross-links:**
- **Evidence:**

## F. Glossary changes
Terms to add (term → definition as the product uses it), terms to change, terms to drop.

## G. FAQ changes
Questions to add (with answers and evidence), answers to change, questions to drop.

## H. Needs team confirmation
Brand, legal, compliance, and product decisions that code cannot settle, including every carried-over item from Q-8 that is still open.
```

Notes for you, the responding agent:

- **Section B and Section E are the deliverable.** The writer builds the options pages from B and edits the interval pages from C and E.
- Quote in-app copy verbatim and mark it as a quote.
- If sports and outcomes are truly gone, say so once in Section A and mark every related claim REMOVED in one grouped row; the writer will strip them from six pages.
- If the old Theta Labs options code (V2 options on prediction-market probabilities) was repurposed for US equity options, say so explicitly, because the docs must not describe the old product by accident.
- If a fact would appear on several pages (fees, hours, balances), state it fully once in Section B or C and reference the ID from the briefs.

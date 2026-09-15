# Docs Rebuild Request — Updown documentation (post-pivot from Theta Labs)

> **To the agent receiving this file:** You are working inside the **Updown** production codebase. This file was generated from the public documentation site (Mintlify), which still documents the **previous product, Theta Labs**. The team has since done a full product pivot to Updown, so **assume every page of the current docs is stale** — including the product name, the trading model, the money flow, and the navigation.
>
> Your job is **not** to patch the old docs. It is to describe the product **as it ships today**, in enough detail that a docs writer who has **no access to your codebase** can rewrite every page from scratch. Return a single file, `docs-rebuild-response.md`, in the format specified in Section 6.

---

## 1. Your mission and rules of engagement

**What to do, in priority order:**

1. Answer the **product fundamentals questionnaire** (Section 3). This is the most important part — the writer cannot start without it.
2. Fill in the **page briefs** (Section 4) — one per docs page the new site should have. Propose the page list yourself; the current page list (Section 2) is a starting point, not a constraint.
3. Go through the **stale baseline** (Section 5) and mark each Theta Labs-era fact as KEEP / REPLACE / DROP so the writer knows what survived the pivot.
4. Save the response as `docs-rebuild-response.md` using the template in Section 6.

**Rules:**

- **Verify by reading code, not by assumption.** Route trees, navigation components, page/modal components, trade tickets, constants and config, API handlers, settlement jobs, feature flags, and in-app copy strings are your sources of truth. Cite a file path (and symbol/constant where useful) for every fact.
- **Report what ships today.** If something is built but behind a disabled flag, unreleased, or reachable only by direct URL, say so explicitly — the writer needs to know whether to document it.
- **Exact values matter.** Fees, spreads, minimums, maximums, time windows, refresh intervals, expiry times, prize amounts, caps. When there is a number, report the number and where it lives.
- **Exact UI labels matter.** Docs say "Click **Deposit**." Report button, tab, page, modal, and column labels **verbatim**, with their casing.
- **Quote in-app copy.** Explainer modals, tooltips, empty states, warnings, and landing-page copy are the best source of the product's own vocabulary and tone. Quote them.
- **Write for a docs audience.** Describe user-facing behavior and flows. Implementation detail belongs in the evidence column only.
- **Don't guess.** `UNKNOWN` with a note about what you searched beats a confident wrong answer. If a fact is a team/brand/legal decision rather than a code fact, put it in Section H (needs team confirmation).

**Good places to look:** the app's route tree and top-level layout/nav (desktop and mobile), the landing/marketing page, login and onboarding, the wallet/deposit/withdraw components, every trade ticket, the portfolio/positions views, settlement and resolution logic, constants files, feature-flag definitions, geo/compliance middleware, any in-repo docs, changelogs, ADRs, or internal "product spec"/"pivot" notes, and any prior audit records (the last docs refresh was informed by an internal `Claude Docs/26-CODEBASE-AUDIT.md` — if a similar record exists for the Updown pivot, cite it).

---

## 2. How the docs site is built

Knowing the shape of the site helps you answer in a form the writer can drop straight in.

- **Platform:** Mintlify. `docs.json` defines the site name, colors, favicon, navigation (tabs → groups → pages), and footer socials.
- **Pages:** MDX files with YAML frontmatter (`title`, `description`). One tab ("Guides"), five groups, 14 pages today.
- **Conventions the writer uses** (so you know what raw material each needs):
  - **Flows** are written as ordered `<Steps>` with one exact UI label per step → give numbered steps with verbatim labels.
  - **UI anatomy** (columns, cards, cells, chain rows) is written as a two-column table → give field name + meaning.
  - **Money risk** is called out in `<Warning>` blocks → tell us what can go wrong and what the app says about it.
  - **FAQ** is `<AccordionGroup>` Q&A → give real questions users hit and the answer.
  - **Glossary** is an alphabetical term → definition list → give the product's own definitions.
- **Style:** second person, active voice, sentence-case headings, bold for UI elements.

### 2.1 Current site map (all Theta Labs-era content — assume stale)

| Group | Page (path) | What it covers today | What we need from you |
|---|---|---|---|
| Get Started | `introduction` | What Theta Labs is, key concepts table, 4-step "what you can do" | The Updown elevator pitch, the 4–6 things a user does, the 5–8 concepts a new user must learn |
| Get Started | `quickstart` | Sign in → Enable trading → deposit → first options trade → first spot trade | The real first-session path, start to first position, with exact labels |
| Get Started | `account-setup` | Privy login, embedded wallet, one-time approval, wallet addresses, funding, balance cells, Profile page | Auth, wallets, custody, approvals, onboarding, profile/settings as they are now |
| Trading | `trading/options-trading` | Yes Calls / No Calls, chain columns, payoff, expiries, TWAP settlement, sell-to-close | Whatever the primary trading product is now (see F-7) — full mechanics |
| Trading | `trading/spot-trading` | Polymarket YES/NO shares, buy/sell, minimums, redeem | Whether a second trading mode exists and its full mechanics |
| Trading | `trading/order-types` | Market orders only, CLOB, spread, slippage guard, "Limit · soon" | Order types, execution model, slippage/price protection, limits |
| Portfolio | `portfolio/overview` | Balance card (Available / In position / Total traded), value chart, P&L | Portfolio page anatomy, balance model, value formula, chart ranges, refresh |
| Portfolio | `portfolio/deposit-withdraw` | USDC on Polygon → pUSD auto-conversion; withdraw to any Polygon address, min $2 | Money in / money out, exactly |
| Portfolio | `portfolio/positions` | Options rows, spot rows, Sell ticket, Share card, Redeem | Positions/history UI, closing, settlement claims |
| Platform | `platform/markets` | Spot page sort tabs, categories, search, card anatomy, market detail page | Market discovery: how users find what to trade |
| Platform | `platform/rewards` | Trade / Streak / Referral free-share rewards | Any incentives, referrals, promos that exist now |
| Platform | `platform/leaderboard` | Weekly options volume competition, prizes | Any leaderboard/competition/social surface that exists now |
| Help | `help/faq` | Fees, wallet, eligibility, support, deposits, market basics, options | Real FAQ candidates for Updown |
| Help | `help/glossary` | ~30 terms (pUSD, Yes Call, Breakeven, Sold out, …) | The Updown vocabulary |

`docs.json` today: name "Theta Labs", gray theme colors, favicon hosted on brand.dev, footer socials X `@thetalabsgg` and Discord `discord.gg/d9gfrYWSAu`. All of that needs replacing (see F-1).

---

## 3. Product fundamentals questionnaire

Answer every item. Each has an ID; reference IDs in your response.

### F-1 — Brand, naming, links
- Exact product name and casing as used in the app and marketing ("Updown"? "UpDown"? "Up/Down"?). Any legal entity name that appears in footers/terms.
- Tagline and one-sentence product description (quote the landing page / meta description).
- Is "Theta Labs" still used anywhere user-facing (company name, footer, emails, legal)? If so, in what relationship to Updown?
- URLs: live app, marketing site, docs site, terms of service, privacy policy, status page, GitHub (if public).
- Support: email, in-app contact surface, X/Twitter, Discord, Telegram, other.
- Brand assets: logo/favicon URL or repo path, primary/light/dark hex colors (for `docs.json`).

### F-2 — What the product is
- In one paragraph: what does a user do on Updown, and what do they win or lose?
- Is it still built on prediction markets? Is it options? Binary up/down contracts? Something else? Name the **unit** a user trades (contract, share, ticket, position, bet, round) and use that word consistently.
- Real money or not? Are there any paper/demo/practice modes? If yes, how does a user enter/exit them and what are the balances?
- Who is the target user, per the marketing copy?

### F-3 — Underlying markets, venues, and chains
- What do users trade **on**: third-party prediction markets (Polymarket? Kalshi? others?), Updown's own markets, price feeds/oracles (which assets, which oracle), sports data, other?
- For each source: is it live in the UI, read-only, or backend-only scaffolding?
- Chains and tokens involved anywhere a user can see them (deposit chain, settlement asset, in-app balance unit). Is pUSD still a thing?
- Who is the counterparty: an orderbook, an AMM/pool, a platform dealer/house, peer-to-peer?

### F-4 — Authentication, wallets, custody
- Auth provider (still Privy?) and the exact list of login methods offered (email, Google, Apple, X, wallet, passkey, phone…).
- Embedded wallet: created? on which chain(s)? which are user-visible?
- Custody story in the app's own words (non-custodial? delegated signing? one-time "Enable trading"-style approval? revocable how?).
- Post-signup onboarding: steps, fields collected, referral capture.
- Profile/settings page: what can a user view and edit? Where are wallet addresses shown?

### F-5 — Money in, money out
- Deposit: accepted assets and networks (exact), how the address is presented (modal name, QR, copy), any conversion (to what unit), minimum/maximum, timing, staged status strings, first-deposit setup steps, fees, fiat on-ramp if any.
- Withdraw: destinations allowed (own wallet only vs any address), asset and network paid out, minimum/maximum, fees, gas handling, timing, receipt/explorer link, daily limits, holds.
- What happens if a user sends the wrong asset/network — the app's exact warning string and any recovery path.

### F-6 — Balance and portfolio model
- Single balance or multiple? Exact names of every balance cell/pill (e.g. "Available", "In position") and what each means.
- Formula for total portfolio value as displayed.
- What increases/decreases the spendable balance (buy, sell, settlement, claim/redeem, rewards, prizes, fees).
- Portfolio value chart: ranges, default, hover behavior. Refresh intervals per panel.
- P&L: realized vs unrealized, time-range behavior, colors.

### F-7 — Trading modes (the core of the docs)
List **every** way a user can take a position. For **each** mode, answer all of the following:
- Name as shown in the nav/UI, and route.
- What exactly is being bought (and sold). Price range/units (cents? dollars? multiplier? odds?).
- Payoff rule at settlement (formula) and worked example with real in-app numbers.
- Maximum loss for the user. Can users ever be short / write / owe more than they put in?
- How an order fills: orderbook, dealer/house quote, AMM, batch/round, instant vs pending. Is there slippage? Price protection? Partial fills?
- Ticket anatomy: every input, toggle, readout, and the confirm button label (e.g. "Buy for $X"). Sizing options (dollars / units / percent).
- Minimums, maximums, per-user caps, per-market capacity, "sold out"/"full" states.
- Fees and spreads, and where they are disclosed in the UI.
- Time structure: expiries/rounds/durations, cadence, expiry times and timezone, when trading closes, final-expiry rules.
- Settlement: what price/outcome is used (last, TWAP, oracle, official resolution), timing, automatic vs manual claim, what the user sees, in-app explainer copy (quote it).
- Closing early: allowed? how? at what price? minimums?
- Position display: every column/field on the position row, and the actions available.
- Any explainer/how-it-works modal — quote it in full.

### F-8 — Navigation and page inventory
- Exact top-level navigation, desktop and mobile, in order, with routes. Which route is the default landing after login?
- Every secondary surface reachable from the chrome: pills, icons (trophy, bell, search), user menu items, modals (name them).
- Every page a signed-in user can reach, including ones not linked from the nav (say which are orphaned).
- Keyboard shortcuts (e.g. Ctrl/⌘+K).

### F-9 — Fees and pricing (consolidated)
- Platform fee (explicit)? Spread/markup (implicit)? Pass-through venue fees? Withdrawal fees? Gas?
- Where each is shown to the user before confirming.

### F-10 — Eligibility, compliance, risk
- Geo-restriction behavior: what blocked users can and cannot do, exact strings shown.
- Age/KYC/identity requirements and where they are enforced in the flow.
- Any legal/compliance-approved copy in the codebase (risk disclosures, "not available in", regulatory statements). Quote verbatim and cite.
- Links to terms/privacy/risk pages.

### F-11 — Incentives: rewards, referrals, competitions, leaderboards
- Do any of these exist today? For each: where in the UI, how to qualify, what is paid (unit, amounts, distribution), limits, timing, exact labels, rules-modal copy.
- Referral: link format, who gets what, when.
- Leaderboards/competitions: ranking metric, rounds/cadence and timezone, prizes, columns, refresh, eligibility.

### F-12 — Social and community features
- Activity feeds, profiles, following, sharing/share cards, comments, "request a market", community links. Which are live?

### F-13 — Everything else
- Notifications/alerts (in-app, push, email), mobile app or PWA, public API or API keys, AI features, settings pages, language/currency options, dark/light mode, accessibility notes worth documenting.

### F-14 — Support and operational facts
- Support channels (see F-1) and any in-app support form (fields, limits).
- Status/incident page. Expected response times if stated anywhere.

### F-15 — Theta Labs-era leftovers
- List every Theta Labs-era feature that is still reachable (by nav or direct URL) but deprecated, paper-funded, or unmaintained, so the writer knows to **omit** it. Examples from the last audit: `/app/narratives`, `/app/social`, watchlist backend, Kalshi scaffolding, Solana wallet creation.

---

## 4. Page briefs

Propose the docs page list Updown needs (keep, rename, merge, delete, add relative to Section 2.1), then write **one brief per proposed page** using this shape:

```
### <group> / <page-path> — "<Page title>"
- **Purpose:** one sentence — what question this page answers for the user.
- **Status:** NEW | REWRITE of <old path> | KEEP with edits | DELETE (reason)
- **User flows:** numbered steps with verbatim labels (one flow per task the page covers).
- **UI anatomy:** tables of fields/columns/cells and what each means.
- **Key numbers/limits:** every figure the page must state.
- **Warnings/caveats:** what can go wrong; flags; eligibility; beta status.
- **In-app copy to reuse:** quoted strings from modals/tooltips/explainers.
- **Cross-links:** other pages this should link to.
- **Evidence:** file paths / symbols.
```

Minimum expected briefs (rename freely): introduction, quickstart, account setup, one page **per trading mode** from F-7, order execution (if it's a distinct topic), portfolio overview, deposits & withdrawals, positions, market discovery, incentives (if F-11 is non-empty), leaderboard/competition (if it exists), FAQ, glossary. Add pages for anything in F-12/F-13 that a user needs a guide for.

---

## 5. Stale baseline — facts the current docs assert

Mark each row **KEEP** (still true, same wording works), **REPLACE** (give the replacement fact), or **DROP** (no longer exists). Group identical dispositions if you like. This tells the writer what survived the pivot.

| ID | Theta Labs-era fact currently in the docs |
|---|---|
| B-1 | Product name is "Theta Labs"; support email `hello@thetalabs.gg`; X `@thetalabsgg`; Discord `discord.gg/d9gfrYWSAu` |
| B-2 | Landing CTA is **Start Trading** |
| B-3 | Auth via Privy; login with email, Google, or external wallet; embedded wallet created on first sign-in; non-custodial |
| B-4 | Post-signup onboarding collects name, optional phone, trading-background questions |
| B-5 | One-time **Enable trading** approval in the Wallet modal; revocable |
| B-6 | One user-facing wallet on Polygon; **Deposit wallet (Polygon)** and **Signing wallet (Polygon)** shown on Profile with Polygonscan links |
| B-7 | Deposit = USDC on Polygon via the **Wallet** modal (QR + copy); auto-converts to **pUSD**; staged status "Deposit detected… → Finalizing… → Converting your deposit to pUSD…"; first deposit adds a one-time setup |
| B-8 | Withdraw = native USDC to any Polygon address, gas-free, usually instant, **minimum $2**, Polygonscan receipt |
| B-9 | Single balance; Balance card cells **Available / In position / Total traded**; **Deposit** and **Withdraw** buttons on it; cash pill in top nav opens Wallet modal |
| B-10 | Total portfolio value = Available + open spot value + open options value; portfolio pill in nav shows it |
| B-11 | Portfolio value chart ranges 1D / 1W / 1M / 1Y / ALL, default 1W; hover cursor; refresh 15–60 s per panel |
| B-12 | Top nav: **Options · Spot · Portfolio**, trophy icon → Leaderboard, search box, cash pill, portfolio pill, user menu; `/app` redirects to Options; mobile bottom tabs Options / Spot / Portfolio |
| B-13 | Flagship product = real-money cash-settled **Yes Calls / No Calls** on a market's probability; no puts; buyers only (no writing) |
| B-14 | Options chain columns **Strike / Breakeven / Max return / Ask**; "Spot {pct}%" marker; per-strike capacity, **Sold out** state |
| B-15 | Options ticket: multi-leg, **Contracts / Dollars** toggle, payoff chart with draggable cursor, **Max profit / Max loss / Breakeven**, **Buy for $X**; minimum order 1¢ |
| B-16 | Options fill instantly against the platform dealer; no explicit fee; ~2% market-making spread rising as capacity fills |
| B-17 | Expiries: daily, Mon/Wed/Fri, weekly (Fri), monthly (3rd Fri); 8:00 pm ET; some markets have an earlier final expiry |
| B-18 | Options settlement = time-weighted average of Yes/No price over the final 60 minutes (or exact 0/1 on early resolution); payoff max(settlement − strike, 0); automatic hourly; explainer "How & When This Settles" |
| B-19 | Options position row: side + strike, **Qty / Avg / Now / Exp / P&L**; actions **Buy / Sell / Share**; Sell ticket shows bid, You receive, Realized P&L, **Sell for $X** |
| B-20 | Spot trading = Polymarket YES/NO shares priced 0–$1, settle to $1/$0; Polymarket is the only venue; Kalshi not live |
| B-21 | Spot ticket: **Yes / No** buttons with price in cents, **USD/Shares** toggle, Avg price / Cost / To win, **Buy**; buy min max($1, 5 shares); sell min 5 shares |
| B-22 | Spot orders are market orders only; "Market order · fills at the best available price"; ~2% automatic slippage guard; **Limit · soon** disabled button |
| B-23 | Spot settlement: manual **Redeem** button on the Portfolio row ("resolved — you won"); pays pUSD |
| B-24 | Spot page sort tabs **Trending / Hot (default) / New / Ending soon**; categories **All, Politics, Sports, Esports, Crypto, Economy, Tech, Culture, World**; per-game sports props filtered out |
| B-25 | Search: nav box "Search markets…" + **Ctrl/⌘+K** command palette |
| B-26 | Market card: image, title, YES % "chance" (green ≥50 / red <50), Yes/No buttons, top-2 outcomes for multi-outcome, footer volume + time left |
| B-27 | Market detail page: header + **Share**, price chart 1D/1W/1M/ALL, outcomes with 24h change and expandable chart + order book (7 levels), About, Recent orders, docked Buy/Sell ticket |
| B-28 | Positions tab sections **Options** and **Markets**; history tab filters **All / Options / Spot** |
| B-29 | Rewards: "Reveal your reward" free-share rewards on the Options hero — **Trade Reward** (first deposit, unlocks after $1 options premium), **Streak Rewards** (3/5/7-day), **Referral Rewards** (`/r/{code}`, up to $5 each) |
| B-30 | Leaderboard = weekly options competition by notional volume; rounds Fri 12:00 AM → Thu 11:59 PM EST; prizes $100 / $50 / $25 / $5 × 4th–10th in pUSD; columns **# / Trader / Contracts Traded / Volume**; **Rules & Prizes** modal |
| B-31 | Geo-blocking: nav indicator + "Viewing only · trading unavailable in your region" |
| B-32 | Fees: no explicit platform fee; Polymarket market fees pass through as "Est. fee"; options include spread |
| B-33 | Support: in-app **Contact** page form (5,000-char message), email, X, Discord |
| B-34 | Glossary terms: Available, Breakeven, Call option, CLOB, Embedded wallet, Event, Expiry, Implied probability, Liquidity, Market order, Max return, No Call, NO share, P&L, Polymarket, Premium, pUSD, Realized P&L, Settlement (options), Settlement (spot), Sold out, Spot trading, Spread, Strike, Unrealized P&L, Volume, Yes Call, YES share |
| B-35 | Deprecated but reachable last time: `/app/narratives` (paper), `/app/social` (unlinked), `/app/api-docs` ("API — Coming soon"), watchlist backend, dormant Solana wallet |

---

## 6. Required response format

Return **one file**, `docs-rebuild-response.md`, structured exactly like this. The writer will apply it without access to your codebase, so completeness beats brevity — but keep everything user-facing.

```markdown
# Docs Rebuild Response — generated <date> from <repo>/<branch>@<commit>

## A. Product summary
One paragraph, in the product's own voice, that the Introduction page can be built from.
Then a 5–8 row "key concepts" table (term → one-line meaning) a brand-new user must learn.

## B. Fundamentals
### F-1 …
### F-2 …
(through F-15; every sub-question answered, UNKNOWN where applicable, evidence cited)

## C. Proposed docs navigation
Groups → pages, in order, each with a one-line scope. Mark each page NEW / REWRITE / KEEP / DELETE
relative to the current site map in Section 2.1, with a reason for every DELETE and rename.

## D. Page briefs
One brief per page in Section C, using the Section 4 shape.

## E. Stale baseline disposition
| ID | Disposition (KEEP / REPLACE / DROP) | Replacement fact (if REPLACE) | Evidence |

## F. Glossary
Term → definition as the product uses it. Include every term that appears as a label in the UI
and every term in Section A's concepts table. Note terms from B-34 that should be dropped.

## G. FAQ candidates
10–20 questions users actually hit (fees, eligibility, deposits, settlement, losing money,
support), each with a 2–4 sentence answer and evidence.

## H. Needs team confirmation
Anything that is a brand, legal, or product decision rather than a code fact
(official URLs not in code, jurisdiction lists, compliance wording, program terms,
name-origin stories, whether orphaned pages are staying).
```

Notes for you, the responding agent:

- **Section B and Section D are the deliverable.** The writer will build every page from them. If a fact would appear on several pages (e.g. the deposit asset, the max-loss rule), state it fully once in Section B and reference the F-ID from the briefs.
- Quote in-app copy verbatim and mark it as a quote — it is the writer's best source of tone and vocabulary.
- If the codebase has its own docs, changelogs, marketing copy, or a pivot/decommission record that postdates the Theta Labs docs, cite them.
- If the pivot is partial (some Theta Labs surfaces intentionally remain under the Updown brand), say so plainly at the top of Section A — that changes how the writer structures the site.

# Docs Update Response — generated 2026-09-24 from thetalabs/main@df4fbba

> Audit basis: `main @ df4fbba` (2026-09-23 22:37 ET), 262 commits after `5248006`. This was a read-only code audit: route tree, nav, tickets, pricing and settlement code, crons (`apps/web/vercel.json`), shared constants, and the in-app strings. Nothing was run against production, and **no production environment variables were read**. Where a fact depends on an env var, the entry says so and repeats it in Section H.
>
> Conventions: text in "double quotes" is verbatim UI copy, with casing as written in source. "(shown uppercase)" means the source string is mixed-case and CSS uppercases it. `{…}` marks a runtime value. Paths are repo-relative. `apps/web/app/(app)/` is shortened to `(app)/`.

---

## A. What changed (Q-1, Q-2, Q-3 in one place)

updown now has two products. The flagship is **Options**: same-day and next-day **calls and puts on the biggest US stocks and ETFs**. They are priced and cash-settled by updown against each stock's **token on Robinhood Chain**, paid for in **USDG**, and bought in dollars or fractional contracts from a chain or a one-tap popup. The second product is **Interval markets**, the original Up-or-Down windows. They survive **for stocks and crypto only**, at **5, 15 and 60 minutes**, in one hub now called **Interval** (`/trade`, page title "Browse markets"). The site's own description of itself (`(app)/(home)/page.tsx`): "Same-day options on the biggest stocks, and short-term Up or Down markets on stocks and crypto. Pick a direction, tap to buy, and know your max loss before you're in."

**Flagship: Options.** It is the first nav link. On phones, the middle "Trade" tab opens `/0dte/spy`. Home's featured panel always opens on an options stock. The "Options" card row sits above the interval sections.

**Default landing:** there is no marketing landing page any more. `/` is **Home**, labelled **"Explore"** in the nav, and it is inside the app shell. Logging in opens Privy's popup **in place**, and the user stays on the page they were on. The only redirect-based entry is a referral link (`/r/<code>` → `/login` → `/`). No internal record captures the decision to add options or to narrow intervals as a single decision. The records are `Claude Docs/37-INTERVAL-MARKETS.md` (cadence and category narrowing, "the owner's call"), `Claude Docs/38-MULTICHAIN-DEPOSITS.md` §Model A (USDG on Robinhood Chain), `Claude Docs/39-GROWTH-PLAYBOOK-STOCK-OPTIONS.md` (go-to-market plan built on options), and commit messages `e3d99a2` (options lead Home) and `bfb9b03` (options become real money).

**Every user-visible change since 5248006:**

- **New: Options** (`/0dte/<symbol>`). Details are in Section B.
  - 09-21: launched as an admin-only play-money page.
  - 09-22: made public, and it began leading Home.
  - 09-23: became **real money**. Premium comes out of the user's USDG wallet on Robinhood Chain, with a $100 cap per buy.
- **Money moved to Robinhood Chain ("Model A").** Cash is **USDG in the user's own wallet on Robinhood Chain**, no longer pUSD on Polygon. The switch is controlled by an env var in production (see H-1).
  - New deposit picker: **"Deposit with" → "Crypto" / "Card or bank"**. Crypto accepts Robinhood Chain (USDG) and Polygon (USDC, routed automatically). Five other chains are listed as "Soon".
  - **A card / bank on-ramp now exists** (Stripe via Privy).
  - Withdrawals are sent as USDG on Robinhood Chain.
- **An interval taker fee now exists** (since 09-16): 0.07 × contracts × p × (1 − p), on opens and closes, never on settlement. It is built into the price and has no separate fee line. It can be switched off by an env var (see H-2).
- **Intervals were narrowed.**
  - Sports, Culture ("Mentions") and Outcomes are **hidden**: their pages return 404, new positions are refused, and old positions still settle.
  - Weekly and monthly markets were retired (09-19). Stock and crypto **daily** markets were retired (09-22) and replaced by **60-minute** markets.
  - **5-minute** markets returned for stocks and crypto (09-23). The lineup is now 5 / 15 / 60 min for 10 stocks and 5 cryptos.
- **Live and Trade merged** into one hub at `/trade`. Its nav label is **"Interval"** and its H1 is **"Browse markets"**. It has category and interval dropdowns, and there are separate **`/stocks`** and **`/crypto`** pages. `/live` redirects to `/trade`.
- **The `/app` URL prefix is gone.** Every page serves from the root (`/trade`, `/portfolio` …), and old `/app/*` links redirect.
- **Navigation was rebuilt.**
  - Desktop: "Options" · "Interval" · "Explore" · "Portfolio" · "Tournament", plus a search pill, then **Cash** and **Portfolio** figures.
  - Phones: a floating tab bar reading "Feed" · "Explore" · "Trade" · "Portfolio", plus a ☰ drawer.
  - A **left side panel** (desktop ≥ 1280 px) has tabs "Stocks" · "Leaderboard" · "Feed".
- **Signup has no questions.** The onboarding form is gone, and each new account gets a generated username (e.g. "BlueAngrySquare") and a shape avatar.
- **Profile and Portfolio merged** at `/portfolio`. **Public profiles** live at `/u/<name>`, with **Follow**, followers and following lists, an optional **X handle**, privacy toggles ("Wallet" / "Positions" / "Activity"), and a Rank stat.
- **Social layer:**
  - A global **trade feed**.
  - **Takes**: an optional note of up to 280 characters, or a GIF, posted after a trade. Takes show "Take" / "🔥 Hot take" badges and can be liked with a heart.
  - **Avatar pins** on charts showing who bought where.
- **Tournament returned**, replacing the Weekly Trading Competition. It is a fixed two-week event, **Thu Sep 24 → Wed Oct 7 2026 ET**, with a **$1,000** pool paid **$600 / $300 / $100**, ranked by realized **interval** P&L. `/leaderboard` redirects to `/tournament`. The side panel also has rolling **24H / 7D / 30D / ALL** boards.
- **Search (Ctrl/⌘+K)** now searches option stocks and interval markets only. The legacy prediction-market search is gone.
- **Multi-buy**: pick up to 20 interval outcomes and place them in one tap. These are independent buys, not a parlay.
- **Home** (`/`) contains a Patterns ticker, a rotating featured panel, "Up next" clocks, an Options row, Stocks and Crypto sections, and a right rail.
- **Quick buy is now tap-to-buy.** The ~0.7 s hold gesture is gone.
- **Crypto prices come from Coinbase. Stocks come from Pyth, and stocks now trade 24/5 (Sun 8 PM → Fri 8 PM ET).** Eight stocks also stay open over the weekend on a Hyperliquid perpetual price. Only PLTR and SPY close for the weekend.
- **Referrals are hidden.** `/referrals` redirects to `/portfolio`, and the menu links were removed.
- **Removed:** the landing page and its "Start Trading" CTA; the "Tell us about you" form; the Weekly competition frame; the old "Trade" hub (Daily / Weekly / Monthly); the Sports and Outcomes rows; the category row under the nav.

---

## B. US equity options

### E-1 — What it is

**Product voice** (verbatim):
- Stock page meta description (`(app)/0dte/[symbol]/page.tsx`): "Options on {Name} priced from its Robinhood Chain token: pick a strike and an expiry, see the cost, the break-even and the payoff before you're in."
- Home meta description: "Same-day options on the biggest stocks, and short-term Up or Down markets on stocks and crypto. Pick a direction, tap to buy, and know your max loss before you're in."
- Token explainer on every stock page (`options-view.tsx` `KeyStats`): "{SYMBOL} on Robinhood Chain is a token backed by {the ETF's shares | the company's shares}. One token is {1.0011} shares — dividends are reinvested into the token instead of paid out, so that number creeps up over time. It trades around the clock in dollar pools on the chain; the price on this page, and the price your options settle at, come from those pools."

**Which of these is it? → (b): updown's own cash-settled contracts.** They are **not** listed exchange options. There is no broker, no OCC clearing and no brokerage account.
- **Instrument.** Vanilla **European calls and puts**, **cash-settled** in USDG.
  - Payoff per contract: call = max(settle − strike, 0); put = max(strike − settle, 0).
  - Evidence: `apps/web/lib/token-options/ledger.ts` `payoffPerContract`; `packages/shared/src/options/token-option.ts` ("European, cash-settled option on the token").
  - They are **not** binaries: payout scales with how far past the strike the price finishes.
- **Underlying.** The **stock token on Robinhood Chain**, not the share itself.
  - Strikes and settlement are in *token* dollars (`token-option.ts` header, "user decision 2026-09-21").
  - One token = `multiplier` shares. The multiplier is ≈ 1.00x and drifts up as dividends are reinvested; SPY's is 1.00172 (`apps/web/lib/robinhood/universe.json`).
- **Quote source.** `apps/web/lib/robinhood/token-options.ts` `quoteTokenChain`, served by `GET /api/robinhood/option-chain`.
  - The spot price is the median of the token's deepest on-chain pools, read live from the chain (`lib/robinhood/universe.ts` `pricePools`).
  - The model is updown's same-day pricing engine (`packages/shared/src/options/zero-dte.ts`), anchored where possible to Cboe's delayed listed options prices.
- **Settlement code.** `apps/web/lib/token-options/real.ts` `settleDue`, run by cron `/api/cron/token-options-settle`. The price rule is in `lib/robinhood/settlement-math.ts` (`SETTLEMENT_RULE`) and `universe.ts` `tokenSettlement`.
  - **Settlement price:** the **average token price over the 5 minutes before the close**. There is one sample every 5 s (60 samples); each sample is the median across the token's pools, and at least 80% of samples must be readable.
  - **Cross-check:** for regular ("solid") tokens, the settlement price must be within **0.5%** (50 bps) of the real stock's last 1-minute close × the multiplier. "Thin" tokens settle on the average alone.
  - **Refund:** if no final price exists within **6 hours** of expiry, the premium is **refunded** and the position shows as "void".
- **How it differs from intervals:**

  | | Interval markets | Options |
  |---|---|---|
  | What wins | Up or Down on a window | Payoff grows with distance past a strike |
  | Payout | Fixed $1 per winning contract | Scales with the final price |
  | Price source | Pyth (stocks) / Coinbase (crypto) | The stock's on-chain token price |
  | Expiry | Every 5 / 15 / 60 minutes | The 4:00 PM ET close, same day or next session |
  | Money on open | None moves; cost is reserved | Premium leaves the wallet immediately |

- **Counterparty.** **updown's house wallet is the only counterparty.** There is no order book and no other traders on the other side ("The price is ALWAYS ours", `real.ts`). The house is **not hedged** (`Claude Docs/40-TREASURY-DELTA-HEDGING.md`: "Nothing in this doc is live").
- **Real money: yes, since 2026-09-23** (`bfb9b03`).
  - Buying sends USDG from the user's wallet to the house. Selling back and settlement send USDG from the house to the user.
  - Each transaction hash is stored (`open_tx` / `close_tx` in `token_option_positions`).
- **Paper / demo mode: none reachable.** The earlier play-money version ($10,000 practice balance, "Reset account") is no longer routed. Its code is still in the tree, and one background leftover is noted in H-20.
- **Do not confuse with `/options`.** That is the archived Theta Labs **prediction-market** options product (Yes/No calls on event odds). It is untouched, unlinked, and closed to new opens (`OPTIONS_OPENS_DISABLED = true`). **The new product does not reuse it**; only pure math helpers are shared. The docs must not describe `/options`.

### E-2 — Where it lives

- **Desktop nav:** **"Options"** is the first link. It goes to `/0dte`, which redirects to **`/0dte/spy`**, and it stays lit on any `/0dte/…` page (`components/app-shell/Navbar.tsx` `MARKET_LINKS`).
- **Phones:** the floating tab bar's **"Trade"** tab opens `/0dte/spy` (`MobileNav.tsx` `TABS`). The ☰ drawer lists "Options" under "Trade".
- **Routes:** there is **no options list or hub page**. Each stock's page *is* the options page: `/0dte/<symbol>`, lowercase (e.g. `/0dte/aapl`). Unknown symbols return 404. The tab title is "{SYM} options".
- **Default landing after login:** the user stays where they were (see A). `/` = Home ("Explore") leads with options.
- **Other entry points:**
  - **Home**:
    - the featured panel (options slides, labelled "Options · expires at today’s close");
    - the **"Options"** card row with "See all";
    - the rail's **"Options"** movers card;
    - the "Options 0DTE · Expire in" clock (shown uppercase).
  - **Search** (Ctrl/⌘+K).
  - **Side panel "Stocks" tab** (desktop ≥ 1280 px). On phones this is the "Stocks" tab of `/feed`.
  - **Portfolio** rows ("Trade").

**Page anatomy** (`(app)/0dte/options-view.tsx`). From 1024 px (lg) there are two columns, with the order panel sticky on the right. Phones get one column (see E-6 for phone buying).

1. **Header.**
   - Logo, ticker (e.g. "SPY"), name.
   - Optional **"thin"** tag, with tooltip "Only a thin pool prices this token".
   - Tiles **"Volume"** (USDG traded on-chain in 24 h) and **"Liquidity"** (dollars in the token's pools), shown from 640 px up.
   - Big price with the move: "▲ $1.23 (0.45%) today".
2. **Chart** (`candle-chart.tsx`): on-chain candles.
   - Interval buttons "1m" · "5m" · "15m" · "1h" · "D", default 5m. Style toggle "Candles" / "Line".
   - Hint "On-chain price · scroll to zoom, drag to pan". Legend O/H/L/C, "Vol $1.2K".
   - Phase pill **"Market hours" / "Extended hours" / "Overnight"** (shown uppercase). Extended hours are tinted orange; overnight, weekends and holidays purple. Times are ET.
   - Error state: "The chart is not answering right now — retrying."
3. **Quick trade / Chain** toggle (**≥ 768 px only; hidden on phones**).
   - Quick trade heading: "What does **SPY** $765.43 (+0.45%) do by [Thu, Sep 24 ▾]".
   - Chain heading: "Option chain".
   - The expiry dropdown lists **two** expiries, each like "Thu, Sep 24" with "4:00 PM ET" beneath.
4. **Order panel** (E-6), with a strip "{n} open on SPY ↓" and total P&L.
5. **Positions** table (E-7). This is also visible on phones.
6. **"About {Company}"**: description with "Show more" / "Show less", plus "Industry", "Employees", "Exchange", "Listed".
7. **"The {SYMBOL} token"**: explainer (E-1) and "Shares per token", "Priced from" ("N pools" / "· thin"), "Contract" (address link), "Settles": "4:00 PM ET, on-chain price".
8. **"Stats"**: "Market cap" (sub "P/E x"), "Volume" (sub "+N% vs average"), "On-chain liquidity" (sub "N pools on Robinhood Chain"), "Day range" (sub "opened $x"), "Previous close", "52 week range", "Dividend yield" (sub "next ex-date …").
9. **Phones only:** the docked quick-buy bar (E-6).

**Quick trade** (`QuickBuilder`, `lib/token-options/strategies.ts`):
- Step 1 is four outlooks: **"Up"** — "finishes higher" · **"Down"** — "finishes lower" · **"Volatile"** — "big move, either way" · **"Neutral"** — "stays put".
- Step 2 is three ready-made trades per outlook:

  | Outlook | Trades | Card titles |
  |---|---|---|
  | Up | a **call** at the 1st, 2nd and 3rd strikes above the price | "Above $770" |
  | Down | a **put** at the 1st, 2nd and 3rd strikes below | "Below $760" |
  | Volatile | buys **both** a call and a put: a straddle at the price (when a strike is within 0.6%), plus strangles one and two rungs out | "Exits $760.12–$770.88" |
  | Neutral | **sells** a strangle | "Stays $758–$773" |

- **Neutral is not available.** Every Neutral card's button reads **"Coming soon"**, and the order panel shows **"Selling options is coming soon."**
- **Each card shows:**
  - the title and its distance ("+0.59%" / "±0.70%");
  - "**NN%** chance" (not shown on Volatile);
  - "$1.23 per contract";
  - "2× your money at" and "5× at" with target prices.
  - Neutral cards show "you collect", "Keep it if it stays", and "Max loss" "Uncapped".
- **Card buttons:** "Add to order" → "In your order" → "✓ Done".
- **Empty state:** "Nothing to offer for this outlook right now."

**Empty / coming-soon states:**
- "Coming soon" (selling).
- The chain shows "Not quoting this expiry right now (inside the last 15 minutes)." / "(the on-chain price is stale)." / "(no variance)."
- "No quotes: {error}".
- There is **no market-closed banner**, because options quote around the clock (E-9).

### E-3 — Underlying universe

- **Every stock token on Robinhood Chain that has a trading pool gets an options page: 93 symbols** (`(app)/0dte/[symbol]/page.tsx` `generateStaticParams` over `lib/robinhood/universe.json`, generated 2026-09-22). 34 of them are flagged **thin**, meaning only shallow pools price them.
- The list is **generated from the chain**, not admin-added and not the full US listed universe. New tokens appear only when `universe.json` is regenerated.
- **ETFs: yes.** SPY, QQQ, GLD, SLV, USO, SGOV, SOXX and INDA are solid; VTI and EWY are thin. **No index options** (no SPX/NDX). **No crypto options** (COIN and MSTR are equity tokens).
- **A featured set of 9** gets Home, the rotating panel, and phone buying: AAPL, NVDA, TSLA, GOOGL, META, PLTR, COIN, SPCX, SPY (`packages/shared/src/zero-dte/stocks.ts` `ZERO_DTE_STOCKS`; HOOD is listed but has no token and is skipped).
- **How users find a stock:**
  - **Search** (Ctrl/⌘+K or the "Search markets…" pill; placeholder "Search stocks, crypto and options…"). The "Options" section shows up to 8 results, each "{SYMBOL}", "{Name} · Options", price and move.
  - **Side panel "Stocks" tab**, with chips "Watchlist" · "Trending" (default) · "Most volume" · "Winners" · "Losers".
    - Star a row to add it to the Watchlist. The watchlist is **stored in this browser only**.
    - Empty states: "Star a stock to watch it here." / "Nothing up today." / "Nothing down today." / "Nothing here."
    - Tokens with no price are folded under "No liquidity yet · N" and link to the block explorer, not to an options page.
  - Home cards and movers.
- **Quote availability caveat:** a token only quotes if updown has at least 10 prior sessions of the real stock's 5-minute bars. Otherwise the chain shows "No quotes: not enough sessions of bars for the level". Whether every exotic ticker quotes is UNVERIFIED.

### E-4 — Contract specifications

| Item | Answer | Evidence |
|---|---|---|
| Calls / puts | **Both.** In the UI a call is "Above $X" / "Up", and a put is "Below $X" / "Down". | `strategies.ts`, `options-view.tsx` |
| Buy vs write | **Buy-to-open only.** Writing / selling-to-open is "Coming soon". Users can **sell back** (close) what they hold. No margin or collateral. | `real.ts` header "Long only: writing is not offered." |
| Legs | At most **2 legs** per order, each a buy: single call, single put, straddle, or strangle ("Up or Down" / "Volatile"). | `real.ts` (`legs.length > 2` → "Bad order.") |
| Contract multiplier | **1 contract = 1 token** (≈ 1 share × multiplier), **not 100**. Contracts can be **fractional, to 0.01**. | `token-options.ts` header; `real.ts` `r2` |
| Quote units | **Dollars (USDG) per contract**, in $0.01 ticks. Minimum ask $0.05. The chain's "Buy" cell shows e.g. "1.23" with no $ sign. | `zero-dte.ts` `quoteZeroDte` |
| Strike ladder | **6 strikes**, at **±1%, ±2%, ±3%** of the current price, rounded to a tick: $1 at prices ≥ $100, $0.50 at ≥ $20, $0.10 at ≥ $5, $0.01 below. No at-the-money strike on the ladder. The straddle uses the nearest strike, and strikes you already hold are also quoted. Example at $250: 243, 245, 248 / 253, 255, 258. | `lib/robinhood/strike-ladder.ts` |
| Expiries | **Two at a time: the next two regular-session closes that are more than 15 minutes away.** During a session that is today + the next session; after 3:45 PM ET, overnight or at weekends it is the next two sessions. No weeklies, monthlies or LEAPS. The code comment reads "Same-day and next-day only for now (the owner's call, 2026-09-22)". | `token-options.ts` `nextCloses(nowMs, 2)` |
| Expiry time | **4:00 PM ET** (America/New_York), or **1:00 PM ET on NYSE half-days**. The static label "4:00 PM ET" is wrong on half-days (H-20). | `sessionCloseMs`; `NYSE_CALENDAR` |
| Last trading time | Trading stops **15 minutes before an expiry**. That expiry then drops off the list and the chain rolls to the next close. A buy against it is refused: "That expiry is no longer offered." / "Too close to the close to trade." | `CUTOFF_MIN = 15` |
| Exercise style | **European, automatic, cash-settled.** No exercise button, no early exercise, no assignment. | `token-option.ts` |
| Settlement | **Cash in USDG.** ITM contracts are paid (settle price − strike, or strike − settle) × contracts, straight to the wallet. OTM contracts get $0 and the position is marked settled; no transfer is sent. | `real.ts` `settleDue` |
| When cash lands | On the first run of the settle job after expiry once the price is final. The job runs **every 5 minutes, 24/7** (`vercel.json`). Expect minutes after the close; the exact delay is UNVERIFIED. | `/api/cron/token-options-settle` |
| No price | If no final price exists within **6 h**, the **premium is refunded** and the position shows "void". History: "Refund" · "no settlement price, premium refunded". | `VOID_AFTER_MS` |
| Early close | Sell back any time before the cutoff, at **updown's current bid** × contracts (E-7). | `closeReal` |
| Assignment | N/A (users cannot write). | — |
| Corporate actions / halts | **No user-facing handling or copy.** Dividends are reinvested into the token, so the price doesn't gap on the ex-date. If the stock is halted with no 1-minute bar in the 30 minutes before the close, a solid token has no cross-check, so the position ends as a **void + refund** after 6 h. Splits: no handling found (UNVERIFIED). | `universe.ts`, `settlement.ts` |

### E-5 — Quotes, pricing, and counterparty

- **Price source: updown's own model.** It is **not** an exchange NBBO and there is no broker or market maker.
  - The model is Black's formula on the token forward, with variance measured on a trading clock (an intraday U-shaped profile plus a daily forecast from the real stock's recent 5-minute bars) and a volatility smile.
  - When Cboe's delayed listed chain for the same expiry has enough quotes, the model is scaled to match the listed at-the-money price (`packages/api/src/lib/stock-options/cboe-chain.ts`).
  - The code marks its spread constants "PROVISIONAL" (`zero-dte.ts`).
- **Spread (the house's margin):**
  - Half-spread = 4% of the option's volatility value + 1% per standardized unit of distance from the money, + a "travel" allowance for 10 s of movement.
  - Floor **$0.02**. Ask rounded **up**, bid rounded **down**. Minimum ask **$0.05**. Bid shows 0 below $0.01.
- **What the user sees:**
  - Chain: **"% chance"** (|delta|), **"Break-even"**, **"% change"** (the ask's move since the last close), **"Buy"** (the ask).
  - Order panel: **"Cost"**, **"Max loss"**, **"Max profit"**, **"Breakeven(s)"**, **"% chance ITM"**, and a **payoff chart**.
  - Builder cards: "NN% chance", "2× your money at", "5× at".
  - **Not shown:** bid (except inside the "Sell $X" button), size, IV, greeks, volume, open interest.
- **Counterparty:** the house, and only the house. There is **no order book**, and every buy is re-quoted server-side.
- **Fees:** **none.** No commission, no per-contract fee, no pass-through regulatory fees (none apply, since nothing is exchange-listed). The house earns the bid/ask spread. The ticket shows no fee line.
- **Price protection:** the server re-quotes and refuses the buy if the new cost is **more than 2% above** what the user saw: "Price moved: {now} now, you saw {seen}." A lower price fills at the lower price. There is **no slippage copy in the UI**.
- **Stale price:** there is no quote if the on-chain price is more than **15 s** old (the chain shows "(the on-chain price is stale).").

### E-6 — The options chain and ticket

**Chain** (`ChainTable`, desktop / tablet ≥ 768 px):
- **Header row:** "Calls ↑" | "SPY 765.43 [Thu, Sep 24 ▾] Expires in 3h 25m" | "Puts ↓".
- **Columns, as written:**
  - Calls: "% chance" · "Break-even" · "% change" · "Buy"
  - Centre: "Strike"
  - Puts, mirrored: "Buy" · "% change" · "Break-even" · "% chance"
- **Tooltips:** "% chance" = "Chance it finishes past the strike"; "Break-even" = "Where a bought contract has paid for itself"; "% change" = "The option's price move since the last close"; "Buy" = "What one contract costs you".
- **Rows:** 6 strikes, highest first, each "$770" with its distance ("+0.59%").
- **Near-the-money marker:** a horizontal rule between the strikes above and below the price, with the price in a white pill. In-the-money cells are tinted (calls green, puts red).
- **"Buy" cell:** clicking it **selects** that contract into the order panel. It does not buy. A cell flashes "Bought" after a buy, or "✕" on a failure.
- **Phones:** **no chain.** A phone layout exists in the code but never renders.

**Order panel** (`OrderPanel`, ≥ 768 px):
- **Title:** "{trade} · {date}" (e.g. "Above $770 · Sep 24"), or "No trade selected". The first Up trade is pre-selected.
- **Rows:** "Outlook" (chip Up / Down / Volatile / Neutral) and "Instrument" or "Legs" (e.g. "Buy $770 Call · Sep 24").
- **"Amount":** toggle **"Contracts"** (default, starts at "1") / **"Dollars"**. The other unit shows alongside ("= $1.23" / "= 4.07 contracts"). Dollars convert to contracts at the ask, rounded to 0.01.
- **Button, depending on state:**
  - "Sign in to trade" (signed out; opens the sign-in popup)
  - "Deposit to buy" (cost > wallet; opens "Deposit with")
  - "Placing…"
  - "Coming soon" (Neutral)
  - **"Buy $4.92"**
- **Readouts:** "Cost", "Max loss", "Max profit" ("Uncapped" for calls and two-sided trades), "Breakeven" / "Breakevens", "% chance ITM", "Wallet" "$X.XX USDG".
- **Payoff block:** heading "Payoff"; strip "Max loss" | "Break even" (while dragging: "Profit at $X" "+$2.10 (+43%)") | "Max profit". The chart shows P&L at the close.
- **Order types:** **market only, at updown's ask.** No limit or stop orders, no day/GTC. Up to 2 legs (straddle / strangle).
- **Confirmation:**
  - Success: **"Bought for $4.92"** in green for ~2.5 s, then the take box (E-12).
  - Failure: the server's message in red for ~6 s.
  - Partial fills: none (all-or-nothing).

**Quick-buy popup** (`(app)/0dte/quick-buy-modal.tsx`). Opened from Home and from the phone bar; real money.
- **Layout:** a bottom sheet on phones, a centred dialog from 640 px.
- **Header:** logo, name, "SPY · $765.43 · by 4:00 PM" (or "by Thu 4:00 PM"). It always uses the **nearest expiry**, with no expiry choice.
- **Body:**
  - A compact payoff chart.
  - Tabs **"Up"** / **"Down"** / **"Up or Down"** ("Up or Down" buys a call and a put, i.e. a strangle).
  - Three rows, e.g. "Above $770.00" with a "▲ $4.57" distance tag and a per-contract price.
- **Amount:** chips **"$1" "$5" "$10"** or a "$" field; opens on **$5**. Contracts = dollars ÷ price, rounded **down** to 0.01, so it never charges more than the amount entered.
- **Summary line:** "Max loss $4.92 · profit above $771.23" (or "profit below $X or above $Y").
- **Button:** "▲ Buy above $770.00 · $4.92" / "Buy Up or Down · $4.92", "Placing…", "Deposit to buy", "Sign in to buy".
- **Footer line:**
  - "Couldn't load prices — try again in a moment"
  - "Closed for the last 15 minutes of the session"
  - "Not quoting right now"
  - "Enter at least $1.00"
  - **"Bought for $4.92 — settles at the close"**
  - otherwise "Wallet $X.XX USDG"

**Phone layout** (`mobile-quick-bar.tsx`):
- The stock page shows the chart, positions and stats, plus a docked bar **"▲ Up" · "▼ Down" · "Up or Down"** that opens the quick-buy popup.
- **The bar only appears for the 9 featured stocks** (E-3). On the other 84 pages, phones cannot buy (H-14).

**Limits and checks:**
- Minimum 0.01 contracts and $0.01 of cost (popup: at least $1).
- **Maximum $100 of premium per buy**: "One buy is capped at $100 for now (this one is ${x})."
- No per-user position cap, daily cap or open-position limit.
- **Buying power = the USDG in the wallet.** The UI swaps the button to "Deposit to buy" when short; the chain transfer fails otherwise.

**Every error string** (from `apps/web/lib/token-options/real.ts` and `app/api/robinhood/options/route.ts`, shown verbatim to the user):
- **Account:**
  - "Sign in first."
  - "Account not set up yet — sign in again."
  - "This account can't trade."
  - "No wallet on this account yet."
- **Order:**
  - "Bad order."
  - "Unknown token."
  - "That expiry is no longer offered."
  - "Too close to the close to trade."
  - "Not quoting right now."
  - "No quote for the {strike} {call|put}."
  - "Price moved: {now} now, you saw {seen}."
  - "Too small."
  - "One buy is capped at $100 for now (this one is ${x})."
- **Payment:** "Couldn't take {cost} USDG from your wallet: {wallet error}". A missing trading permission shows the wallet's raw message, e.g. "Trading permission is required to move funds.".
- **Sell-back:**
  - "No such position."
  - "Already closed."
  - "Expired — it settles at the close price."
  - "Not quoting right now — it will settle at the close."
  - "Already closing."
  - "Couldn't pay out: {msg}. The sale is being confirmed — check back in a few minutes."
- **Low-level:** "bad body" / "bad order" / "unknown action" / "failed" / "HTTP {status}".

### E-7 — Positions, closing, and history

**On the stock page** (`PositionsTable`):
- **Header:** "Positions" · "{n} on SPY" · total P&L.
- **Columns:** "Contract" · "Qty" · "Paid" · "Now" · "P&L".
- **Row:** "↑ $770 Call · Sep 24" (↓ for puts), quantity, paid, "Now" (= updown's bid × contracts), P&L "+$0.27".
- **Empty state:** "Nothing open on SPY."
- **Footer:** "{n} more open on AAPL, NVDA" (links), "History · N" / "Hide history".
- **To close:** tap **"Sell $1.50"** (current value at the bid). There is **no confirm step**. Success: **"Sold for $1.50 — paid to your wallet."**
  - The Sell button shows only for positions on the **expiry currently selected** in the dropdown. Positions on the other expiry show "—" until you switch.
  - In the **last 15 minutes** before an expiry, positions on that expiry cannot be sold and ride to settlement (the cutoff; E-4).
- **History rows:** "SPY $770 Call · {sold | settled | void}", with qty, paid, proceeds and P&L.

**On Portfolio** (`(app)/portfolio/portfolio-shared.tsx` `TokenOptionRow`), under "Your positions" → "Open", after interval rows:
- Title "SPY above $770" / "SPY below $762".
- Line "▲ Call · Option · closes in 3h 05m". It changes to "paying out" while a payout is in flight, and "settling" after the close.
- Line "1.00 contracts · $1.23 → $1.50" (paid → now).
- Right side: value "$1.50" and "+$0.27 (+22.0%)", or "in progress" / "not quoted now".
- Button **"Trade"** opens the stock page. **You cannot sell from Portfolio.**

**History labels** (Portfolio activity table, filter **"Options"**):
- **"Buy"**, **"Sell"**, **"Won"**, **"Lost"**, **"Refund"**.
- Details: "1.00 contracts @ $1.23", "… · settled in the money", "… · expired worthless", "… · no settlement price, premium refunded".
- No "exercised" or "assigned" events exist.

**Statuses behind the scenes** (for the writer's understanding; users see "open" / "sold" / "settled" / "void"):

| Status | Meaning |
|---|---|
| pending | Premium being collected; never shown |
| open | Held |
| closing | Sell-back payout in flight; shown as open or "paying out" |
| closed | Sold back; shown as "sold" |
| settling | Expiry payout in flight; shown as open or "paying out" |
| settled | Expired and paid |
| void | Expired with no price; premium refunded |

**P&L and marking:**
- Open positions are marked at **updown's bid**, and at **cost** when not quoted.
- Options are included in the Portfolio page's "$X in positions", total value, **"Volume"** (contracts × the stock price at purchase; a sale counts again) and **"Realized P&L"**.
- They are **not** included on **public** profiles (`/u/<name>`), in the value-chart history snapshots, or on the Tournament board.

### E-8 — Money, balances, and funding

- **Same cash as everything else.** There is no separate options account or brokerage cash. Premium is paid from the **USDG** in the user's own Robinhood Chain wallet. It **leaves immediately** at purchase (a sponsored, gas-free transfer to updown's house wallet), and payouts arrive in the same wallet.
- **Nav "Cash"** is the wallet's USDG, so it drops by the premium at once. **Nav "Portfolio"** adds open options at their bid value (`Navbar.tsx`).
- **Portfolio card** (`portfolio-view.tsx`): "Available cash" = USDG − cost reserved by open **interval** positions. "$X in positions" includes options at bid. Details in C / PF-3.
- **Withdrawal lock:** options lock nothing, because the premium has already left. Only open interval positions reserve cash.
- **Settlement holds:** none. There is no T+1, and payouts go straight to the wallet.
- **"Total traded"** no longer exists as a cell. The Portfolio header's **"Volume"** counts option buys and sales at **contracts × stock price at purchase**.

### E-9 — Hours and time

- **Trading hours: around the clock, every day.** The code has no session gate on options quotes, buys or sales ("the options trade around the clock (the owner's call, 2026-09-23)", `(app)/(home)/options.tsx`).
- Overnight and at weekends, the offered expiries are the next two session closes. Whether quotes are reliable in production on weekends is UNVERIFIED.
- **Expiry:** regular NYSE session closes only, **4:00 PM ET**, or **1:00 PM ET** on half-days: 2026-11-27, 2026-12-24 and 2027-11-26. NYSE holidays are skipped. The calendar runs through 2027 (`NYSE_CALENDAR`).
- **Cutoff:** **15 minutes** before each expiry, so the same-day expiry disappears at **3:45 PM ET** (12:45 PM on half-days).
- **Time zone:** ET everywhere (chart axis, expiry labels, "4:00 PM ET").
- **Stale quotes:** refused when the on-chain price is more than 15 s old. The chart refreshes every 30 s; positions every 10 s.
- **Maintenance mode** does **not** pause options (H-19).

### E-10 — Eligibility, compliance, risk

- **KYC / identity / suitability / options approval levels: none anywhere.** Signup is automatic on first sign-in (email or Google via Privy). The only third-party KYC is Stripe's, inside the card on-ramp (`DepositCardModal.tsx` comment "KYC is Stripe's, we never see it").
- **Geo: not enforced for options, or for anything else.**
  - The middleware has a 56-country blocked list **including the US** (`apps/web/middleware.ts` `BLOCKED_COUNTRIES`). It only drives a nav globe icon ("Trading is unavailable in your region") and a drawer notice. No trade path checks it.
  - A code comment says "Stock tokens are NOT offered to US / UK / CA / CH persons" (`apps/web/lib/robinhood/chain.ts`). **Nothing enforces it.** → H-6.
- **Disclosures / agreements: none.** There is no OCC "Characteristics and Risks of Standardized Options" acknowledgement (and it would not apply, since these are not standardized options), and no risk checkbox. The only risk-adjacent copy is inline: "Max loss", "Most you can lose", "You know your max loss before you're in."
- **Terms / privacy / risk URLs: none exist.** → H-7.

### E-11 — Explainers (quoted in full)

- **Token explainer:** see E-1. Beside it: "Settles": "4:00 PM ET, on-chain price".
- **Chain tooltips:** "Chance it finishes past the strike" · "Where a bought contract has paid for itself" · "The option's price move since the last close" · "What one contract costs you".
- **"thin" tooltip:** "Only a thin pool prices this token".
- **Chart hint:** "On-chain price · scroll to zoom, drag to pan".
- **Selling:** "Selling options is coming soon."
- **Popup:** "Max loss $X · profit above $Y" · "Bought for $X — settles at the close" · "Closed for the last 15 minutes of the session".
- **Home featured panel:** "Options · expires at today’s close" · "Live · closes in …" / "Delayed · …" · "since today’s open" · "last session, open to close" · "Full chart and positions".
- **There is no "how it works", settlement-rules, or risk block** for options anywhere in the app. **The settlement method** (the 5-minute on-chain average, the 0.5% cross-check, the 6-hour refund) **is not explained in the UI.** The docs will be the only place it is written down; H-8 asks the team to confirm this wording.

### E-12 — Interaction with the rest of the app

- **Leaderboard / Tournament: options do NOT count**, for ranking or for the public "Volume" column (`packages/api/src/lib/competition/interval-standings.ts`: "options and spot activity do not count at all"). A trader with only options trades does not appear on the board. Admins see options volume separately.
- **Referrals: no.** Referrals are hidden. The fee share (10%) accrues only on the archived `/options` product's fees, and the new options charge no fee.
- **Rewards / promos:** none tied to options.
- **Notifications:** none for buys, expiry, settlement or refunds.
- **Share cards:** none for options (the existing share card only serves the archived `/options` product).
- **Social:**
  - After a buy, the **take box** appears: "Add your take" · "Shown on the feed with this trade" · placeholder "Why this trade?" · "N / 280" · "GIF" · "Skip" · "Post".
  - The buy is pinned to the stock's chart as the buyer's avatar (calls above the candle, puts below). Tooltip: "{name}", "$4.92 on the call at $770", then the take.
  - The feed shows the buy as "SPY above $770" with an **"Option"** tag and "Bought $4.92 ▲ Up $4.57".
  - The feed's options-empty copy still says "Every practice-option buy lands here." — stale (H-20).

---

## C. Claim verdicts

### Confirmed

BR-1, IM-2, IM-3 (plus the fee noted under FC-1/TK-6), IM-12, LH-5, MP-3, MP-4, TK-1, TK-3, TK-4, TK-7, PH-3, PH-4, NV-4 (desktop only; on phones the same links are in the drawer's "More" section), AU-1, AU-5, FC-3, FC-4, FC-7, LO-2

### Corrections

| ID | Verdict | What the app does now | Evidence |
|---|---|---|---|
| **IM-9, IM-10, LH-4 (sports line), TH-2 (Outcomes row), MP-2 (End-of-Week / End-of-Month), IM-4 (GAME OVER / FINAL / UPCOMING / "Series ended" tags), TK-8 (game-over string)** | REMOVED | **Sports, Outcomes and Culture ("Mentions") are hidden.** Their pages (`/sports`, `/outcomes`, `/culture`, their market pages) return 404. New positions are refused ("This market is closed to new positions. Open positions settle at the window's close."). They are dropped from every browse surface, search and the feed. Sports auto-listing is off. Positions held from before stay in Portfolio **without a link**, with a "Settles at close" chip, and settle normally. Every sports-only label (team sides, "Down / Flat", game-over tags, "View the underlying market →", "Trading paused — game over", "Rangers up") is unreachable. | `packages/shared/src/interval/hidden.ts` `HIDDEN_INTERVAL_CATEGORIES = ["sports","mentions","predictions"]`; `packages/api/src/lib/interval/discover.ts` `SPORTS_KEEPER_LISTS_NEW_GAMES = false`; `engine.ts` `executeFill` |
| **TH-1, TH-2, TH-3, TH-4, MP-2 (End-of-Day), BR-5 (Trade description)** | REMOVED | **The separate Trade hub is gone.** It merged into `/trade` "Browse markets" (2026-09-19). Daily / weekly / monthly markets are retired for every visible category (weekly and monthly 09-19; stock and crypto daily 09-22, replaced by 60-minute markets). `-1d` / `-weekly` / `-monthly` pages return 404, and the expiry menu no longer offers them. | `packages/shared/src/interval/retired.ts` `intervalCadenceRetired`, `RETIRED_DAILY_CATEGORIES`; `lib/interval/resolve.ts` |
| IM-1 | OUTDATED | **Stocks and crypto run 5-, 15- and 60-minute windows, each its own market** (slug `<sym>-5m` / `-15m` / `-60m`). Windows sit on the UTC grid; 60-minute windows start on the UTC hour. There are no daily / weekly / monthly windows. **To beat** = the price at the window's open instant. Crypto uses the close of Coinbase's 1-minute candle ending at the boundary (final 10 s after). Stocks use the last price at or before the open. | `retired.ts` `FIVE_MINUTE_CATEGORIES`; `lib/interval/markets.ts` `windowFor`, `CADENCE_KEYS`; `shared/interval/boundary.ts` |
| IM-4 | OUTDATED (partly) | Window line "{September 23} · {2:00 PM} – {2:15 PM}" (local time; phones drop the date). Stock and crypto markets show only the **"LIVE"** tag. **"To beat" / "Now" / "Time left"** (shown uppercase) are confirmed, with Now's sub-line "+$2.00 (+0.60%)". Time left is white while more than 50% of the window remains, then amber, and red below 20%. After the close: "Ended {Sep 23, 2:15 PM EDT}". | `(app)/live/[slug]/interval-market-view.tsx` |
| IM-5 | OUTDATED | Close vs open, **strictly above = Up** (confirmed). The settlement job now runs **every minute**. The page-triggered pass (throttled to 45 s) only records window history. **Price sources:** stocks = **Pyth** (1-second prints; weekend prices come from a Hyperliquid perpetual for 8 stocks); crypto = **Coinbase**. **Money:** one net transfer per user per window in **USDG on Robinhood Chain** (win paid by the house, loss collected from the user's wallet), assuming production runs Model A (H-1). Selling back mid-window settles realized P&L immediately. Portfolio strings are confirmed: "settling", "awaiting settle", history "Won" / "Lost" / "Settled" with "· paid $1 each" / "· settled worthless" / "· nothing owed either way". | `vercel.json` `interval-settle` `* * * * *`; `app/api/interval/settle-due/route.ts`; `engine.ts` `settleClosedWindows`; `lib/settlement/rh-chain.ts` |
| IM-6 | OUTDATED (minor) | Page confirmed: eyebrow "{category} · {15 min} · settled" (shown uppercase), "Up won" / "Down won", "Open" / "Close", chart pill "Target {$x}", and the empty state "This window isn't recorded yet" / "Windows are saved shortly after they close." / "Back to the live market". "Rangers up" and "unchanged — settles down" are **team-market-only (hidden)**. A plain market that finished unchanged shows "Down won". | `(app)/live/[slug]/[window]/window-view.tsx` |
| IM-7 | OUTDATED | The "Settlement rules" accordion still reads, for **both stocks and crypto**: "A new window opens every {5\|15\|60} minutes. Its opening price is {AAPL}'s price at the moment the window opens." / "Up wins only if {AAPL}'s price closes strictly above that opening price. Down wins if it closes at or below it — so a window that finishes exactly unchanged settles Down." / "Each winning contract pays $1.00 and each losing contract expires at zero. Every position is fully collateralized when you enter, so an Up at p cents and its matching Down at 100 minus p cents together set aside exactly $1.00." / "Prices come from the Robinhood tape, the live bid and ask midpoint for {SYM}. The tape follows US market hours, so it pauses on weekends and market holidays. The same feed powers the chart above, so pricing and settlement share one source of truth." Stocks on the weekend list add: "From Friday 8:00 PM to Sunday 8:00 PM ET, while US markets are shut, this market stays open and prices off the {SYM} perpetual on Hyperliquid, a 24/7 market that tracks where traders expect the stock to reopen. It is not an exchange quote, and it can differ from the price when trading resumes. Weekend windows open and settle entirely on that feed." **The fourth paragraph is wrong against the code** (stocks use Pyth, 24/5; crypto uses Coinbase, 24/7), so the docs should not repeat it (H-8). The odds variant is hidden. | `interval-market-view.tsx` `SettlementTerms` |
| IM-8 | OUTDATED | **Stocks trade 24/5: Sunday 8:00 PM → Friday 8:00 PM ET.** AAPL, NVDA, SPCX, TSLA, GOOGL, META, HOOD and COIN **also stay open at weekends** on a Hyperliquid perpetual price, so **only PLTR and SPY close** (Fri 8 PM → Sun 8 PM ET). Holidays are not special-cased. Closed page: "Market closed — {PLTR} trades {Sunday 8:00 PM – Friday 8:00 PM ET}." / "Crypto markets stay open 24/7." / button **"Back to markets"** (→ `/trade`). Closed trade dialog: "Market closed — this stock trades {label}." Stock cards still drop off the hub while closed. Stock cards show a phase pill "Market hours" / "Extended hours" / "Overnight". | `shared/interval/weekend.ts` `WEEKEND_STOCK_COINS`; `markets.ts` `stockMarketOpen`, `stockHoursLabel` |
| IM-11 | OUTDATED | **Stocks (10): AAPL, NVDA, SPCX, TSLA, GOOGL, PLTR, META, HOOD, SPY, COIN. Crypto (5): BTC, ETH, SOL, DOGE, HYPE.** Each subject is available at 5, 15 and 60 minutes. The 15-minute markets are defined in code (`INTERVAL_MARKETS`) and can be overridden by DB rows; the 5- and 60-minute markets are **DB rows only** (manual migrations). Admins can add subjects (they get all three cadences). | `lib/interval/markets.ts`; `packages/db/manual-migrations/2026-09-19-five-minute-markets.sql`, `2026-09-22-sixty-minute-markets.sql`, `2026-09-19-hype-markets.sql` |
| LH-1 | OUTDATED | Nav item **"Interval"** → `/trade`. H1 **"Browse markets"**, sub "{All Categories} · {All Intervals} · {n} markets". Two dropdowns: category ("All Categories" / "Stocks" / "Crypto", with counts) and interval ("All Intervals" / "5 min" / "15 min" / "60 min"; "24 hr" is in the list but never has markets). A **"Multi-buy"** toggle, and a grid of compact cards. Filters are kept in the URL (`?cat=` / `?interval=`). **Category pages** `/stocks` and `/crypto` (title "Stocks markets \| updown"): H1 "Stocks" / "Crypto", an interval tab row "5 min" / "15 min" / "60 min" with counts (default **15 min**, no "All"), a "Multi-buy" toggle, the featured panel, and a 2-column grid of live cards. They are reached from Home's section "See all" (the category row under the nav is gone). `/live` → `/trade`. | `(app)/live/interval-view.tsx` `MarketsHub`; `lib/interval/hub.ts` `HUB_INTERVALS`; `(app)/(category)/[cat]/page.tsx` |
| LH-2 | OUTDATED (partly) | Featured panel (category pages and Home): "{Bitcoin (BTC)} · {15 min}", heads "Market" / "Pays out" / "Odds", rows "Up" "{1.96}x" "{51}¢", "Closes in" / "Opens in", "To beat" / "Now". **The chart now shows the whole current window** (open → now) with a "Target" pill and buyer avatars; it is no longer a 2-minute tape. **No "3 of 12" pager.** Home rotates panels automatically every 9 s, with dots. | `interval-view.tsx` `FeaturedTrend`; `(home)/home-view.tsx` |
| LH-3 | OUTDATED | **Two card types.** (1) **Compact card** on `/trade`: image, title, "To beat" \| "Now" + "▲ {$1.20}", buttons "Up {51¢}" / "Down {49¢}", footer "{15 min}" + countdown; no LIVE tag, no Last 6. (2) **Live card** on category pages and Home: image, title, "{15 min}", countdown, "To beat" (sub "{2:15 PM} close"), "Now" (sub "+$x (+y%)"), rows "Up" / "Down" with "{51}¢" buttons and bars, footer "Live" (shown uppercase) or "Upcoming", **"Last {n}"** triangles (tooltip "{2:00 PM} window · {$325.40} → {$325.62} · Up"). On Home only, the live card also shows **"Also"** links to other cadences. | `(app)/live/simple-market-card.tsx`; `interval-view.tsx` `IntervalMarketCard` |
| LH-4 | OUTDATED | Kept: "New recurring markets roll out here as their metrics go live.", "Waiting for the first quote", "Waiting for a price…". New: **"No markets right now"**, **"No stock markets right now"** / "Stock markets list while their price feed is live.", button "Browse all markets". "Trading opens in …" only appears for markets with a scheduled start (effectively sports, hidden). | `interval-view.tsx` |
| MP-1 | OUTDATED | Chart buttons **"Live" · "5m" · "15m" · "1H" · "MKT"** (shown uppercase) on every market. MKT = the whole window so far. The page opens on Live (a 2-minute tape). 1D / 1W / 1M no longer appear (they need windows of a day or longer). | `interval-market-view.tsx` `LOOKBACKS` |
| MP-2 | OUTDATED | Last-4 chips (tooltip "{2:00 PM} window · {$a} → {$b} · settled Up") and a **"Past"** dropdown of up to 8 windows ("{2:15 PM} ET · Today" / "· {September 22}"), both confirmed. The "Change expiry" menu now offers **"5 Minutes" / "15 Minutes" / "60 Minutes"**. Headline "{Apple (AAPL)} \| [{15 Minutes} ▾] Up or Down". Browser tab: "{Apple (AAPL) \| 15 Minute Up or Down} — Interval \| Theta Labs" (brand leftover, H-3). | `shared.tsx` `RecentCloses`; `CADENCE_KEYS` |
| MP-5 | OUTDATED (minor) | Confirmed: "Trade Up" / "Trade Down", "Price" / "Shares" / "Total", "Asks" / "Bids". The divider's "Last {mid}¢" is the **quote midpoint**, not a last trade; "Spread {n}¢" confirmed. Footnote now reads "…A Up at p¢ and a Down at 100−p¢ lock $1 between them." (sic). Order-book windows (see Q-7) use "Resting orders in this window's order book — the market maker's and other traders' …". | `interval-market-view.tsx` `OrderBook` |
| MP-6 | OUTDATED | **Stocks:** 1-second Pyth prints pushed to the browser, with a 1.5 s poll only as a fallback when the stream has been quiet for more than 5 s. **Crypto:** a direct Coinbase feed, every trade. | `shared.tsx` `useIntervalLive` |
| TK-2 | OUTDATED | **Tap to buy; the hold gesture is gone.** Side pills "▲ Up 51¢" / "▼ Down 49¢" (unselected until picked); a "$ Custom" amount field with "Buy" and "win $x"; boxes "$1" / "$5" / "$10", each "win $9.80". Status line: "Pick Up or Down" → "Tap an amount to buy" → "Placing…". Box tooltip "Buy $5 Up". | `shared.tsx` `WindowTicket`, `QUICK_AMOUNTS` |
| TK-5 | OUTDATED | "Trade executed" / "Bought ${5.00} {Up} @ {51}¢" / "Pays ${9.80} if {Up} wins"; sells "Sold $x Up @ y¢" and "+$0.40 realized" (" · settling…"); partials "Filled $X of your $Y — book was thin here" — all confirmed. **There is no auto-dismiss any more.** The card stays open with the **take box** until the take is posted or skipped. Errors clear after 4 s. | `shared.tsx` `TradeExecutedCard` |
| TK-6 | OUTDATED | Ladder confirmed: 6 levels per side sized 30/24/18/13/9/6%, `OI_CAP = 20` per side per window at every cadence, half-spread 1¢ + up to 4¢, minimum spread 2¢, skew up to 3¢, mid clamped 2–98¢. 5-minute windows are spread as if they were 15-minute windows. **`FEE_BPS = 0` is gone**; there is now a taker fee (FC-1). | `shared/interval/quote.ts`; `shared/interval/fees.ts` |
| TK-8 | OUTDATED | Current refusals: "This market is closed to new positions. Open positions settle at the window's close." · "Market is not open for trading right now." · "You only hold {n} contracts on this side." · "No liquidity available at these levels." · "Buying power is required to open a position." · **"Insufficient balance: need {5.00} pUSD, have {2.10}."** (still says pUSD; H-20) · "We're upgrading the platform — trading and transfers are paused for a few minutes." · "We're busy right now — try again in a few seconds." · "Another request on your account is still in progress — try again in a moment." · "Account not set up yet. Sign in again to finish account setup." · the restricted-account message (FC-3). | `engine.ts` `executeFill`; `routers/interval.ts` |
| TK-9 | OUTDATED | Phone market page: docked pills **"Buy Up {51¢}" / "Buy Down {49¢}"**. Tapping one **opens the trade dialog** (not a sheet). The dialog has "Open market page" ↗, **the live window chart on top** (Target pill, avatars), then the desktop ticket with a countdown and "{Apple (AAPL)} {15 min} · {$331.67} target". The same dialog opens from any card's price button. Signed out → sign-in popup first. | `interval-market-view.tsx` `MobileTicketSheet`; `(app)/live/trade-modal.tsx` |
| TK-10 | OUTDATED (minor) | "Sell" in Live Positions is **a single tap, not a hold**. It sells the whole side with no minimum-price guard. Ticket Sell mode (dollars ÷ price, with the price guard) is confirmed. Fills at the bid ladder; no minimum; maximum 10,000 contracts; the fee applies; realized P&L is paid immediately. | `shared.tsx` `PositionPanel`; `routers/interval.ts` `sellToClose` |
| BR-2 | OUTDATED | **There is no landing page.** `/` = Home inside the app. The tagline survives only in titles and link previews: title **"updown \| Trade Movement on Everything"**. Home description: "Same-day options on the biggest stocks, and short-term Up or Down markets on stocks and crypto. Pick a direction, tap to buy, and know your max loss before you're in." Site-wide default description: "Trade short-term directional movement on stocks, crypto, and more. Pick Up or Down for the window, tap to buy, and know your max loss before you're in." | `app/layout.tsx`; `(app)/(home)/page.tsx` |
| BR-3 | REMOVED | No "Start Trading" CTA. Entry is Home (`/`). | `(home)/page.tsx` |
| BR-4 | OUTDATED | **The app contradicts itself; see H-3 / H-4.** Contact page: **"hello@updown.fast"**, X **"@updown_markets"**, Discord `discord.gg/d9gfrYWSAu`. Status bar / drawer: X `x.com/thetalabsgg`, Discord `discord.com/invite/d9gfrYWSAu`, Docs `https://docs.thetalabs.gg/introduction`. The "Account flagged" modal still says **hello@thetalabs.gg**. **No page links `app.thetalabs.gg`**; the app serves from the root of whatever host it is on. `updown.fast` appears as the public site URL in Discord alerts and code comments. The site metadata base is still `https://thetalabs.gg`. | `(app)/contact/page.tsx`; `components/app-shell/SocialIcons.tsx`; `BannedNotice.tsx`; `packages/api/src/lib/notify/discord.ts` |
| BR-5 | OUTDATED | Live description is gone. Current metadata: `/` and `/trade` title "updown \| Trade Movement on Everything"; `/stocks` "Stocks markets \| updown" — "Trade short-term Up or Down markets on stocks — pick a side for the window, tap to buy, and know your max loss before you're in." (crypto likewise); `/0dte/<sym>` "{SYM} options" (E-1); `/tournament` "Tournament — updown" — "Two weeks, ranked by realized P&L — a $1,000 prize pool for the top three."; `/portfolio` inherits the site default. | files as named |
| NV-1 | OUTDATED | Desktop bar, left to right: mark + "updown" → `/`; **"Options"** (`/0dte`) · **"Interval"** (`/trade`) · **"Explore"** (`/`) · **"Portfolio"** (`/portfolio`) · **"Tournament"** (`/tournament`); centre search pill **"Search markets…"** with a "Ctrl K" hint; right: "Admin" (admins only), region globe (only when geo-flagged), then signed in: **"Cash"** figure (tooltip "Cash = available to trade · click to deposit"; opens "Deposit with") and **"Portfolio"** figure (tooltip "Portfolio = cash + open positions"; links to `/portfolio`), then the avatar menu. Signed out: **"Login"**. Avatar menu: name header, **"Profile"** (→ `/portfolio`), **"Light mode"** / **"Dark mode"**, **"Log out"**. "Referral" was removed. **Left side panel** (≥ 1280 px, every page except `/feed`): tabs **"Stocks"** · **"Leaderboard"** · **"Feed"** (first visit opens on Feed). | `Navbar.tsx`; `UserMenu.tsx`; `components/side-panel/SidePanel.tsx` |
| NV-2 | OUTDATED | `/app` → `/`; `/app/live`, `/app/interval` → `/trade`; any other `/app/<path>` → `/<path>`. Login opens Privy's popup **in place**, so the user stays on the same page. The `/login` page (reached only via referral links or an account-setup error) returns to `?returnTo` or `/`. | `apps/web/next.config.ts`; `components/…/LoginButton.tsx`; `app/login/page.tsx` |
| NV-3 | OUTDATED | Phones: no top bar, only a centred "updown" mark with a ☰ button ("Open menu"). **Floating tab bar: "Feed" (`/feed`) · "Explore" (`/`) · "Trade" (`/0dte/spy`) · "Portfolio"** (shows your avatar when signed in). **Drawer:** "Search markets…"; "Login" (signed out); the region notice when flagged; **TRADE**: "Options", "Interval", "Explore", "Portfolio", "Tournament", "Feed"; ACCOUNT: "Admin" (admins); **PROFILE**: "Profile", "Light mode" / "Dark mode", "Log out"; **MORE**: "Contact us", "Docs", Discord, X. | `MobileNav.tsx` `TABS`; `Navbar.tsx` |
| NV-5 | OUTDATED | Maintenance strip confirmed (admin message + " Resuming in {2h 14m}."). **The competition banner now counts down the Tournament** in its last 6 hours: "Tournament ends in {2h 14m} — {you're #4, $3.10 behind #3 \| you're #1 — defend it \| you're #N \| trade now to get on the board}". It links to `/tournament`. "Account flagged" modal confirmed (FC-3). | `components/app-shell/CompetitionDeadlineBanner.tsx`; `MaintenanceBanner.tsx` |
| NV-6 | OUTDATED | **"Tournament" is in the nav**: the last desktop link, and in the phone drawer. It is also lit on `/leaderboard`, which redirects to `/tournament`. On phones the "Feed" tab stays lit on `/tournament`. The side panel's **"Leaderboard"** tab shows rolling boards. | `Navbar.tsx`; `(app)/leaderboard/page.tsx` |
| NV-7 | OUTDATED | Ctrl/⌘+K, or the search pill, opens a palette searching **only what updown trades**. Placeholder "Search stocks, crypto and options…". Sections **"Options"** (each token as "{Name} · Options", → `/0dte/<sym>`) and **"Interval markets"** (one row per subject, "{category} · Up or Down" with cadence chips "5 min" / "15 min" / "60 min"; the row opens the 15-minute market). States: "Nothing matches “{q}”", "Nothing to show", "Loading…", "Clear". The legacy prediction-market search is gone. | `components/app-shell/CommandPalette.tsx`; `lib/search/markets.ts` |
| AU-2 | REMOVED | **No onboarding form.** The first sign-in creates the account automatically with a generated username (Color + Adjective + Shape, e.g. "BlueAngrySquare"), avatar, disc color and rotation. Refusals: "Please sign up with a permanent email address." / "Please sign up with your regular email address (no dot-alias variants)." A failed setup lands on `/login`: "We couldn't set up your account" / "Sign out and try another account". The referral cookie is still **30 days** (`theta_ref`). | `hooks/use-user.ts`; `packages/api/src/routers/user.ts` `create`; `packages/shared/src/identity.ts`; `app/r/[code]/route.ts` |
| AU-3 | OUTDATED | Shown inside the deposit dialogs until permission is granted. Idle: **"Enable trading"** / "One approval. After that, trades go through with no signing." / checks "Your keys stay yours" · "Only the trades you place" · "Revoke anytime" / button **"Allow trading"**. Approving: "Enabling trading" / "A few seconds. You can close this, it keeps going." Done: **"Trading enabled"** / "Deposit to trade." / **"Continue"**. "Theta Labs" and "Only works with Polymarket & Kalshi" are gone. **There is still no revoke control**, even though the copy says "Revoke anytime" (H-5). | `components/trading/AllowTrading.tsx` |
| AU-4 | OUTDATED | `/profile` → `/portfolio`. **Portfolio is the profile.** Its layout is described under PF-2 / Q-6. **"Edit profile"** dialog: "Username" (rule "3–24 letters, numbers or underscores"; hints "Checking…", "That username is taken.", "Available"), read-only "Email", **"X account"** ("Optional. Shows an X icon next to your name that links to your account. Paste a handle or profile URL."), **"Icon"** ("Shuffle", 26 colors + "Custom", "Background" disc color with "Black" reset, 16 shapes, a rotation knob "Rotate icon"), **"Visibility"** ("What other people see on your profile. Hidden sections are still visible to you." — "Wallet" / "Positions" / "Activity", each "Public" / "Hidden"), "Save" / "Cancel". Name / Phone are gone. The wallet icon (tooltip "Wallet addresses") opens a **"Wallet"** dialog that still reads "pUSD balance", "Deposit wallet (Polygon)", "Signing wallet (Polygon)" with "View on Polygonscan" (stale; H-20). Sign-out is an icon (tooltip "Sign out"). | `components/profile/EditProfileDialog.tsx`; `components/profile/WalletDetailsDialog.tsx` |
| MO-1 | OUTDATED | **Two deposit flows exist (H-13).** **Main flow** (nav "Cash", Home's phone "Deposit", options "Deposit to buy"): **"Deposit with"** → **"Crypto"** ("Transfer USDC from a crypto wallet") or **"Card or bank"** ("Debit, credit, Apple Pay or US bank"). **"Deposit with crypto"**: a network dropdown with **"Robinhood Chain"** (default) and **"Polygon"** live; "Arbitrum", "Base", "Solana", "BNB Chain", "Ethereum" greyed out with **"Soon"**. Rule: "Send {USDG \| USDC} on the {Robinhood Chain \| Polygon} network to this address." / "Deposit at least ${0.50 \| 2.00} to add to your cash balance." Polygon adds "We route Polygon USDC into your Robinhood Chain balance automatically." One QR code plus the full address with copy ("Address copied"). The same address works on both chains. **Legacy flow** (the Portfolio page's own "Deposit" button): the old **"Wallet"** modal, unchanged, still saying pUSD / Polygon. | `components/wallet/DepositWithModal.tsx`, `DepositCryptoModal.tsx`, `WalletModal.tsx`; `packages/shared/src/deposits.ts` `DEPOSIT_CHAINS` |
| MO-2 | OUTDATED | Minimum **$2** for Polygon USDC and card deposits. Robinhood Chain shows **$0.50**, but USDG simply lands in the wallet and counts at once, with nothing converted. Below the minimum (new dialogs): "We received $X — send $Y more to reach the $2 minimum." Progress: "Adding your deposit to your balance…". Toasts: "Detected $X USDC — adding to your balance…" → **"Deposit added to your balance"**; failure "Deposit failed to convert — funds were not lost. Try again." Polling: sweep 20 s, status 5 s, address info 8 s while open. (The legacy modal keeps the old pUSD strings.) | `DepositCryptoModal.tsx`; `hooks/use-deposit-sweep.ts`; `lib/settlement/route-to-rh.ts` `ROUTE_MIN_USD` |
| MO-3 | OUTDATED | **"Withdraw"** — "Send USDG to any address on Robinhood Chain. Gas-free, usually instant." "Available $X". Fields **"Recipient address (Robinhood Chain)"** (placeholder "0x…", error "Invalid address"), **"Amount (USDG)"** + **"MAX"** (errors "Exceeds your balance" / "Minimum $2"). **"You receive"** "≈ $X USDG". Button **"Withdraw"** / "Withdrawing…". Footnote "Gas is sponsored. Withdrawals are irreversible — double-check the address." (unchanged). Minimum **$2**. Success: **"Withdrawal confirmed"** / "≈ $X in USDG is on its way to 0x1234…abcd." / "View transaction →" (opens the Robinhood Chain explorer, Blockscout) / "Withdraw again" / "Done". Accounts with an old pUSD balance ≥ $2 get a link: "You also have $X from before the upgrade — withdraw it as USDC on Polygon". | `components/portfolio/WithdrawModal.tsx`; `routers/deposits.ts` `withdraw` |
| MO-4 | OUTDATED (wording) | "Amount $X exceeds your available $Y ($Z is committed to open positions)." ("interval" dropped). With nothing reserved: "Amount $X exceeds your balance of $Y." Only open **interval** positions reserve cash; options don't. Confirmed: the modal's "Available" and "MAX" show the raw balance. | `routers/deposits.ts` |
| MO-5 | OUTDATED | **A card / bank on-ramp now exists** ("Card or bank": Stripe through Privy, buys USDC on Polygon that is routed in automatically). Copy: "Card and Apple Pay purchases usually land within a few minutes. Bank transfers can take a few days. You can close this — we'll add it to your balance when it arrives." Gas is sponsored (confirmed). Restricted accounts can still withdraw (confirmed). **Fees:** see FC-1 (the Polygon / card route passes through a bridge that takes a small cut). | `DepositCardModal.tsx`; `packages/api/src/trpc.ts` `BANNED_ALLOWED_PATHS` |
| PF-1 | OUTDATED | Two separate figures, **"Cash"** first (opens "Deposit with"; tooltip "Cash = available to trade · click to deposit"), then **"Portfolio"** (links to `/portfolio`; tooltip "Portfolio = cash + open positions"; hidden on the narrowest phones). Refresh every 30 s. Cash = the wallet's USDG. Portfolio = cash + open interval positions (at the bid) + open options (at the bid). | `Navbar.tsx` |
| PF-2 | OUTDATED | Portfolio value card: a large **total** with no "Balance" heading, a change line, range pills; then **"Available cash"** $X with "$X in positions" beneath; buttons **"Withdraw"** and **"Deposit"**. **"Total traded" is gone.** Volume moved to the page header as **"Volume"**. | `(app)/portfolio/portfolio-shared.tsx` `ValueCard` |
| PF-3 | OUTDATED | **Available cash** = wallet USDG − cost reserved by open interval positions. **In positions** = interval positions (contracts × midpoint; at cost while settling) + options (at updown's bid; at cost when unquoted or paying out) + any legacy positions. **Total** = sum. **Volume** (header) = interval ($1 per contract over every buy and sell) + options (contracts × stock price at purchase, sales counted again) + legacy. **Realized P&L** (header) = interval + options. | `portfolio-view.tsx`; `lib/token-options/portfolio.ts`; `packages/api/src/lib/interval/portfolio.ts` |
| PF-4 | OUTDATED (label) | Pills "1D" "1W" "1M" "1Y" "ALL", default "1W" (confirmed). The change line is now **"+$X (+Y%) 7d"** ("24h" / "7d" / "30d" / "1y" / "all time"). Snapshots every 15 min; refresh cadence confirmed (15 s / 10 s / 30 s / 60 s; options 10 s). With too little data: "Not enough history to chart yet". The history line only counts cash + legacy positions, not open interval or option value (H-20). | `portfolio-value-chart.tsx`; `vercel.json` |
| PF-5 | OUTDATED | Left: card **"Your positions"** with a segmented control **"Open"** (count) / **"Orders"** (count). Orders empty state: "No open orders" + "Interval trades fill instantly, so nothing rests here yet. Limit orders for intervals arrive with the order book." + "Browse markets". Right: activity table with filters **"All activity" / "Interval" / "Options" / "Spot"**, columns **"Market" / "Action" / "Amount" / "P&L" / "Time"**. There are no section headings such as "Interval markets". | `portfolio-view.tsx`; `portfolio-shared.tsx` |
| PF-6 | OUTDATED | One list, no section headings, in this order: interval positions, **new options** ("SPY above $770" rows, E-7), then legacy prediction-market options / writes / spot for accounts that still hold them. **The new options product does not reuse the legacy sections.** | `portfolio-shared.tsx` `TokenOptionRow` |
| PH-1 | OUTDATED | Interval row: image, title; line "▲ 15m · closes in 12m" / "15m · settling" (the team name appears only on team markets; cadences "5m" / "15m" / "60m"); one detail line **"3.00 contracts · 51¢ → 53¢ · pays $3.00 if up"**; right: value and "+$X (+Y%)" or "awaiting settle"; button **"Trade"** (or **"View"** while settling), or a **"Settles at close"** chip for hidden / retired markets. | `portfolio-shared.tsx` `PositionRows` |
| PH-2 | OUTDATED | Chips **"Buy" / "Sell" / "Won" / "Lost" / "Settled"** (not "Bought" / "Sold"). Details "3.00 contracts @ 51¢", "· paid $1 each", "· settled worthless", "· nothing owed either way". Time is relative ("now", "12m", "3h", "5d", then a date). Interval history caps at 200 rows. Options add "Refund" (E-7). **Withdrawals:** chip "Withdraw", "Withdrawal to 0x1234…abcd", "native USDC to external wallet". These appear **only for old Polygon withdrawals**; USDG withdrawals are not listed in activity (H-20). **Withdrawals never appear on public profiles** (since df4fbba). | `portfolio-shared.tsx`; `packages/api/src/lib/trading/spot-orders.ts` |
| LB-1 | OUTDATED | The name "Weekly Trading Competition" is no longer shown anywhere. The same rule ranks **the Tournament** and the side panel's rolling boards: realized P&L on **interval** markets for windows closing inside the period, sell-to-close included, ties broken by interval volume, and only completed settlements count. | `packages/api/src/lib/competition/interval-standings.ts`, `standings.ts` `compareStandings` |
| LB-2 | OUTDATED | **The Tournament runs Thursday Sep 24, 12:00 AM → Wednesday Oct 7, 11:59 PM ET (two weeks).** Header shows "two weeks" (shown uppercase), "Sep 24 – Oct 7 ET", and "Starts in" / "Ends in" with a countdown, or "Status" "Final" once over. The side panel also offers rolling **24H / 7D / 30D / ALL** windows. There are no weekly resets in the UI. | `packages/shared/src/competition.ts` `TOURNAMENT`; `(app)/leaderboard/leaderboard-view.tsx` |
| LB-3 | OUTDATED | **$1,000 pool: 1st $600, 2nd $300, 3rd $100** (top three only). Copy: "Prizes are paid out after the tournament ends." No asset is named. **Payout is manual** (there is no payout code). | `competition.ts` `TOURNAMENT.prizesUsd`; `leaderboard-view.tsx` `RulesModal` |
| LB-4 | OUTDATED | Rules modal ("Rules & Prizes"), verbatim: "Ranked by realized P&L on interval markets across the whole tournament — what your settled windows paid out minus what you staked, sell-to-close included. Only windows that close inside the tournament count; ties break on traded volume." / "Runs two weeks: Thursday, Sep 24, 12:00 AM → Wednesday, Oct 7, 11:59 PM ET." / "The top three when it ends share a $1,000 prize pool. Prizes are paid out after the tournament ends." Tiers "1st place $600", "2nd place $300", "3rd place $100". | `leaderboard-view.tsx` `RulesModal` |
| LB-5 | OUTDATED (partly) | Confirmed: columns "#" / avatar / "Trader" / "Volume" / "P&L" / "24h"; podium once 3+ traders are ranked (it shows the prize beside each trophy); "You" tag; pinned own row outside the top 50; refresh 10 s. `/tournament`: h1 "Tournament", sub "$1,000 prize pool · ranked by realized P&L", link "Rules & Prizes"; list heading "All traders · ranked by P&L" / "Final standings · ranked by P&L". Empty state: **"No trades yet."** / "Be the first on the board."; before the start: "The tournament starts Thursday, Sep 24 at 12:00 AM ET." / "Trade any market once it opens to get on the board." **Correction:** the public board excludes only accounts flagged hidden. **Admins and house bots can appear.** "Volume" = interval only ($1 per contract). | `leaderboard-view.tsx`; `packages/api/src/lib/competition/board.ts` |
| FC-1 | OUTDATED | **Interval markets now charge a taker fee**: 0.07 × contracts × p × (1 − p) on every open and close, never on settlement. At 50¢ that is 1.75¢ per contract; at 10¢ or 90¢, 0.63¢. It is **folded into the price** you're shown, with no separate line. Built 2026-09-16; it can be switched off by env var (H-2). **Options: no fee**, spread only. **Deposits:** none on Robinhood Chain USDG; the Polygon / card route passes through the Across bridge, which takes a small cut (the code comment's example is $5.00 → $4.97). **Withdrawals and gas:** free / sponsored. | `packages/shared/src/interval/fees.ts` `INTERVAL_FEE_RATE = 0.07`; `lib/settlement/route-to-rh.ts` header |
| FC-2 | OUTDATED (enforcement unchanged) | Same 56-country list (including the US), unchanged. It still **only shows the globe icon** ("Trading is unavailable in your region") plus a drawer notice. **No trade path is blocked**, for intervals or options. The detection may also not work in production: the API route reads headers the middleware sets on the response, not the request (inferred from code; needs a runtime check). | `apps/web/middleware.ts` `BLOCKED_COUNTRIES`; `app/api/geo/route.ts` |
| FC-5 | OUTDATED | "Get in Touch" / "Contact Us" / "Questions, feedback, or partnership inquiries — we'd love to hear from you." Cards **"EMAIL"** "hello@updown.fast" · **"X (TWITTER)"** "@updown_markets" · **"DISCORD"** "Join our server". Divider "Send a Message". Form "Username" (read-only), "Email" (placeholder "you@example.com"), "Message" ("Tell us how we can help...", "{n} / 5000"), **"Send Message"** → "Sending…" → "Message Sent!"; error "Something went wrong. Please try again." Sending **requires sign-in**. | `(app)/contact/page.tsx` |
| FC-6 | REMOVED (hidden) | **Referrals are hidden.** `/referrals` → `/portfolio`, and the links were removed from the menu and drawer. `/r/<code>` still records sign-ups (30-day cookie). Constants are unchanged (10% for 6 months, $5 minimum payout), but the share accrues only on the archived `/options` product's fees, so **no current trade earns referrers anything**. | `(app)/referrals/page.tsx`; `packages/api/src/lib/rewards/referral.ts` |
| LO-1 | OUTDATED | **In nav:** `/`, `/0dte/<sym>` (`/0dte` → `/0dte/spy`), `/trade`, `/portfolio`, `/tournament`, `/feed` (drawer and phone tab), `/contact`, `/admin` (admins). **Linked, not in nav:** `/stocks`, `/crypto`, `/live/<slug>`, `/live/<slug>/<window>`, `/u/<name>`, `/login`. **Redirects:** `/live` → `/trade`; `/leaderboard` → `/tournament`; `/referrals` and `/profile` → `/portfolio`; `/social` → `/trade`; `/r/<code>` → `/login`; `/app/*` → root. **404:** `/sports`, `/culture`, `/outcomes`; every `/trade/<slug>` (all day-plus markets are retired or hidden). **Archived but still reachable by URL:** `/predictions`, `/spot`, `/spot/<slug>`, **`/options`, `/options/<slug>` (the old Theta Labs prediction-market options; NOT reused, and closed to new opens)**, `/arena` + 7 sub-pages, `/narratives`, `/community`, `/survey`, `/api-docs` ("Coming soon"). **Admin-gated:** `/collect`, `/compute`, `/robinhood`. **Dev only:** `/pilot/<window>`, `/pilot-lab` (404 unless a local flag is set). | `apps/web/app/**/page.tsx` |
| LO-3 | OUTDATED | Still "Theta Labs": the login page (logo alt and visible text); tab titles on `/live/<slug>` ("— Interval \| Theta Labs"), past windows, `/trade/<slug>`, `/feed` ("Feed \| Theta Labs"), `/u/<name>` ("{name} — Theta Labs"), and the archived pages; the **default link-preview image** for app pages without their own (including `/portfolio`, `/0dte`, `/feed`: "thetalabs.gg" / "Trade Prediction Markets" / "Options, spot markets, and portfolio — all in one terminal.", although its logo row now shows updown); the portfolio share card watermark "thetalabs.gg"; "hello@thetalabs.gg" in the flagged-account modal; the site metadata base. **Fixed:** the Enable-trading copy; the Home, `/trade`, `/live` and `/tournament` link previews; the Contact page. | `app/login/page.tsx`; `(app)/opengraph-image.tsx`; `(app)/portfolio/share-card.tsx` |

---

## D. Proposed docs navigation

Options pages come **first** in Trading, because options are the flagship and lead Home and the nav. Interval pages follow as their own group.

| Group | Page (path) | Status | Reason |
|---|---|---|---|
| Get started | `introduction` | REWRITE | Two products now; options lead; no landing page, no "Start Trading". |
| Get started | `quickstart` | REWRITE | Flow is now: sign in (no form) → Allow trading → deposit USDG/USDC → buy an option or an interval. |
| Get started | `account-setup` → **rename `account-and-profile`** | REWRITE | Profile merged into Portfolio; generated usernames; avatar, X handle and privacy; the Wallet is on Robinhood Chain. Keep a redirect from the old path. |
| Options *(new group)* | `options/how-options-work` | **NEW** | Explains the product: calls and puts on stock tokens, contract = 1 token, European cash settlement, the settlement rule, hours and cutoffs, house as counterparty. |
| Options | `options/the-options-page` | **NEW** | Stock page anatomy: chart, Quick trade outlooks, Chain columns, Stats, token panel, finding stocks. |
| Options | `options/buying-options` | **NEW** | Order panel, quick-buy popup, phone bar, sizing, limits ($100), price protection, error strings. |
| Options | `options/positions-and-settlement` | **NEW** | Sell back, the 15-minute cutoff, settlement timing, refunds ("void"), history labels. |
| Interval markets | `trading/how-windows-work` → **rename `interval/how-windows-work`** | REWRITE | 5 / 15 / 60 min only; stocks and crypto only; new price sources; 24/5 stock hours plus weekend perps; the fee; sports and outcomes removed. |
| Interval markets | `trading/live-markets` → **rename `interval/browsing-markets`** | REWRITE | Live and Trade merged into `/trade` "Browse markets" plus `/stocks` and `/crypto`; two card types; the featured panel; the market page anatomy moves here. |
| Interval markets | `trading/trade-markets` | **DELETE** | The Trade hub and its Daily / Weekly / Monthly markets no longer exist (redirect to `interval/browsing-markets`). |
| Interval markets | `trading/placing-orders` → **rename `interval/placing-orders`** | EDIT | Tap to buy; the trade dialog with the live chart; the confirmation card holds for the take box; fee; new error strings. |
| Interval markets | `interval/multi-buy` | **NEW** | A new feature with its own flow and limits (20 markets, independent buys). |
| Portfolio | `portfolio/overview` | REWRITE | New card ("Available cash" / "in positions"), header stats (Volume / Realized P&L / Rank), USDG, options included, public profile relationship. |
| Portfolio | `portfolio/deposit-withdraw` | REWRITE | "Deposit with" picker, Robinhood Chain USDG, Polygon routing, card on-ramp, USDG withdrawals. |
| Portfolio | `portfolio/positions-and-history` | EDIT | New row formats, "Buy" / "Sell" chips, Options filter and labels, the Orders tab. |
| Platform | `platform/finding-markets` → **rename `platform/navigating-updown`** | REWRITE | Home ("Explore"), the new nav and tab bar, the side panel, search palette, watchlist. |
| Platform | `platform/leaderboard` → **rename `platform/tournament`** | REWRITE | The two-week Tournament replaces the weekly competition; plus the rolling side-panel boards. |
| Platform | `platform/profiles-and-social` | **NEW** | Public profiles, follow, feed, takes / GIFs / likes / Hot takes, chart avatars, privacy toggles. |
| Help | `help/faq` | EDIT | See G. |
| Help | `help/support` | EDIT | Contact addresses (after H-3); "Send Message" needs sign-in; restricted-account copy; mis-sent deposits now involve Robinhood Chain as well as Polygon. |
| Help | `help/glossary` | EDIT | See F. |

`docs.json`: update the favicon (H-11), footer socials (H-3) and the group structure above. There are 20 pages in total.

---

## E. Page briefs

### Get started / introduction — "Introduction"
- **Purpose:** What updown is: two ways to trade short-term moves.
- **Status:** REWRITE.
- **User flows:** none (overview). Link out to Quickstart.
- **UI anatomy:**

  | | Options | Interval markets |
  |---|---|---|
  | What you buy | A call ("Above $X") or put ("Below $X") on a stock or ETF | Up or Down on the next window |
  | Expiry | Today's or the next session's 4:00 PM ET close | Every 5, 15 or 60 minutes |
  | Payout | Grows with the move past the strike; cash in USDG | $1.00 per winning contract |
  | Max loss | The premium | The price paid |
  | Where | Nav **"Options"** / phone tab **"Trade"** | Nav **"Interval"** (`/trade`) |

- **Key numbers / limits:** options up to $100 per buy; interval contracts 1¢–99¢, $1 payout; stocks: 10 interval subjects and 93 option underlyings; crypto: BTC, ETH, SOL, DOGE, HYPE.
- **Warnings / caveats:** do not mention sports, outcomes, daily / weekly / monthly markets, pUSD or Polymarket.
- **In-app copy to reuse:** "Same-day options on the biggest stocks, and short-term Up or Down markets on stocks and crypto. Pick a direction, tap to buy, and know your max loss before you're in." · tagline "Trade Movement on Everything".
- **Cross-links:** quickstart, options/how-options-work, interval/how-windows-work.
- **Evidence:** `(app)/(home)/page.tsx`; §A, §B.

### Get started / quickstart — "Quickstart"
- **Purpose:** From zero to a first trade.
- **Status:** REWRITE.
- **User flows:**
  1. Click **"Login"** (top right; on phones, ☰ → "Login") and sign in with email or Google. The account is created automatically, with no form.
  2. Click **"Cash"** in the nav (phones: Home's **"Deposit"**) → **"Deposit with"** → **"Crypto"**. If prompted, click **"Allow trading"** → "Trading enabled" → **"Continue"**.
  3. Pick **"Robinhood Chain"** (send **USDG**, at least $0.50) or **"Polygon"** (send **USDC**, at least $2; routed automatically). Or pick **"Card or bank"**.
  4. **Option:** open **"Options"** (phones: **"Trade"**), choose **"Up"**, pick a card → **"Add to order"** → **"Buy $X"**. On phones, tap "▲ Up" → pick a row and an amount → "▲ Buy above $X · $Y".
  5. **Interval:** open **"Interval"**, tap **"Up 51¢"** on a card → tap **"$5"** in the dialog.
  6. Optionally add a take ("Add your take" → "Post", or "Skip").
  7. Watch it on **Portfolio**. Options settle after the 4:00 PM ET close; intervals settle when their window ends.
- **Key numbers / limits:** deposit minimums $0.50 (USDG) / $2 (USDC, card); options buy cap $100; phones can buy options only on the 9 featured stocks.
- **Warnings / caveats:** wrong network or token may be lost (reuse the deposit warning once H-13 settles which copy is live).
- **Cross-links:** account-and-profile, portfolio/deposit-withdraw, options/buying-options, interval/placing-orders.
- **Evidence:** C / MO-1, AU-2, AU-3; B / E-6.

### Get started / account-and-profile — "Account & profile"
- **Purpose:** Signing in, your identity, your wallet, privacy.
- **Status:** REWRITE (renamed from `account-setup`).
- **User flows:**
  - **Sign in:** "Login" → Privy (email / Google) → auto-created account with a username like "BlueAngrySquare".
  - **Edit profile:** Portfolio → **"Edit profile"** → change "Username", "X account", "Icon" (Shuffle, color, "Background", shape, rotate by dragging the knob), "Visibility" → **"Save"** (toast "Profile updated").
  - **Wallet addresses:** Portfolio → wallet icon ("Wallet addresses").
  - **Sign out:** avatar menu → "Log out", or the sign-out icon on Portfolio.
- **UI anatomy:** avatar menu ("Profile", "Light mode" / "Dark mode", "Log out"); Visibility rows "Wallet" — "Balance, portfolio value and chart", "Positions" — "Open interval, options and market positions", "Activity" — "Trade history".
- **Key numbers / limits:** username 3–24 letters, numbers or underscores (unique, case-insensitive); X handle 1–15 characters.
- **Warnings / caveats:** usernames of accounts created before 2026-09-23 are lowercase. Email and wallet addresses are never shown to others. The Wallet dialog's Polygon labels are stale (H-20).
- **In-app copy to reuse:** "What other people see on your profile. Hidden sections are still visible to you." · "Optional. Shows an X icon next to your name that links to your account. Paste a handle or profile URL." · Allow-trading copy (C / AU-3).
- **Cross-links:** platform/profiles-and-social, portfolio/overview.
- **Evidence:** C / AU-2, AU-3, AU-4.

### Options / how-options-work — "How options work"
- **Purpose:** The contract, precisely.
- **Status:** NEW.
- **User flows:** none (concepts).
- **UI anatomy:**

  | Term | Meaning |
  |---|---|
  | Call ("Above $X", "Up") | Pays (settle − strike) × contracts if the token closes above the strike |
  | Put ("Below $X", "Down") | Pays (strike − settle) × contracts if it closes below |
  | Contract | One token (≈ 1 share × "Shares per token"); fractional to 0.01 |
  | Premium | The price you pay; your max loss |
  | Expiry | 4:00 PM ET (1:00 PM half-days); two expiries offered at a time |

- **Key numbers / limits:**
  - Strikes at ±1%, ±2%, ±3%.
  - Trading stops **15 minutes** before each expiry.
  - Settlement = the **average on-chain token price over the last 5 minutes** before the close, cross-checked within 0.5% of the real stock.
  - Payout via a settlement job that runs every 5 minutes.
  - **Refund if no price within 6 hours.**
  - Buy-only (selling or writing "coming soon").
  - Trades around the clock.
- **Warnings / caveats (must state):**
  - These are **updown's own cash-settled contracts**, not exchange-listed options: no broker, no OCC, no exercise or assignment.
  - **updown is the counterparty.**
  - The underlying is the **Robinhood Chain token price**, which can differ from the stock's price.
  - Thin tokens settle on the on-chain average alone.
  - Payout depends on the final price, not a fixed $1.
- **In-app copy to reuse:** token explainer (E-1); "Settles": "4:00 PM ET, on-chain price"; "Selling options is coming soon."
- **Cross-links:** the-options-page, positions-and-settlement, interval/how-windows-work (contrast table in E-1).
- **Evidence:** B / E-1, E-4, E-9.

### Options / the-options-page — "The options page"
- **Purpose:** Reading a stock page and finding stocks.
- **Status:** NEW.
- **User flows:**
  - **Find a stock:** Ctrl/⌘+K → type → pick the "Options" row; or the side panel's "Stocks" tab (star to watch); or Home.
  - **Pick an expiry:** the date dropdown in the heading or the chain.
  - **Quick trade:** choose an outlook → pick one of the three cards.
  - **Chain:** toggle **"Chain"** → click a "Buy" price to load it into the order panel.
- **UI anatomy:** header tiles "Volume" / "Liquidity"; chart intervals "1m" "5m" "15m" "1h" "D", "Candles" / "Line", phase pill; outlooks table (E-2); chain columns and tooltips (E-6); "About", "The {SYM} token", "Stats" rows (E-2).
- **Key numbers / limits:** 93 stocks and ETFs; 6 strikes; 2 expiries.
- **Warnings / caveats:** the chain and Quick trade are desktop / tablet only. Phones see the chart, positions and stats plus a quick-buy bar on 9 featured stocks. "thin" tokens.
- **In-app copy to reuse:** "What does SPY … do by …", outlook blurbs, chain tooltips, "On-chain price · scroll to zoom, drag to pan".
- **Cross-links:** buying-options, platform/navigating-updown.
- **Evidence:** B / E-2, E-3, E-6.

### Options / buying-options — "Buying options"
- **Purpose:** Placing an order.
- **Status:** NEW.
- **User flows:**
  - **Desktop:** select a trade → "Amount" as **"Contracts"** or **"Dollars"** → review "Cost", "Max loss", "Max profit", "Breakeven", "% chance ITM", payoff → **"Buy $X"** → "Bought for $X" → take box.
  - **Popup and phones:** "▲ Up" / "▼ Down" / "Up or Down" → pick a row → "$1" / "$5" / "$10" or type an amount → "▲ Buy above $X · $Y" → "Bought for $X — settles at the close".
- **UI anatomy:** order panel and popup fields (E-6).
- **Key numbers / limits:** **$100 premium cap per buy**; minimum 0.01 contracts (popup: $1); max 2 legs; market orders only at updown's ask; refused if the price rose more than 2% since you saw it; no fees.
- **Warnings / caveats:** buying power = the USDG in your wallet ("Deposit to buy"); a failed buy takes nothing; selling / Neutral is coming soon; phones buy only the 9 featured stocks.
- **In-app copy to reuse:** every error string in E-6.
- **Cross-links:** portfolio/deposit-withdraw, positions-and-settlement.
- **Evidence:** B / E-5, E-6.

### Options / positions-and-settlement — "Positions & settlement"
- **Purpose:** What happens after you buy.
- **Status:** NEW.
- **User flows:**
  - **Sell back:** stock page → Positions → **"Sell $X"** (no confirm) → "Sold for $X — paid to your wallet." If it's on the other expiry, switch the expiry dropdown first.
  - **Hold to expiry:** payout lands in your wallet after the close.
- **UI anatomy:** Positions columns "Contract" · "Qty" · "Paid" · "Now" · "P&L"; Portfolio row (E-7); history labels "Buy" / "Sell" / "Won" / "Lost" / "Refund".
- **Key numbers / limits:** sell at updown's bid; the last 15 minutes before an expiry are sell-locked; settlement average over 5 minutes; refund after 6 hours without a price.
- **Warnings / caveats:** "Now" is what updown would pay right now (the bid), not the midpoint. Options don't appear on public profiles or the Tournament. There are no expiry notifications.
- **In-app copy to reuse:** "Expired — it settles at the close price." · "Not quoting right now — it will settle at the close." · "… · settled in the money" / "· expired worthless" / "· no settlement price, premium refunded".
- **Cross-links:** how-options-work, portfolio/positions-and-history.
- **Evidence:** B / E-4, E-7.

### Interval markets / how-windows-work — "How windows work"
- **Purpose:** The Up/Down mechanic.
- **Status:** REWRITE.
- **User flows:** none.
- **UI anatomy:** window header (C / IM-4); settled-window page (IM-6).
- **Key numbers / limits:**
  - 5 / 15 / 60-minute windows on the UTC grid.
  - 10 stocks + 5 cryptos (IM-11).
  - Up wins strictly above the open; unchanged = Down.
  - 1¢–99¢, $1 payout.
  - Fee 0.07 × C × p(1−p) (FC-1).
  - 20 contracts per side per window from the house.
  - Settlement job every minute.
- **Stock hours:** 24/5 (Sun 8 PM → Fri 8 PM ET). 8 stocks trade weekends on a perp price; PLTR and SPY close. Crypto 24/7.
- **Warnings / caveats:** the in-app "Settlement rules" paragraph about "the Robinhood tape" is inaccurate. Write the price sources from H-8's confirmed wording. Remove all sports, outcomes, game-over, daily / weekly / monthly content.
- **In-app copy to reuse:** "If the price is unchanged, it settles as Down. Up needs a real move." Settlement-rules paragraphs 1–3 (IM-7).
- **Cross-links:** browsing-markets, placing-orders, options/how-options-work.
- **Evidence:** C / IM-*.

### Interval markets / browsing-markets — "Browsing interval markets"
- **Purpose:** The hub, category pages, cards and market page.
- **Status:** REWRITE (merges the old live-markets and trade-markets pages).
- **User flows:** nav **"Interval"** → pick a category ("All Categories" / "Stocks" / "Crypto") and an interval ("All Intervals" / "5 min" / "15 min" / "60 min") → tap a card's price to trade, or the card body to open its market page. Home section "See all" → `/stocks` or `/crypto` → interval tabs.
- **UI anatomy:**
  - compact card vs live card (LH-3);
  - featured panel (LH-2);
  - market page: header, "Change expiry" menu ("5 Minutes" / "15 Minutes" / "60 Minutes"), chart buttons (MP-1), "Past" (MP-2), outcome row, order book (MP-5), "Live Positions" (MP-3), "Settlement rules", **"Similar markets"** (up to 3), buyer avatars on the chart.
- **Key numbers / limits:** registry refresh 60 s; stock prices every second; crypto every trade.
- **Warnings / caveats:** stock markets leave the hub while closed.
- **In-app copy to reuse:** "Browse markets"; empty states (LH-4).
- **Cross-links:** placing-orders, multi-buy, platform/navigating-updown.
- **Evidence:** C / LH-*, MP-*.

### Interval markets / placing-orders — edit list
- Replace hold-to-buy with **tap** (TK-2), including "Pick Up or Down" → "Tap an amount to buy".
- Phone flow: the docked "Buy Up / Buy Down" pills open the **trade dialog** with the live chart (TK-9). Remove the Quick / Market / Limit bottom sheet.
- The confirmation card now **stays open with the take box**; remove "auto-dismiss ~2 s" (TK-5).
- Add the fee (FC-1); remove "no fees".
- Replace the error-string list with TK-8. Keep the TK-7 strings.
- "Sell" in Live Positions is a **tap** (TK-10).
- Remove every "game over" or team-side mention.
- Mention the Multi-buy page.

### Interval markets / multi-buy — "Multi-buy"
- **Purpose:** Placing several interval buys at once.
- **Status:** NEW.
- **User flows:**
  1. On `/trade`, `/stocks` or `/crypto`, turn on **"Multi-buy"**.
  2. Tap outcomes; each becomes a leg.
  3. Set "$ per market" ("$1" / "$5" / "$10" or type an amount; default $5).
  4. Review "Total cost", "Pays if every pick wins", "Cash".
  5. **"Place N orders · $X"**.
  6. Refused legs stay in the slip to retry.
  7. One take for the batch.
- **UI anatomy:**
  - Slip heading "Multi-buy {n}", "Clear all".
  - Leg rows "Up 51¢ · 15 min", "to win $x".
  - Phone bar "{n} picks · $x" → "Review".
- **Key numbers / limits:** up to **20 markets** ("An order holds up to 20 markets — place this one first."); one leg per market; each leg is an **independent buy, not a parlay**.
- **Warnings / caveats:** the whole order is refused if it costs more than your cash ("This order costs more than your cash — lower the amounts or deposit."). A partial leg: "Only $x is on offer right now — the rest won't be spent." If the result is unknown: "We couldn't confirm this order. Check your positions before placing it again."
- **In-app copy to reuse:** "Tap any outcome to add it to your order." / "Pick as many markets as you like — each one is its own buy."
- **Cross-links:** placing-orders.
- **Evidence:** `components/multi-buy/*`; `apps/web/lib/interval/multi-buy.ts`; `packages/api/src/lib/interval/multi-buy.ts`.

### Portfolio / overview — "Portfolio"
- **Purpose:** Your balance, positions, stats; also your public profile.
- **Status:** REWRITE.
- **User flows:** nav **"Portfolio"** (or avatar → "Profile").
- **UI anatomy (top to bottom):**
  1. Header: avatar, name, X link, "@name"; stats **"Volume"**, **"Realized P&L"**, **"Rank"** (sub "tournament", "#N" or "—"); "N Following", "N Followers"; "Edit profile", wallet icon, sign-out icon; meta "N trades", "Joined Sep 2026", your email.
  2. Value card: total, "+$X (+Y%) 7d", "1D" "1W" "1M" "1Y" "ALL", **"Available cash"**, "$X in positions", **"Withdraw"**, **"Deposit"**.
  3. **"Your positions"**, "Open" / "Orders".
  4. Your top take (if any).
  5. Activity table.
- **Key numbers / limits:** balance model in Q-6 below.
- **Warnings / caveats:** the nav "Cash" (raw wallet) can exceed "Available cash" (after interval reservations). Others see your page at `/u/<name>` minus your email and options, subject to your Visibility settings.
- **In-app copy to reuse:** tooltips "Cash = available to trade · click to deposit", "Portfolio = cash + open positions"; signed out: "Sign in to view your profile" / "Your cash, positions and history live here."
- **Cross-links:** deposit-withdraw, positions-and-history, platform/profiles-and-social, platform/tournament.
- **Evidence:** C / PF-*.

**Q-6 — the full balance model (both products):**

| Figure | Where | Formula | Locks |
|---|---|---|---|
| "Cash" | Nav | USDG in your Robinhood Chain wallet | — |
| "Portfolio" | Nav | Cash + open interval positions (at bid) + open options (at bid) | — |
| "Available cash" | Portfolio card | Cash − cost reserved by open interval positions (including closed-but-unsettled windows and resting buy orders) | Reserved cash can't buy intervals or be withdrawn |
| "$X in positions" | Portfolio card | Interval at midpoint (at cost while settling) + options at bid (at cost if unquoted) + legacy | — |
| Total (card headline) | Portfolio card | Available cash + in positions | — |
| "Available" / "MAX" | Withdraw dialog | Raw USDG. The withdrawal is refused beyond Cash − interval reservations. | See MO-4 |
| "Wallet $X USDG" | Options ticket | Raw USDG | — |
| "Buying power" | Market page | Interval buying power (Cash − reservations) | — |

- **Interval buys** move no money when placed; the cost is reserved. At the window's close, a loss is collected from the wallet and a win is paid in.
- **Option buys** take the premium from the wallet immediately and reserve nothing.

### Portfolio / deposit-withdraw — "Deposit & withdraw"
- **Purpose:** Moving money in and out.
- **Status:** REWRITE.
- **User flows:**
  - **Crypto deposit:** nav "Cash" → "Deposit with" → "Crypto" → (Allow trading if prompted) → network "Robinhood Chain" or "Polygon" → copy the address or scan the QR → send.
  - **Card:** "Card or bank" → Stripe checkout → "Waiting for your USDC" → "Adding to your balance…".
  - **Withdraw:** Portfolio → "Withdraw" → address + amount / "MAX" → "Withdraw" → "Withdrawal confirmed" → "View transaction →".
- **UI anatomy:** MO-1 to MO-3.
- **Key numbers / limits:** minimums: USDG $0.50, Polygon USDC $2, card $2, withdrawal $2. Supported: **USDG on Robinhood Chain**, **USDC on Polygon** (converted). Others "Soon".
- **Warnings / caveats:** withdrawals go **only to Robinhood Chain addresses, as USDG**, and are irreversible. Cash reserved by open interval windows can't be withdrawn. Restricted accounts can still withdraw. The Polygon / card route has a small bridge cut.
- **In-app copy to reuse:** "Gas is sponsored. Withdrawals are irreversible — double-check the address." · "We route Polygon USDC into your Robinhood Chain balance automatically." · card-timing copy (MO-5).
- **Cross-links:** portfolio/overview, help/support (mis-sent funds).
- **Evidence:** C / MO-*.

### Portfolio / positions-and-history — edit list
- Rewrite the interval position row to the single-line format (PH-1). Remove the "Contracts / Entry / Now / Pays" cells.
- Add options rows and the rule that you can't sell from Portfolio (E-7).
- History chips: "Buy" / "Sell" (not Bought / Sold); add the options labels including "Refund"; filters "All activity" / "Interval" / "Options" / "Spot"; columns (PF-5).
- Withdrawals: only old Polygon withdrawals appear; never on public profiles (PH-2).
- "Settles at close" chip for hidden or retired markets (sports and outcomes leftovers).
- The "Live Positions" panel's Sell is a tap.
- Keep PH-3 / PH-4.

### Platform / navigating-updown — "Navigating updown"
- **Purpose:** Home, nav, search, side panel.
- **Status:** REWRITE (renamed from `finding-markets`).
- **User flows:** NV-1 / NV-3 flows; Ctrl/⌘+K search; side panel tab switching; starring stocks.
- **UI anatomy:**
  - Nav (NV-1), phone tabs and drawer (NV-3), side panel (NV-1), search palette (NV-7).
  - **Home "Explore"**, top to bottom:
    1. (phones) balance + "$X cash to trade" + "Deposit"
    2. Patterns ticker ("Up 6 in a row"; stock and crypto runs of 4+ same outcomes or alternations)
    3. Rotating featured panel (options · interval · options · interval · options; dots)
    4. Four "Up next" clocks ("5 min · Next window in", "15 min …", "60 min …", "Options 0DTE · Expire in")
    5. "Options" row with "See all"
    6. "Stocks" and "Crypto" sections with "See all"
    7. Right rail: "Your positions" (or "How it works" + "Sign in to trade"), "Options" movers, "Trending", "Most volatile", "New"
- **Key numbers / limits:** featured panel rotates every 9 s; watchlist is stored per browser.
- **Warnings / caveats:** Home's signed-out "How it works" copy still mentions "a live game, a count" and "the day". Don't quote it (H-20).
- **In-app copy to reuse:** "Search stocks, crypto and options…"; Patterns labels.
- **Cross-links:** options/the-options-page, interval/browsing-markets.
- **Evidence:** C / NV-*; `(app)/(home)/home-view.tsx`.

### Platform / tournament — "Tournament"
- **Purpose:** The current competition and the rolling boards.
- **Status:** REWRITE (renamed from `leaderboard`).
- **User flows:** nav **"Tournament"** → **"Rules & Prizes"**; side panel **"Leaderboard"** → "24H" / "7D" / "30D" / "ALL" / "TOURNAMENT" → "Full board →".
- **UI anatomy:** LB-2, LB-5; banner (NV-5); "Rank" stat on profiles.
- **Key numbers / limits:** **Thu Sep 24 → Wed Oct 7 2026 ET**; $1,000 pool, $600 / $300 / $100; ranked by realized interval P&L; ties broken by volume; board refresh 10 s (side panel 15 s); top 50 shown plus your pinned row.
- **Warnings / caveats:** **options trades don't count.** Only interval windows that close inside the event count. Prizes are paid manually after the end (asset: H-10). House bots can appear on the board.
- **In-app copy to reuse:** LB-4 rules verbatim.
- **Cross-links:** interval/how-windows-work, portfolio/overview.
- **Evidence:** C / LB-*.

### Platform / profiles-and-social — "Profiles & social"
- **Purpose:** Public profiles, following, the feed, takes.
- **Status:** NEW.
- **User flows:**
  - **Visit a profile:** click any avatar or name → `/u/<name>` → **"Follow"** (hover **"Unfollow"**).
  - **Post a take:** after a trade → "Add your take" → type (≤ 280) or **"GIF"** → pick → **"Post"** (or "Skip").
  - **Like a take:** the heart under it.
  - **Feed:** desktop side panel "Feed" tab; phones "Feed" tab → filters "All" / "Options" / "Interval".
- **UI anatomy:**
  - Profile: header stats, "Positions", top take, activity; hidden sections show "Wallet hidden" / "Positions hidden" / "Activity hidden" with "@name keeps this private."
  - Feed row: avatar, name, age, trade card, **"Take"** badge (blue) or **"🔥 Hot take"** (a long shot: interval bought under 20¢), heart + count.
  - Chart avatars: tooltip "{name}", "$5.00 on Up", the take.
- **Key numbers / limits:** takes up to 280 characters or a GIF (Powered by KLIPY); 20 posts per minute; feed polls every 5 s; chart pins show the last 24 h on options and the current window on intervals.
- **Warnings / caveats:** takes can't be edited later. Public profiles never show email, wallet addresses, withdrawals or options positions.
- **In-app copy to reuse:** take box strings and errors (E-12, MP "takes"); "No one by that name" / "Check the spelling and try again."; "@name holds nothing right now." / "@name hasn't traded yet."
- **Cross-links:** account-and-profile, portfolio/overview.
- **Evidence:** `(app)/u/[name]/profile-view.tsx`; `components/feed/TradeFeed.tsx`; `components/takes/*`; `apps/web/lib/feed/takes.ts`.

### Help / support — edit list
- Contact details: use the result of H-3. The Contact page shows hello@updown.fast / @updown_markets.
- "Send Message" requires sign-in.
- Restricted accounts: FC-3 copy (confirmed).
- Mis-sent deposits: only USDG on Robinhood Chain and USDC on Polygon are supported; other networks and tokens may be lost.
- Status: the bottom status bar's "Online" / "Offline" (desktop only).

---

## F. Glossary changes

**Add**
- **Options** — updown's own calls and puts on a stock's Robinhood Chain token, cash-settled in USDG at the close; updown is the counterparty.
- **Call** ("Above $X") — pays the amount the token closes above the strike, per contract.
- **Put** ("Below $X") — pays the amount the token closes below the strike, per contract.
- **Strike** — the price in a call or put; six per expiry, at ±1/2/3% of the price.
- **Expiry** — the 4:00 PM ET close (1:00 PM on half-days) the option settles at; two are offered at a time.
- **Premium** — what you pay for an option; the most you can lose.
- **Option contract** — one token of the underlying (≈ 1 share × "Shares per token"); can be fractional.
- **Break-even** — the price where a bought option has paid for itself.
- **% chance** — the model's chance the option finishes past its strike.
- **Settlement price (options)** — the average on-chain token price over the 5 minutes before the close.
- **Void / Refund** — an option with no final price 6 h after expiry; the premium comes back.
- **Stock token** — a Robinhood Chain token backed by shares; dividends reinvested.
- **Shares per token** — how many shares one token represents; creeps up over time.
- **Thin** — a token priced only by shallow pools; it settles on the on-chain average alone.
- **Outlook** — Quick trade's "Up" / "Down" / "Volatile" / "Neutral" choice.
- **Straddle / Strangle** — buying a call and a put together ("Volatile", "Up or Down").
- **USDG** — the dollar token your cash is held in, on Robinhood Chain.
- **Robinhood Chain** — the network your wallet and cash live on.
- **Cash** — USDG in your wallet.
- **Available cash** — Cash minus what open interval positions reserve.
- **Allow trading** — the one-time approval that lets updown move funds for the trades you place.
- **Interval** — the Up/Down window product (nav "Interval").
- **Multi-buy** — placing several interval buys in one order.
- **Take / Hot take** — a note or GIF on your trade; "Hot" if the bet was a long shot.
- **Feed** — the live list of everyone's trades.
- **Tournament** — the two-week prize competition.
- **Rank** — your Tournament position.
- **Patterns** — Home's strip of markets on a streak.
- **Up next** — Home's countdown clocks.
- **Explore** — Home.
- **Watchlist** — starred stocks, kept in your browser.
- **Taker fee (interval)** — 0.07 × contracts × p × (1 − p), built into the price.

**Change**
- **Contract (interval)** — keep, and add: "not the same as an option contract".
- **Cadence / Window** — 5, 15 or 60 minutes only.
- **Available**, **Balance**, **Portfolio** — per Q-6.
- **Leaderboard** — now the Tournament and the rolling boards.
- **Deposit / Withdraw** — USDG / Robinhood Chain.
- **Stock hours** — 24/5, with weekend perps.
- **Price source** — Pyth (stocks) / Coinbase (crypto) / the on-chain token (options).
- **Fees** — see FC-1.

**Drop**
- pUSD
- Polymarket (as the cash venue)
- Signing wallet / Deposit wallet (Polygon)
- Daily / Weekly / Monthly
- End-of-Day / End-of-Week / End-of-Month
- Outcomes
- Sports / Game over / Series ended / Flat
- Mentions
- Live hub / Trade hub
- Weekly Trading Competition / Resets in
- Referral (while hidden)
- Hold to buy

---

## G. FAQ changes

**Add**
- *Are these real stock options?* — No. They are updown's own calls and puts on the stock's Robinhood Chain token, cash-settled in USDG. They are not exchange-listed, there's no broker, and updown is the other side of every trade. (E-1)
- *When do options expire and settle?* — At the 4:00 PM ET close (1:00 PM on half-days), today or the next session. The payout lands in your wallet within minutes after the close. (E-4, E-9)
- *What price do options settle at?* — The average on-chain token price over the last 5 minutes before the close, checked against the real stock. (E-1)
- *Can I sell an option before expiry?* — Yes, at updown's current bid, until 15 minutes before its expiry. (E-7)
- *Can I sell or write options?* — Not yet ("coming soon"). (E-4)
- *What's the most I can spend on one option order?* — $100 of premium per buy. (E-6)
- *Why can't I buy options for this stock on my phone?* — Phone buying is available on the nine featured stocks. (E-6; see H-14)
- *What happens if there's no settlement price?* — After 6 hours, the premium is refunded ("Refund"). (E-4)
- *Do options count for the Tournament?* — No, only interval markets. (E-12)
- *What does a trade cost?* — Intervals: a small taker fee built into the price. Options: no fee; the spread is updown's margin. (FC-1, E-5)
- *Which networks can I deposit from?* — USDG on Robinhood Chain, or USDC on Polygon (converted); card or bank via "Card or bank". (MO-1, MO-5)
- *Can I pay by card?* — Yes, "Card or bank". (MO-5)
- *Who can see my profile?* — Anyone. Hide "Wallet", "Positions" or "Activity" in Edit profile. Email and wallet are never shown. (AU-4)
- *What is a take?* — See profiles-and-social.
- *Which stocks trade on weekends?* — See IM-8.

**Change**
- Deposit minimum / assets / pUSD answers → USDG / Robinhood Chain (MO-*).
- "No fees" → FC-1.
- Market hours → IM-8 (24/5 plus weekend perps).
- Leaderboard answers → Tournament (LB-*).
- Account setup → no form (AU-2).
- Withdrawal answers → USDG on Robinhood Chain (MO-3).
- "Where do I find markets" → nav, search, side panel (NV-*).

**Drop**
- Every sports / game-over / outcomes / daily-weekly-monthly question.
- The onboarding form question.
- Referral payout questions (while hidden).
- The Ctrl+K legacy search warning.

---

## H. Needs team confirmation

**Brand, legal, domains (carried from Q-8)**
- **H-1 · Production money model.** Robinhood Chain / USDG ("Model A") is selected by the env var `SETTLEMENT_CHAIN=robinhood`. The code defaults to Polygon / pUSD, and doc 38 lists the Vercel switch as "Not done yet". Confirm production runs Model A before the docs say USDG everywhere. (The options product only works on Robinhood Chain regardless.)
- **H-2 · Interval fee live?** The 7% × p(1−p) taker fee is on unless `INTERVAL_FEES=off`. Confirm the production setting and the official fee statement ("no fees" is no longer true by default).
- **H-3 · Contact identity is inconsistent in the app.** hello@updown.fast vs hello@thetalabs.gg; X @updown_markets vs @thetalabsgg. Pick the official ones for the docs footer and the Support page.
- **H-4 · Domains.** The app host is unstated (updown.fast appears in code as the public site; the metadata base is thetalabs.gg; the docs link is docs.thetalabs.gg). Confirm the official app, marketing and docs domains.
- **H-5 · Custody wording.** Cash sits in the user's own Privy wallet. "Allow trading" grants updown's server delegated signing for trades, settlements and routing. The copy says "Your keys stay yours" / "Revoke anytime", **but there is no revoke control in the app.** Approve the custody description.
- **H-6 · Geo / eligibility.** A 56-country list including the US exists but blocks nothing (and detection may be broken in production). A code comment says stock tokens are "NOT offered to US / UK / CA / CH persons", which is unenforced. Given real-money options on stock tokens, confirm the official eligibility statement and the region list.
- **H-7 · Terms / privacy / risk disclosure.** None exist. Options probably need a risk disclosure (house counterparty, token vs stock price, unhedged book, settlement rule). Confirm URLs, or whether the docs should carry the risk text.
- **H-8 · Data-vendor wording.** The in-app settlement copy cites "the Robinhood tape… follows US market hours". The code uses Pyth (stocks, 24/5), Hyperliquid perps (weekend stocks), Coinbase (crypto), and on-chain pools + Massive/Polygon.io stock bars + Cboe delayed quotes (options). Confirm what the docs may name.
- **H-9 · Company / legal name.** None appears anywhere in the app (no ©, Inc or LLC). Theta Labs strings remain (LO-3). Is Theta Labs the company behind updown, and should the docs say so?
- **H-10 · Tournament prizes.** Asset (USDG?), timing and eligibility (bots and admins can rank publicly) of the manual payout. Also: the "Trade any market" copy implies options count, and they don't.
- **H-11 · Favicon and colors.** The app icon is the updown mark (green `#00bf63` up / red `#ff3131` down). The docs' `#3ecf8e` / `#7ce3ad` / `#0b0b11` match the app's UI profit and background tokens. `docs.json`'s favicon is still Theta Labs. Confirm the brand palette for the docs.
- **H-12 · Nav placement.** The Tournament **is** in the nav. Referrals are **hidden**. Confirm whether referrals should be documented at all.

**Product decisions the docs depend on**
- **H-13 · Which deposit dialog is canonical.** The nav's "Cash" opens the new "Deposit with" flow. The Portfolio page's "Deposit" button still opens the old "Wallet" modal with pUSD / Polygon copy. Document the new one only?
- **H-14 · Phone options buying.** Phones can buy only the 9 featured stocks, and there is no chain on phones. Intended, or a gap to fix before documenting?
- **H-15 · "0DTE" naming.** Home says "Options 0DTE · Expire in" and "expires at today’s close", but the product also offers next-session expiries and trades overnight. Pick the docs' term (same-day / next-day options?).
- **H-16 · Scope of "coming soon".** Should the docs mention selling / Neutral as coming?
- **H-17 · Weekend stock trading on perps.** Confirm it is intended to be public-facing (IM-8). The in-app copy exists.
- **H-18 · Order book on intervals.** A limit-order book ("Limit" tab, "Open orders", "Cancel") is built and switched per market in the DB. Whether any market uses it in production is unknown. If none, the docs keep Limit as "disabled".

**Engineering issues found during the audit** (not docs content, but they affect the accuracy of what the docs say)
- **H-19 · Money and risk.**
  - Option buys don't check interval-reserved cash or take the per-user money lock, so an options buy can spend cash already committed to open interval windows.
  - Maintenance mode doesn't pause options buys, sells or settlement.
  - The options book is unhedged.
  - In the last 15 minutes before an expiry, the server values (and would pay a direct API sell of) today's options at the *next* session's bid.
  - The settlement cross-check uses a static token multiplier that drifts with dividends. Solid-token settlements could start failing the 0.5% check and fall through to 6-hour refunds.
- **H-20 · Stale or wrong copy in the app.**
  - The nav "Portfolio" double-counts open interval positions under Model A.
  - The value chart's history excludes open interval and option value.
  - USDG withdrawals are not listed in activity.
  - "Insufficient balance: need X pUSD" and the multi-buy's pUSD message.
  - The Wallet dialog ("pUSD balance", Polygon addresses).
  - The Feed's "Every practice-option buy lands here."
  - Home's "How it works" (games, counts, "the day").
  - The hub's "24 hr" interval.
  - The static "4:00 PM ET" / "today’s close" labels (wrong on half-days and next-day expiries).
  - Settlement-rules copy (H-8).
  - Theta Labs titles and link previews (LO-3).
  - Home's featured options panel shows strike distances from the retired practice pricer, which can differ from the popup it opens.
  - Home's "N open" badges on option cards count old practice positions, not real ones.

# Docs Rebuild Response — generated 2026-09-15 from github.com/shah625/thetalabs / main @ 5248006

> **How to read this.** Every fact below was taken from the code on `main` at commit `5248006` ("landing page"). Paths are relative to the repo root. Quoted strings are verbatim in-app copy unless marked *paraphrase*. `UNKNOWN` means the codebase does not settle the question; Section H collects everything the team must decide.
>
> **The pivot is partial and the writer needs to know that up front.** The brand, landing page, nav, and the two launch surfaces (**Live** and **Trade**) are fully Updown. But the money rails (Privy login, Polygon embedded wallet, USDC → pUSD deposits, the **Enable trading** approval, the **Wallet** modal, the Withdraw flow) are carried over from Theta Labs almost unchanged, and a large tail of Theta Labs-era pages is still reachable by direct URL (see F-15). Several user-facing strings still say "Theta Labs" or point at `thetalabs.gg` (see F-1). The domain in code is still `thetalabs.gg` / `app.thetalabs.gg`.

---

## A. Product summary

**In the product's own voice** (built from `apps/web/app/layout.tsx` metadata, `apps/web/app/app/options/options-landing.tsx`, and `apps/web/app/app/live/page.tsx`):

> updown — Trade Movement on Everything. Trade short-term directional movement on stocks, crypto, sports, outcomes, and more. Pick Up or Down for the window, hold to buy, and know your max loss before you're in.

**Plain-language version for the Introduction page.** updown runs recurring "up or down" markets on live numbers: a stock price, a crypto price, a team's live win odds, or a prediction-market outcome's chance. Every market is cut into windows. A window opens, stamps the number at that instant as the level **to beat**, and closes a fixed time later (15 minutes on **Live**; end of day, week, or month on **Trade**). You buy **Up** or **Down** contracts against the house's live quote, priced in cents between 1¢ and 99¢. If the number closes the window above where it opened, every Up contract pays **$1.00**; otherwise every Down contract pays $1.00 (a window that closes exactly unchanged settles **Down**). The most you can lose is what you paid. You can sell back to the house at any time before the window closes. Funds are real money: USDC deposited on Polygon, held as pUSD, with winnings paid to your wallet automatically when the window settles.

**Key concepts a new user must learn:**

| Term | One-line meaning |
|---|---|
| **Market** | One tracked number (e.g. "Apple (AAPL)", "Bitcoin (BTC)", "Rangers vs. Mariners — live win odds") that runs a series of windows. |
| **Window** | One round of a market: opens on a fixed grid, stamps the opening value, and closes after the cadence (15 min / daily / weekly / monthly). |
| **To beat** | The market's value at the instant the window opened. Up wins only if the close is strictly above it. |
| **Up / Down** | The two sides of the window's $1 binary. Each contract pays $1.00 if its side wins and $0 if it loses. An unchanged close settles Down. |
| **Contract** | The unit you trade. Priced 1¢–99¢; the price is the market's implied chance that side wins. "Pays $X if Up" = number of contracts held. |
| **Quick Buy** | Hold-to-buy boxes for $1 / $5 / $10 — press and hold ~0.7 s to fill at the live quote. |
| **Live vs. Trade** | **Live** = 15-minute windows on stocks, crypto and in-play sports. **Trade** = daily / weekly / monthly windows on stocks, crypto and prediction-market outcomes. |
| **Cash vs. Portfolio** | **Cash** = pUSD free to trade or withdraw. **Portfolio** = cash plus the live value of open positions. |
| **pUSD** | Your trading balance. USDC deposited on Polygon converts to pUSD automatically; withdrawals convert back to native USDC. |
| **Settlement** | Automatic. When a window closes, winners are paid $1 per contract and losers' cost is collected, on-chain, within about 5 minutes. |

---

## B. Fundamentals

### F-1 — Brand, naming, links

- **Product name and casing:** `updown`, always lowercase, everywhere it appears in the UI: the landing wordmark (`options-landing.tsx` line 48), the app nav brand (`components/app-shell/Navbar.tsx` line 293: `updown`, styled `lowercase`), the Live hub H1 (`live/interval-view.tsx` line 184), the `<title>` "updown | Trade Movement on Everything" and OpenGraph `siteName: "updown"` (`apps/web/app/layout.tsx` lines 29–48), the Live page title "updown — Live Markets" (`live/page.tsx`). The logo `alt` is `"updown"`. No "UpDown" / "Up/Down" spellings exist in user-facing code.
- **Legal entity:** none appears anywhere. The landing footer shows only the mark + `updown` and the links X / Docs / Discord (`apps/web/app/page.tsx` lines 75–98). No terms, privacy, or company-name footer. → **H**.
- **Tagline:** "Trade Movement on Everything" (`options-landing.tsx` line 52; also the `<title>`).
- **One-sentence description (meta description, verbatim):** "Trade short-term directional movement on stocks, crypto, sports, outcomes, and more. Pick Up or Down for the window, hold to buy, and know your max loss before you're in." (`layout.tsx` lines 30–31). Landing body copy: "Trade Short-Term Directional Movement on Stocks, Crypto, Sports, Outcomes, and More." (line 56).
- **Is "Theta Labs" still user-facing? Yes, as leftovers**, not as a deliberate parent brand:
  - Login page logo label and text: `alt="Theta Labs"` / "Theta Labs" (`apps/web/app/login/page.tsx` lines 161–162).
  - Enable-trading explainer: "A one-time approval so Theta Labs can convert your deposits and place trades instantly — you won't sign every transaction." (`components/trading/AllowTrading.tsx` line 82).
  - Browser tab titles: "Trade | Theta Labs" (`trade/page.tsx`), "<market> — Interval | Theta Labs" (`live/[slug]/page.tsx`), "<market> — Trade | Theta Labs" (`trade/[slug]/page.tsx`), "<market> — past window | Theta Labs" (`live/[slug]/[window]/page.tsx`).
  - Social share (OpenGraph) fallback image for every `/app/*` page: eyebrow "thetalabs.gg", title "Trade Prediction Markets", subtitle "Options, spot markets, and portfolio — all in one terminal." (`apps/web/app/app/opengraph-image.tsx`).
  - Portfolio share card watermark "thetalabs.gg" (`portfolio/share-card.tsx`) — options-only surface.
  - Support email, X handle, Discord, docs URL (below).
  - `metadataBase` is `https://thetalabs.gg` (`layout.tsx` line 32); the app host is `app.thetalabs.gg` (`apps/web/middleware.ts` line 3); referral links are built from whatever origin serves the page (`referrals/page.tsx` line 38).
  → Whether "Theta Labs" survives as the company name behind updown is **H**.
- **URLs found in code:**
  - Marketing/landing: `https://thetalabs.gg` (root `/` renders the landing).
  - App: `https://app.thetalabs.gg` (middleware rewrites `/*` → `/app/*` on that host) — `/app/...` paths also work on the main host.
  - Docs: `https://docs.thetalabs.gg/introduction` (landing footer "Docs" and the app status bar "Docs").
  - Terms of service / privacy / risk / status page: **none in code** → **H**.
  - GitHub: `https://github.com/shah625/thetalabs` (private/public status UNKNOWN → **H**).
- **Support / community (all Theta Labs-branded):**
  - Email `hello@thetalabs.gg` (`app/app/contact/page.tsx` line 123; `BannedNotice.tsx` line 68).
  - X `@thetalabsgg` → `https://x.com/thetalabsgg` (landing nav + footer, status bar, contact page).
  - Discord `https://discord.com/invite/d9gfrYWSAu` (landing, status bar, banned notice); contact page uses `https://discord.gg/d9gfrYWSAu`.
  - In-app: **Contact us** link in the desktop status bar → `/app/contact` (form, see F-14). Telegram: none.
- **Brand assets:**
  - Logo mark: `apps/web/public/logo-icon.svg` (served at `/logo-icon.svg`). Favicon: `apps/web/app/icon.svg` (Next file convention; content-hashed URL). Apple icon: `apps/web/app/apple-icon.png`. A legacy `/logo-full.png` is referenced only by the archived options "How it works" modal.
  - Wordmark treatment on the landing: animated gradient `#00bf63 → #7ce3ad → #ffffff → #ff9c9c → #ff3131` (green → white → red) (`options-landing.tsx` line 47).
  - Colors (from `DESIGN.md` + `CLAUDE.md`): background `#0b0b11`, surface `#151520`, profit/Up green `#3ecf8e`, loss/Down red `#f87171`, muted text `#a5adbe`, dim text `#939bad`, input border `#1e2433`. Competition/purple accent `#8b8bf5` (light `#a5a5ff`). Warning amber `#fbbf24`. Privy modal accent `#00ff88` (`PrivyClientProvider.tsx`). Light mode exists (see F-13); its base is `#f5f5f7`, surface `#fff`, text `#0f172a` (`DESIGN.md` decisions log 2026-08-28).
  - Suggested `docs.json`: primary `#3ecf8e`, light `#7ce3ad`, dark `#0b0b11`. → confirm in **H**.

### F-2 — What the product is

- **One paragraph:** A user picks a market (a stock, a crypto asset, a live game's win odds, or a prediction-market outcome's chance), sees the level to beat and the countdown for the current window, and buys **Up** or **Down** contracts at the house's live quote in cents. When the window closes, each contract on the winning side pays **$1.00**; the losing side pays $0. The user wins (payout − cost) or loses the cost of the contracts. They can also sell back to the house before the close and realize the difference. Evidence: `packages/api/src/lib/interval/engine.ts` (`settleClosedWindows`, lines 946–956), `live/[slug]/interval-market-view.tsx` `SettlementTerms` (lines 653–724).
- **Still prediction markets? Options?** Neither, as the product. It is **binary up/down contracts on a tracked metric** ("interval markets" internally — `Claude Docs/37-INTERVAL-MARKETS.md`). Prediction markets survive only as one *metric source*: the Trade hub's "Outcomes" row and the Live hub's sports markets track Polymarket odds (F-3). Options (V2) and Polymarket spot trading still exist behind un-linked routes (F-15) — omit them.
- **The unit:** **contract**. Used throughout: "Contracts" input label, "5.00 contracts", "pays $5.00 if Up", "Each winning contract pays $1.00", history rows "3.00 contracts @ 51¢", Rules "$1 per contract" (`live/shared.tsx`, `portfolio/portfolio-view.tsx`, `engine.ts`). The order book column is labelled **Shares** (`interval-market-view.tsx` line 615) — the only place; recommend the docs say "contracts" and note the column header.
- **Real money?** Yes. Balances are pUSD on Polygon, buys debit and settlements pay on-chain (`Claude Docs/37` §10–11; `engine.ts` `settleClosedWindows`, `settleCloseRealized`). **No paper / demo / practice mode exists.** (The `/app/survey` page references a historical "Paper Trading Tournament, Apr 6 – Apr 20, 2026" — decommissioned, F-15.)
- **Target user per marketing copy:** not stated beyond "Trade Short-Term Directional Movement on Stocks, Crypto, Sports, Outcomes, and More." The onboarding form asks "What kind of trader are you?" (Retail trader / Professional / Institutional / Just exploring) and "Where do you trade today?" (Polymarket / Kalshi / Both / New to prediction markets) (`login/page.tsx` lines 14–25), so the assumed audience is prediction-market and short-term retail traders. → **H** for positioning language.

### F-3 — Underlying markets, venues, and chains

What users trade **on** is a live number ("metric") per market. Sources (`apps/web/lib/interval/markets.ts` `IntervalMetricSource`, `engine.ts` `metricSeries`):

| Source | Markets | Live in UI? | Data feed (user-facing wording) |
|---|---|---|---|
| **Stocks** (`robinhood-stock`) | AAPL, NVDA, SPCX, TSLA, GOOGL, PLTR 15-min (code registry `INTERVAL_MARKETS`), plus any admin-added subject on Trade (daily/weekly/monthly) | Yes, weekdays 4:00 AM – 8:00 PM ET only | "Massive tape — AAPL last trade" (metric label). Settlement rules copy says "Prices come from the Robinhood tape, the live bid and ask midpoint" — **stale vs. the code**, which uses Massive (Polygon.io) real-time trades with Robinhood as keyless fallback and a Pyth pilot for TSLA (`metricLabel: "Pyth Pro — TSLA price"`). → **H** on which wording the docs use. |
| **Crypto** (`crypto-symbol`) | BTC, ETH, SOL, DOGE 15-min (code) + admin-added Trade subjects | Yes, 24/7 | "Massive tape — BTC/USD last trade"; the live tick is Coinbase's public ticker websocket from the browser (`live/shared.tsx` lines 495–502), history from Coinbase candles (`engine.ts` `coinbaseCryptoSeries`). |
| **Sports** (`polymarket-odds`) | In-play two-team games, auto-listed by the keeper cron (busiest live game per sport whose odds sit in the 5–95 % band; `api/cron/interval-keeper`, every 5 min) plus admin-listed games; 3-way (soccer) games track one team's win chance | Yes, session-based: windows start at kickoff, series ends when the game ends | "Polymarket win probability — <team>" / on-page: "<Team> win odds — up/down". Settlement copy: "The metric is the Polymarket last trade probability for this outcome, sampled once a minute." |
| **Outcomes** (`polymarket-odds`, category `predictions`) | Trade hub row "Outcomes" — a Polymarket outcome's YES chance as daily / weekly / monthly up-or-down (code pilot: Marco Rubio 2028 nominee, Clarity Act 2026, U.S. invade Iran before 2027 — `PV2_BASES`; admin can add more via the registry) | Yes on `/app/trade` (only if rows exist in the `interval_markets` table — the Trade hub reads the DB, not the code list) | Row blurb: "Live chance of a prediction-market outcome". |
| Mentions / tweet counts | category `mentions` | **No** — filtered off the hub ("Mentions are paused", `interval-view.tsx` line 112) | — |
| polymarket.us page-scrape (`polymarket-us`) | .us-only underliers | Backend/config only; no current listing | — |
| Kalshi | — | **Not used** by the interval product. Legacy references remain in the Enable-trading bullets ("Only works with Polymarket & Kalshi") and the Ctrl+K palette placeholder. | — |

- **Chains / tokens a user sees:** Polygon only. Deposit asset **USDC on Polygon** ("⚠ USDC on Polygon" card); balance unit **pUSD** ("pUSD — your tradeable balance on Polymarket." — `WalletModal.tsx` line 186, wording still Polymarket-centric); withdrawal pays **native USDC** on Polygon. Polygonscan links on Profile and withdraw receipts. **Yes, pUSD is still the balance unit.** A Solana embedded wallet is created at signup but never surfaced (F-4).
- **Counterparty:** the platform's own treasury ("house") quoting a two-sided ladder. `engine.ts` header: "Users trade against the treasury's quote (we're the sole counterparty)." The on-page order book footnote says so in product voice: "Preview of the treasury quote: size is capped to the per-window risk budget (~20 contracts/side) and tapered toward top-of-book, requoting as the model mid moves. An Up at p¢ and a Down at 100−p¢ lock $1 between them." (`interval-market-view.tsx` lines 642–646). No peer-to-peer book yet: "Limit orders open with the order book" (disabled button). Portfolio "Open orders" empty state: "Interval trades fill instantly, so nothing rests here yet. Limit orders for intervals arrive with the order book."

### F-4 — Authentication, wallets, custody

- **Auth provider:** Privy (`components/providers/PrivyClientProvider.tsx`). **Login methods offered: email and Google only** (`loginMethods: ["email", "google"]`, line 26). No wallet login, Apple, X, passkey, or phone. (The old "external wallet" login is gone.)
- **Embedded wallets:** created for all users on login on **Ethereum (used as Polygon)** and **Solana** (`createOnLogin: "all-users"` for both, lines 27–34). Only the Polygon wallet is user-visible. Two Polygon addresses are shown on Profile: **Deposit wallet (Polygon)** (the smart deposit wallet that holds pUSD) and **Signing wallet (Polygon)** (the Privy embedded EOA — the address you send USDC to) (`profile/page.tsx` lines 167–170). Solana address is returned by the API (`deposits.info.solanaAddress`) but never rendered.
- **Custody story in the app's words** (`AllowTrading.tsx`): heading "Enable trading"; "A one-time approval so Theta Labs can convert your deposits and place trades instantly — you won't sign every transaction."; bullets "We never hold your keys", "Only works with Polymarket & Kalshi", "Revocable anytime"; button **Allow trading**. States: "Approving trading access…" / "This can take a moment. You can close this window — it keeps going, and your access will be ready when you come back." → "Trading enabled" / "You're all set — deposit funds and start trading." / **Done**. Mechanism: Privy session-signer delegation (`hooks/use-trading-permission.ts`) on both the EVM and Solana embedded wallets; the server signs with the app's authorization key. **How to revoke is not surfaced anywhere in the UI** — → **H** (the docs currently say "revocable"; the product has no revoke button).
  - Note for the docs' honesty: trades themselves are ledger entries; money moves on-chain at deposit, sell-to-close, per-window settlement, and withdraw (`Claude Docs/37` §10: "on-chain at the edges, ledger in the middle").
- **Post-signup onboarding** (`apps/web/app/login/page.tsx`): a new account (authenticated but no DB user) is redirected to `/login` (`components/onboarding/OnboardingRedirect.tsx`), preserving `?returnTo=` so they land back on the market they came from. Step 1: heading "Sign in / Sign up", button **Sign in / Sign up with Privy**. Step 2: heading "Tell us about you", sub "A few quick questions so we can tailor your experience.", fields: **Name** (required, placeholder "Your name"), **Phone (optional)**, selects **What kind of trader are you?** (Retail trader / Professional / Institutional / Just exploring), **Where do you trade today?** (Polymarket / Kalshi / Both / New to prediction markets), **Experience level** (New to trading / Some experience / Experienced), **How'd you hear about us?** (Twitter / X, Discord, A friend, Other); button **Submit** ("Submitting…"). Referral capture: the `/r/CODE` route sets a `theta_ref` cookie for 30 days and redirects to `/login`; the form reads it and attributes the referral on create (`apps/web/app/r/[code]/route.ts`, `login/page.tsx` lines 128–140).
- **Profile page** (`/app/profile`, from the avatar menu **Profile**): header card (avatar, name, email, "Member since <Month D, YYYY>"); **Account** card with **Edit** → fields **Name**, **Phone** (placeholder "Optional"), **Email** (read-only, "Email is the login identity"), buttons **Save** / **Cancel**, toast "Profile updated"; **Wallet** card with **Balance** ($ pUSD), **Deposit wallet (Polygon)** and **Signing wallet (Polygon)** rows each with copy (tooltip "Copy address") and Polygonscan link ("View on Polygonscan"); bottom button **Sign out**. Signed-out state: "Sign in to view your profile".
- **User menu** (avatar, top-right; `UserMenu.tsx`): display name (or email, or truncated address), optional badge "Liquidity Provider" (legacy LP flag), items **Profile**, **Referral**, **Light mode** / **Dark mode** toggle, **Log out**. Signed-out nav shows a **Login** button.

### F-5 — Money in, money out

**Deposit** (`components/wallet/WalletModal.tsx`; opened from the nav Portfolio/Cash chip and Portfolio **Deposit** button):
- Modal title **Wallet**. If trading isn't enabled yet the modal shows the Enable-trading panel (F-4) instead of the QR.
- Balance block: label **Available**, USDC icon + `$X.XX`, sub "pUSD — your tradeable balance on Polymarket."; note "Please keep this page open while your deposit transfers — or come back here once it's done."
- Section **Deposit**, rules text (verbatim): "Send at least **$2 of USDC** on **Polygon** to the address below — it converts automatically. Wrong tokens or networks may result in permanent loss."
- Card: "⚠ USDC on Polygon", QR code, address, button **Copy Address** → "Copied!" (toast "Polygon address copied").
- **Accepted asset/network:** native USDC on Polygon only. The address shown is the signing (EOA) wallet; a sweep forwards it to the Polymarket bridge which credits pUSD to the deposit wallet (`packages/api/src/routers/deposits.ts` `sweep`).
- **Minimum:** $2 (`BRIDGE_MIN_USD = 2`, `packages/api/src/lib/trading/constants.ts` line 75). Below-minimum copy (verbatim): "We received $X.XX, but deposits under $2 can't be converted. Send at least $Y.YY more USDC and it all converts automatically — your funds are safe meanwhile." **Maximum:** none in code.
- **Timing/status strings:** toast "Detected $X.XX USDC — converting to pUSD…"; stage line "Deposit detected…" → "Finalizing…" → "Converting your deposit to pUSD…"; success toast "Deposit converted to pUSD"; failure toast "Deposit conversion failed — funds were not lost. Try again." The modal polls the sweep every 20 s and the bridge status every 5 s while open. First deposit additionally deploys the deposit wallet on-chain (~5–15 s, `ensureWalletDeployed`) — no user-visible copy for that step.
- **Fees:** none charged by the app; gas sponsored. Fiat on-ramp: none.
- Footer buttons **Go to Portfolio** / **Close**.

**Withdraw** (`components/portfolio/WithdrawModal.tsx`, from Portfolio **Withdraw**):
- Title **Withdraw**; description "Send native USDC to any Polygon address. Gas-free, usually instant."
- Fields: **Available** ($ pUSD), **Recipient address (Polygon)** (placeholder `0x…`, error "Invalid address"), **Amount (USDC)** with **MAX** button, errors "Minimum $2" / "Exceeds your balance"; readout **You receive** "≈ $X.XX USDC"; button **Withdraw** ("Withdrawing…"); footnote "Gas is sponsored. Withdrawals are irreversible — double-check the address."
- **Destination:** any Polygon `0x` address (own or external). **Asset paid:** native USDC on Polygon. **Minimum:** $2 (`MIN_WITHDRAW_USD = BRIDGE_MIN_USD`). **Maximum / daily limits / holds:** none, except the **open-position lock**: funds committed to open interval positions cannot be withdrawn; server error (verbatim): "Amount $X.XX exceeds your available $Y.YY ($Z.ZZ is committed to open interval positions)." (`deposits.ts` lines 266–277). The modal's **Available** shows the raw pUSD balance, not the locked-adjusted figure — → note as a caveat.
- **Receipt:** success view "Withdrawal confirmed" or "Withdrawal submitted"; "≈ $X.XX in native USDC is on its way to 0x1234…abcd."; link **View transaction →** (Polygonscan tx) or **Track arrival on explorer →** (recipient's token transfers while mining); buttons **Withdraw again** / **Done**. A history row "Withdrawal to 0x…" appears under Portfolio → history (Spot filter) labelled **Withdrew** / "native USDC to external wallet".
- **Banned accounts** can still withdraw (F-10).

**Wrong asset / network:** the only warning is the deposit rules sentence "Wrong tokens or networks may result in permanent loss." No recovery path exists in the product. → **H** for support policy.

### F-6 — Balance and portfolio model

- **Balances shown:**
  - Nav chip (`Navbar.tsx` lines 330–344): two cells **Portfolio** and **Cash**; tooltip "Portfolio = cash + open positions · Cash = available to trade". Clicking opens the **Wallet** modal. Refetch every 30 s; last value cached so it renders instantly.
  - Portfolio page hero (`portfolio/portfolio-view.tsx` lines 366–392): card **Balance** (big number = total value) with cells **Available**, **In position**, **Total traded**, buttons **Deposit** / **Withdraw**; card **Portfolio value** with the range chart.
  - Wallet modal **Available** = raw pUSD balance. Withdraw modal **Available** = raw pUSD balance. Market page **Buying power** = pUSD − open interval exposure (`interval.myPosition.availablePusd`).
- **Formulas** (`packages/api/src/routers/portfolio.ts` `summary`, `portfolio-view.tsx` lines 218–225):
  - Cash / Available = pUSD balance − cost basis of contracts still held in unsettled windows (the "exposure").
  - In position = live value of open interval positions (contracts × current midpoint; while a closed window is "settling", its cost basis) + legacy spot + legacy options value.
  - Portfolio / Balance = Available + In position.
  - Total traded = face-value volume, **$1 per contract over every fill, buys and sells alike** (+ legacy options volume). Not dollars spent. (`packages/api/src/lib/interval/portfolio.ts` `myIntervalVolume`; `CLAUDE.md`.)
- **What moves the spendable balance:** buy (−cost, held as exposure until settle), sell-to-close (realized P&L paid/collected on-chain immediately, cost basis released), window settlement (+$1 × winning contracts − held cost, net moved on-chain once), deposit (+), withdraw (−), referral cash-out (+, legacy). No fees today (F-9).
- **Portfolio value chart:** ranges **1D / 1W / 1M / 1Y / ALL**, default **1W**; change line reads "+$X.XX (+Y.YY%) Past week" (labels "Past day / Past week / Past month / Past year / All time"); hovering the chart replaces the header value with the cursor value. Snapshots come from a cron every 15 min (`vercel.json` `portfolio-snapshots`). Refresh intervals on the page: balance 15 s, interval positions 10 s, interval history 30 s, volume 60 s, chart 60 s, options/spot legacy 20–30 s.
- **P&L:** Unrealized shown per open position (value − held cost, with %; "awaiting settle" once the window has closed); realized shown per history row (sell rows: proceeds − avg cost × qty; settlement rows: net paid) and on the market page as **Total return** (realized $ and % of staked, this market only). Colors: green `#3ecf8e` positive, red `#f87171` negative, muted `#a5adbe` zero. Portfolio chart line is white (design rule).

### F-7 — Trading modes

There is **one trading mechanic** (up/down window contracts against the treasury quote) exposed on **two surfaces** that differ only by cadence. Everything below applies to both; differences are called out.

#### Mode 1 — **Live** (`/app/live`, 15-minute windows)

- **Nav label / route:** **Live** (heartbeat icon, red) → `/app/live`; market page `/app/live/<slug>`; settled-window page `/app/live/<slug>/<YYMMDD-HHMM-HHMM>`. `/app` redirects to `/app/live`; the brand mark links there; landing **Start Trading** goes there.
- **What is bought:** Up or Down **contracts** on whether the market's value closes the 15-minute window strictly above its opening value. Price 1¢–99¢ per contract, displayed as cents ("Up 51¢") and as odds ("51% chance"). On team markets the sides carry the team names (e.g. "Rangers" / "Mariners") and colors; on 3-way soccer listings the page reads "<Matchup> | [Team] Win Odds" and sides are "Up" / "Down / Flat".
- **Payoff:** winning contract → $1.00; losing → $0. Worked example from the ticket's own math: Market mode, **Dollars** $50 at Up 51¢ → "Odds 51% chance", "Max payout $98.04" (= 50 / 0.51). If Up wins you receive $98.04 (profit $48.04); if not you lose $50. Quick Buy box "$5 · win $9.80" (walks the ask ladder, so slightly less than 5/0.51 on any spread). Settlement copy: "Each winning contract pays $1.00 and each losing contract expires at zero."
- **Max loss:** what you paid. Positions are "fully collateralized when you enter". Users can never be short, write, or owe more. Sell-to-close can only reduce a held side ("You only hold N contracts on this side.").
- **How orders fill:** instantly against the house's ladder (`serverQuote` → `binaryQuote`, 6 price levels per side, sizes tapered 30/24/18/13/9/6 % of the remaining budget). Partial fills happen when the ladder is thin — confirmation shows "Filled $X of your $Y — book was thin here". **Price protection** is automatic: the client sends a cap of (shown price × 1.3 + 3¢) on buys and a floor of (shown price × 0.7 − 3¢) on sells; if the average fill would breach it the order is rejected: "Price moved to 55¢ (above your 40¢ limit) — order not filled." / "Price moved to 2¢ (below your 5¢ limit) — order not filled." (`live/shared.tsx` lines 811–818, `engine.ts` lines 571–580). No user-set slippage control.
- **Ticket anatomy** (desktop right rail, `live/shared.tsx` `WindowTicket`):
  - Top row: **Buy** / **Sell** pill (hidden in Quick Buy, replaced by the ⓘ tie note) and the order-type dropdown **Quick Buy** / **Market** / **Limit**. Team markets add the outcome toggle (two pills with the team names).
  - **Quick Buy:** two columns headed by filled pills "▲ Up 51¢" / "▼ Down 49¢"; each has three hold-to-buy boxes **$1 / $5 / $10** reading "$5" over "win $9.80"; helper line "Hold a box to buy" → "Placing…". Hold ~0.7 s to commit (haptic tap on phones); releasing early drains the box. Box tooltip: "Hold to buy $5 Up".
  - **Market:** side pill "Up 51¢ | Down 49¢" + ⓘ; input **Dollars** `$ [50]` (default 50); readouts **Odds** "51% chance", **Max payout** `$98.04`; button **Buy Up** / **Buy Down** (green/side color) or, in Sell mode, **Sell Up** / **Sell Down** (red) ("Placing…"). Sell closes `dollars ÷ price` contracts.
  - **Limit:** inputs **Contracts**, **Limit price** `¢` (placeholder = current ask); line "Ask 51¢ · pays $1 if it wins"; checkbox "Submit as resting order only"; readouts **Cost**, **Max payout**; button is **disabled**: "Limit orders open with the order book" (tooltip "Resting limit orders open with the order book (CLOB). Market orders trade now."). **Do not document Limit as available.**
  - Confirmation overlay "**Trade executed**" — "Bought $5.00 **Up** @ 51¢" + "Pays $9.80 if Up wins"; on a sell "Sold $4.90 Up @ 49¢" + "+$0.40 realized" (with "· settling…" while the on-chain payout confirms). Auto-dismisses after ~2 s.
  - Sizing options: dollars only (Quick Buy presets or Market input). No contract or percent sizing on buys; Limit's contract input is inert.
  - **Phone:** the ticket is a docked bottom sheet with a **Quick / Market / Limit** segmented toggle; Quick is two steps (pick **Up 51¢** or **Down 49¢**, then that side's $1/$5/$10 row with a back arrow and "Hold a box to buy"); collapsed it shows "Buy Up 51¢" / "Buy Down 49¢" pills.
- **Minimums / maximums / capacity:** no minimum order in code beyond > $0 (Quick Buy starts at $1). Per-window capacity: the house offers at most **20 contracts of net exposure per side per window** (`OI_CAP = 20`, `engine.ts` line 35); once the ladder is consumed you get "No liquidity available at these levels." or a partial fill. API hard caps: 10,000 contracts / $100,000 budget per order (`routers/interval.ts`) — not user-facing. Insufficient funds: "Insufficient balance: need 5.00 pUSD, have 2.10." No per-user caps.
- **Fees and spreads:** **no fee** (`FEE_BPS = 0`). Spread: half-spread = 1¢ base + up to 4¢ "pin-widen" that grows near 50/50 as time runs out; minimum quoted spread 2¢; the center shifts up to 3¢ to shed house inventory (`packages/shared/src/interval/quote.ts`). Disclosed in the order book ("Spread 2¢") and the footnote quoted in F-3. No fee line anywhere in the ticket.
- **Time structure:** windows sit on the **UTC 15-minute grid** (…:00, :15, :30, :45) and roll continuously; the page flips to the new window at the boundary. Countdown "Time left" turns amber under 50 % and red under 20 % of the window. **Stocks run only weekdays 4:00 AM – 8:00 PM ET**; outside that the market page shows "Market closed — stock markets trade weekdays 4:00 AM – 8:00 PM ET." / "Crypto markets stay open 24/7." / **Back to Live**, and stock cards disappear from the hub. Crypto: 24/7. Sports: windows begin at kickoff ("UPCOMING · opens 7:30 PM", card "Trading opens in 12:04"), keep rolling through the game, and the series ends when the game's market closes — the window in progress becomes the final one ("FINAL" tag) and settles at the frozen odds.
- **Settlement:** outcome = value at close (from the minute-bar history feed, same feed as the chart) vs. value at open; strictly higher → Up, else Down. Automatic: the `interval-settle` cron runs every 5 minutes (`apps/web/vercel.json`) and the page itself pings a settle endpoint when a window rolls (throttled to once per 45 s), so a closed window is usually recorded within seconds and paid within ~5 minutes. Net (payout − held cost) is moved on-chain once per user per window; no claim step. What the user sees: Portfolio position row shows "settling" / "awaiting settle", then a history row **Won** ("3.00 contracts · paid $1 each", "+$1.47") or **Lost** ("· settled worthless") or **Settled** ("· nothing owed either way"). The settled-window page shows **Result** "Up won" (or "Rangers up"), **Open**, **Close**, and the full static chart with the "Target $325.40" line; before it's recorded: "This window isn't recorded yet" / "Windows are saved shortly after they close." / **Back to the live market**.
  - **Tie rule (verbatim ⓘ copy):** "If the price is **unchanged**, it settles as **Down**. **Up** needs a real move." (On a mirrored team view the tie side flips and the labels read "Up / Flat".)
  - **Game-over rule** (ticket copy): "Trading paused — game over" / "The odds froze at 10:41 PM EDT and can't move again, so no more trades are taken. This window settles **Down** at the frozen odds; payouts follow shortly." Header tag "GAME OVER · SETTLING". API error if you try: "Trading is paused — the game has ended. This window settles at the final odds." After the series: "Series ended" / "The underlying's market closed <time> — interval windows never outlive what they track."
- **Closing early:** yes, any time before the close, at the house **bid** ladder. Two ways: (1) **Live Positions** panel on the market page — hold the red **Sell** button on a side's row (fills red as you hold, "Hold…", "Selling…") to sell that side's entire position; (2) ticket **Sell** mode with a dollar amount. Realized P&L (proceeds − average cost × contracts) is paid or collected on-chain immediately ("+$0.40 realized · settling…"). Minimum: none. Cannot exceed held contracts.
- **Position display:**
  - Market page **Live Positions** panel: header with **Buying power $X.XX**; per side: color dot, side label ("Up", or "49ers Down"), "5.00 contracts", value `$X.XX`, **Sell** button; sub-line "entry 51¢ · now 54¢" and "pays $5.00 if Up"; totals **Value**, **Unrealized** "+$0.15 (+5.9%)", **Total return** "+$1.20 (+12.0%)" (realized for this market); empty "No position in this window yet."
  - Portfolio → positions → section **Interval markets**: image, title, "<Side> · 15m · closes in 12m" (or "· settling"), desktop stats **Contracts / Entry / Now / Pays** ("$5.00" sub "if Up"), value + unrealized P&L (%), button **Trade** (or **View** once settling).
- **Explainer copy — Settlement rules accordion, quoted in full (stock market variant; `SettlementTerms`):**
  > A new window opens every 15 minutes. Its opening price is AAPL's price at the moment the window opens.
  >
  > Up wins only if AAPL's price closes strictly above that opening price. Down wins if it closes at or below it — so a window that finishes exactly unchanged settles Down.
  >
  > Each winning contract pays $1.00 and each losing contract expires at zero. Every position is fully collateralized when you enter, so an Up at p cents and its matching Down at 100 minus p cents together set aside exactly $1.00.
  >
  > Prices come from the Robinhood tape, the live bid and ask midpoint for AAPL. The tape follows US market hours, so it pauses on weekends and market holidays. The same feed powers the chart above, so pricing and settlement share one source of truth.

  Odds (sports / outcomes) variant replaces the last paragraph with:
  > The metric is the Polymarket last trade probability for this outcome, sampled once a minute. The same feed powers the chart above, so pricing and settlement share one source of truth.
  >
  > The series ends when the underlying market closes. That moment is tracked live, since the final result is not known in advance. The window in progress becomes the final one and settles on its last price. No window opens after it.

  There is **no** "How it works" modal on Live/Trade (the old options one is unmounted on these surfaces).

#### Mode 2 — **Trade** (`/app/trade`, daily / weekly / monthly windows)

Identical mechanics, ticket, settlement, and position display. Differences:
- **Nav label / route:** **Trade** → `/app/trade`; market page `/app/trade/<slug>` (same page component as Live); slugs end `-1d`, `-weekly`, `-monthly`. Settled-window pages still live under `/app/live/<slug>/<window>` (history links go there).
- **Hub copy:** H1 **All Markets**; sub "Outcomes, stocks and crypto — each market's live value and recent movement. Pick an expiry to see that window's level to beat and its Up / Down odds." With an expiry selected: "Call whether each market closes this window above or below where it stood at the previous period's close."
- **Time structure** (`lib/interval/markets.ts` `cadencePhrase`, `periodStart`): Daily window = one UTC day, 00:00 → 23:59 UTC ("September 15 · 00:00 – 23:59 UTC"); Weekly = Monday 00:00 UTC → next Monday; Monthly = the 1st 00:00 UTC → next 1st. The opening print is the value at the previous period's close. Settlement-rules phrasing: "A new window opens every day at 00:00 UTC" / "every Monday at 00:00 UTC (one window per week)" / "on the 1st of each month at 00:00 UTC (one window per month)". Card footer "Expires today" (amber) / "Expires Sep 30 UTC" with countdown "18d 5:12:34".
- **Headline expiry picker** on the market page: the title reads "Apple (AAPL) | [End-of-Day ▾] Up or Down"; menu options **15 Minutes** (→ Live page), **End-of-Day**, **End-of-Week**, **End-of-Month** (`lib/interval/resolve.ts`), showing only the cadences that exist for that subject.
- **Chart default:** opens on **MKT** (whole window) instead of the live tape; lookbacks add **1D / 1W / 1M** for long enough windows.
- Stock subjects still respect the weekday 4 AM – 8 PM ET tape hours (page shows the closed notice off-hours), even though the window itself is a full UTC day.

### F-8 — Navigation and page inventory

- **Desktop top bar** (`Navbar.tsx`), left → right: mark + **updown** (→ `/app/live`); **Live** (heartbeat icon) `/app/live`; **Trade** `/app/trade`; right cluster: **Admin** `/app/admin` (admins only), **Portfolio** `/app/portfolio`; a red globe icon when geo-blocked (tooltip "Trading is unavailable in your region"); the **Portfolio / Cash** chip (opens **Wallet**); avatar **user menu** (Profile, Referral, Light mode/Dark mode, Log out) or **Login**.
- **Default after login:** wherever they were (`returnTo`), else `/app` → `/app/live`.
- **Mobile:** hamburger drawer with sections **Trade** (Live, Trade), **Account** (Portfolio), **Profile** (Profile, Referral, theme toggle, Log out); bottom tab bar **Live / Trade / Portfolio** (`MobileNav.tsx`). The Portfolio/Cash chip and avatar remain in the top bar.
- **Bottom status bar** (`StatusBar.tsx`, desktop bottom edge / above the mobile tabs): "Online" / "Offline" dot (probes the API every 60 s), **Contact us** (`/app/contact`), **Docs** (`https://docs.thetalabs.gg/introduction`), Discord icon, X icon.
- **Banners:** maintenance strip (admin-set message, "Resuming in 2h 14m."); **Weekly competition** banner in the last 6 hours before the Thursday-midnight-ET reset: "Weekly competition ends in 2h 14m — you're #4, $3.10 behind #3" (links to `/app/leaderboard`); **Account flagged** modal for banned users (F-10).
- **Keyboard shortcut:** **Ctrl/⌘ + K** opens a search overlay (`CommandPalette.tsx`) — but it searches **Polymarket/Kalshi prediction markets** ("Search prediction markets...", "Type to search Polymarket and Kalshi markets") and routes to the legacy `/app/spot/<slug>` page. It cannot find Live/Trade markets. **Recommend omitting from docs** (or listing under leftovers). There is no search box in the bar.
- **Secondary surfaces reachable from chrome:** Wallet modal (chip, Portfolio Deposit), Withdraw modal (Portfolio), Rules & Prizes modal (Leaderboard), Contact page (status bar), Referral page (user menu), Profile page (user menu), Leaderboard (banner only — **not in the nav** since 2026-09-14).
- **Every page a signed-in user can reach** (`apps/web/app/app/**/page.tsx`):
  - Linked: `/app/live`, `/app/live/[slug]`, `/app/live/[slug]/[window]`, `/app/trade`, `/app/trade/[slug]`, `/app/portfolio`, `/app/profile`, `/app/referrals`, `/app/contact`, `/app/leaderboard` (banner), `/app/admin` (admins).
  - Orphaned / legacy (direct URL only): `/app/predictions` (+ `/app/spot`, `/app/spot/[slug]`, `/app/options`, `/app/options/[slug]`), `/app/arena` and 7 sub-pages, `/app/collect`, `/app/compute`, `/app/narratives`, `/app/narratives/[id]`, `/app/social`, `/app/survey`, `/app/api-docs` ("API — Coming soon"). See F-15.

### F-9 — Fees and pricing (consolidated)

| Charge | Amount today | Where shown |
|---|---|---|
| Platform trading fee | **None** (`FEE_BPS = 0`, `engine.ts` line 37; admin doc: "Fees come from fee_cents (0 while FEE_BPS is off)") | Not shown; the ticket has no fee line |
| Spread / markup | Implicit: ≥ 2¢ quoted spread, widening up to ~+8¢ total near 50/50 in the final minutes; up to 3¢ inventory skew (`quote.ts`) | Order book "Spread N¢" and footnote; Quick Buy "win $X" and Market "Max payout" already net of it |
| Pass-through venue fees | None (the house is the counterparty) | — |
| Deposit fee | None; gas sponsored | "it converts automatically" |
| Withdrawal fee | None; "Gas is sponsored." | Withdraw modal |
| Gas | Sponsored everywhere | Withdraw footnote |

Fee mechanics are wired (`feeCents` column, referral fee share) so a fee could be switched on; the docs should say "no trading fee today" and avoid promising it forever → **H**.

### F-10 — Eligibility, compliance, risk

- **Geo-blocking** (`apps/web/middleware.ts`, `apps/web/app/api/geo/route.ts`): the country list blocks **AU, BE, BY, BI, CF, CD, CU, DE, ET, FR, GB, IR, IQ, IT, KP, LB, LY, MM, NI, NL, RU, SO, SS, SD, SY, UM, VE, YE, ZW** (Polymarket list), **AF, DZ, AO, BO, BG, BF, CM, CA, CI, HT, KE, LA, ML, MC, MZ, NA, NE, CN, PL, SG, CH, TW, TH, UA, AE** (DFlow/Kalshi list), and **US** ("not yet supported on our platform"). Detection is Vercel's IP header, fail-open. **Behavior:** the only effect on the Live/Trade product is the red globe icon in the nav with tooltip "Trading is unavailable in your region". **Interval buys and sells are not blocked** client- or server-side (`useGeo` is consumed only by the nav and the legacy spot/options tickets; `tradingProcedure` checks bans, not geo). The old string "Viewing only · trading unavailable in your region" no longer exists. → **H**: this is a compliance gap the team must resolve before the docs state a policy.
- **Age / KYC / identity:** none. Onboarding collects name and optional phone only.
- **Account restrictions (bans):** modal "Account flagged" — "This account has been flagged for suspicious activity and is restricted from trading, deposits, and rewards. **Your balance is not affected — you can still withdraw at any time.** If you believe this is a mistake, contact us on Discord or at hello@thetalabs.gg and we'll take a look." Checkbox "Don't show this again", button **Got it** (`BannedNotice.tsx`). API error: "This account has been restricted. You can still withdraw your balance. Contact support if you believe this is a mistake." (`packages/api/src/trpc.ts` line 155).
- **Legal / risk copy in code:** none. No terms, privacy, risk-disclosure, or regulatory statement anywhere in `apps/web`. The closest is the deposit warning "Wrong tokens or networks may result in permanent loss." and the withdraw footnote "Withdrawals are irreversible — double-check the address." → **H**.
- **Links to terms/privacy/risk pages:** none exist → **H**.

### F-11 — Incentives: rewards, referrals, competitions, leaderboards

- **Free-share / streak / first-deposit rewards (Theta Labs B-29):** the components (`components/rewards/RewardReveal.tsx`, `StreakPunchCard.tsx`) are mounted only inside the archived options hero (`app/app/options/options-hero.tsx`), i.e. reachable only via `/app/predictions?inst=options`. The `rewards` cron still runs every 15 min. **Not part of the Updown product — omit.**
- **Referrals** (`/app/referrals`, user menu **Referral**; `packages/api/src/lib/rewards/referral.ts`): live page, but its economics are options-era. Copy (verbatim): "Earn **10% of your friends' trading fees** for 6 months" / "Share your link. Every options trade a friend makes for 6 months pays you 10% of the fee — cash it out to your trading wallet once you reach $5.00." How it works: "1 · Share. Copy your link below and send it to a friend." "2 · They trade. Your friend signs up with your link. For 6 months from then, you earn 10% of the fee on every options trade they make." "3 · Cash out. Once your balance reaches $5.00, claim it and the pUSD lands in your trading wallet." Cells: **Available to cash out**, button **Claim $X.XX** ("Payout pending" / "Sending…"), progress "$X.XX more to reach the $5.00 minimum." / "Ready to cash out.", stats **Active referrals / Lifetime earned / Paid out**, **Your referral link** (`<origin>/r/<CODE>`, **Copy**/"Copied"), "N friends have joined with your link." Constants: 1000 bps share, 6-month window, $5 minimum payout. **Because interval trades charge no fee, a referral currently earns $0.** → **H**: document, rewrite, or hide.
- **Weekly competition / leaderboard** (`/app/leaderboard`, `leaderboard-view.tsx`, `packages/shared/src/competition.ts`, `Claude Docs/38`):
  - Name: "Weekly Trading Competition". Reachable only from the pre-deadline banner (removed from the nav 2026-09-14) — → **H** whether to document it.
  - **Ranking metric (since Fri Sep 11 2026):** realized P&L on interval markets for windows that **close** inside the week, sell-to-close included; ties broken by interval volume. Options/spot activity does not count. Only windows whose settlement succeeded count; failed/pending settlements are excluded.
  - **Cadence / timezone:** "Each round runs **Friday 12:00 AM → Thursday 11:59 PM EST**, and a fresh competition starts every week." (Code approximates ET as UTC−4; header "This week Sep 11 – Sep 17 ET", **Resets in** countdown.)
  - **Prizes (Rules & Prizes modal):** 1st place **$100**, 2nd **$50**, 3rd **$25**, 4th – 10th **$5 each**; "Winners are paid in **pUSD straight to their wallet** at the end of each round." **No automated payout code exists** (grep for prize/payout routines found none) — prizes are paid manually → **H**.
  - Rules copy (P&L mode, verbatim): "Ranked by **realized P&L on interval markets** this week — what your settled windows paid out minus what you staked, sell-to-close included. Volume alone doesn't rank you; only windows that close inside the week count."
  - **Columns:** `#` / avatar / **Trader** / **Volume** (muted, interval face value) / **P&L** (signed, colored) / **24h** (rank change ▲n ▼n). Top-3 podium with medal gems. "You" tag on your row; your row pinned below the list if outside the top. Empty: "No trades yet this week." / "Be the first on the board." Refresh 10 s.
  - Eligibility: admin and hidden accounts are excluded from standings (`REAL_USER_IDS`).

### F-12 — Social and community features

- Activity feed / following / comments / market requests: **none live** (`/app/social` is orphaned, F-15).
- Share cards: only the options position share card (`portfolio/share-card.tsx`) on legacy rows. No share for interval positions or windows.
- Community links: Discord and X in the landing nav/footer, status bar, contact page. **Discord alerts:** every new interval market listing (admin-created or keeper-listed) posts an embed to the team's Discord webhook ("📈 **New interval market:** <title>", with category, cadence(s), sides, "Windows open: <time>") (`packages/api/src/lib/notify/discord.ts`). Worth one line in a "Community" section if the channel is public → **H**.
- Profiles: private profile page only; no public profiles. Leaderboard shows display names + generated avatars.

### F-13 — Everything else

- **Notifications:** a `notifications` table is written on withdraw, but there is no bell or inbox UI. No push or email notifications. Toasts (bottom-right) only.
- **Mobile app / PWA:** none; the web app is responsive with a bottom tab bar and docked ticket sheet.
- **Public API / API keys:** `/app/api-docs` says "API" / "Coming soon". Not in the nav.
- **AI features:** none.
- **Settings:** Profile page only (name, phone). Theme toggle in the user menu: **Light mode** / **Dark mode** (dark default, persisted in localStorage).
- **Language / currency:** English only; USD/pUSD only. Times: window countdowns are live; window open/close times display in the viewer's local time; daily/weekly/monthly windows are labelled in UTC; the market page's "Past" list shows close times in ET; the leaderboard week is ET.
- **Accessibility notes worth documenting:** Quick Buy boxes support keyboard hold (Space/Enter held), hold-to-buy vibrates on commit (phones), confirmation card is `aria-live`.
- **Admin:** an **Admin** nav link and `/app/admin` dashboard exist for admin accounts only — not for end-user docs.

### F-14 — Support and operational facts

- **Channels:** email `hello@thetalabs.gg`, X `@thetalabsgg`, Discord invite `d9gfrYWSAu`.
- **In-app form** (`/app/contact`, status bar **Contact us**; page copy: eyebrow "Get in Touch", H1 "Contact Us", "Questions, feedback, or partnership inquiries — we'd love to hear from you."; channel cards **EMAIL** hello@thetalabs.gg, **X (TWITTER)** @thetalabsgg, **DISCORD** "Join our server"; divider "Send a Message"): fields **Username** (read-only, prefilled from the account), **Email** (prefilled, editable, max 255), **Message** (max **5000** chars, counter "N / 5000"); button **Send Message** → "Sending…" → "Message Sent!"; error "Something went wrong. Please try again." Requires all three fields; works signed out except Username stays empty (the button disables until it fills), so effectively **signed-in only** → note as caveat. Contact link is desktop-only chrome (status bar); on phones the status bar also renders above the tab bar, so it is reachable there too.
- **Status page:** none. The status bar's "Online / Offline" dot is a local API reachability probe (every 60 s).
- **Response times:** none stated anywhere → **H**.

### F-15 — Theta Labs-era leftovers (reachable, omit from docs)

| Route / feature | State | Evidence |
|---|---|---|
| `/app/predictions` (`?inst=spot` / `?inst=options`), `/app/spot`, `/app/spot/[slug]`, `/app/options`, `/app/options/[slug]` | Live real-money Polymarket spot + options V2 order book; archived from the nav 2026-09; still fully functional by URL, shows its own sub-bar (Options / Spot + topic chips) | `Navbar.tsx` lines 55–58, 136–210; `CLAUDE.md` |
| Portfolio sections **Options**, **Written (covered)**, **Markets** (spot), tab **Open orders**, history filters **Spot** / **Options**, Sell ticket, Share card, **Redeem** button | Render only for users with legacy positions | `portfolio-view.tsx` |
| Ctrl/⌘+K command palette | Searches Polymarket/Kalshi, routes to `/app/spot/...` | `CommandPalette.tsx` |
| `/app/leaderboard` | Live (P&L mode) but removed from nav; only the deadline banner links to it | `Navbar.tsx` line 66 |
| `/app/referrals` | Live but pays on options fees only (= $0 today) | F-11 |
| Rewards (reveal, streak, first-deposit) + `rewards` cron | Only on the archived options hero | `options-hero.tsx` |
| `/app/arena`, `/app/arena/{ai-labs,coding-agents,crypto,games,mag7,perp-dexs,premier-league}` | Mindshare/dominance index pages, admin-only in nav, orphaned | `Claude Docs/34` |
| `/app/collect`, `/app/compute` | "Coming soon" teasers (titles "Collectables — Coming soon", "Compute — Coming soon"); their back-link says "In the meantime, options on prediction markets are live. Trade options →" | `ComingSoon.tsx` |
| `/app/narratives`, `/app/narratives/[id]` | Paper-funded, nav-orphaned, archived | `CLAUDE.md` |
| `/app/social` | Unlinked social feed | `social-view.tsx` |
| `/app/survey` | Post-tournament survey for "Paper Trading Tournament · Apr 6 – Apr 20, 2026", back-links to Social | `survey/page.tsx` |
| `/app/api-docs` | "API — Coming soon" | `api-docs/page.tsx` |
| Solana embedded wallet + Solana delegation | Created and delegated on Enable trading, never shown | `PrivyClientProvider.tsx`, `use-trading-permission.ts` |
| Kalshi / DFlow | Only in the Enable-trading bullet "Only works with Polymarket & Kalshi", the palette placeholder, and the geo list comments | `AllowTrading.tsx` line 87 |
| "Theta Labs" strings, `thetalabs.gg` domain, OG images, share-card watermark | See F-1 | — |
| Tournament-snapshot cron (`5 4 * * 5`) | Computes an options-only top-10 nobody reads | `Claude Docs/38` follow-ups |
| `interval-view.tsx` "mentions" category | Paused, filtered off the hub | line 112 |

---

## C. Proposed docs navigation

One tab, five groups. Statuses are relative to Section 2.1 of the request.

**Get started**
1. `introduction` — What updown is, the window/contract model, the five things you do. **REWRITE**.
2. `quickstart` — Sign in → Enable trading → deposit $2+ USDC → first Quick Buy → watch it settle. **REWRITE**.
3. `account-setup` — Privy login (email/Google), onboarding form, Enable trading, the two Polygon addresses, Profile, theme, log out. **REWRITE**.

**Trading**
4. `trading/how-windows-work` — The single mechanic: market, window, to beat, Up/Down, $1 payout, tie rule, settlement timing, game-over rule. **NEW** (replaces `trading/options-trading`; reason: the flagship product changed from options to up/down windows).
5. `trading/live-markets` — The Live hub and 15-minute markets: hub anatomy, categories, stock hours, sports sessions, team markets. **NEW** (replaces `trading/spot-trading`; reason: spot is archived).
6. `trading/trade-markets` — The Trade hub: daily / weekly / monthly expiries, Outcomes / Stocks / Crypto rows, the expiry picker, UTC window rules. **NEW**.
7. `trading/placing-orders` — The ticket: Quick Buy (hold-to-buy), Market (dollars), Limit (not yet), price protection, partial fills, selling back, the order book, all error strings. **REWRITE of `trading/order-types`**.

**Portfolio**
8. `portfolio/overview` — Balance card, Cash vs Portfolio, formulas, value chart, refresh. **REWRITE**.
9. `portfolio/deposit-withdraw` — Wallet modal, $2 minimum, status stages, withdraw flow, open-position lock. **KEEP with edits** (rails unchanged; labels and lock are new).
10. `portfolio/positions-and-history` — Interval position rows, Live Positions panel, history labels Bought/Sold/Won/Lost/Settled, settled-window pages. **REWRITE of `portfolio/positions`** (rename: history is now half the page; Redeem/Sell ticket/Share are legacy).

**Platform**
11. `platform/finding-markets` — Live hub filters/featured panel, Trade hub rails, market page header, cadence menu, Past windows. **REWRITE of `platform/markets`** (rename: no search/categories/sort tabs anymore).
12. `platform/leaderboard` — Weekly P&L competition. **KEEP with edits** — **conditional on H-6** (page is nav-orphaned and prizes are manual).
13. `platform/rewards` — **DELETE**. Reason: free-share/streak/first-deposit rewards are only on the archived options surface; referrals pay 0 on interval trades. Fold a one-paragraph "Referrals" note into `account-setup` only if the team keeps the page (H-5).

**Help**
14. `help/faq` — **REWRITE**.
15. `help/glossary` — **REWRITE**.
16. `help/support` — **NEW** (was a section of the FAQ): contact form, channels, banned-account policy, wrong-network deposits. Optional; merge into FAQ if the team prefers 14 pages.

---

## D. Page briefs

### Get started / `introduction` — "Introduction"
- **Purpose:** Tell a new visitor what updown is and what they can do in five minutes.
- **Status:** REWRITE of `introduction`.
- **User flows:** none (overview). The "what you can do" list: 1. Sign in with email or Google. 2. Click **Allow trading** once. 3. Send **$2+ of USDC on Polygon** to your address. 4. Open **Live** or **Trade**, pick a market, hold a **$1 / $5 / $10** box on **Up** or **Down**. 5. Watch the window close — winners are paid $1 per contract automatically. 6. Sell back early any time from **Live Positions**.
- **UI anatomy:** the key-concepts table from Section A.
- **Key numbers/limits:** $1 per winning contract; prices 1¢–99¢; 15-minute windows on Live; daily/weekly/monthly on Trade; $2 minimum deposit/withdrawal; no trading fee; stocks weekdays 4:00 AM – 8:00 PM ET, crypto 24/7.
- **Warnings/caveats:** Real money. Max loss = what you paid. Unchanged close settles Down. US and the geo list are "not supported" (H-3 wording). The product was formerly Theta Labs; support handles still use that name.
- **In-app copy to reuse:** tagline "Trade Movement on Everything"; meta description (F-1); Live page description "Live 15-minute up-or-down markets on stocks, crypto, and sports — pick a side, hold to buy."; Trade description "Daily, weekly and monthly up-or-down markets on live prediction-market odds."
- **Cross-links:** quickstart, how-windows-work, live-markets, trade-markets, deposit-withdraw.
- **Evidence:** `apps/web/app/layout.tsx`, `options-landing.tsx`, `live/page.tsx`, `trade/page.tsx`, `Claude Docs/37-INTERVAL-MARKETS.md` §1.

### Get started / `quickstart` — "Quickstart"
- **Purpose:** Get from a fresh browser to a settled first trade.
- **Status:** REWRITE.
- **User flows:**
  1. Go to the landing page and click **Start Trading** (lands on **Live**). Click **Login** (top right). In the Privy dialog sign in with **email** or **Google**.
  2. First time only: on the **Tell us about you** form enter **Name** (required), optional **Phone**, answer the four dropdowns, click **Submit**.
  3. Click the **Portfolio / Cash** chip in the top bar. In the **Wallet** modal read the **Enable trading** panel and click **Allow trading**. Wait for "Trading enabled", click **Done**.
  4. In the **Wallet** modal under **Deposit**, click **Copy Address** and send at least **$2 of USDC on Polygon** to it. Keep the modal open; wait for "Deposit detected…" → "Finalizing…" → "Converting your deposit to pUSD…" → toast "Deposit converted to pUSD". Click **Go to Portfolio** to confirm **Available**.
  5. Click **Live**. Pick a card (e.g. "Bitcoin (BTC) · 15 min"). Read **To beat** and **Now**.
  6. In the ticket (right rail, or the bottom sheet on a phone) stay on **Quick Buy**, press and hold the **$1** box under **Up** (or **Down**) until it fills and shows **Trade executed** — "Bought $1.00 Up @ 51¢ · Pays $1.96 if Up wins".
  7. Watch **Time left**. When the window closes the position shows "settling"; within about 5 minutes **Portfolio → history** shows **Won** or **Lost** and **Cash** updates.
  8. Optional: sell back before the close by holding **Sell** on your row in **Live Positions**.
- **UI anatomy:** short table of the ticket's Quick Buy column: side pill (label + price), three boxes ($1/$5/$10 with "win $X"), helper "Hold a box to buy".
- **Key numbers/limits:** $2 deposit minimum; ~0.7 s hold; $1/$5/$10 presets; ~5-minute settlement; stock hours.
- **Warnings/caveats:** below-$2 deposits sit unconverted until topped up; wrong token/network may be lost; a $1 quick buy on a thin ladder can fill partially ("Filled $X of your $Y — book was thin here"); prices move between quote and fill — the app rejects fills more than ~30 %+3¢ worse than shown.
- **In-app copy to reuse:** deposit rules sentence; "Please keep this page open while your deposit transfers — or come back here once it's done."; "Trade executed"; "Hold a box to buy".
- **Cross-links:** account-setup, deposit-withdraw, placing-orders, how-windows-work.
- **Evidence:** `page.tsx` (landing), `login/page.tsx`, `WalletModal.tsx`, `AllowTrading.tsx`, `live/shared.tsx` (`QuickBuyBox`, `TradeExecutedCard`, `QUICK_AMOUNTS`, `HOLD_MS`), `vercel.json` (`interval-settle` `*/5`).

### Get started / `account-setup` — "Account setup"
- **Purpose:** Explain login, onboarding, the one-time approval, wallets and the Profile page.
- **Status:** REWRITE.
- **User flows:** (a) Sign in: **Login** → Privy → email code or Google. (b) Onboarding form (fields in F-4) → **Submit**. (c) Enable trading: **Wallet** → **Allow trading** → "Approving trading access…" → "Trading enabled" → **Done**. (d) Edit profile: avatar → **Profile** → **Edit** → change **Name** / **Phone** → **Save** (toast "Profile updated"). (e) Theme: avatar → **Light mode** / **Dark mode**. (f) Sign out: avatar → **Log out**, or Profile → **Sign out**.
- **UI anatomy:** Profile page cards: Header (avatar, name, email, "Member since …"); Account (Name, Email, Phone); Wallet (Balance; **Deposit wallet (Polygon)** = holds your pUSD; **Signing wallet (Polygon)** = the address you deposit to; copy + Polygonscan). User menu items table.
- **Key numbers/limits:** name ≤ 255 chars, phone ≤ 50; referral cookie 30 days.
- **Warnings/caveats:** login methods are email and Google only; the approval covers an unseen Solana wallet too; no revoke control in the app (H-2); the approval copy still says "Theta Labs" and "Polymarket & Kalshi" (H-1).
- **In-app copy to reuse:** all Enable-trading strings (F-4); "Sign in / Sign up with Privy"; "Tell us about you" / "A few quick questions so we can tailor your experience."
- **Cross-links:** deposit-withdraw, quickstart, support.
- **Evidence:** `PrivyClientProvider.tsx`, `login/page.tsx`, `OnboardingRedirect.tsx`, `AllowTrading.tsx`, `use-trading-permission.ts`, `profile/page.tsx`, `UserMenu.tsx`, `r/[code]/route.ts`.

### Trading / `trading/how-windows-work` — "How windows work"
- **Purpose:** The one page that explains the product's mechanic, payout and settlement so every other page can link to it.
- **Status:** NEW (supersedes `trading/options-trading`).
- **User flows:** none; concept page with a worked example: "Apple (AAPL) window 2:00–2:15 PM, To beat $325.40. You hold a $5 box on Up at 51¢ → 9.80 contracts. Close $325.62 → Up wins → you receive $9.80 (profit $4.80). Close $325.40 or lower → Down wins → your $5 is gone."
- **UI anatomy:** Window header fields: window time range; status tags **LIVE** / **GAME OVER · SETTLING** / **· FINAL** / **UPCOMING · opens 7:30 PM** / "Series ended"; stat header **To beat** / **Now** (+ signed move and %) / **Time left** (white → amber → red).
- **Key numbers/limits:** $1.00 per winning contract; 1¢–99¢; Live grid every 15 minutes on UTC boundaries; Trade: daily 00:00–23:59 UTC, weekly Monday 00:00 UTC, monthly the 1st 00:00 UTC; settlement cron every 5 min (plus an immediate page-triggered pass); stock hours; sports windows start at kickoff.
- **Warnings/caveats:** tie → Down; the settled price is the minute-bar feed's value at the close, which may differ by a tick from the last tape print; a game ending mid-window freezes the odds and pauses trading; a sell after the close is impossible; a "failed" settlement is held for manual review (position shows "awaiting settle").
- **In-app copy to reuse:** the full **Settlement rules** text (both variants, F-7); the ⓘ tie note; "Trading paused — game over" copy; "Series ended" copy; history details "paid $1 each" / "settled worthless" / "nothing owed either way".
- **Cross-links:** live-markets, trade-markets, placing-orders, positions-and-history.
- **Evidence:** `interval-market-view.tsx` `SettlementTerms`, `WindowChart`; `live/shared.tsx` `TieInfo`, `WindowTicket` (halted/ended branches); `lib/interval/markets.ts` (`windowFor`, `periodStart`, `cadencePhrase`, `stockHoursOpen`); `engine.ts` `settleClosedWindows`; `api/cron/interval-settle/route.ts`; `api/interval/settle-due/route.ts`.

### Trading / `trading/live-markets` — "Live markets"
- **Purpose:** Show what's on the Live hub and how a 15-minute market page reads.
- **Status:** NEW (replaces `trading/spot-trading`).
- **User flows:** 1. Click **Live**. 2. Filter with **All / Sports / Stocks / Crypto** (left rail; chips on phones). 3. Use the featured panel's **‹ ›** pager ("3 of 12") or scroll the cards. 4. Click a card, or its **Up 51¢** / **Down 49¢** button to open the market with that side preselected. 5. On team markets, switch the viewed team with the two-pill toggle above the chart (also in the ticket).
- **UI anatomy:**
  - Featured panel: category eyebrow; title "Bitcoin (BTC) · 15 min"; rows **Market / Pays out / Odds** (e.g. "Up | 1.96x | 51¢"; sports: team rows with **Up** / **Down** price buttons); **Closes in** (or **Opens in**); right: **To beat** (sub = close time), **Now** (sub = move), live game clock/score, 2-minute tape chart.
  - Card: image, title, "15 min", countdown; **To beat** ("2:15 PM close") / **Now** (move); side rows "▲ Up 51¢" with a bar; footer **LIVE** + "Last 6" outcome triangles (tooltip "2:00 PM window · $325.40 → $325.62 · Up").
  - Market page: header (image, title, window line, status), chart with lookbacks **Live / 5m / 15m / 1H**, **Past** dropdown + last-4 chips, **Live Positions**, outcome row "Up or Down | BTC Price: $77,271.11 · 51% ▲1" with **Up 51¢ / Down 49¢** + ⓘ, expandable **order book** (**Trade Up / Trade Down**; columns **Price / Shares / Total**; tags **Asks** / **Bids**; "Last 50¢ · Spread 2¢"; footnote), **Settlement rules**, right-rail ticket, "View the underlying market →" on sports.
  - Empty states: "No live sports markets right now" / "New recurring markets roll out here as their metrics go live."; "Waiting for the first quote"; "Waiting for a price…"; "Trading opens in 12:04" / **Upcoming**.
- **Key numbers/limits:** 15-minute windows; stocks weekdays 4:00 AM – 8:00 PM ET (hidden otherwise); crypto 24/7; sports listed automatically when a two-team game is in play with odds between 5 % and 95 %; ~20 contracts per side per window of house liquidity; tape refresh ~1.5 s (stocks), tick-by-tick (crypto/sports).
- **Warnings/caveats:** stock cards vanish off-hours; a sports market can end mid-window; the "Pays out" multiplier is 1/price and ignores the spread you'll actually pay.
- **In-app copy to reuse:** "Market closed — stock markets trade weekdays 4:00 AM – 8:00 PM ET." / "Crypto markets stay open 24/7."; order-book footnote; card/hub strings above.
- **Cross-links:** how-windows-work, placing-orders, trade-markets.
- **Evidence:** `live/interval-view.tsx`, `interval-market-view.tsx`, `live/shared.tsx` (`useIntervalLive`, `RecentCloses`, `GameClock`), `lib/interval/markets.ts` (`INTERVAL_MARKETS`, `STOCK_HOURS_LABEL`), `api/cron/interval-keeper/route.ts`, `packages/api/src/lib/interval/discover.ts` (band 5–95 %).

### Trading / `trading/trade-markets` — "Trade markets"
- **Purpose:** Explain the daily / weekly / monthly surface and its expiry picker.
- **Status:** NEW.
- **User flows:** 1. Click **Trade**. 2. Choose **All** (one card per subject, no prices) or an **Expiry**: **Daily / Weekly / Monthly**. 3. Click **Up** / **Down** on a card (prices show only with an expiry selected) or the card itself. 4. On the market page change the expiry from the headline menu: **15 Minutes / End-of-Day / End-of-Week / End-of-Month**.
- **UI anatomy:** rows **Outcomes** ("Live chance of a prediction-market outcome"), **Stocks** ("Live share price off the tape"), **Crypto** ("Live USD price, 24/7"); card: image, question/subject, expiry tag **DAILY / WEEKLY / MONTHLY**, **To beat / Now** (+ ▲/▼ move) or, in All, the live value with "chance"/"price" and "▲ 0.8 24h"; buttons **▲ Up 51¢ / ▼ Down 49¢**; footer "Expires today" / "Expires Sep 30 UTC" + countdown, or "Priced per expiry · daily, weekly, monthly". Empty: "No markets listed" / "Subjects are added from the admin's Markets tab."
- **Key numbers/limits:** window boundaries in UTC (daily 00:00–23:59, weekly Monday 00:00, monthly the 1st); countdown formats "18d 5:12:34"; chart opens on **MKT** with **1D / 1W / 1M** available.
- **Warnings/caveats:** Outcomes markets track a Polymarket probability, not the event itself — you're calling whether the chance rises; the value to beat is the previous period's close, which can be days old over a weekend for stocks; stock subjects still go dark outside tape hours.
- **In-app copy to reuse:** the two hub sub-headlines (F-7 Mode 2); "Call whether each market closes this window above or below where it stood at the previous period's close."
- **Cross-links:** how-windows-work, live-markets, placing-orders.
- **Evidence:** `trade/trade-view.tsx`, `trade/[slug]/page.tsx`, `lib/interval/resolve.ts` (`CADENCE_SIBLINGS`), `lib/interval/markets.ts` (`PV2_*`, `TRADE_CATEGORIES`, `periodStart`), `interval-market-view.tsx` (`CadenceMenu`, `LOOKBACKS`).

### Trading / `trading/placing-orders` — "Placing and closing orders"
- **Purpose:** Every control in the ticket, how fills work, and how to get out.
- **Status:** REWRITE of `trading/order-types`.
- **User flows:**
  - Quick Buy: 1. Open a market. 2. Ticket → **Quick Buy** (default). 3. Press and hold **$1 / $5 / $10** under **Up** or **Down** until the box fills. 4. Read **Trade executed**.
  - Market buy: 1. Order type → **Market**. 2. **Buy**. 3. Pick the side pill (**Up 51¢** / **Down 49¢**). 4. Enter **Dollars**. 5. Check **Odds** and **Max payout**. 6. Click **Buy Up**.
  - Sell back (ticket): 1. **Market** → **Sell**. 2. Side pill. 3. **Dollars** to sell. 4. **Sell Up**.
  - Sell back (panel): hold **Sell** on the side's row in **Live Positions** until "Selling…".
  - Phone: expand the bottom sheet (grip), **Quick** → tap **Up 51¢** → hold a box; or **Market** / **Limit** forms.
- **UI anatomy:** table of every ticket element (F-7 ticket anatomy); order book columns; **Trade executed** card fields (verb, $ amount, side, price, "Pays $X if <side> wins", "+$X realized", "· settling…", partial-fill line).
- **Key numbers/limits:** presets $1/$5/$10; default Market amount $50; hold ~0.7 s; house depth ~20 contracts/side/window over 6 price levels; automatic price protection ±30 % + 3¢; no fee; spread ≥ 2¢; no minimum; sells limited to contracts held.
- **Warnings/caveats:** **Limit orders do not work yet** — the button reads "Limit orders open with the order book"; partial fills; rejections when the price moves; "Insufficient balance: need X pUSD, have Y."; buying power excludes money in open windows; trading is refused after game over and outside stock hours ("Market is not open for trading right now."); a fresh position marks at the midpoint on Portfolio but at the realizable bid on the market page, so the two can differ slightly.
- **In-app copy to reuse:** all error strings in F-7; "Hold a box to buy"; "Placing…"; "Resting limit orders open with the order book (CLOB). Market orders trade now."; "Interval trades fill instantly, so nothing rests here yet. Limit orders for intervals arrive with the order book."
- **Cross-links:** how-windows-work, positions-and-history, portfolio overview.
- **Evidence:** `live/shared.tsx` (`WindowTicket`, `useIntervalTrade`, `QuickBuyBox`, `PositionPanel`, `TradeExecutedCard`), `packages/api/src/routers/interval.ts` (`buy`, `sellToClose`), `engine.ts` (`executeFill`, `OI_CAP`, `FEE_BPS`), `packages/shared/src/interval/quote.ts`.

### Portfolio / `portfolio/overview` — "Portfolio overview"
- **Purpose:** Read the Portfolio page and the nav chip; understand Cash vs Portfolio.
- **Status:** REWRITE.
- **User flows:** 1. Click **Portfolio**. 2. Switch the chart range **1D / 1W / 1M / 1Y / ALL**; hover for a point-in-time value. 3. Tabs **positions / Open orders / history**.
- **UI anatomy:** Balance card: **Balance** (total), **Available**, **In position**, **Total traded**, **Deposit**, **Withdraw**. Portfolio value card: value, "+$X (+Y%) Past week", range buttons, white line chart. Nav chip **Portfolio / Cash** (tooltip). Formulas (F-6).
- **Key numbers/limits:** refresh intervals (F-6); chart snapshots every 15 min; default range 1W.
- **Warnings/caveats:** **Total traded** is face value ($1 per contract, both buys and sells), not money spent; **In position** uses the midpoint mark, so a new buy shows ~flat rather than the spread loss; money in open windows is neither withdrawable nor spendable until the window settles; legacy sections (Options, Markets, Open orders list, Written) appear only for older accounts.
- **In-app copy to reuse:** "Sign in to view your portfolio" / "Your cash and positions live here."; "No open positions yet." / "Browse intervals"; "No history yet." / "Your trades and settlements will show up here."; Open orders empty copy.
- **Cross-links:** deposit-withdraw, positions-and-history, placing-orders.
- **Evidence:** `portfolio/portfolio-view.tsx`, `packages/api/src/routers/portfolio.ts` `summary`, `packages/api/src/lib/interval/portfolio.ts`, `Navbar.tsx`, `vercel.json`.

### Portfolio / `portfolio/deposit-withdraw` — "Deposits and withdrawals"
- **Purpose:** Money in and money out, exactly.
- **Status:** KEEP with edits.
- **User flows:** Deposit: 1. Click the **Portfolio / Cash** chip or Portfolio → **Deposit**. 2. (First time) **Allow trading** → **Done**. 3. Under **Deposit** click **Copy Address** (or scan the QR). 4. Send ≥ $2 native USDC on Polygon. 5. Watch the stage line; wait for "Deposit converted to pUSD". 6. **Go to Portfolio**. Withdraw: 1. Portfolio → **Withdraw**. 2. Paste **Recipient address (Polygon)**. 3. Enter **Amount (USDC)** or **MAX**. 4. Confirm **You receive**. 5. Click **Withdraw**. 6. **View transaction →** / **Done**.
- **UI anatomy:** Wallet modal fields and Withdraw modal fields (F-5).
- **Key numbers/limits:** minimum $2 both ways; no maximum; no fees; gas sponsored; sweep check every 20 s, bridge status every 5 s; withdrawals "usually instant"; open-position lock.
- **Warnings/caveats:** "Wrong tokens or networks may result in permanent loss."; sub-$2 deposits wait for a top-up; keep the modal open (or return) for conversion; "Withdrawals are irreversible — double-check the address."; the withdraw **Available** figure does not subtract money in open windows — the server will refuse with "…($Z.ZZ is committed to open interval positions)."; banned accounts can still withdraw; the balance line still says "your tradeable balance on Polymarket" (H-1).
- **In-app copy to reuse:** every string in F-5.
- **Cross-links:** account-setup, overview, support.
- **Evidence:** `WalletModal.tsx`, `WithdrawModal.tsx`, `packages/api/src/routers/deposits.ts`, `packages/api/src/lib/trading/constants.ts` (`BRIDGE_MIN_USD`).

### Portfolio / `portfolio/positions-and-history` — "Positions and history"
- **Purpose:** Read an open position, close it, and read the history feed and settled-window pages.
- **Status:** REWRITE of `portfolio/positions` (renamed).
- **User flows:** 1. Portfolio → **positions** → section **Interval markets** → click **Trade** to open the market (or **View** if it's settling). 2. Close from the market page's **Live Positions** (hold **Sell**). 3. Portfolio → **history** → filter **All / Interval** → click a row title to open the settled-window page.
- **UI anatomy:** Position row fields: image, title, "<Side> · <cadence> · closes in Xm / settling", **Contracts**, **Entry** (¢), **Now** (¢ or —), **Pays** ($ if side), value, unrealized "+$X (+Y%)" or "awaiting settle", **Trade** / **View**. History row: label chip **Bought / Sold / Won / Lost / Settled**, title + side (side color), detail ("3.00 contracts @ 51¢", "· paid $1 each", "· settled worthless", "· nothing owed either way"), timestamp, signed cash ("+$3.00" / "−$1.53"), P&L with %. Settled-window page: header "<category> · 15 min · settled", **Result** ("Up won" / "Rangers up" / "unchanged — settles down"), **Open**, **Close**, static chart with "Target" tag.
- **Key numbers/limits:** positions refresh 10 s; history 30 s; history capped at 200 rows; a settled window page appears within seconds to ~5 minutes.
- **Warnings/caveats:** a sold-out window shows only its Sold rows (no $0 Settled row); a hedged window shows the winning side; legacy Options/Markets/Written sections and the **Redeem**, **Sell**, **Share** buttons are options/spot-era.
- **In-app copy to reuse:** "This window isn't recorded yet" / "Windows are saved shortly after they close."; "No position in this window yet."
- **Cross-links:** how-windows-work, placing-orders, overview.
- **Evidence:** `portfolio-view.tsx`, `packages/api/src/lib/interval/portfolio.ts`, `live/shared.tsx` `PositionPanel`, `live/[slug]/[window]/window-view.tsx`.

### Platform / `platform/finding-markets` — "Finding markets"
- **Purpose:** How a user discovers what to trade now that there is no search.
- **Status:** REWRITE of `platform/markets` (renamed).
- **User flows:** Live: category rail → featured pager → cards. Trade: expiry rail → rows → cards. Market page: cadence menu; **Past** dropdown lists recent closes ("2:15 PM ET · Today") linking to window pages.
- **UI anatomy:** rails, cards and header (F-7, D-5, D-6).
- **Key numbers/limits:** hub registry refresh 60 s; "Last 6" outcomes on cards; **Past** shows up to 8 windows.
- **Warnings/caveats:** **Ctrl/⌘+K** opens a legacy search that only finds prediction markets and leads to the archived spot page — advise against it; stock markets hide off-hours; sports markets appear/disappear with games.
- **In-app copy to reuse:** hub empty states; "Change expiry" tooltip.
- **Cross-links:** live-markets, trade-markets.
- **Evidence:** `interval-view.tsx`, `trade-view.tsx`, `live/shared.tsx` `RecentCloses`, `CommandPalette.tsx`.

### Platform / `platform/leaderboard` — "Weekly competition" *(conditional on H-6)*
- **Purpose:** Explain the weekly P&L contest.
- **Status:** KEEP with edits.
- **User flows:** 1. Open `/app/leaderboard` (or click the "Weekly competition ends in …" banner in the final 6 hours). 2. Click **Rules & Prizes**.
- **UI anatomy:** header (title, "Weekly Trading Competition", **This week** "Sep 11 – Sep 17 ET", **Resets in**); podium; table `#` / Trader / **Volume** / **P&L** / **24h**; your row pinned; banner text.
- **Key numbers/limits:** Friday 12:00 AM → Thursday 11:59 PM ET; prizes $100 / $50 / $25 / $5 × 7; refresh 10 s; only settled windows closing inside the week count.
- **Warnings/caveats:** not in the nav; prizes paid manually (no payout code); ET is approximated as UTC−4 in code; admin/hidden accounts excluded.
- **In-app copy to reuse:** rules bullets (F-11); "No trades yet this week." / "Be the first on the board."
- **Cross-links:** how-windows-work, positions-and-history.
- **Evidence:** `leaderboard/leaderboard-view.tsx`, `packages/shared/src/competition.ts`, `CompetitionDeadlineBanner.tsx`, `Claude Docs/38`.

### Platform / `platform/rewards` — DELETE
- Reason: no incentive is wired to the Updown product (F-11). If H-5 keeps referrals, add a short "Referrals" section to `account-setup` using the page copy in F-11 with the caveat that interval trades carry no fee.

### Help / `help/faq` — "FAQ"
- **Status:** REWRITE. Content: Section G.

### Help / `help/glossary` — "Glossary"
- **Status:** REWRITE. Content: Section F.

### Help / `help/support` — "Support" (NEW, optional)
- **Purpose:** How to reach the team and what happens to restricted accounts.
- **User flows:** 1. Click **Contact us** in the bottom bar. 2. Check **Username** / **Email**. 3. Type **Message** (≤ 5000). 4. **Send Message** → "Message Sent!".
- **UI anatomy:** channel cards EMAIL / X (TWITTER) / DISCORD; form fields.
- **Warnings/caveats:** form needs a signed-in account (Username is prefilled and required); no status page; response times unstated; restricted accounts keep withdrawals.
- **In-app copy to reuse:** F-10 banned strings; F-14 page copy.
- **Evidence:** `contact/page.tsx`, `StatusBar.tsx`, `BannedNotice.tsx`, `trpc.ts`.

---

## E. Stale baseline disposition

| ID | Disposition | Replacement fact (if REPLACE) | Evidence |
|---|---|---|---|
| B-1 | REPLACE | Product name **updown** (lowercase). Support handles unchanged: `hello@thetalabs.gg`, X `@thetalabsgg`, Discord `discord.gg/d9gfrYWSAu` (also `discord.com/invite/d9gfrYWSAu`). Whether the company remains "Theta Labs" → H-1. | `Navbar.tsx`, `layout.tsx`, `contact/page.tsx` |
| B-2 | KEEP | **Start Trading** (landing nav and hero) → now lands on `/app/live`. | `page.tsx`, `options-landing.tsx` |
| B-3 | REPLACE | Privy; login with **email or Google only** (no external wallet); embedded wallets created on Polygon *and* Solana (Solana hidden); "non-custodial" is the app's claim ("We never hold your keys") but the server signs via delegation — H-2 for wording. | `PrivyClientProvider.tsx`, `AllowTrading.tsx` |
| B-4 | KEEP | Name (required), Phone (optional), four discovery dropdowns (trader type, where you trade, experience, how you heard). | `login/page.tsx` |
| B-5 | REPLACE | One-time **Allow trading** (panel heading "Enable trading") inside the **Wallet** modal; no revoke control in the app (copy says "Revocable anytime"). | `AllowTrading.tsx`, `use-trading-permission.ts` |
| B-6 | KEEP | **Deposit wallet (Polygon)** and **Signing wallet (Polygon)** on Profile with copy + Polygonscan. | `profile/page.tsx` |
| B-7 | KEEP | Same flow and strings; the rules sentence is now "Send at least **$2 of USDC** on **Polygon** to the address below — it converts automatically. Wrong tokens or networks may result in permanent loss." First deposit deploys the deposit wallet silently. | `WalletModal.tsx`, `deposits.ts` |
| B-8 | REPLACE | Same (native USDC, any Polygon address, gas-free, "usually instant", **minimum $2**, Polygonscan link) **plus** the open-position lock: funds committed to open windows cannot be withdrawn. | `WithdrawModal.tsx`, `deposits.ts` lines 266–277 |
| B-9 | REPLACE | Balance card cells **Available / In position / Total traded** with **Deposit** / **Withdraw** stay; the nav now shows a two-cell **Portfolio / Cash** chip (not a cash pill) that opens **Wallet**. | `portfolio-view.tsx`, `Navbar.tsx` |
| B-10 | REPLACE | Portfolio = Cash (pUSD − money committed to open windows) + live value of open positions. Nav chip **Portfolio** shows it; **Cash** shows the free balance. | `routers/portfolio.ts` `summary` |
| B-11 | KEEP | 1D / 1W / 1M / 1Y / ALL, default 1W, hover cursor; refresh 10–60 s per panel. | `portfolio-view.tsx` |
| B-12 | REPLACE | Top nav **Live · Trade** (left) and **Portfolio** (right, plus **Admin** for admins); no trophy, no search box; **Portfolio / Cash** chip; user menu. `/app` → **Live**. Mobile bottom tabs **Live / Trade / Portfolio**; hamburger drawer. Leaderboard reachable only via the deadline banner. | `Navbar.tsx`, `MobileNav.tsx`, `app/app/page.tsx` |
| B-13 | REPLACE | Flagship = real-money **Up / Down contracts** on whether a live number closes a window above its open; $1 per winning contract; buyers only, no shorting/writing. Options are archived. | F-7 |
| B-14 | DROP | No options chain. (Order book columns are **Price / Shares / Total**; capacity shows as thin ladders/partial fills, no "Sold out".) | `interval-market-view.tsx` |
| B-15 | REPLACE | Ticket: **Quick Buy** ($1/$5/$10 hold-to-buy), **Market** (**Dollars** input, **Odds**, **Max payout**, **Buy Up** / **Sell Up**), **Limit** (disabled). No payoff chart, no contracts/dollars toggle, no 1¢ minimum. | `live/shared.tsx` |
| B-16 | REPLACE | Fills instantly against the house quote; **no fee**; spread ≥ 2¢ widening toward expiry near 50/50; automatic price protection ±30 % + 3¢. | `quote.ts`, `engine.ts` |
| B-17 | REPLACE | Live: every 15 minutes on the UTC grid, rolling continuously. Trade: daily (00:00–23:59 UTC), weekly (Mon 00:00 UTC), monthly (1st 00:00 UTC). Stocks weekdays 4:00 AM – 8:00 PM ET; crypto 24/7; sports from kickoff to final whistle. No 8 pm ET expiry. | `markets.ts` |
| B-18 | REPLACE | Settlement = value at the window close vs value at the open (minute feed; frozen odds at game over); strictly higher → Up else Down; payout $1 × winning contracts, net moved on-chain automatically within ~5 min; explainer is the **Settlement rules** accordion (no "How & When This Settles"). No TWAP. | `engine.ts`, `SettlementTerms` |
| B-19 | REPLACE | Position row: side label, cadence, "closes in …"; **Contracts / Entry / Now / Pays**; value + unrealized; action **Trade** / **View**. Selling = hold **Sell** in **Live Positions** or ticket **Sell** mode; realized P&L shown on the **Trade executed** card. No Buy/Share buttons. | `portfolio-view.tsx`, `PositionPanel` |
| B-20 | DROP | Spot trading is archived (reachable only at `/app/predictions?inst=spot`). Polymarket now appears only as a data source for Outcomes/sports. | F-15 |
| B-21 | DROP | — | — |
| B-22 | REPLACE | Interval orders are market-style fills against the house; "Limit orders open with the order book" (disabled button). | `WindowTicket` |
| B-23 | DROP | No Redeem step; settlement pays automatically. (The **Redeem** button survives only on legacy spot rows.) | `portfolio-view.tsx` |
| B-24 | REPLACE | Live hub filter **All / Sports / Stocks / Crypto**; Trade hub rail **All / Daily / Weekly / Monthly** with rows **Outcomes / Stocks / Crypto**. No Trending/Hot/New/Ending-soon, no topic categories. | `interval-view.tsx`, `trade-view.tsx` |
| B-25 | REPLACE | No search box. **Ctrl/⌘+K** still opens a legacy prediction-market search that routes to the archived spot page — omit or list as legacy. | `CommandPalette.tsx` |
| B-26 | REPLACE | Live card: image, title, "15 min", countdown, **To beat / Now**, side rows "▲ Up 51¢" with bars, **LIVE**, "Last 6" triangles. Trade card: expiry tag, **To beat / Now**, **Up 51¢ / Down 49¢**, "Expires today". | D-5, D-6 |
| B-27 | REPLACE | Market page: header + cadence menu, **To beat / Now / Time left**, tape chart (**Live / 5m / 15m / 1H**, longer on Trade), **Past**, **Live Positions**, outcome row, order book (**Trade Up / Trade Down**, 6 levels each side), **Settlement rules**, right-rail ticket; sports add the game clock and "View the underlying market →". No Share, no About, no Recent orders. | `interval-market-view.tsx` |
| B-28 | REPLACE | Positions sections **Interval markets** (+ legacy **Options**, **Written (covered)**, **Markets**); tabs **positions / Open orders / history**; history filters **All / Interval / Spot / Options**. | `portfolio-view.tsx` |
| B-29 | DROP | Reward reveal / streak / first-deposit rewards live only on the archived options hero. | `options-hero.tsx` |
| B-30 | REPLACE | Weekly competition now ranks **realized interval-market P&L** (from Sep 11 2026); same round (Fri 12:00 AM → Thu 11:59 PM ET) and prizes ($100 / $50 / $25 / $5 × 4th–10th, pUSD); columns **# / Trader / Volume / P&L / 24h**; **Rules & Prizes** modal; page is off the nav. | `leaderboard-view.tsx`, `competition.ts` |
| B-31 | REPLACE | Nav globe icon with tooltip "Trading is unavailable in your region"; no viewing-only banner; **interval trading is not actually blocked** (H-3). | `Navbar.tsx`, `middleware.ts` |
| B-32 | REPLACE | No platform fee, no pass-through fees; only the house spread. | `engine.ts` `FEE_BPS` |
| B-33 | KEEP | **Contact us** page (5,000-char message), email, X, Discord. | `contact/page.tsx` |
| B-34 | REPLACE | See Section F for the new list and the drops. | — |
| B-35 | REPLACE | Now also: `/app/predictions` (+ spot/options), `/app/arena/*`, `/app/collect`, `/app/compute`, `/app/survey`, `/app/referrals` (options-fee based), `/app/leaderboard` (off-nav), Ctrl+K palette; still `/app/narratives`, `/app/social`, `/app/api-docs`, Solana wallet. | F-15 |

---

## F. Glossary

Terms as the product uses them. **(UI)** = appears as a label on screen.

- **Allow trading (UI)** — The one-time approval button in the Wallet modal that lets updown convert deposits and place trades without a signature per action.
- **Available (UI)** — pUSD you can spend or withdraw right now: your balance minus what's committed to open windows. Also the balance line in the Wallet and Withdraw modals (there: the raw pUSD balance).
- **Balance (UI)** — The Portfolio page's headline figure: Available + In position.
- **Buying power (UI)** — Same idea as Available, shown in the market page's Live Positions panel.
- **Cash (UI)** — The nav chip's free-balance cell: pUSD not tied up in open positions.
- **Contract (UI)** — The unit you buy: one Up or Down claim on a window, priced 1¢–99¢, paying $1.00 if its side wins and $0 if not.
- **Daily / Weekly / Monthly (UI)** — Trade expiries. One window per UTC day (00:00–23:59), per week (Monday 00:00 UTC), or per month (the 1st, 00:00 UTC).
- **Down (UI)** — The side that wins if the market's value closes the window at or below the level to beat (ties go to Down). Team markets name it after the second team.
- **End-of-Day / End-of-Week / End-of-Month (UI)** — The cadence menu's names for the Daily / Weekly / Monthly markets of a subject.
- **Entry (UI)** — Your average purchase price for a side, in cents.
- **Expiry (UI)** — On Trade, which window length you're trading; a card's "Expires today / Expires Sep 30 UTC" footer.
- **In position (UI)** — The live value of everything you hold in open windows, marked at the current midpoint.
- **Interval market** — Internal name for any up/down window market (appears in Portfolio as the "Interval markets" section and the "Interval" history filter).
- **Limit (UI)** — An order type in the ticket that is not yet available ("Limit orders open with the order book").
- **Live (UI)** — The hub of 15-minute markets on stocks, crypto and in-play sports.
- **Live Positions (UI)** — The market page panel listing your contracts in the current window with Sell buttons.
- **Market (UI)** — One tracked number running a series of windows (e.g. "Apple (AAPL)"); also the ticket's order type that spends a dollar amount at the live quote.
- **Max payout (UI)** — What a Market order returns if the side wins: dollars ÷ price.
- **Now (UI)** — The market's live value.
- **Odds (UI)** — The chosen side's price read as a probability ("51% chance").
- **Order book (UI)** — The house's resting Up/Down quote: Asks above, Bids below, "Last" and "Spread" in between. Columns Price / Shares / Total.
- **Past (UI)** — The market page dropdown of recently settled windows, each linking to its history page.
- **Pays (UI)** — Dollars you receive if the side wins = contracts held ("pays $5.00 if Up").
- **Polygon** — The blockchain your wallet lives on; deposits and withdrawals are USDC on Polygon.
- **Portfolio (UI)** — Cash plus the live value of open positions (nav chip and page).
- **pUSD** — Your trading balance, created automatically from USDC deposits and turned back into native USDC on withdrawal.
- **Quick Buy (UI)** — Hold-to-buy boxes for $1, $5 and $10.
- **Realized (UI)** — Profit or loss locked in by selling back or by settlement ("+$0.40 realized").
- **Settlement / Settled (UI)** — The automatic close-out of a window: winning contracts are paid $1 each, the net moved on-chain; history rows read Won, Lost or Settled.
- **Settling (UI)** — A position whose window has closed but hasn't been paid yet (usually under 5 minutes).
- **Signing wallet (Polygon) / Deposit wallet (Polygon) (UI)** — Your two Polygon addresses: the one you send USDC to, and the one that holds pUSD.
- **Spread (UI)** — The gap between the house's best ask and best bid, at least 2¢.
- **Time left (UI)** — Countdown to the window close; turns amber, then red.
- **To beat (UI)** — The market's value when the window opened; Up needs a close strictly above it.
- **Total return (UI)** — Your realized P&L on one market, in dollars and as a percentage of what you staked.
- **Total traded (UI)** — Face-value volume: $1 per contract across every buy and sell.
- **Trade (UI)** — The hub of daily / weekly / monthly markets on outcomes, stocks and crypto.
- **Trade executed (UI)** — The confirmation card after a fill.
- **Unrealized (UI)** — Current value minus cost of the contracts you still hold.
- **Up (UI)** — The side that wins if the value closes the window strictly above the level to beat. Team markets name it after the tracked team.
- **USDC** — The stablecoin you deposit and withdraw (native USDC on Polygon).
- **Window (UI)** — One round of a market with a fixed open and close.
- **Won / Lost (UI)** — History labels for settled positions.

**Drop from B-34:** Breakeven, Call option, CLOB, Event, Implied probability, Liquidity, Market order (as a distinct concept), Max return, No Call, NO share, Polymarket (keep only as a data-source mention), Premium, Settlement (options), Settlement (spot), Sold out, Spot trading, Strike, Volume (replace with Total traded), Yes Call, YES share. **Keep with new definitions:** Available, Embedded wallet (fold into Signing wallet), Expiry, P&L, pUSD, Realized P&L, Spread, Unrealized P&L.

---

## G. FAQ candidates

1. **What am I actually betting on?** Whether a live number — a stock or crypto price, a team's live win odds, or a prediction-market outcome's chance — closes a window above where it opened. You buy Up or Down contracts; each winning contract pays $1.00. *Evidence: `SettlementTerms`, `engine.ts`.*
2. **What is the most I can lose?** What you paid for your contracts. There is no leverage, shorting or margin; positions are fully collateralized when you enter. *`SettlementTerms` paragraph 3; `engine.ts` `executeFill`.*
3. **What happens if the price finishes exactly where it started?** It settles Down: "If the price is unchanged, it settles as Down. Up needs a real move." *`TieInfo`, `engine.ts` line 946.*
4. **Is there a trading fee?** No. There is no fee today; the only cost is the spread between the house's bid and ask (at least 2¢, wider near the close when the market is 50/50). *`engine.ts` `FEE_BPS = 0`; `quote.ts`.*
5. **Who is on the other side of my trade?** updown's treasury quotes both sides of every window. It caps its exposure at roughly 20 contracts per side per window, so large orders can fill partially. *Order-book footnote; `OI_CAP`.*
6. **Why did my order get rejected with "Price moved to …"?** The quote changed between what you saw and the fill. The app refuses fills more than about 30 % + 3¢ worse than the price shown. Just try again at the new price. *`useIntervalTrade`, `executeFill`.*
7. **Can I sell before the window closes?** Yes. Hold **Sell** on your row in **Live Positions**, or use **Market → Sell** in the ticket. You're paid or charged the realized difference immediately. *`PositionPanel`, `sellToClose`.*
8. **When do I get paid?** Automatically. A settlement pass runs every 5 minutes (and the page triggers one when a window rolls); your history shows **Won** or **Lost** and Cash updates. Until then the position reads "settling" / "awaiting settle". *`vercel.json`, `settle-due/route.ts`, `portfolio-view.tsx`.*
9. **Why can't I trade Apple right now?** Stock markets run only weekdays 4:00 AM – 8:00 PM ET; the page says "Market closed — stock markets trade weekdays 4:00 AM – 8:00 PM ET." Crypto markets stay open 24/7. *`stockHoursOpen`, `interval-market-view.tsx`.*
10. **The game ended in the middle of a window — what happens?** The odds freeze, trading pauses ("Trading paused — game over"), and the window settles at the frozen odds; payouts follow shortly. No new windows open after the final one. *`WindowTicket` halted branch; `serverQuote` `frozenAtMs`.*
11. **How do I deposit?** Open the **Wallet** modal from the Portfolio/Cash chip, click **Allow trading** once, then send at least $2 of USDC on Polygon to the address shown. It converts to pUSD automatically; keep the page open or come back later. *`WalletModal.tsx`.*
12. **I sent less than $2 — is it lost?** No. "…deposits under $2 can't be converted. Send at least $X more USDC and it all converts automatically — your funds are safe meanwhile." *`WalletModal.tsx` lines 194–200.*
13. **I sent the wrong token or network.** The app warns that "Wrong tokens or networks may result in permanent loss." Contact support with the transaction hash; there is no in-app recovery. *`WalletModal.tsx`; H-9.*
14. **How do I withdraw, and is there a fee?** Portfolio → **Withdraw**, any Polygon address, minimum $2, paid in native USDC, gas sponsored, usually instant. Money in open windows can't be withdrawn until those windows settle. *`WithdrawModal.tsx`, `deposits.ts`.*
15. **What's the difference between Portfolio and Cash?** Cash is pUSD not tied up in open windows. Portfolio adds the live value of your open positions. *Nav tooltip; `portfolio.summary`.*
16. **What's the difference between Live and Trade?** Live runs 15-minute windows on stocks, crypto and in-play sports. Trade runs one window per day, week or month on stocks, crypto and prediction-market outcomes. Same ticket and rules. *`Navbar.tsx`, `markets.ts`.*
17. **Can I place a limit order?** Not yet. The Limit form is visible but its button reads "Limit orders open with the order book". *`WindowTicket`.*
18. **Why does my position show a slightly different value on the market page and on Portfolio?** The market page marks at what the house would actually pay you now (its bids); Portfolio marks at the midpoint. Both converge as you sell or settle. *`PositionPanel`, `myIntervalPositions`.*
19. **Which countries can't use updown?** The app flags visitors from a list that includes the US, UK, Canada, Australia, most of the EU and others with "Trading is unavailable in your region." *`middleware.ts`; H-3 for the official wording.*
20. **My account says "Account flagged" — what now?** Trading, deposits and rewards are restricted, but "Your balance is not affected — you can still withdraw at any time." Contact Discord or hello@thetalabs.gg. *`BannedNotice.tsx`.*
21. **How do I contact support?** **Contact us** in the bottom bar (form, up to 5,000 characters), email hello@thetalabs.gg, X @thetalabsgg, or Discord. *`contact/page.tsx`, `StatusBar.tsx`.*

---

## H. Needs team confirmation

1. **Company / legal name and leftover branding.** Is "Theta Labs" still the entity behind updown? The login page, the Enable-trading copy, tab titles, OG images, and every support handle (`hello@thetalabs.gg`, `@thetalabsgg`, `thetalabs.gg`, `docs.thetalabs.gg`) still say Theta Labs. Decide whether docs mention it and whether these strings will be rebranded before the docs ship.
2. **Custody wording.** The app says "We never hold your keys" and "Revocable anytime", but trades are ledger entries executed by the server under a Privy delegation, and no revoke control exists. Confirm the sentence the docs should use.
3. **Geo policy.** The blocked-country list (incl. **US**) only shows a nav icon; interval trading is not enforced server- or client-side. Confirm the official "not available in" list and whether enforcement ships before the docs state it.
4. **Terms of service, privacy policy, risk disclosure.** None exist in code. Provide URLs and any required disclaimer text.
5. **Referrals page.** It pays 10 % of *options* fees; interval trades have no fee, so referrers earn nothing. Keep, rewrite, or hide? (It's linked from the user menu.)
6. **Weekly competition.** Off the nav since Sep 14; prizes ($100/$50/$25/$5×7) have no payout code, so they're manual. Document it or not? Confirm prize amounts and the ET/EDT convention.
7. **Data-source wording.** The settlement rules say "Robinhood tape, the live bid and ask midpoint" while the code uses Massive (Polygon.io) last trades, Coinbase for crypto, and a Pyth pilot for TSLA. Choose the public wording (and whether vendors are named at all).
8. **Fee statement.** Fees are off (`FEE_BPS = 0`) but wired. Should the docs say "no fees" or "no fees at launch"?
9. **Support policy for wrong-network deposits**, expected response times, and whether a status page exists.
10. **Official URLs**: confirm `thetalabs.gg` / `app.thetalabs.gg` / `docs.thetalabs.gg` remain, or the updown domains that replace them (the referral link uses whatever origin serves the page).
11. **Brand colors for `docs.json`** and the favicon source (`apps/web/app/icon.svg` vs a hosted asset).
12. **Orphaned pages** (F-15): confirm they stay unpublished in docs and whether `/app/leaderboard` and `/app/referrals` are returning to the nav.
13. **Discord market-listing alerts**: is the channel public? If so the docs can point users there for new listings.
14. **Sports listing rules** for the docs: the keeper auto-lists "the busiest in-play two-team game whose odds are still in doubt (5–95 %)" per sport — confirm that's the public description.
15. **Order-book column "Shares"** vs the product word "contract": confirm the docs may call them contracts throughout.

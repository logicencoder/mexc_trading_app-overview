# MEXC Trading Dashboard + Spot Bot

![MEXC Trading Dashboard — live book, tape, balances, and bot controls](assets/trading-dashboard.png)

Self-hosted **MEXC spot trading console** — live order book, trade tape, balances, open orders, fill history, deposit notifications, manual order buttons, and **five automation modes** in one browser dashboard on **a machine you control**. Active traders and bot operators keep API keys local; traffic goes outbound to MEXC only. The exchange retail UI scatters book, account, and bot state across pages and often shows **stale depth after a symbol change**; this product keeps everything on one screen with protobuf WebSocket feeds, REST backfill when a stream lags, and symbol-switch hygiene so old feeds do not paint the wrong market.

## Tech stack

| Layer | Technologies |
|-------|--------------|
| Backend | Python 3, FastAPI, uvicorn, asyncio worker queues |
| Bot engine | MODE1–MODE5 automation profiles, multi-bot registry |
| Frontend | HTML, CSS (Tailwind-style dark UI), vanilla JavaScript |
| Exchange | MEXC Spot V3 REST + protobuf WebSocket |
| Public streams | Aggregated deals, limit depth order book |
| Private streams | Listen-key account, orders, deals; deposit poll + WS queue |
| Persistence | SQLite for funds events, deals, and bot log history |
| Diagnostics | System Stats and Debug Logs tabs, client log ingest |
| Deployment | Shell launcher or optional Docker on the same host |

## Top bar — symbol, health, and alerts

The header stays visible on every workspace tab:

- **Live symbol** with last price and direction arrow; **symbol input** normalizes pairs (e.g. `DNX` → `DNXUSDT`) and switches the active market so public and private feeds unsubscribe the old pair before subscribing the new one.
- **Feed LEDs** — trades, balances, orders, deals, deposits, order book, bot logs, status WebSocket, and REST API access — each with a message counter so you see which pipe is alive without opening logs.
- **Favorites** — star the active pair; up to five quick-switch buttons in the notification bar.
- **Notification ticker** — scrollable bar for fills, deposits, withdrawals, bot events, and connection notices; filters and TTL live in Settings (bot logs, funds, connections).
- **Settings menu** — panel heights (trades, order book, chart, activity), notification bar size, coin-search width, layout save/reset, UI snapshot export.

## Trading workspace layout

The default **Trading** tab is a three-column, **drag-and-drop** dashboard. Every panel collapses, reorders (persisted in the browser), and resizes where allowed.

### Bot panel — five automation modes

The left **Bot** column is the automation control center: mode picker, buy/sell side, price range, sizing, budgets, randomization, presets, dry-run, start/stop, live status (executions, volume, safety halt), and a **Bot Settings Summary** card.

| Mode | Behavior |
|------|----------------|
| **MODE1 — Instant execution** | Watches the live order book; when liquidity appears inside your min/max range, fires **limit IOC** orders (no resting liquidity). Execution style: **TAKE**, **PING**, **50/50 ALTERNATE**, or **RANDOM**; optional `ping_hold_ms` pause between fills. Cooldown and stuck-order recovery if a cancel fails. |
| **MODE2 — Grid limit orders** | Places a limit inside the band and **replenishes after each fill**; optional random price pick within range. |
| **MODE3 — Step re-entry ladder** | One limit at a time; after each **full fill**, moves the next order by a **step percent** from start price or last fill. Direction up/down, optional **stop price**, continue-until-budget vs stop-at-price. |
| **MODE4 — Front of book** | Keeps **one resting limit at best bid/ask** (plus configurable tick jump); poll loop **re-quotes** when displaced from the front of the book. |
| **MODE5 — Scheduled exact** | Arms a **single limit at HH:MM:SS** with exact price and quantity; live countdown; optional **one-tick jump** using top-of-book before submit. One shot per start. |

Shared bot limits (all modes that need them): min/max price band, per-order size, max order size cap, **total budget** in coins or USDT (`max_sum_use`), random price (MODE1/2) and random amount range, min-notional guard from exchange filters, **dry run**, and **safety halt**. **Presets** save/load/delete full bot configs in localStorage; **Multi Bots** (below) launches independent profiles with their own `bot_id`.

### Trading panel — manual orders

Manual **Buy/Sell** with limit or market type, price and quantity spinners, **25/50/75/100%** balance shortcuts, quote/base preset buttons ($25–$250 / 0.1–1 coin), market **slippage tolerance**, server-side precision and min-notional checks, and REST refresh when the orders WebSocket goes stale.

### Chart panel

**TradingView** embed or **Custom** placeholder chart tied to the active symbol; trading-allowed badge from exchange filters.

### Order book

Dual-sided depth (asks/bids) with cumulative coins and USDT, spread line, sortable price columns, optional **highlight my resting orders**, live protobuf depth feed.

### Activity panel

Three sub-tabs in one stack:

- **Open orders** — cancel/modify actions, current-symbol vs all-symbol toggle, REST refresh button.
- **History** — recent closed/canceled orders for the session.
- **Bot logs** — streaming bot log lines while a bot runs (also persisted server-side).

### Account balances

Available, locked, and total per asset for monitored base/quote coins; privacy hide toggle and manual balance refresh.

### Live trades tape

Public agg-deals stream: time, price, amount, USDT notional, side; trade count and last-price arrow in the panel header.

## Funds History workspace

Separate tab — **Funds Full History** from SQLite: deposits and withdrawals with asset, status, amount, network, tx id, and timestamp. Filter by kind (all / deposit / withdrawal) and asset code; complements the live deposit notification stream in the top bar.

## System Stats workspace

Feed-health dashboard for incident triage — per-stream message totals, stale-feed count, average WebSocket age, API OK/OFF, debug log totals/errors, open-order cache size, and a per-feed **last data age** breakdown. Drives the same stale detection that triggers REST open-order refresh in the UI.

![System Stats — feed health and stream diagnostics](assets/system-stats.png)

## Multi Bots workspace

**Multi Bot Profiles** — save named profiles (symbol, MODE1–MODE5, optional note) in the browser, then **start/stop each profile independently**. Run different modes on different symbols concurrently without overwriting the main Bot panel config.

## Debug Logs workspace

Searchable forensic stream from the debug WebSocket and client log batching: filter by level, kind (order, balance, bot, orderbook, funds, ws, perf, ui), and source; hide ping noise; search by trace id, order id, or plain-text query. Counters for total, client, warnings, errors, and order traces — same events land in a server-side log file for sharing after an incident.

![Debug Logs — filtered client log stream](assets/debug-logs.png)

## Reliability behaviors

- **Symbol switch** — unsubscribes old public depth/trades and rebinds private streams; anti-crossfeed ignores late frames from the previous symbol.
- **Stale private feeds** — UI shows age in seconds; orders panel may run REST open-order refresh when the orders WebSocket stops updating.
- **MEXC compliance** — client PING every 15s, listen-key extend, proactive WS rotate before 24h cap, signed REST orders with query-in-URL body pattern.

Private code: [mexc_trading_app](https://github.com/logicencoder/mexc_trading_app)

See [REPOS.md](REPOS.md).

---

**Made by [Logic Encoder](https://logicencoder.com)** · [GitHub](https://github.com/logicencoder) · [Contact](https://logicencoder.com/contact/)

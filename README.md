# MEXC Trading Dashboard

**Single-screen MEXC Spot operator desk — manual limit and market orders, five bot modes, multi-bot profiles, and execution diagnostics where reaction time is the product.**

The **MEXC Trading Dashboard** is a self-hosted FastAPI console that wires MEXC REST and protobuf WebSockets into one draggable layout: place and modify orders by hand, run **MODE1 through MODE5** automation with budget caps and dry-run, watch orderbook, trades, balances, and deposits update in real time, and drill into performance timings and debug traces when latency or reconciliation breaks.

This is the **canonical product** (`mexc_trading_app`). The related [mexc-trading-dashboard-bot-suite](https://github.com/logicencoder/mexc-trading-dashboard-bot-suite) tree is an archived export of the same architecture — portfolio docs should describe this repo.

**Made by [Logic Encoder](https://logicencoder.com)**

Private source: [logicencoder/mexc_trading_app](https://github.com/logicencoder/mexc_trading_app)

---

## What you can do

| Area | In plain language |
|------|-------------------|
| **Manual trading** | Limit and market buy/sell, balance % shortcuts, cancel and modify from Activity |
| **MODE1** | Book-reactive LIMIT IOC or PING against live depth inside your price band |
| **MODE2** | One resting limit at a time; replenishes on fill inside min/max range |
| **MODE3** | Step re-entry ladder after each full fill |
| **MODE4** | Front-of-book quoting with re-quote when displaced |
| **MODE5** | Scheduled exact limit at HH:MM:SS |
| **Multi Bots** | Parallel profiles on different symbols and modes |
| **Funds** | Deposit/withdraw notifications and Funds History tab |
| **Diagnostics** | Performance panel, Debug Logs, System Stats for stale feeds |
| **Layout** | Drag, collapse, Save Layout, Snapshot UI |

---

## Feature examples (two per capability)

#### Realtime public market WebSocket
1. Public trades stream updates the Live Trades panel without refresh.
2. Protobuf orderbook depth fills the spread line and cumulative qty columns.

#### Private account WebSocket
1. Balance updates arrive on the private account stream after a fill.
2. Order and deal streams reconcile open orders with REST when WS stalls.

#### MODE1 instant execution
1. You set SELL MODE1 with TAKE style — LIMIT IOC fires when a bid enters your band.
2. PING style posts GTC, waits ping hold ms, then cancels via finalize timing logged in Performance.

#### MODE2 grid replenishment
1. MODE2 BUY rests at min price until filled, then places the next limit automatically.
2. Random Price picks a new level inside the band on each replenishment cycle.

#### MODE3 step ladder
1. After each full fill the next limit steps down by your configured percent from last fill.
2. Stop price halts the ladder when the next level would cross your floor.

#### MODE4 front of book
1. MODE4 SELL keeps a limit one tick inside best ask and re-quotes when displaced.
2. You shorten poll interval during a fast market session.

#### MODE5 scheduled order
1. You arm Mode5 SELL at 18:30:00 with exact price — one limit places at trigger.
2. Jump one tick nudges price using top-of-book when another order sits at target.

#### Multi-bot runtime
1. You start a MODE3 profile on `DNXUSDT` while MODE4 runs on another symbol via Multi Bots tab.
2. `/api/bots/status` lists all running bot_id entries with mode and symbol.

#### Manual order placement
1. You place a manual LIMIT buy and see it in Open Orders with trace id in Debug Logs.
2. You modify an open order — Replace runs cancel+place with modify timing in the API response.

#### Order activity UI
1. Open Orders shows cancel and modify buttons per row for the active symbol.
2. History tab plus show-all-symbols toggle reviews fills across the account.

#### Orderbook UX
1. My orders highlight marks your resting prices on the depth grid.
2. You click an ask row to fill the manual trading price input.

#### Charting
1. TradingView embed loads for the current symbol on the center panel.
2. Custom view toggle shows last-price summary when you collapse the chart body.

#### Balances
1. WS-first balance list updates Available and Locked after each trade.
2. Privacy hide masks values during screen share; Refresh forces REST snapshot.

#### Funds monitoring
1. A confirmed deposit appears in the notification ticker with expandable detail.
2. Funds History tab filters deposits and links to block explorer when network matches.

#### Symbol navigation
1. You type `BTC` in the top bar and the app normalizes to `BTCUSDT`.
2. Favorites star adds quick-switch buttons in the notification bar (up to five).

#### Layout customization
1. You drag the orderbook above the chart and Save Layout — order persists after reload.
2. Settings raise panel heights and Snapshot UI exports layout JSON for backup.

#### Bot safety and sizing
1. Total Amount cap in USDT halts the bot with Safety status when budget is spent.
2. Max order size prevents individual executions above your coin ceiling.

#### Dry run
1. Dry Run ON — MODE1 logs would-place lines without REST order IDs.
2. You validate band and sizing in dry run before switching live.

#### Debug Logs forensics
1. You filter Debug Logs to order + ERROR and search an orderId during a stuck cancel.
2. Client console capture forwards browser errors into the same stream.

#### Performance panel
1. Live poll shows place_order REST ms and order_ws_latency_ms after each bot fire.
2. You reset the view after a volatile session to compare fresh averages.

#### System Stats
1. Stale feed count rises after a WS drop — you know which LED to watch in the top bar.
2. Per-feed message counters help confirm which stream recovered first.

#### SQLite persistence
1. Order and deal history survives browser reload via persisted APIs.
2. Bot logs append to storage for post-session review on the Bot Logs tab.

---

## What it does not do

- **Not** futures or margin — MEXC Spot only
- **Not** a hosted SaaS — self-hosted with your API keys
- **Not** a separate product from bot-suite — bot-suite is an archived duplicate export
- **Not** guaranteed exchange uptime — stale-feed tooling helps but outages are external

API keys and trade history stay local — not published in this overview repo.

---

## Tech stack

| Layer | Technologies |
|-------|----------------|
| Backend | Python 3, FastAPI, uvicorn, asyncio |
| Bot engine | MODE1–MODE5, managed multi-bot, protobuf orderbook worker |
| Exchange | MEXC Spot REST v3 + protobuf WebSocket |
| Persistence | SQLite — orders, deals, funds, bot logs |
| Frontend | Vanilla HTML/JS, TradingView embed, draggable panels |
| Diagnostics | Debug WS stream, performance metrics, client log ingest |

---

## Quick start

```bash
pip install -r requirements.txt
# Configure MEXC API credentials per private repo README
uvicorn mexc_trading_app:app --host 0.0.0.0 --port 8005
```

See the private repo README and [REPOS.md](REPOS.md).

---

## Related repositories

| Repository | Role |
|------------|------|
| [mexc_trading_app](https://github.com/logicencoder/mexc_trading_app) | Private application code (canonical) |
| [mexc_trading_app-overview](https://github.com/logicencoder/mexc_trading_app-overview) | This product overview |
| [mexc-trading-dashboard-bot-suite-overview](https://github.com/logicencoder/mexc-trading-dashboard-bot-suite-overview) | Archived duplicate export — same product family |

See [REPOS.md](REPOS.md).

---

**Made by [Logic Encoder](https://logicencoder.com)** · [GitHub](https://github.com/logicencoder) · [Contact](https://logicencoder.com/contact/)

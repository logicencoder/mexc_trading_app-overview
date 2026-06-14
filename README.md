# MEXC Trading Dashboard + Spot Bot

Self-hosted **MEXC spot trading console** — live order book, trade tape, balances, open orders, fill history, deposit notifications, manual order buttons, and a two-mode trading bot in one browser dashboard. Your API keys stay on your machine; the app talks outbound to MEXC only.

Private source: [logicencoder/mexc_trading_app](https://github.com/logicencoder/mexc_trading_app).

## The problem it solves

The exchange website is built for retail click-trading, not for watching one altcoin book while automating reactions inside a price band. Switching symbols on the default UI often leaves stale depth or account data on screen. This product keeps **book, tape, balances, open orders, and bot logic on one screen** with protobuf WebSocket feeds, anti-stale order filtering after cancel/modify, and immediate lifecycle feedback when fills or deposits arrive.

You run the process locally (or in Docker on the same host you trust). Nothing is hosted as a shared SaaS — credentials never leave your environment except to MEXC’s API.

## Trading workspace

The **Trading** tab is the primary screen: searchable symbol picker, live order book with spread, trades tape, ticker context, and panels for balances, open orders, and manual buy/sell buttons. Change pair and the server unsubscribes the old symbol so public depth and private account streams do not cross-contaminate.

Optional highlighting shows where your resting orders sit in the book. WebSocket endpoints `/ws/orderbook` and `/ws/trades` fan out from server-side MEXC connections; account updates arrive on the private listen-key stream with REST reconciliation when a feed goes stale.

## Manual order execution

Place, cancel, and modify limit orders from the UI. Exchange precision, min-notional checks, and quantity rounding happen server-side before an order hits the wire. When a WebSocket frame is delayed, REST refresh backfills open orders so the panel matches reality before you click again.

## Funds history

The **Funds History** tab tracks deposits, withdrawals, and balance movements over time — useful when a bot or manual session ends and you need to reconcile what moved without exporting CSV from the exchange UI.

## Bot modes (MODE1 and MODE2)

| Mode | Behavior |
|------|----------|
| **MODE1** | Book-reactive — when price enters your band, fires **limit IOC** orders (no resting liquidity left behind) |
| **MODE2** | Grid-style limit orders within a configured range with a monitoring loop |

Dry-run mode lets you validate band logic without sending real orders. Presets save and load band parameters, sizes, and mode settings. Connection health indicators in the header show which feeds (trades, balances, orders, order book, bot logs) are live and how old the last update is.

For the full **MODE1–MODE5** suite with multi-bot profiles, see [mexc-trading-dashboard-bot-suite-overview](https://github.com/logicencoder/mexc-trading-dashboard-bot-suite-overview).

## Multi Bots panel

The **Multi Bots** tab exposes bot instance controls when more than one profile is configured (single-bot deployments still use the same layout). Start/stop, mode selection, and status for each instance stay adjacent to the trading panels so you are not alt-tabbing between tools.

## System Stats

The **System Stats** tab is the feed-health dashboard: per-stream counters, stale-feed warnings, WebSocket age, REST fallback indicators, debug totals, and last-update timestamps for trades, balances, orders, deals, order book, bot logs, and API status. When a private stream stops updating, the UI shows age in seconds and may trigger REST refresh automatically — the same condition you would diagnose manually with log grep.

![System Stats — feed health and stream diagnostics](assets/system-stats.png)

## Debug Logs

The **Debug Logs** tab is a searchable, filterable event table: timestamp, source, level, page, and message. Filter by level, kind, or source; hide ping noise; search by trace ID, order ID, or free text. Client-side order traces, precision updates, monitored coin lists, and WS stale warnings land here — equivalent to opening devtools on every panel at once.

![Debug Logs — filtered client log stream](assets/debug-logs.png)

## Client log capture

The UI posts structured client events to `POST /api/client-logs` so operators can share a session log after an incident without screen recording. Warnings such as “Orders WS stale; running REST open-orders refresh” appear in both Debug Logs and the persisted log file.

## Stack and runtime

FastAPI backend (`mexc_trading_app.py`), browser UI (`mexc_trading_app.html` + `mexc_trading_app.js`), `bot_engine.py`, and `generated_proto/` for MEXC V3 stream decode. Local SQLite under `data/` stores runtime history. Optional Docker packaging for the same host.

## Quick start

```bash
# private repo — add mexc_keys.json or .env locally
./run_server.sh
# typical: http://127.0.0.1:8005
```

## Screenshots

### Trading dashboard

Live book, tape, balances, and manual controls for the active symbol.

![MEXC standalone dashboard](assets/mexc-trading-dashboard-bot-overview.png)

### System Stats

Per-feed health, stale detection, and REST fallback indicators.

![System Stats — feed health and stream diagnostics](assets/system-stats.png)

### Debug Logs

Filterable client log stream with order traces and WS diagnostics.

![Debug Logs — filtered client log stream](assets/debug-logs.png)

---

**Made by [Logic Encoder](https://logicencoder.com)** · [GitHub](https://github.com/logicencoder) · [Contact](https://logicencoder.com/contact/)

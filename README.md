# MEXC Trading Dashboard + Spot Bot

![MEXC standalone dashboard](assets/mexc-trading-dashboard-bot-overview.png)

Self-hosted **MEXC spot trading console** — live order book, trade tape, balances, open orders, manual order buttons, and a two-mode trading bot in one browser dashboard. Your API keys stay on your machine; the app talks outbound to MEXC only.

Private source: [logicencoder/mexc_trading_app](https://github.com/logicencoder/mexc_trading_app).

## The problem it solves

The exchange website is built for retail click-trading, not for watching one altcoin book while automating reactions inside a price band. This product keeps **book, account state, and bot logic on one screen** with protobuf WebSocket feeds, anti-stale order filtering after cancel/modify, and immediate lifecycle feedback.

## Realtime market panels

- **Order book** — bids and asks with spread; optional highlight of your resting orders
- **Trades tape** — time, price, size, side from MEXC V3 protobuf public stream
- **Ticker** — REST snapshot for 24h context
- **Symbol switch** — change pair; server unsubscribes the old symbol to prevent cross-feed glitches

WebSocket endpoints `/ws/orderbook` and `/ws/trades` fan out from server-side MEXC connections.

## Account visibility

Live panels for balances (free/locked), open orders, order history, deals (executions), and deposit notifications. Manual trading supports place, modify, and cancel with reconciliation between WebSocket events and REST snapshots.

## Bot modes (MODE1 and MODE2)

| Mode | Behavior |
|------|----------|
| **MODE1** | Book-reactive — when price enters your band, fires **limit IOC** orders (no resting liquidity left behind) |
| **MODE2** | Grid-style limit orders within a configured range with monitoring loop |

Dry-run support, preset save/load, connection health indicators, and client log capture for incident debugging.

## System Stats

Per-feed counters, stale-feed warnings, WebSocket age, and REST fallback when a private stream stops updating.

![System Stats — feed health and stream diagnostics](assets/system-stats.png)

## Debug Logs

Searchable event table with level, source, page, and message — order traces, precision updates, and WS stale warnings.

![Debug Logs — filtered client log stream](assets/debug-logs.png)

## Stack

FastAPI backend (`mexc_trading_app.py`), browser UI (`mexc_trading_app.html` + `mexc_trading_app.js`), `bot_engine.py`, and `generated_proto/` for stream decode. Local SQLite runtime history under `data/`.

## Quick start

```bash
# private repo — add mexc_keys.json or .env locally
./run_server.sh
# typical: http://127.0.0.1:8005
```

For the full **MODE1–MODE5** bot suite with multi-bot profiles, see [mexc-trading-dashboard-bot-suite-overview](https://github.com/logicencoder/mexc-trading-dashboard-bot-suite-overview).

---

**Made by [Logic Encoder](https://logicencoder.com)** · [GitHub](https://github.com/logicencoder) · [Contact](https://logicencoder.com/contact/)

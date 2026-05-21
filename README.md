# MEXC Trading Dashboard + Spot Bot — Public Overview

How the **standalone local trading console** works for operators who run it on their own hardware.  
No API keys, hostnames, or private source in this repository.

**Private implementation:** [mexc_trading_app](https://github.com/logicencoder/mexc_trading_app) (invitation only)

![MEXC standalone dashboard](assets/mexc-trading-dashboard-bot-overview.png)

---

## Purpose

### What

A self-hosted application that opens in the browser on the operator’s machine: live MEXC spot order book, trade tape, wallet balances, open orders, fill history, deposit notifications, manual order buttons, and a two-mode trading bot (MODE1 book-reactive, MODE2 grid limit).

### Why it exists

Exchange websites optimize for retail click-trading, not for:

- Watching one altcoin book while automating reactions inside a price band  
- Streaming private account protobuf frames next to public depth  
- Switching symbols without the UI mixing stale feed data  

This product keeps control local: your keys, your process, your risk parameters.

### Who benefits

| Audience | Value |
|----------|--------|
| **Active spot traders** | One screen for book + account + bot |
| **Market makers / support traders** | MODE1 reacts when liquidity appears in a band |
| **Operators** | Connection health, client log capture, dry-run bot |

### How it fits

Not the same as [mexc-live-stats-backend](https://github.com/logicencoder/mexc-live-stats-backend-overview) (public stats site). This app is **local-first**, not the LogicEncoder.com multi-coin stats dashboard.

---

## Delivery model

### What

Runs as FastAPI + static HTML/JS on localhost (or Docker on the same host). Connects outbound to MEXC only.

### Why

Avoids hosting custody of user API secrets on a shared server.

### Who

Operators comfortable running Python on SOL/WSL/desktop.

### How

Download private repo, add `mexc_keys.json` locally, start `run_server.sh`.

---

## Realtime market observation

### What

- **Order book** — bids/asks, spread; optional highlight of your resting orders  
- **Trades tape** — time, price, size, side from MEXC V3 protobuf public stream  
- **Ticker** — REST snapshot for 24h context  
- **Symbol switch** — change pair; server unsubscribes old symbol to prevent cross-feed glitches  

### Why

Manual and bot decisions need synchronized book + tape + account state.

### Who

Traders monitoring thin altcoin books (e.g. DNX/USDT style pairs).

### How

WebSocket endpoints `/ws/orderbook` and `/ws/trades` fan out from server-side MEXC connections.

---

## Account visibility

### What

Live panels for:

- **Balances** (free/locked per asset)  
- **Open orders** and **order history**  
- **Deals** (executions)  
- **Deposits** (polling + events)  

### Why

Bots and manual orders both need immediate feedback when fills or deposits arrive.

### Who

Anyone running inventory-aware strategies.

### How

Private MEXC listen-key WebSocket with protobuf account/order/deal channels; REST for history and deposits.

---

## Manual order execution

### What

Place, cancel, and modify limit orders from the UI via REST API wrappers with exchange precision and min-notional checks.

### Why

Operators want one-click actions without leaving the custom layout.

### Who

Humans overriding or supplementing the bot.

### How

`POST /api/orders/place`, `DELETE /api/orders/cancel`, `PUT /api/orders/modify`.

---

## Bot MODE1 — book-reactive execution

### What

When the bot runs in MODE1 it watches the order book. If opposing liquidity appears between **min price** and **max price**, it sends a **limit IOC** order (immediate-or-cancel — does not leave resting liquidity on the book).

Configurable:

- **Side** — BUY or SELL  
- **Size cap** — max coins per hit (0 = unlimited within budget)  
- **Budget** — max total USDT or coins to spend  
- **Execution style** — TAKE, PING, ALTERNATE, RANDOM  
- **Random amount** — optional random coin size per hit  
- **Dry run** — log without sending orders  

### Why

Useful when you want to **take** or **ping** liquidity that appears in a band without manually clicking — e.g. supporting a market or reacting to shown size.

### Who

Operators automating reactions in a known price corridor.

### How

`bot_engine.TradingBot.on_orderbook_update()` → signed REST limit IOC; cooldown between executions; stuck-order recovery if cancel fails.

---

## Bot MODE2 — grid limit

### What

Places a limit order inside the price range; when filled, replaces with a new limit at another price in the band (optional random price).

### Why

Classic grid-style quoting inside min/max without building a full external grid framework.

### Who

Operators providing two-sided or one-sided liquidity in a range.

### How

`start_mode2()` + monitor task watching private order/fill stream.

---

## Bot safety and limits

### What

- **Max order size** toggle  
- **Total amount to use** (USDT or coin denomination)  
- **Min notional** from exchange rules  
- **Safety halt** inside engine  
- **Stop** endpoint tears down bot and monitor tasks  

### Why

Prevents runaway loops when feeds glitch or parameters are mis-set.

### Who

Anyone running real capital (not dry-run).

### How

Validated in `POST /api/bot/start` before `TradingBot.start()`.

---

## Connection health

### What

Status API and `/ws/status` report which feeds are alive: trades, account, order book, REST API. UI can mark feeds stale.

### Why

Protobuf WS drops silently without visible cues — operators need to pause bots when blind.

### Who

Maintainers during network or exchange incidents.

### How

`connection_status` dict updated in feed loops; logged on reconnect.

---

## Diagnostics

### What

- Browser console log batch upload to server file  
- Bot log WebSocket stream  
- `/health` liveness  

### Why

Reproduce UI issues without screen recording.

### Who

Developers and operators post-morteming a session.

### How

`POST /api/client-logs` → `logs/client_console.log`.

---

## Workspace UI

### What

Dark, panelized dashboard: trading tab, funds history, stats, multi-bot panel (single bot instance in current code), debug logs.

### Why

Reduce eye travel during fast markets.

### Who

Power users on large monitors.

### How

`mexc_trading_app.html` + `mexc_trading_app.js` — no separate frontend build chain.

---

## Tech stack (non-secret)

| Piece | Choice |
|-------|--------|
| API server | Python, FastAPI, asyncio |
| Exchange protocol | MEXC Spot V3 REST + WebSocket protobuf |
| UI | Static HTML/CSS/JS |
| Optional packaging | Docker |

---

## What this product is not

- Not a cloud SaaS — you run the process  
- Not the MEXC Live Stats public website product  
- Not futures/perpetuals — spot-focused codebase  
- Not MODE3–MODE5 — only **MODE1** and **MODE2** exist in the current engine  

---

## Repository map

| Repo | Visibility |
|------|------------|
| `mexc_trading_app` | Private code |
| `mexc_trading_app-overview` | This document |

---

## Evaluation

For access to the private repository or operational questions, contact LogicEncoder via the website contact channels.  
Do not request API keys in public issues on this overview repo.

# MEXC Trading Dashboard

![MEXC Trading Dashboard workspace](assets/trading-dashboard.png)

Active trading can require repeatedly moving between a chart, order book, balances, order controls and automation settings. MEXC Trading Dashboard brings those views into a configurable trading desk so the operator can inspect the market and manage activity in one place.

## Tech stack

| Layer | Technologies |
|---|---|
| Application | Python, FastAPI |
| Supporting technologies | HTML, CSS, JavaScript; MEXC; TradingView; WebSockets |

## Arrange the trading desk

Move, resize and collapse supported panels to match the work at hand. Layout preferences and display controls help keep frequently used information close together. Symbol and connection information remain visible so the operator can recognise the market and the state of the session.

![MEXC Trading Dashboard — Arrange the trading desk](assets/settings-ui-layout.png)

![MEXC Trading Dashboard — Arrange the trading desk](assets/settings-notifications.png)

## Inspect the market

The chart, order book and live trade tape provide different views of current activity. Search and symbol controls make it easier to move between markets, while available notifications draw attention to relevant changes. These views support judgement rather than replacing it.

## Place and follow manual orders

Order entry sits beside account balances and activity panels, helping connect a proposed trade with available funds and existing orders. Open-order and recent-activity information support checking what happened after an action. Funds notifications and history add another view of account movement.

![MEXC Trading Dashboard — Place and follow manual orders](assets/performance-panel.png)

## Configure automation deliberately

Bot controls expose the supported modes, action choices and execution ranges. Mode-specific settings and a summary of the current configuration help the operator understand the selected behaviour before starting it. Status and stop controls remain part of the operating workflow.

![MEXC Trading Dashboard — Configure automation deliberately](assets/bot-modes.png)

![MEXC Trading Dashboard — Configure automation deliberately](assets/bot-random-action.png)

![MEXC Trading Dashboard — Configure automation deliberately](assets/bot-execution-range.png)

![MEXC Trading Dashboard — Configure automation deliberately](assets/bot-mode1-status.png)

![MEXC Trading Dashboard — Configure automation deliberately](assets/bot-settings-summary.png)

## Manage several activities

The Multi Bots workspace supports reviewing more than one configured bot context. Performance information helps examine trade and bot activity, while system statistics and debug logs assist with investigation. These tools keep operational checks close to the trading desk.

![MEXC Trading Dashboard — Manage several activities](assets/activity-open-orders.png)

![MEXC Trading Dashboard — Manage several activities](assets/system-stats.png)

![MEXC Trading Dashboard — Manage several activities](assets/debug-logs.png)

## Keep the workspace usable

Notification, search and presentation settings support different working routines. Saved layouts reduce repeated setup, and resizable views help balance detail with overview.

---

Private code: [mexc_trading_app](https://github.com/logicencoder/mexc_trading_app)

See [REPOS.md](REPOS.md) for related repositories.

**[Logic Encoder](https://logicencoder.com)** · [GitHub](https://github.com/logicencoder) · [Contact](https://logicencoder.com/contact/)

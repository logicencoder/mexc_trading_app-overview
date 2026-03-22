# MEXC Dashboard + Multi-Mode Bot (Standalone) — Overview

> This repository is an **overview repository**.
> It is intentionally non-sensitive and does not contain private implementation code.

## Positioning

This product is a **standalone local application** (desktop-style local runtime), **not a hosted SaaS web app**.
It runs on your own machine, serves a local interface, and connects directly to exchange APIs/streams.

## UI Snapshot

![MEXC Standalone Dashboard](assets/mexc-trading-dashboard-bot-overview.png)

## What the platform is

A single operator console for:

- Realtime market observation
- Fast manual order execution
- Multi-mode bot automation
- Runtime diagnostics and operational control

## Tech Stack Used

- **Backend**: Python, FastAPI, async API handlers, exchange REST/WS integrations
- **Frontend**: HTML/CSS/JavaScript single-page operator dashboard
- **Realtime transport**: WebSocket + REST hybrid data flow
- **Storage/runtime state**: local persistence + runtime snapshots/log-style artifacts
- **Deployment style**: standalone local runtime with direct exchange connectivity

## High-Level Architecture

- **Local Frontend Runtime**: interactive trading console, local state orchestration, panelized workspace UI
- **Local API Layer**: order/bot endpoints, market/account data aggregation, diagnostics APIs
- **Exchange Integration Layer**: REST + WS streaming for prices, orders, balances, fills, and account events
- **Bot Engine Layer**: MODE1..MODE5 execution strategies, safety controls, scheduling, and runtime state
- **Persistence Layer**: local runtime artifacts (history/debug snapshots, optional local DB/logs)

## Full Feature Inventory (Comprehensive)

### A) Workspace & Operator Experience

1. Standalone local runtime (no hosted backend dependency)
2. Dark low-latency operator dashboard layout
3. Multi-tab workspace (Trading / Funds History / System Stats / Multi Bots / Debug Logs)
4. Top status badge strip for key feed domains
5. Symbol-focused trading context at top bar
6. Fast symbol switch control
7. Favorites/quick symbol access
8. Local notification ticker for important events
9. Compact panel collapse/expand behavior
10. Persisted workspace/UI settings between sessions

### B) Market Visibility

11. Realtime trade tape
12. Realtime orderbook stream
13. Best bid/ask spread visibility
14. Orderbook cumulative quantity columns
15. Orderbook cumulative notional columns
16. Bid/ask independent sorting controls
17. "Highlight my orders" on orderbook
18. Last traded price with directional arrow
19. Integrated chart panel (TV/custom modes)
20. Symbol synchronization across chart + trading panels

### C) Manual Trading Workflows

21. Buy/Sell manual side controls
22. Market and Limit order support in UI flow
23. Quantity input with numeric steppers
24. Price input with numeric steppers
25. Amount-based entry helpers
26. Unit switching (`COINS` / `USDT`) for sizing logic
27. Slippage tolerance input for market-like flows
28. Quick amount shortcut buttons
29. Validation of required fields before submission
30. Immediate feedback alerts on order action outcome

### D) Open Orders & Activity

31. Open orders panel with action controls
32. Order history panel in same activity area
33. Optional cross-symbol open-order visibility
34. On-row cancel action
35. On-row modify/replace action
36. Immediate pending visual feedback on cancel
37. Immediate pending visual feedback on replace
38. Anti-resurrection local hide logic for stale rows
39. REST + WS reconciliation to reduce stale UI state
40. Strict fallback refresh after final order transitions

### E) Bot Core & Strategies

41. MODE1 execution strategy support
42. MODE2 grid-mint style support
43. MODE3 re-entry ladder support
44. MODE4 front-book requote support
45. MODE5 scheduled execution support
46. Mode-specific controls shown/hidden by selection
47. Bot side selection (`BUY` / `SELL`)
48. Bot unit selection (`COINS` / `USDT`)
49. Mode1 execution style variants (TAKE/PING/50-50/RANDOM)
50. Mode5 exact time scheduling (`HH:MM:SS`)

### F) Bot Safety, Limits, and Randomization

51. Max sum use limits
52. Max order sizing constraints
53. Min order notional guardrails
54. Tiny leftover handling thresholds
55. Random price variation toggle
56. Random amount range controls
57. Dry-run mode
58. Ping-hold timing controls
59. Mode3 stop conditions
60. Continue-until-budget controls

### G) Multi-Bot & Presets

61. Multi-bot profile creation
62. Multi-bot profile load/apply
63. Multi-bot profile start/stop controls
64. Profile-specific preset snapshots
65. Main panel preset save
66. Main panel preset load
67. Main panel preset delete
68. Preset-aware mode/side/unit restoration

### H) Diagnostics, Logging, and Reliability

69. System status panel with feed-level visibility
70. API/feed connectivity indicators
71. Performance panel with runtime metrics
72. Backend timing support for modify-order path analysis
73. Client-side perf metrics tracking in UI
74. Debug logs panel with filters/search
75. Client console mirroring to backend endpoint
76. Operational refresh controls (orders/perf/status)
77. Notification controls (including symbol-switch alert mute)
78. Runtime error visibility surfaces

## Security & Exposure Policy

- This overview repo excludes private source internals.
- No exchange credentials are stored in this repo.
- Intended for architecture/features communication only.

## Intended Audience

- Product/strategy collaborators
- Technical reviewers
- Integration stakeholders needing a capability map without private implementation disclosure

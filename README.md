# Aegis Paper Trading Data

Multi-device aggregation for Project Aegis paper trading.

## Devices

- **laptopclaw** - Portable trading (Windows)
- **ratclaw** - Workstation
- **tokyovps** - Vultr Tokyo VPS (Project Aegis execution)

## Structure

```
.
├── devices/
│   ├── laptopclaw/trades_2025.jsonl
│   ├── ratclaw/trades_2025.jsonl
│   └── tokyovps/trades_2025.jsonl
├── aggregated/
│   ├── all_trades.jsonl
│   └── summary.json
└── .github/workflows/
    └── aggregate.yml
```

## Trade Format

```json
{
  "tradeId": "ratclaw_1711036800000_001",
  "deviceId": "ratclaw",
  "deviceName": "RatClaw Workstation",
  "timestamp": 1711036800000,
  "token": "SOL",
  "direction": "BUY",
  "size": 1.0,
  "entryPrice": 150.0,
  "exitPrice": null,
  "pnl": null,
  "status": "OPEN"
}
```

## Aggregation

GitHub Actions automatically aggregates data from all devices on every push.

## Local Query Examples

```bash
# Total trades across all devices
cat aggregated/all_trades.jsonl | wc -l

# PnL by device
cat aggregated/all_trades.jsonl | jq -s '
  group_by(.deviceId) |
  map({
    device: .[0].deviceId,
    trades: length,
    totalPnl: map(.pnl // 0) | add
  })
'
```

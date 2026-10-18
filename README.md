# radar-command-deck-data

雷达指挥舱的**公开市场数据**，每 4 小时自动同步。供 Codex / 网页端读取。

- 数据文件：[`radar-latest.json`](https://raw.githubusercontent.com/cherry7144/radar-command-deck-data/main/radar-latest.json)
- 更新节奏：每 4 小时从各雷达最新快照合并生成；上传前经过敏感信息检查（含私人持仓、人名、密钥、联系方式等），检查失败则停止上传。
- 数据内容：各币种价格、涨跌幅、技术结构、资金状态、产业催化、风险，以及 BTC/ETH ETF 资金流。只含公开市场研究数据，**不含任何私人持仓与账户信息**。

## 字段速览

每个数据字段都是 `{value, source, updated_at, status}` 信封：
看数字先看 `updated_at`（采集时间）与 `status`（`ok` 正常 / `stale` 已过期仍沿用旧值 / `missing` 无数据）。

```json
{
  "meta":    { "generated_at": "...", "generator": "merge_radar_latest.py v1.0" },
  "sources": { "prelaunch": {"round": "20261018-0410", "status": "ok", ...}, "...": {} },
  "assets": {
    "XPL": {
      "symbol": "XPL",
      "price":           {"value": 0.0792, "source": "prelaunch", "updated_at": "...", "status": "ok"},
      "change_24h_pct":  {"value": -3.29,  "source": "capital",    "updated_at": "...", "status": "ok"},
      "ratings":         {"catalyst": {"value": "D1", ...}, "capital": {"value": "A形成中", ...}},
      "technical":       [{"value": "...", "source": "prelaunch", ...}],
      "capital":         {"value": "主动净买USD(1h+35K, 4h+207K, 24h+65K)；强证据", ...},
      "catalyst":        {"value": "Celestia 上线 AI Agent 实验链 Eden（D1·轻度兑现）", ...},
      "risks":           [{"value": "...", ...}]
    }
  },
  "market": { "btc": {...}, "etf_btc": {...}, "etf_eth": {...} }
}
```

注意：
- `ratings` 按雷达分别存放（`catalyst` 的 D0/D1/D2、`capital` 的 A 状态、`ema_squeeze` 的 M0–M3、`integration` 的 S/A/B/C），**永不跨雷达合并**，取用时请按雷达分别读取。
- `integration` 为手动整合报告（约每日一次），非自动雷达。
- 本数据为研究记录，不含买卖建议。

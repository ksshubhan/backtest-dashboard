# QuantVision backend

The Python API behind [QuantVision](https://quantvision.vercel.app), a dashboard for backtesting simple trading strategies on real stock data. The full write-up, including how the backtest works and its known limitations, is in the frontend repository: [quantvision-frontend](https://github.com/ksshubhan/quantvision-frontend).

## API

`GET /run_strategy?name=<strategy>&ticker=<symbol>`

| Parameter | Values |
| --- | --- |
| `name` | `momentum`, `mean_reversion` or `sma_crossover` |
| `ticker` | Any Yahoo Finance symbol, such as `AAPL` |

It downloads six months of daily prices, runs the strategy and returns:

```json
{
  "equity_curve": [["2026-04-01", 100.0], ["2026-04-02", 100.8], "…"],
  "metrics": { "sharpe_ratio": 1.29, "max_drawdown": 7.5, "annual_return": 26.35, "volatility": 18.15 }
}
```

An unknown strategy returns `400`, and a ticker with no data returns `404`, each with a `detail` message.

## Running it locally

```bash
pip install -r requirements.txt
uvicorn main:app --reload    # serves on http://localhost:8000
```

Built with FastAPI, pandas, NumPy and yfinance, and deployed on Render.

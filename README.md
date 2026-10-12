# Market Research Agent

A Python tool-calling agent that researches a public company from its ticker and writes a report.

## What it does

The agent runs on Llama 3.3 70B (`llama-3.3-70b-versatile`) through the Groq API. It decides which of four tools to call, runs the chosen calls in parallel, reads the results, and either calls more tools or writes the final answer.

| Tool | What it returns | Source |
|---|---|---|
| `get_earnings` | Earnings and company fundamentals | Yahoo Finance via yfinance |
| `get_news_sentiment` | Recent headlines from the last 7 days with a VADER sentiment score | NewsAPI |
| `get_sec_filings` | Links to recent 10-K and 10-Q filings | SEC EDGAR |
| `get_price_history` | 52-week range, position in that range, volume and 14-day RSI | Yahoo Finance via yfinance |

## Run it

There is no requirements file yet. Install the packages imported in `agent.py` and `tools.py`: `groq`, `python-dotenv`, `httpx`, `yfinance`, `newsapi-python` and `vaderSentiment`.

Create a `.env` file (no spaces around `=`):

```
GROQ_API_KEY=your_key
NEWS_API_KEY=your_key
```

Then run:

```bash
python main.py AAPL
```

`main.py` defaults to AAPL if you pass no ticker.

## Limitations

- The Groq call is synchronous inside an async function, so it blocks the event loop.
- yfinance is an unofficial API and can break without notice.
- The report is the model's summary of tool output, with no fact check. It is not investment advice.
- No tests and no evaluation of how well it picks tools.

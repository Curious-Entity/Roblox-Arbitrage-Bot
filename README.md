# Roblox Limited Listing Monitor

Thi is my Python passion project that monitors Roblox limited-item resale listings and attempts a purchase when the lowest available listing falls below a configured percentage of its Rolimons valuation.

The project combines Roblox marketplace data, Rolimons valuation data, configurable price thresholds, and automated purchase requests into a multi-item monitoring workflow.

> This repository is a personal automation experiment. It is not financial advice, does not guarantee profit, and should only be used in compliance with Roblox and Rolimons terms of service.

## Features

- Monitors multiple Roblox limited items concurrently
- Retrieves Rolimons valuation with RAP fallback
- Finds the lowest available resale listing
- Calculates an acceptable purchase price from a configurable threshold
- Attempts a purchase when a listing meets the configured criteria
- Handles Roblox authentication and CSRF tokens
- Reuses HTTP sessions to reduce request overhead
- Includes rate-limit backoff handling
- Logs unusually discounted listings for later analysis
- Includes endpoint performance and rate-limit diagnostic utilities

## Repository structure

```text
├── snipe.py                 # Main listing-monitoring and purchase workflow
├── perf_probe.py            # Endpoint performance experiment
├── proxy_tail_bench.py      # Network-latency benchmark utility
├── rate_limit_probe.py      # Conservative rate-limit testing utility
└── analyze_retry_after.py   # HTTP 429 / Retry-After analysis utility
```

## Requirements

- Python 3.10+
- `requests`

```bash
git clone https://github.com/Curious-Entity/Online-Arbitrage-Bot.git
cd Online-Arbitrage-Bot
python -m venv .venv
source .venv/bin/activate
python -m pip install requests
```

## Configuration

Configure monitored items near the top of `snipe.py`:

```python
ITEMS_TO_SNIPE = [
    {
        "asset_id": 123456789,
        "name": "Example Limited",
        "discount_threshold": 0.10,
    },
]
```

`discount_threshold` is the maximum fraction of the Rolimons value the project will consider. For example, `0.10` corresponds to 10% of the current valuation.

Relevant settings include:

```python
POLL_INTERVAL = 0.15
REFRESH_TIME = 86400
PURCHASE_TIMEOUT = 2
```

Start with conservative request settings and respect HTTP 429 responses.

## Authentication and security

The project requires authenticated Roblox requests for account checks and purchase attempts. Set the cookie only in your local environment:

```bash
export ROBLOSECURITY='your-cookie-value'
```

IMPORTANT: Never commit `.ROBLOSECURITY` cookies

## Run

After configuring items and local-only credentials:

```bash
python snipe.py
```

At startup, the program validates authentication, retrieves valuation data, and starts a monitoring loop for each configured item.

## Diagnostic utilities

The repository also includes small tools used to measure endpoint behavior during development:

```bash
python perf_probe.py
python rate_limit_probe.py
python analyze_retry_after.py --include-direct
python proxy_tail_bench.py
```

## Disclaimer

This project is provided for educational and personal research purposes. It does not guarantee profit.

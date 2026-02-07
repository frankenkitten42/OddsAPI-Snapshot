# OddsAPI Snapshot Tool

Fetch, store, and normalize odds data from [The Odds API](https://the-odds-api.com).

## Setup

1. Clone the repo and navigate to the project folder
2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
3. Get an API key from [The Odds API](https://the-odds-api.com) — free tier is available

## How It Works

The tool is a two-step pipeline:

1. **Snapshot** — Fetches raw odds data from the API and saves it as timestamped JSON files, organized by sport.
2. **Normalize** — Reads those raw snapshots and flattens them into clean, analysis-ready JSON and CSV files with implied probabilities and vig-adjusted fair probabilities calculated.

```
snapshot_oddsapi.py  →  snapshots_oddsapi/{sport}/{sport}_{timestamp}.json
                                    ↓
normalize_odds.py    →  normalized/{sport}/json/normalized_{timestamp}.json
                    →  normalized/{sport}/csv/normalized_{timestamp}.csv
```

## API Key Management

Your key is saved to `~/.oddsapi_key` on first use, or you can set it as an environment variable:

```bash
export ODDS_API_KEY="your_key_here"
```

You can also manage it from the CLI at any time:

```bash
# Show your saved key (masked)
python snapshot_oddsapi.py key show

# Save a new key
python snapshot_oddsapi.py key set

# Delete your saved key
python snapshot_oddsapi.py key clear
```

## Snapshotting

### Check your key and API usage

```bash
python snapshot_oddsapi.py doctor
```

### List available sports

```bash
python snapshot_oddsapi.py list
```

### Fetch odds

```bash
# Single sport
python snapshot_oddsapi.py snapshot --sports basketball_nba

# Multiple sports at once
python snapshot_oddsapi.py snapshot --sports basketball_nba hockey_nhl soccer_epl

# Custom region and markets
python snapshot_oddsapi.py snapshot --sports basketball_nba --regions us --markets h2h,spreads
```

**Snapshot options:**

| Flag | Default | Description |
|------|---------|-------------|
| `--sports` | *(required)* | One or more sport keys to fetch |
| `--regions` | `us` | Region to pull odds from |
| `--markets` | `h2h,spreads,totals` | Comma-separated list of markets |

## Normalizing

Run from the same folder as the snapshots:

```bash
python normalize_odds.py
```

This processes everything currently in `snapshots_oddsapi/` and writes the output to `normalized/`. Each sport gets its own subfolder with separate `json/` and `csv/` directories.

### Normalized output fields

| Field | Description |
|-------|-------------|
| `sport` | Sport key (e.g. `basketball_nba`) |
| `event_id` | Unique event identifier from the API |
| `commence_time` | Event start time (ISO 8601) |
| `home_team` | Home team name |
| `away_team` | Away team name |
| `bookmaker` | Bookmaker key (e.g. `draftkings`) |
| `market` | Market type (e.g. `h2h`, `spreads`, `totals`) |
| `outcome` | Outcome name or team |
| `point` | Point spread or total (where applicable) |
| `price` | Decimal odds |
| `implied_prob` | Implied probability from the listed price |
| `fair_prob` | Vig-adjusted fair probability |

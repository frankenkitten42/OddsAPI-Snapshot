========================================
VERSION HISTORY
========================================

----------------------------------------
snapshot_oddsapi.py
----------------------------------------

V1.0 — 2026-01-31
  Initial release
  - API key handling: prompt on first run, save to ~/.oddsapi_key
  - Supports environment variable override via ODDS_API_KEY
  - Multi-sport snapshot fetching via --sports flag
  - Saves raw JSON snapshots to snapshots_oddsapi/{sport}/
  - Doctor command: validates key, shows API usage stats
  - Key management: show, set, clear via CLI
  - Custom help with all subcommands and flags documented

V1.1 — 2026-01-31
  CLI cleanup and restructure
  - Moved sport listing to its own `list` subcommand
  - Removed duplicate list_sports() function, consolidated into list_available_sports()
  - Removed --list-sports from snapshot subcommand
  - Removed --sports flag from doctor subcommand
  - Made --sports required in snapshot argparse (cleaner error handling)
  - Rewrote print_help() to document all subcommands, flags, and defaults
  - Added total sport count to list output

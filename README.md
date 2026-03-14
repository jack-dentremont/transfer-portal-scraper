# 🏀 NCAA Transfer Portal Scraper

A Python automation tool that scrapes NCAA basketball player data from the NCAA transfer portal, paired with data from Sports Reference and ncaa.com, and exports everything to a live Google Sheet used directly by coaching staff for recruiting decisions.

## Overview

The NCAA transfer portal has transformed college basketball recruiting.  Coaches now evaluate hundreds of potential transfers each cycle under tight timelines. Manually tracking portal entrants, pulling their stats, and comparing candidates is time-consuming, tedious, and makes a stressful process all the more painstaking.

This tool automates the entire pipeline: it scrapes data from the portal, normalizes it across conferences and divisions, pairs players to their stats found on either Sports Reference (DI), or ncaa.com (DII), and pushes structured output to a shared Google Sheet for our coaching staff to reference in real time. What used to take hours of manual research now runs with a single command.

## What It Does

**Data Collection**
- Scrapes player names, portal status, and current school/conference information via Selenium
- Pulls season statistics (PPG, RPG, APG, and more) from Sports Reference or ncaa.com

**Data Processing**
- Normalizes conference names across divisions (D1, D2) to ensure consistent filtering and comparison
- Handles edge cases: preserves manually entered staff notes and stats, skips players already in the sheet, and manages blank or missing stat lines gracefully
- Prevents overwriting coaching staff's manual annotations

**Data Export**
- Pushes cleaned, structured data directly to Google Sheets via the gspread API
- Coaching staff accesses the sheet as a live recruiting board that is always up to date
- On-demand execution allows for fresh data at any point during the recruiting cycle

## Tech Stack

| Tool | Purpose |
|------|---------|
| **Python** | Core language |
| **Selenium** | Web scraping and browser automation for dynamic pages |
| **gspread** | Google Sheets API integration for live data export |
| **Google Sheets** | End-user interface for coaching staff |

## Architecture

```
NCAA Transfer Portal ──► Selenium Scraper ──► Stats Database ──► Data Cleaning & Normalization ──► Google Sheets API ──► Live Recruiting Board
```

**Key design decisions:**
- **Selenium over BeautifulSoup** — the source sites rely on JavaScript-rendered content, requiring a headless browser
- **Google Sheets as the frontend** — Allows for live, shared access; no adoption friction, no new tools to learn
- **Preserve manual data** — the scraper detects and protects hand-entered notes and overrides from coaching staff, ensuring automation never overwrites human judgment
- **Conference normalization** — a custom mapping layer standardizes conference names across divisions, preventing duplicate or unmatched entries

## Sample Output

> *Screenshots omitted to protect program-specific recruiting data and the privacy of the athletes in the portal!*

The Google Sheet is organized with the following structure:

| Column | Description |
|--------|-------------|
| Player Name | Full name as listed in the portal |
| Position | As noted in the Stats Database
| Previous School | Most recent institution |
| Rank | Manual evaluation as determined by coaching staff |
| Height | Listed Height |
| PPG | Points per game |
| RPG | Rebounds per game |
| APG | Assists per game |
| MPG | Minutes per game |
| FG% | Field goal percentage |
| 3P% | 3 point field goal percentage |
| FT% | Free throw percentage |
| Portal Entry Date | Date the athlete entered the NCAA transfer portal |
| Coach | Column for coach who completed the evaluation to initial |
| Agent | Column to leave contact information for athlete's agent, if applicable |
| Notes | Column to comment on player's ability and fit for our program |

## Challenges Solved

- **Dynamic content rendering** — stats databases load data asynchronously via JavaScript, requiring Selenium with explicit waits rather than simple HTTP requests
- **Conference name inconsistency** — source data uses dozens of variations for the same conference; a custom normalization map ensures clean grouping
- **Preserving manual staff input** — the scraper checks for existing data before writing, protecting hand-entered scouting notes and stat overrides
- **Handling missing data gracefully** — players with incomplete stat lines or no available data are flagged rather than erroring out, keeping the pipeline robust across hundreds of players

> **Note:** This repository is private to protect program-specific configurations. README and architecture are public for portfolio purposes.

## Impact

- Reduced manual recruiting research time from hours to minutes per cycle
- Provides coaching staff with a single, always-current source of truth for portal evaluation
- Enables faster, data-informed decisions during time-sensitive recruiting windows
- Currently in active use by Mount St. Mary's Men's Basketball program

## Future Work

- Add scheduled execution via cron for fully automated scheduled refreshes
- Integrate additional data sources (advanced metrics, highlight film links)
- Build a lightweight dashboard layer on top of the Google Sheet for visual comparisons
- Add filtering presets for coaching staff (e.g., "D1 guards, 15+ PPG, 6'2+")

## About

Built by **Jack d'Entremont** — MBA candidate (Data Science) at Mount St. Mary's University, Graduate Assistant for Men's Basketball, and former college basketball player at both the DIII and DI levels. This tool was built to solve a real operational problem in college basketball recruiting, combining web scraping, data engineering, and domain experience.

## License

This project is proprietary and maintained for use by Mount St. Mary's Men's Basketball. README shared for portfolio purposes.

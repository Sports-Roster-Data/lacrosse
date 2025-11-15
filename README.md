# Women's Lacrosse Roster Scraper

NCAA Women's Lacrosse roster scraper for Division I and Division II teams for the 2025 season.

## Overview

This scraper collects roster data from NCAA Division I and Division II women's lacrosse team websites. It's based on the women's soccer scraper architecture and adapted for lacrosse-specific data fields and positions.

## Features

- Scrapes roster data from 240+ women's lacrosse teams
  - 131 Division I teams
  - 112 Division II teams
- Extracts player information including:
  - Name
  - Jersey number
  - Position (G, D, M, A)
  - Height
  - Class/Year
  - Hometown
  - High School
  - Previous School
  - Major (when available)
- Supports multiple website formats (Sidearm Sports, custom CMS, etc.)
- Automatic fallback URL patterns
- Error tracking and reporting
- JSON and CSV output formats

## Installation

```bash
pip install beautifulsoup4 requests tldextract
```

## Usage

### Scrape all Division I teams:
```bash
python src/wlacrosse_roster_scraper.py --season 2025 --division I
```

### Scrape all Division II teams:
```bash
python src/wlacrosse_roster_scraper.py --season 2025 --division II \
  --teams-csv teams/2025_II_teams_with_urls.csv
```

### Scrape both Division I and II teams:
```bash
# Division I
python src/wlacrosse_roster_scraper.py --season 2025 --division I

# Division II
python src/wlacrosse_roster_scraper.py --season 2025 --division II \
  --teams-csv teams/2025_II_teams_with_urls.csv
```

### Scrape a specific team by ID:
```bash
python src/wlacrosse_roster_scraper.py --team 593889 --season 2025
```

### Test with limited teams:
```bash
python src/wlacrosse_roster_scraper.py --season 2025 --division I --limit 5
```

## Lacrosse Positions

The scraper recognizes and normalizes the following lacrosse positions:
- **G** (Goalie) - variations: GK, Goalie, Goalkeeper, Keeper
- **D** (Defense) - variations: DEF, Defense, Defender
- **M** (Midfield) - variations: MID, Midfield, Midfielder
- **A** (Attack) - variations: ATT, Attack, Attacker

## Data Structure

### Team CSV Files

**Division I:** `teams/2025_I_teams_with_urls.csv` (131 teams)
**Division II:** `teams/2025_II_teams_with_urls.csv` (112 teams)

CSV columns:
- `team_id` - NCAA team ID
- `school_name` - School name
- `url` - Team roster base URL
- `ncaa_division_formatted` - Division (I, II, or III)
- `team_conference_name` - Conference name
- `season` - Season year

## Output

Results are saved to `data/raw/` organized by division:

**Division I:**
- `json/rosters_wlax_2025_I.json` - JSON format
- `csv/rosters_wlax_2025_I.csv` - CSV format
- `csv/rosters_wlax_2025_I_zero_players.csv` - Teams with zero players found
- `csv/rosters_wlax_2025_I_failed.csv` - Teams that failed to scrape

**Division II:**
- `json/rosters_wlax_2025_II.json` - JSON format
- `csv/rosters_wlax_2025_II.csv` - CSV format
- `csv/rosters_wlax_2025_II_zero_players.csv` - Teams with zero players found
- `csv/rosters_wlax_2025_II_failed.csv` - Teams that failed to scrape

## Architecture

Based on the women's soccer scraper with adaptations for lacrosse:
- **StandardScraper**: Handles standard Sidearm Sports sites
- **JSScraper**: For sites requiring JavaScript rendering
- **URLBuilder**: Constructs roster URLs for different site patterns
- **FieldExtractors**: Parses player data fields
- **SeasonVerifier**: Validates roster page season
- **TeamConfig**: Team-specific configurations

## Notes

- Some sites may require JavaScript rendering or have anti-scraping measures
- SSL/connection errors may occur with certain sites due to TLS configuration
- The scraper includes automatic retry logic and fallback URL patterns

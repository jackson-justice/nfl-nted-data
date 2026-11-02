# NFL National Tight Ends Day Data

This repository contains the data-acquisition pipeline and cleaned dataset for a STAT 386 project about National Tight Ends Day (NTED).

## Research question

Do NFL tight ends perform better on National Tight Ends Day than on other Sundays? If performance changes, is the difference specific to tight ends, or do wide receivers and running backs show similar changes?

## Data source and scope

The project uses weekly player statistics and game schedules from [nflverse](https://github.com/nflverse), accessed in Python with [`nflreadpy`](https://nflreadpy.nflverse.com/api/load_functions/). It covers the 2019--2025 NFL regular seasons and retains player-game observations for tight ends, wide receivers, and running backs.

The cleaned dataset contains 35,840 player-game observations and 34 variables. Variables include targets, receptions, receiving yards and touchdowns, carries, game dates, NTED indicators, team tight end target share, and prior player-season averages.

## Repository contents

- `acquire_nted_data.py`: downloads, joins, validates, transforms, and saves the data.
- `nted_player_games_2019_2025.parquet`: analysis-ready player-game dataset.
- `requirements.txt`: pinned Python dependencies.
- `tests/test_acquire_nted_data.py`: tests for the NTED calendar rule, join validation, and target-share calculation.

When the acquisition script runs, it also creates:

- `data/nted_player_games_2019_2025.parquet`
- `data/nted_validation_2019_2025.json`
- `data/nted_position_summary_2019_2025.csv`
- `figures/nted_mean_receiving_yards.png`

## How the dataset was created

1.  Download weekly player statistics and schedules for 2019--2025.
2.  Confirm that schedule `game_id` values are unique.
3.  Join player rows to schedule rows with a validated many-to-one join.
4.  Keep regular-season TE, WR, and RB observations.
5.  Calculate the fourth Sunday in October for each season and flag games played on that date.
6.  Calculate team TE target share and prior player-season averages.
7.  Check keys, missing values, denominators, and row uniqueness before saving.

All 129,812 source player-stat rows matched a schedule record. The final data have no duplicate player-team-game rows or missing key identifiers.

## Reproduce the data

Create a virtual environment and install the dependencies:

``` powershell
python -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
```

Run the acquisition pipeline:

``` powershell
.\.venv\Scripts\python.exe acquire_nted_data.py
```

Run the tests:

``` powershell
.\.venv\Scripts\python.exe -m unittest discover -s tests -v
```

The pipeline downloads current copies of the specified nflverse releases, so source totals could change if nflverse republishes historical data.

## Data use and attribution

At the time of acquisition, nflverse identified these data under the [Creative Commons Attribution 4.0 International License](https://github.com/nflverse/nflverse-data/blob/main/LICENSE.md). Credit for the source data belongs to nflverse. Check the applicable nflverse release and license again before redistributing the data. The data are provided without warranty.
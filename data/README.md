# Dataset documentation

## Files

- `nted_player_games_2019_2025.parquet` is the analysis-ready dataset.
- `nted_validation_2019_2025.json` records source dimensions, join checks,
  missing-value checks, NTED dates, and final row counts.
- `nted_position_summary_2019_2025.csv` contains descriptive averages for TE,
  WR, and RB player-games on NTED and other Sundays.

## Unit of observation

Each row in the Parquet file represents one regular-season player-team-game
for a tight end (`TE`), wide receiver (`WR`), or running back (`RB`) during the
2019--2025 NFL seasons. The file contains 35,840 rows and 34 columns.

## Column groups

- Game context: `season`, `week`, `game_id`, `game_date`,
  `schedule_weekday`, `away_team`, and `home_team`.
- Player context: `player_id`, `player_display_name`, `position`, `team`, and
  `opponent_team`.
- Box-score production: `targets`, `receptions`, `receiving_yards`,
  `receiving_tds`, `carries`, `rushing_yards`, and `rushing_tds`.
- NTED context: `is_sunday`, `nted_date`, `nted_week`, `is_nted`,
  `is_other_sunday`, and `week_relative_to_nted`.
- Team opportunity: `team_targets`, `team_te_targets`, and
  `team_te_target_share`.
- Player baselines: `prior_player_games` and the five `prior_avg_*` columns.

Player baseline values are intentionally missing for a player's first recorded
appearance of a season because no prior player-season observation exists.
Zero-denominator team target shares would also be missing, although none were
present in this acquisition.

## Source and attribution

The files were derived from the
[nflverse-data](https://github.com/nflverse/nflverse-data) weekly player
statistics and schedules using
[`nflreadpy`](https://nflreadpy.nflverse.com/api/load_functions/). The source
tables were joined, restricted, and modified with calendar, prior
player-season, and team target-share features. See
[`../acquire_nted_data.py`](../acquire_nted_data.py) for the complete process.

At the time of acquisition, nflverse identified these data under the
[Creative Commons Attribution 4.0 International
License](https://github.com/nflverse/nflverse-data/blob/main/LICENSE.md).
Credit: nflverse. Check the applicable release and license before
redistributing these files. The data are provided without warranty.

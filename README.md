# 🏀 WNBA Draft Analytics (2026 Prospect Board)

## Overview
This project analyzes WNBA draft prospects using a combination of raw player statistics and derived performance metrics to create a structured prospect board with archetypes, draft scores, and draft grades.

The purpose of this project is not to predict the draft with certainty, but to demonstrate strong SQL fundamentals, feature engineering, and analytical thinking in a sports analytics context. This repository is intended as a portfolio project showcasing data modeling and querying skills.

---

## Data Sources
The data used in this project comes from publicly available WNBA and NCAA prospect statistics and is stored locally in a SQLite database (`wnba_draft.db`).

### Core Tables
**prospects**
- Contains raw player statistics such as games played, minutes, points, rebounds, assists, shooting splits, and team information.
- Primary identifier: `player_name`

**prospect_board_archetypes**
- Derived table created from `prospects`
- Includes standardized metrics, a composite `draft_score`, a qualitative `draft_grade`, and an assigned player `archetype`.

---

## Feature Engineering
Several advanced metrics are computed directly in SQL to demonstrate analytical fluency and transformation logic, including:

- Per-40 statistics (PTS, REB, AST, STL, BLK, TOV)
- Shooting efficiency metrics (eFG%, TS%)
- Shot profile rates (three-point rate, free-throw rate)
- Assist-to-turnover ratio
- Defensive activity (stocks per 40)

These features are later standardized and combined to form draft-oriented evaluation signals.

---

## Prospect Archetypes
Players are assigned archetypes using rule-based logic applied to standardized metrics. This approach prioritizes interpretability and transparency over black-box clustering techniques.

Examples include scoring-focused guards, playmaking guards, two-way wings, and movement shooters.

---

## SQL Design Choices
### Joins Over Multiple SELECTs
Player profiles are generated using JOINs between raw data and derived tables. This ensures:

- One clean output per query
- Clear relational modeling
- Recruiter-friendly SQL that demonstrates real-world querying patterns

Example:
```sql
SELECT
    pb.Name,
    pb.POS,
    pb.draft_score,
    pb.draft_grade,
    pb.archetype,
    p.team_abbr,
    p.GP,
    p.MIN,
    p.PTS
FROM prospect_board_archetypes pb
LEFT JOIN prospects p
    ON pb.Name = p.player_name
WHERE pb.Name = 'Berry Wallace';

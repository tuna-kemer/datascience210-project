# NBA Position Aging Analysis

**DSA 210 — Introduction to Data Science | Spring 2025-2026**

## Research Question
Which NBA positions age better, and are these differences related to how key skills (athleticism, shooting, playmaking) decline over time?

---

## Data Sources
- **nba_api:** Season-level per-game stats for 2010-2024 (14 seasons, 7190 raw observations)
- **Kaggle (Damir Dizdarevic):** Position labels and advanced stats (PER, BPM, VORP, WS)

## Dataset
- 3570 player-season observations after merging
- 5 positions: PG, SG, SF, PF, C
- Filter: 1000+ minutes per season
- 26 features including TS%, BPM, PER, VORP, WS

---

## Exploratory Data Analysis

| Visualization | Description |
|---|---|
| Age Distribution by Position | All positions peak at ages 25-27 |
| BPM Trend by Age | Performance improves until ~28, then levels off |
| Stats by Position (Boxplot) | PGs lead assists, Cs lead blocks |
| Correlation Heatmap | PER, BPM, VORP, WS are highly correlated (0.80-0.95) |
| TS% and BPM Trends (Smoothed) | Centers maintain highest TS% across all ages |
| Skill Group Analysis | Athleticism declines, shooting improves, playmaking stable with age |

---

## Hypothesis Tests

### Test 1: C vs PG BPM (t-test)
- **H₀:** No difference in BPM between Centers and Guards
- **Result:** p=0.234 → No significant difference
- Centers BPM: 0.726, Guards BPM: 0.548

### Test 2: Age vs TS% (Pearson Correlation)
- **H₀:** No correlation between age and True Shooting %
- **Result:** r=0.086, p<0.001 → Significant positive correlation
- PGs show strongest improvement with age (r=0.251)
- Centers show no significant change (r=-0.049)

### Test 3: BPM Before/After Age 28 (ANOVA)
- **H₀:** No difference in BPM across positions
- **Result:** F=30.423, p<0.001 → Significant difference
- All positions improve BPM after age 28 (SF: +0.55, PG: +0.42)

### Test 4: Athleticism Decline (Pearson Correlation)
- **H₀:** No relationship between age and athleticism
- **Result:** PG steals decline (r=-0.104, p<0.01), PF blocks decline most (r=-0.212, p<0.001)
- Centers maintain blocking ability with age (not significant)

---

## Key Findings
- Centers maintain the highest shooting efficiency (TS%) across all ages
- PG shooting improves most significantly with age (r=0.251, p<0.001)
- All positions improve overall performance (BPM) after age 28
- PF athleticism declines most sharply with age (r=-0.212)
- Significant BPM differences exist across positions (ANOVA p<0.001)

---

## Project Structure
```
datascience210-project/
├── nba_analysis.ipynb     # Main notebook (data collection, EDA, hypothesis tests)
├── nba_final.csv          # Processed dataset (3570 rows, 26 features)
├── requirements.txt       # Python dependencies
└── README.md
```

## How to Run
1. Open `nba_analysis.ipynb` in Google Colab
2. Run all cells in order
3. Upload `archive.zip` (Kaggle dataset) when prompted at Cell 4

## Dependencies
```
nba_api
pandas
matplotlib
seaborn
scipy
```

---

## Notes
This project was completed as part of DSA210 coursework at Sabancı University.
AI assistance (Claude) was used in accordance with course guidelines for coding support and debugging.

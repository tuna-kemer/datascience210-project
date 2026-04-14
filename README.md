# NBA Position Aging Analysis

**DSA 210 — Introduction to Data Science | Spring 2025-2026**

---

## 1. Research Question
Which NBA positions age better, and are these differences related to how key skills (athleticism, shooting, playmaking) decline over time?

---

## 2. Data Sources & Dataset

### 2.1 Data Sources
- **nba_api:** Season-level per-game stats for 2010-2024 (14 seasons, 7190 raw observations)
- **Kaggle (Damir Dizdarevic):** Position labels and advanced stats (PER, BPM, VORP, WS)

### 2.2 Dataset Characteristics
- 3570 player-season observations after merging
- 5 positions: PG, SG, SF, PF, C
- Filter: 1000+ minutes per season
- 26 features including TS%, BPM, PER, VORP, WS

---

## 3. Exploratory Data Analysis

### 3.1 Visualizations

| Visualization | Description |
|---|---|
| Age Distribution by Position | All positions peak at ages 25-27 |
| BPM Trend by Age | Performance improves until ~28, then levels off |
| Stats by Position (Boxplot) | PGs lead assists, Cs lead blocks |
| Correlation Heatmap | PER, BPM, VORP, WS are highly correlated (0.80-0.95) |
| TS% and BPM Trends (Smoothed) | Centers maintain highest TS% across all ages |
| Skill Group Analysis | Athleticism declines, shooting improves, playmaking stable with age |

---

## 4. Hypothesis Testing

To validate the findings from exploratory data analysis, formal statistical tests were conducted.

### 4.1 Do Centers Age Better Than Guards in Terms of BPM? (t-test)

- **Null Hypothesis (H₀):** There is no difference in BPM between Centers and Point Guards.
- **Alternative Hypothesis (H₁):** Centers maintain higher BPM than Guards with age.
- **Test Used:** Independent samples t-test
- **Result:** T-statistic=1.191, p=0.234

**Conclusion:**
The null hypothesis cannot be rejected. There is no statistically significant difference in BPM between Centers (mean=0.726) and Guards (mean=0.548).

---

### 4.2 Does True Shooting % Change With Age? (Pearson Correlation)

- **Null Hypothesis (H₀):** There is no correlation between age and True Shooting %.
- **Alternative Hypothesis (H₁):** True Shooting % changes significantly with age.
- **Test Used:** Pearson Correlation
- **Result:** r=0.086, p<0.001

**Conclusion:**
The null hypothesis is rejected. There is a statistically significant positive correlation between age and TS%. PGs show the strongest improvement (r=0.251), while Centers show no significant change (r=-0.049, p=0.225).

---

### 4.3 Do Positions Differ in Performance After Age 28? (ANOVA)

- **Null Hypothesis (H₀):** There is no difference in BPM across positions for players over 28.
- **Alternative Hypothesis (H₁):** Some positions maintain performance better after age 28.
- **Test Used:** One-way ANOVA
- **Result:** F=30.423, p<0.001

**Conclusion:**
The null hypothesis is rejected. BPM differs significantly across positions. All positions improve after age 28, with SF showing the largest gain (+0.55) and PF the smallest (+0.30).

---

### 4.4 Does Athleticism Decline With Age? (Pearson Correlation)

- **Null Hypothesis (H₀):** There is no relationship between age and athleticism metrics.
- **Alternative Hypothesis (H₁):** Athleticism declines significantly with age.
- **Test Used:** Pearson Correlation (STL for guards, BLK for bigs)
- **Result:** PG steals: r=-0.104 (p<0.01), PF blocks: r=-0.212 (p<0.001)

**Conclusion:**
The null hypothesis is rejected for PG and PF. PF athleticism declines most sharply with age. Centers maintain their blocking ability (r=-0.055, p=0.175).

---

## 5. Key Findings
- Centers maintain the highest shooting efficiency (TS%) across all ages
- PG shooting improves most significantly with age (r=0.251, p<0.001)
- All positions improve overall performance (BPM) after age 28
- PF athleticism declines most sharply with age (r=-0.212, p<0.001)
- Significant BPM differences exist across positions (ANOVA p<0.001)

---

## 6. Project Structure
```
datascience210-project/
├── nba_analysis.ipynb     # Main notebook (data collection, EDA, hypothesis tests)
├── nba_final.csv          # Processed dataset (3570 rows, 26 features)
├── requirements.txt       # Python dependencies
└── README.md
```


## 7. Dependencies
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

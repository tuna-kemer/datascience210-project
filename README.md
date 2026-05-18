# NBA Position Aging Analysis

🌐 Live Website: https://tuna-kemer.github.io/datascience210-project/

**DSA 210 — Introduction to Data Science | Spring 2025-2026**  
**Sabancı University**

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

All EDA visualizations are available in `figures/eda/`.

---

## 4. Hypothesis Testing

To validate the findings from exploratory data analysis, formal statistical tests were conducted.

### 4.1 Do Centers Age Better Than Guards in Terms of BPM? (t-test)

- **Null Hypothesis (H₀):** There is no difference in BPM between Centers and Point Guards.
- **Alternative Hypothesis (H₁):** Centers maintain higher BPM than Guards with age.
- **Test Used:** Independent samples t-test
- **Result:** T-statistic=1.191, p=0.234, Cohen's d=0.066 (negligible effect)

**Conclusion:** The null hypothesis cannot be rejected. No statistically significant BPM difference between Centers (mean=0.726) and Guards (mean=0.548).

---

### 4.2 Does True Shooting % Change With Age? (Pearson Correlation)

- **Null Hypothesis (H₀):** There is no correlation between age and True Shooting %.
- **Alternative Hypothesis (H₁):** True Shooting % changes significantly with age.
- **Test Used:** Pearson Correlation
- **Result:** r=0.086, p<0.001 (negligible overall; PG: r=0.251, small effect)

**Conclusion:** The null hypothesis is rejected. TS% increases with age. PGs show the strongest improvement (r=0.251), Centers show no significant change (r=-0.049, p=0.225).

---

### 4.3 Do Positions Differ in Performance After Age 28? (ANOVA)

- **Null Hypothesis (H₀):** There is no difference in BPM across positions for players over 28.
- **Alternative Hypothesis (H₁):** Some positions maintain performance better after age 28.
- **Test Used:** One-way ANOVA
- **Result:** F=30.423, p<0.001, Eta²=0.031 (small effect)

**Conclusion:** The null hypothesis is rejected. BPM differs significantly across positions after age 28. SF shows the largest gain (+0.55), PF the smallest (+0.30).

---

### 4.4 Does Athleticism Decline With Age? (Pearson Correlation)

- **Null Hypothesis (H₀):** There is no relationship between age and athleticism metrics.
- **Alternative Hypothesis (H₁):** Athleticism declines significantly with age.
- **Test Used:** Pearson Correlation (STL for guards, BLK for bigs)
- **Result:** PG steals: r=-0.104 (p<0.01, small effect), PF blocks: r=-0.212 (p<0.001, small effect)

**Conclusion:** The null hypothesis is rejected for PG and PF. PF athleticism declines most sharply. Centers maintain blocking ability (r=-0.055, p=0.175, negligible).

---

## 5. Machine Learning

### 5.1 Feature Engineering
Career slope features were computed for each player with 3+ seasons (510 players):
- **BPM_SLOPE:** Overall performance trend per year of age
- **STL_SLOPE, BLK_SLOPE:** Athleticism trends
- **TS_SLOPE:** Shooting efficiency trend
- **AST_SLOPE:** Playmaking trend

### 5.2 Models

**Ridge Regression**
- Task: Predict BPM slope from skill trends and position
- Result: R²=0.524 (5-fold CV), R²=0.630 (test)
- Key finding: TS_SLOPE is the strongest predictor of good aging

**Model Comparison (Baseline vs Extended)**
- Baseline (skill slopes only): R²=0.515
- Extended (+ position + age): R²=0.524
- Improvement: +0.009 → position adds negligible predictive value

**K-Means Clustering (k=3)**
- Task: Group players by aging profile
- Result: 3 clusters — declining, stable, improving
- Key finding: Cluster composition is similar across positions — skill profile matters more than position

**Logistic Regression**
- Task: Classify players as "good ager" vs "not good ager"
- Result: Accuracy=0.957 (5-fold CV), 0.941 (test)
- Key finding: STL_SLOPE and AST_SLOPE are the strongest signals of good aging

All ML visualizations are available in `figures/ml/`.

---

## 6. Key Findings
- Centers maintain the highest shooting efficiency (TS%) across all ages
- PG shooting improves most significantly with age (r=0.251, p<0.001)
- All positions improve overall performance (BPM) after age 28
- PF athleticism declines most sharply with age (r=-0.212, p<0.001)
- Significant BPM differences exist across positions (ANOVA p<0.001)
- Adding position to ML model improves R² by only +0.009 — skill profile predicts aging better than position

---

## 7. Project Structure

```
datascience210-project/
├── data/
│   ├── nba_raw.csv          # Raw NBA stats from nba_api (3683 rows)
│   ├── nba_kaggle.csv       # Kaggle advanced stats dataset
│   └── nba_final.csv        # Merged final dataset (3570 rows, 26 features)
├── figures/
│   ├── eda/                 # EDA visualizations (6 figures)
│   └── ml/                  # ML visualizations (6 figures)
├── notebooks/
│   └── nba_analysis.ipynb   # Main notebook (data collection, EDA, hypothesis tests, ML)
├── Project_Proposal_Document.pdf
├── final_report.md
├── final_report.pdf
├── index.html              # Interactive project website
├── requirements.txt
└── README.md
```

---

## 8. How to Reproduce

1. Clone the repository:
```bash
git clone https://github.com/tuna-kemer/datascience210-project.git
cd datascience210-project
```

2. Install dependencies:
```bash
pip install -r requirements.txt
```

3. Upload `nba_final.csv` from the `data/` folder to your Colab session, then open and run:
```
notebooks/nba_analysis.ipynb
```

All figures will be saved to `figures/eda/` and `figures/ml/` automatically.

---

## 9. Dependencies

```
nba_api
pandas
matplotlib
seaborn
scipy
scikit-learn
```

---

## 10. AI Usage Disclosure

AI assistance (Claude by Anthropic) was used in this project in accordance with Sabancı University DSA210 course guidelines. Specifically, Claude was used for:
- Debugging Python code and resolving library errors
- Suggesting and structuring ML model implementations
- Reviewing and improving README and report formatting

All analysis decisions, interpretations, and conclusions are the author's own work.

---

## Notes
This project was completed as part of DSA210 coursework at Sabancı University.

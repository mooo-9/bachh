# ⚖ HR Bias Detection System

A single-file, client-side dashboard that applies **NLP analysis**, **ML bias prediction**, and **fairness metrics** to HR performance evaluations — helping organisations surface and mitigate implicit bias in the review process.

Built as the capstone artefact for a Bachelor's thesis on *AI Analytics for Bias Detection in HR Performance Evaluations* at the German University in Cairo (GUC), 2026.

---

## ✨ Features

The application is organised into six independent modules accessible from the sidebar:

| Module | Description |
|--------|-------------|
| 📝 **Evaluation Form** | Submit a performance review and receive instant NLP analysis, a bias risk score, highlighted problematic phrases, and a debiased rewrite suggestion |
| 📊 **Analytics Dashboard** | Charts and tables showing rating distributions by gender/department, evaluator consistency, sentiment analysis, a demographic disparity heatmap, and three fairness metric cards |
| 🤖 **ML Bias Predictor** | Loads the evaluation dataset, predicts expected ratings from objective KPI signals (tenure, KPI score, peer score), and flags evaluations where the actual rating deviates significantly |
| 🔍 **NLP Analyzer** | Paste any free-text review for standalone analysis — returns highlighted bias phrases, bias type, tone classification, feedback category breakdown, and a debiased rewrite |
| 📋 **Fairness Framework** | Interactive three-stage bias mitigation pipeline covering pre-processing (data reweighting), in-processing (fairness constraints), and post-processing (per-group decision thresholds), plus an audit log |
| 📈 **XAI Explainability** | Feature importance chart and plain-language explanation for any flagged evaluation, with a side-by-side view of the original vs. debiased feedback |

**Additional capabilities:**
- Red alert badge in the top navigation bar showing the live count of flagged evaluations
- **Generate Report** button — exports a structured `.txt` summary of fairness metrics, evaluator and department statistics, and all flagged evaluations
- Cross-module navigation: clicking **Explain →** in the ML Predictor jumps directly to the matching record in the XAI module

---

## 🧠 How It Works

### NLP Bias Detection
The NLP engine scans written feedback against four bias categories using regex-based phrase matching:

- **Gendered language** — terms disproportionately applied to women (e.g. *emotional*, *bossy*, *nurturing*)
- **Personality-focused language** — trait descriptions instead of outcome-based assessment (e.g. *likeable*, *attitude*, *charming*)
- **Vague language** — non-actionable qualifiers applied unevenly across demographics (e.g. *sometimes*, *could try*, *lacks*)
- **Recency bias** — overweighting of recent events at the expense of full-period performance (e.g. *lately*, *just*, *this quarter*)

Each detected phrase is highlighted inline with a tooltip explaining the bias type. A **Bias Risk Score (0–100)** is computed from the weighted frequency of flagged terms.

### ML Bias Predictor
A rule-based regression model predicts an expected rating from three objective input signals:

```
predicted = (tenure × 0.30) + (KPI score × 0.50) + (peer score × 0.20)
```

Evaluations where the actual rating deviates from the prediction by more than **0.8 points** are flagged for potential bias.

### Fairness Metrics
Three industry-standard fairness criteria are computed across gender groups:

| Metric | Threshold | Description |
|--------|-----------|-------------|
| Demographic Parity Difference | < 0.20 | Difference in average ratings between male and female employees |
| Disparate Impact Ratio | ≥ 0.80 | Ratio of female to male average rating |
| Equal Opportunity Difference | < 0.10 | Difference in true positive rates (rating ≥ 4) across groups |

### Seed Dataset
The application ships with **50 pre-processed evaluation records** across 5 departments and 3 evaluators, each exhibiting a distinct bias pattern:

- **MGR-A** — Leniency bias (consistently rates above KPI prediction)
- **MGR-B** — Harshness bias (consistently rates below KPI prediction)
- **MGR-C** — Gender bias (female employees with equivalent KPIs receive ratings 1.5–2 points below male counterparts)

---

## 🚀 Getting Started

No installation, build tools, or server required. All dependencies are loaded via CDN at runtime.

1. **Clone the repository**
   ```bash
   git clone https://github.com/your-username/hr-bias-detection.git
   cd hr-bias-detection
   ```

2. **Open the app**
   ```
   Open hr-bias-detection.html in any modern browser (Chrome, Firefox, Edge, Safari)
   ```

That's it. The app loads instantly with the seed dataset pre-populated.

---

## 🛠 Tech Stack

| Technology | Role |
|------------|------|
| [React 18](https://react.dev/) (via unpkg CDN) | UI component model and state management |
| [Babel Standalone](https://babeljs.io/docs/babel-standalone) | In-browser JSX compilation |
| [Chart.js 4](https://www.chartjs.org/) | Bar charts, horizontal charts, and data visualisations |
| [Tailwind CSS](https://tailwindcss.com/) (via CDN play script) | Utility-first styling |

**No npm. No build step. No backend.** Everything runs client-side in a single `hr-bias-detection.html` file (~106 KB).

---

## 📁 Project Structure

```
hr-bias-detection/
└── hr-bias-detection.html   # The entire application
```

The script block inside the HTML file is organised in this order:

```
Constants → Seed Data → Utility Functions → Shared Components
→ Module 1 (Evaluation Form)
→ Module 2 (Analytics Dashboard)
→ Module 3 (ML Bias Predictor)
→ Module 4 (NLP Analyzer)
→ Module 5 (Fairness Framework)
→ Module 6 (XAI Explainability)
→ App Shell (TopNav + Sidebar + Router)
→ Render
```

---

## 📊 Module Details

### Module 1 — Evaluation Form
Fill in employee demographic data, select a star rating (1–5), and write free-text feedback. On submission:
- Bias phrases are highlighted inline with hover tooltips explaining each flag
- The bias gauge shows a risk score from 0 to 100
- The ML model predicts the expected rating and shows the deviation
- A debiased rewrite is generated by substituting flagged terms with neutral alternatives

### Module 2 — Analytics Dashboard
Aggregates all evaluations (seed + submitted) into:
- **Fairness metric cards** — live status indicators with thresholds
- **Rating distribution chart** — grouped bar chart by gender and department
- **Evaluator consistency panel** — average rating per evaluator with standard deviation
- **Sentiment analysis chart** — average sentiment score per gender group
- **Demographic disparity heatmap** — ethnicity × department grid showing deviation from the company mean rating

### Module 3 — ML Bias Predictor
Simulates a CSV dataset upload, then renders a sortable, filterable table of all evaluations with their actual rating, predicted rating, deviation, and flag status. Flagged rows link directly to the XAI module.

### Module 4 — NLP Analyzer
A standalone text analysis tool. Paste any performance review and receive:
- Inline highlighted bias phrases with per-phrase tooltip explanations
- Bias type and tone classification
- Feedback category radar (Strengths / Weaknesses / Leadership / Development keyword counts)
- A one-click debiased rewrite with a clipboard copy button

### Module 5 — Fairness Framework
An interactive three-stage pipeline:
- **Pre-processing**: Toggle demographic reweighting to see how balancing group representation affects the dataset distribution
- **In-processing**: Enable fairness constraints (Demographic Parity, Equalized Odds, Individual Fairness, Calibration) and observe the trade-off with model accuracy
- **Post-processing**: Adjust per-group decision thresholds and see outcome rate changes in real time
- **Audit log**: Review flagged evaluations with severity ratings and a resolution status dropdown

### Module 6 — XAI Explainability
Select any flagged evaluation to see:
- A **feature importance horizontal bar chart** ranking the six factors that drove the flag (rating deviation, evaluator pattern, sentiment, keyword density, demographic attribute, department context)
- A **plain-language explanation** describing exactly why the evaluation was flagged
- A **side-by-side view** of the original feedback (highlighted) vs. the debiased version

---

## 📄 Generating a Report

Click **Generate Report** in the top navigation bar to download a structured `.txt` file containing:
- Total evaluations analysed and flag rate
- All three fairness metrics with threshold indicators
- Per-evaluator summary (count, average rating, flagged count)
- Per-department summary (average rating)
- Full list of flagged evaluations with deviation details

---

## 🔬 Research Context

This application was developed to support a thesis examining whether AI-assisted NLP and ML tools can reliably detect implicit bias in HR performance evaluations. The seed dataset was designed to embed three known bias patterns (leniency, harshness, and gender bias) so that the detection pipeline could be validated against ground truth.

The fairness metrics implemented follow established AI fairness literature:
- *Feldman et al. (2015)* — Disparate Impact
- *Hardt et al. (2016)* — Equalized Odds
- *Dwork et al. (2012)* — Individual Fairness

---

## 📜 License

This project is released for academic and educational purposes. See `LICENSE` for details.

---

## 👤 Author

**Mo** — Business Informatics, German University in Cairo (GUC), Class of 2026

---

> *"What gets measured gets managed — but only if the measurements are fair."*

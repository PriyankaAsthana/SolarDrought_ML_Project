# ☀️ SolarDrought
### Statistical Identification and Characterization of Solar Energy Droughts

[![Python](https://img.shields.io/badge/Python-3.x-blue?logo=python)](https://www.python.org/)
[![Research](https://img.shields.io/badge/Project-Independent%20Research-purple)]()
[![Status](https://img.shields.io/badge/Status-Ongoing-orange)]()
[![Data](https://img.shields.io/badge/Data-Hourly%20Solar%20Observations-yellow)]()

**SolarDrought** is an independent research project investigating how persistent periods of anomalously low solar energy availability can be statistically identified and characterized using historical solar radiation data.

The project focuses on distinguishing ordinary variations in solar irradiance from persistent low-availability episodes. Rather than interpreting every low irradiance observation as a drought, it uses clear-sky normalization, climatological anomaly detection, temporal persistence analysis, and event-level characterization to develop a statistically grounded identification framework.

---

## 🔍 Research Motivation

Solar energy availability varies with atmospheric conditions, cloud cover, aerosols, solar geometry, and seasonal changes. However, identifying periods of unusually low solar availability is not as straightforward as detecting low irradiance values.

For example:
- Solar irradiance naturally approaches zero at night.
- The same irradiance value can represent different atmospheric conditions depending on the time of day and season.
- Isolated low-irradiance observations do not necessarily constitute an energy drought.
- Persistent periods of below-normal solar availability may have greater implications for renewable energy reliability.

This project therefore investigates:

**Can historical solar irradiance data be used to statistically identify persistent periods of anomalously low solar energy availability, and can these periods be characterized as solar energy drought events?**

---

## 🎯 Research Objectives

1. Explore and quality-check historical solar radiation observations.
2. Normalize observed global horizontal irradiance against a clear-sky reference.
3. Develop and compare climatological anomaly-detection baselines.
4. Evaluate threshold stability using leave-one-year-out validation.
5. Identify temporally persistent low-solar-availability episodes.
6. Characterize candidate events using duration, intensity, and cumulative deficit.
7. Investigate relationships between solar availability and atmospheric conditions.
8. Explore the potential for forecasting persistent solar-availability anomalies.

---

## 📊 Dataset Overview

The current analysis uses hourly solar radiation observations for **Davos, Switzerland**.

| Attribute | Description |
|---|---|
| Study location | Davos, Switzerland |
| Dataset coverage | 1 January 2015 – 2 January 2025 |
| Total observations | 87,696 |
| Temporal resolution | Hourly |
| Complete calendar years | 2015–2024 |
| Primary variable | Global Horizontal Irradiance (GHI) |
| Reference variable | Clear-sky GHI |
| Source | Copernicus Atmosphere Monitoring Service (CAMS) |

### Solar Radiation Variables

The dataset contains:

- **GHI:** Global Horizontal Irradiance
- **BHI:** Beam Horizontal Irradiance
- **DHI:** Diffuse Horizontal Irradiance
- **BNI:** Beam Normal Irradiance
- **Clear-sky counterparts:** Corresponding radiation components under clear-sky conditions
- **Reliability:** Observation reliability indicator

The primary analysis uses GHI and clear-sky GHI to derive a normalized solar-availability measure.

*Note: The dataset includes nine positive-reference CSI observations from January 2025. The climatological validation therefore uses the ten complete calendar years from 2015–2024.*

---

## ⚙️ Methodology

### 1. Data Exploration and Quality Assessment

Initial data analysis examines:
- Temporal coverage and hourly continuity
- Missing and duplicate observations
- Radiation component consistency
- Negative radiation values
- Nighttime and positive-reference observations
- Reliability indicators
- Observed versus clear-sky radiation relationships

Observations are retained in the analytical dataset rather than automatically discarded based on arbitrary quality thresholds. Potentially sensitive observations are flagged for subsequent analysis.

### 2. Clear-Sky Normalization

Raw GHI is normalized against clear-sky GHI using the Clear-Sky Index (CSI):

\[
CSI_t = \frac{GHI_{\text{observed},t}}{GHI_{\text{clear},t}}
\]

The CSI provides a normalized measure of observed solar availability relative to its clear-sky reference.

- Nighttime observations with zero clear-sky reference are excluded from CSI calculations.
- Small positive reference values are flagged for sensitivity analysis.
- CSI values are not automatically clipped to the interval [0,1].

### 3. Statistical Anomaly Detection

Three candidate climatological baselines are evaluated:

| Baseline | Description |
|---|---|
| Global Q10 | Overall 10th-percentile CSI threshold |
| Monthly Q10 | Month-specific 10th-percentile threshold |
| Month × Hour Q10 | Month-and-hour-specific 10th-percentile threshold |

The thresholds are evaluated through **leave-one-year-out validation**, in which each complete year is held out while the remaining nine years are used to estimate its climatological threshold.

This approach helps assess temporal stability without randomly mixing observations from different years.

### 4. Persistence Analysis

Anomalous observations are grouped into consecutive hourly runs.

The analysis investigates three candidate persistence criteria:

- ≥3 consecutive anomalous hours
- ≥6 consecutive anomalous hours
- ≥12 consecutive anomalous hours

Temporal continuity is explicitly checked so that observations separated by nighttime or missing time intervals are not incorrectly treated as consecutive.

### 5. Event Characterization

Persistent anomaly candidates are examined using:

- Event start and end times
- Duration
- Minimum CSI
- Mean CSI
- Intensity
- Mean CSI deficit
- Cumulative CSI deficit
- Seasonal occurrence

The cumulative CSI deficit is calculated as:

\[
S_e = \sum_{t \in e}(1-CSI_t)
\]

For equally spaced hourly observations, this is equivalent to:

\[
S_e = D_e(1-\overline{CSI}_e)
\]

where \(D_e\) is event duration in hours.

This is a normalized cumulative availability-deficit measure, not a direct measurement of lost electrical energy.

---

## 📈 Preliminary Research Findings

The current statistical analysis has established the following results:

| Metric | Result |
|---|---:|
| Hourly observations | 87,696 |
| Complete years analysed | 10 |
| Positive-reference CSI observations | 47,621 |
| Selected primary anomaly baseline | Global Q10 |
| Global Q10 held-out anomaly rate | Approximately 10.0% |
| Global–monthly hourly Jaccard similarity | 0.8246 |
| ≥3-hour persistent anomaly runs | 590 |
| ≥6-hour persistent anomaly runs | 116 |
| ≥12-hour persistent anomaly runs | 4 |

### Primary Methodological Configuration

Based on the combined evaluation of calibration, annual stability, hourly agreement, persistence, event severity, and sensitivity to low-reference observations, the current primary framework is:

**Global Q10 CSI threshold + ≥6 consecutive anomalous hours.**

The ≥3-hour and ≥12-hour persistence criteria are retained as sensitivity analyses. Monthly Q10 and month × hour Q10 are also retained as alternative climatological baselines.

The 116 events identified under the primary framework have:

- Mean duration: **7.04 hours**
- Mean minimum CSI: **0.085**
- Mean cumulative CSI deficit: **6.22 CSI-hours**

These are preliminary statistical anomaly-event results, not yet independently validated physical solar-energy drought events.

---

## 🧰 Technology Stack

| Technology | Application |
|---|---|
| Python | Research implementation |
| Pandas | Data manipulation and preprocessing |
| NumPy | Numerical computation |
| Matplotlib | Data visualization |
| pvlib | Solar position and solar geometry analysis |
| Jupyter Notebook | Exploratory and statistical analysis |
| Git & GitHub | Version control and research documentation |

---

## 📁 Repository Structure

```text
SolarDrought/
│
├── README.md
├── requirements.txt
├── .gitignore
│
├── data/
│   ├── raw/
│   │   ├── solar/
│   │   ├── atmospheric/
│   │   └── forecast/
│   │
│   ├── interim/
│   └── processed/
│
├── notebooks/
│   ├── 01_data_exploration.ipynb
│   ├── 02_data_quality.ipynb
│   ├── 03_solar_normalization.ipynb
│   ├── 03a_historical_data_preparation.ipynb
│   ├── 03b_multi_year_data_preparation.ipynb
│   ├── 04_anomaly_detection.ipynb
│   ├── 05_drought_detection.ipynb
│   ├── 06_atmospheric_analysis.ipynb
│   ├── 07_statistical_analysis.ipynb
│   └── 08_drought_prediction.ipynb
│
├── src/
│   └── solardrought/
│       ├── init.py
│       ├── config.py
│       ├── data/
│       ├── solar/
│       ├── drought/
│       ├── atmosphere/
│       ├── statistics/
│       ├── models/
│       └── visualization/
│
├── results/
│   ├── figures/
│   ├── tables/
│   └── models/
│
├── reports/
│   ├── methodology.md
│   ├── findings.md
│   └── paper/
│
└── tests/
    ├── test_cleaning.py
    ├── test_normalization.py
    └── test_detection.py
```

*The repository structure includes planned modules and notebooks that are still under development.*

---

## 🚀 Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/PriyankaAsthana/SolarDrought_ML_Project.git
cd SolarDrought_ML_Project
```

### 2. Create a virtual environment

```bash
python -m venv .venv
```

Activate it:

**Windows:**
```bash
.venv\Scripts\activate
```

**macOS / Linux:**
```bash
source .venv/bin/activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Launch Jupyter Notebook

```bash
jupyter notebook
```

Start with the data exploration and quality-assessment notebooks, followed by solar normalization, historical data preparation, and anomaly detection.

Ensure the required input datasets are available in their expected `data/` directories before executing the notebooks.

---

## 🔭 Future Research Directions

The project is ongoing. Planned research stages include:

- **Drought-event detection:** Formal event identification and severity characterization.
- **Atmospheric characterization:** Investigating cloud properties, aerosol optical depth, and meteorological conditions.
- **Statistical analysis:** Examining seasonal patterns, relationships, and statistical significance.
- **Drought prediction:** Developing and evaluating forecasting models using historical solar and atmospheric features.
- **Robustness evaluation:** Assessing sensitivity to alternative thresholds, persistence criteria, and data-quality conditions.

The ultimate objective is to develop a statistically defensible framework for identifying, understanding, and potentially forecasting persistent periods of reduced solar energy availability.

---

## 📚 Research Philosophy

SolarDrought follows a research-first approach:

**Data quality before modelling.  
Physical normalization before anomaly detection.  
Temporal validation before prediction.  
Sensitivity analysis before conclusions.**

The project emphasizes reproducibility, transparent methodological decisions, and careful interpretation of statistical results.

---

## 👩‍💻 Author

**Priyanka Asthana**

B.Tech (Honours) — Computer Science & Engineering  
Minor in Robotics

Independent Research Project

- GitHub: [PriyankaAsthana](https://github.com/PriyankaAsthana)
- Project Repository: [SolarDrought_ML_Project](https://github.com/PriyankaAsthana/SolarDrought_ML_Project)

---

*SolarDrought is an ongoing independent research project. Results and methodologies are subject to further validation as the research progresses.*

# Gradually Varied Flow (GVF) – Water Surface Profile Analysis

A Jupyter notebook for computing and visualising the **water surface profile**
of gradually varied flow in a **rectangular open channel** using the
**Direct Step Method** (Manning's equation).

---

## What is Gradually Varied Flow?

Gradually Varied Flow (GVF) is a steady, non-uniform open channel flow where
the depth changes slowly along the channel length.  It occurs when there is a
disturbance to uniform flow (e.g. a gate, a change in slope, a weir).

The governing differential equation is:

```
dy/dx = (So - Sf) / (1 - Fr²)
```

where **Sₒ** = bed slope, **Sf** = friction slope, **Fr** = Froude number.

### Direct Step Method

Instead of solving the ODE directly, the Direct Step Method works backwards from
a known boundary depth.  For each step in depth Δy, it computes the
corresponding distance Δx:

```
Δx = ΔE / (So − S̄f)
```

where **ΔE** is the change in specific energy and **S̄f** is the average friction
slope between two sections.

### Profile Types

| Symbol | Slope | Condition | Description |
|---|---|---|---|
| M1 | Mild | y > yn > yc | Backwater curve |
| M2 | Mild | yn > y > yc | Drawdown curve |
| M3 | Mild | yn > yc > y | Rising curve (supercritical) |
| S1 | Steep | y > yc > yn | Backwater curve |
| S2 | Steep | yc > y > yn | Drawdown curve |
| S3 | Steep | yc > yn > y | Rising curve |

---

## Project Structure

```
gvf-analysis/
│
├── data/
│   └── hydrau.xlsx          ← Input file (edit with your lab measurements)
│
├── GVF_clean.ipynb          ← Main notebook (run this)
│
├── requirements.txt         ← Python dependencies
├── .gitignore               ← Files Git should ignore
└── README.md                ← You are here
```

---

## Input File Format (`data/hydrau.xlsx`)

Fill in one **row per measurement section**, moving downstream:

| Column | Unit | Description |
|---|---|---|
| `distance_'x'(m)` | m | Measured distance along channel |
| `depth_'y'(m)` | m | Measured water depth at this section |
| `time:` | s | Time for the volumetric measurement |
| `Bed_slope` | — | Channel bed slope (dimensionless, e.g. 0.005) |

**Example (11 sections from lab experiment):**

| distance_'x'(m) | depth_'y'(m) | time: | Bed_slope |
|---|---|---|---|
| 0.00 | 0.017 | 12.5 | 0.005 |
| 0.02 | 0.023 | 12.5 | 0.005 |
| 0.04 | 0.031 | 12.5 | 0.005 |
| … | … | … | … |
| 0.20 | 0.060 | 12.5 | 0.005 |

---

## Getting Started

### Option A – Google Colab (no installation)

1. Go to [colab.research.google.com](https://colab.research.google.com).
2. **File → Upload notebook** → upload `GVF_clean.ipynb`.
3. Upload `hydrau.xlsx` via the file panel (folder icon → upload).
4. Update `EXCEL_PATH = "/content/hydrau.xlsx"` in the notebook.
5. **Runtime → Run all** and enter values when prompted:
   - Volume (litres)
   - Manning's roughness n
   - Channel width (m)

### Option B – Run locally

```bash
# 1. Clone the repository
git clone https://github.com/<your-username>/gvf-analysis.git
cd gvf-analysis

# 2. Create a virtual environment (recommended)
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate

# 3. Install dependencies
pip install -r requirements.txt

# 4. Launch Jupyter
jupyter notebook GVF_clean.ipynb
```

---

## Output

The notebook produces three plots (all saved as `.png`):

| File | Description |
|---|---|
| `water_surface_profile.png` | Water surface profile with yc and yn reference lines |
| `energy_profile.png` | Specific energy and depth along the channel |
| `friction_slope.png` | Friction slope Sf vs distance, compared with Sₒ |

It also prints:
- All intermediate hydraulic quantities in a tabular format
- Critical depth **yc**, normal depth **yn**, and profile classification (e.g. **M2**)

---

## Requirements

| Package | Purpose |
|---|---|
| `pandas` | Reading Excel input and tabular calculations |
| `openpyxl` | Excel engine for pandas |
| `numpy` | Numerical operations |
| `matplotlib` | Plotting |
| `scipy` | Solving Manning's equation for normal depth |

---

## Limitations

- Assumes a **rectangular channel** with constant width.
- Bed slope and Manning's n are assumed **uniform** along the channel.
- The Direct Step Method is accurate only when depth increments are small.
- Does not handle **hydraulic jumps** (rapidly varied flow).

---

## Author

*Your Name Here* – open an issue or pull request to suggest improvements.

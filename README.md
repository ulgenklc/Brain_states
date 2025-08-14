# Demo Code for Based Brain State Analysis

This repository contains the source code, a small example dataset, and a Jupyter notebook demonstrating how to perform brain state clustering and network control theory (NCT) energy calculations, as used in "Spatiotemporal asymmetries on brain energy landscape uncover system entrapment related to depression severity" manuscript.

## 1. System Requirements

**Software dependencies**:
- Python 3.9  
- `numpy`, `pandas`, `statsmodels`, `scipy`, `scikit-learn-extra`,  
  `nibabel`, `nilearn`, `nctpy`, `matplotlib`, `seaborn`  
- Optional (for notebook execution): `jupyterlab`, `ipykernel`

**Operating systems tested**:
- macOS 14.x (Sonoma)  
- Ubuntu 22.04 LTS

**Hardware**:
- Standard desktop/laptop, ≥8 GB RAM  
- CPU with ≥4 cores recommended  

---

## 2. Installation Guide

1. **Clone this repository**:
   ```bash
   git clone https://github.com/ulgenklc/Brain_states.git
   cd demo_code
   ```

2. **Create a conda environment**:
   ```bash
   conda env create -f environment.yml
   conda activate demo_code_env
   ```

3. *(Optional)* Add environment to Jupyter:
   ```bash
   python -m ipykernel install --user --name demo_code_env --display-name "Demo Code Env"
   ```

**Typical install time**: 2–4 minutes on a standard desktop.

---

## 3. Demo

**Run the demo notebook**:
```bash
jupyter lab demo_code.ipynb
```

**What the demo does**:
1. Generates random fMRI time series and random structural connectivity matrices for two clinical groups. Also, loads randomly generated clinical scores for each group. 
2. Computes pairwise distances and performs K-Medoids clustering.
3. Calculates explained variance, variance gain, mean silhouette scores, dwell times and fractional occupancies for each cluster.
4. Performs statistical significance tests for the obtained quantities between clinical groups.
5. Visualizes:
   - Violin plots comparing quantitative metrics between clinical groups
   - Regression lines between quantitative metrics and clinical scores of depression severity
   - Cluster medoid maps in anatomical brain space
   - Mapping of each cluster onto a canonically defined resting state brain network as a radar plot
   - Heatmaps of transition probabilities between pairs of states.
6. Outputs quantitative metrics such as:
   - Dwell times, fractional occupancy, transition probabilities, persistence probabilities
   - Mean silhouette score, between and within cluster variance
   - Control energy required for state transitions

**Expected runtime**:
- ~1–2 minutes on a normal desktop CPU


Figures will be displayed inline in the notebook.

---

## 4. Instructions for Use

To run the analysis on your own data:
1. Prepare time-series data in `.csv` format with each row representing a time point and each column a brain region.
2. Replace `example_data.csv` path in the notebook with your own dataset.
3. Adjust parameters (e.g., `K` for clustering) as needed.
4. Run all cells in `demo_code.ipynb`.

Outputs:
- Cluster assignments
- Dwell times and fractional occupancies and persistence probabilities per state and transition probabilities and transition energies between each pair of states
- Brain maps (if atlas paths are provided)



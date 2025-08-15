# Demo Code for Brain State Analysis

This repository contains the source code, a small example dataset, and a Jupyter notebook demonstrating how to perform brain state clustering and network control theory (NCT) energy calculations, as used in "Spatiotemporal asymmetries on brain energy landscape uncover system entrapment related to depression severity" manuscript.

## 1. System Requirements

**Software dependencies**:
- Python 3.9  
- `numpy`, `pandas`, `statsmodels`, `scipy`, `scikit-learn-extra`,  
  `nibabel`, `nilearn`, `nctpy`, `matplotlib`, `seaborn`  
- Optional (for notebook execution): `jupyterlab`, `ipykernel`

**Operating systems tested**:
- macOS 14.x (Sonoma)  

**Hardware**:
- Standard desktop/laptop, ≥8 GB RAM  
- CPU with ≥4 cores recommended  

---

## 2. Installation Guide

1. **Clone this repository**:
   ```bash
   git clone https://github.com/ulgenklc/Brain_states.git
   cd Brain_states
   ```

2. **Create a conda environment**:
   ```bash
   conda env create -f environment.yml
   conda activate test_code2
   ```

3. *(Optional)* Add environment to Jupyter Notebook kernels:
   ```bash
   python -m ipykernel install --user --name test_code2 --display-name BrainStates
   ```

**Typical install time**: 2–4 minutes on a standard desktop.

---

## 3. Demo

**Run the demo notebook**:
```bash
jupyter lab tutorial.ipynb
```

**What the demo does**:
1. Generates random fMRI time series and random structural connectivity matrices for two clinical groups. Also, loads randomly generated clinical scores for each group. 
2. Computes pairwise distances and performs K-Medoids clustering.
3. Calculates explained variance, variance gain, mean silhouette scores, dwell times and fractional occupancies for each cluster.
4. Performs statistical significance tests, FDR correction and fits regression lines for the obtained quantities between clinical groups.
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

### 0. Set your path
```python
working_path = '/Path/to/Brain_states/'
```

### 1. Prepare your fMRI time-series data

Your data should be stored in a **Python dictionary** format, although this is flexible and easily adjustable to **.csv** or something else, where:

- **Keys** = subject IDs (strings, e.g., `"500"`, `"600"`)
- **Values** = 2D NumPy arrays with:
  - **Rows** = brain regions
  - **Columns** = time points

### 2. Extracting Data Dimensions and Subject Count

```python
size, duration = dictFMRI[idx].shape
n_subjects = len(dictFMRI)    # Total number of subjects
```
### 3. Anatomical images and a parcellation
The analysis in the paper is conducted with an ultra-high field 7T MRI scanner. All the acquired images (anatomical T1w, multi-echo fMRI etc..) are stored in a [BIDS directory](https://bids.neuroimaging.io/index.html). The **BIDS_dir** folder in the repo resonates this directory in which [a template T1w anatomical image](https://surfer.nmr.mgh.harvard.edu/fswiki/CorticalParcellation_Yeo2011) is segmented and parcellated as an example using [ConnectomeMapper3](https://connectome-mapper-3.readthedocs.io/en/latest/). All the output parcellations in native space can be found in **derivatives** folder in the **BIDS_dir** folder.

If you want to run this analysis with your own data you have two options based on the output of your preprocessing:

#### 1. If your brains and whole-brain parcellations are in a standard MNI space
You can [download](https://surfer.nmr.mgh.harvard.edu/fswiki/CorticalParcellation_Yeo2011) publicly available Yeo7 parcellations and T1w template and point these paths to the correct files:
``` python
brain = nib.load(path/to/T1w anatomical image)
aparcaseg = nib.load(path/to/Whole-brain parcellation)
yeo7 = nib.load(path/to/Yeo_JNeurophysiol11_MNI152/Yeo2011_7Networks_MNI152_FreeSurferConformed1mm.nii.gz)
```
#### 2. If your brains and whole-brain parcellations are in a native space
Then you will have to point these paths to the folder with the native space files.
``` python
brain = nib.load(working_path + 'brain_templates/sub-001_ses-1_brain.nii.gz') #T1w anatomical image
aparcaseg = nib.load(working_path + 'brain_templates/sub-001_ses-1_atlas-L2018_res-scale1_dseg.nii.gz') # Whole-brain parcellation
```

Additionally, you will have to bring (register) the Yeo7 atlas into your native space using [ANTS](https://github.com/ANTsX/ANTs) or a similar tool and then map **yeo7** variable to the correct nifti file.
``` python
yeo7 = nib.load(working_path + 'brain_templates/Yeo7_Network_sub-001.nii.gz')
```

## License
 This project is covered under the GNU GENERAL PUBLIC LICENSE Version 3.


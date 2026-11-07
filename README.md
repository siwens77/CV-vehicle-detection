# CV-vehicle-detection

Computer vision course project: detecting moving vehicles (cars, trucks, trams) in Distributed Acoustic Sensing (DAS) recordings. A fiber-optic cable along Jana Pawła II Street works as a line of vibration sensors. Plotted as a time × position image, each passing vehicle shows up as a slanted line, and the slope of the line gives its speed.

Only classical methods are used (NumPy, OpenCV, Matplotlib, no neural networks): frequency filtering, per-channel normalization, thresholding and morphology. The settings of each step are chosen by testing all combinations for the best F1 on synthetic data with ground truth, checked with leave-one-out, and then applied to the real recordings.

## Notebooks

- `project.ipynb`: data loading, signal statistics and comparison of the first methods (abs, gradient, median normalization, opening) with ROC curves.
- `search.ipynb`: search over every combination of steps (frequency filter → abs / grad → channel normalization → gamma + blur → percentile threshold → opening), leave-one-out check and plots of the best chains.

## Data

The data is not in the repository (too big, provided by the course). Put it next to the notebooks:

```
20240507/            real recordings, one 10 s file per HHMMSS.npy (time × 52 channels)
Synthetic Dataset/   {id}_signal.npy, {id}_ground_truth.npy, {id}_metadata.json for ids 40-45
```

Sampling: `dt = 0.0016 s` (625 Hz), `dx = 5.1065 m`.

## Setup

```bash
python3.12 -m venv .venv
.venv/bin/pip install -r requirements.txt
```

Then open the notebooks with the `.venv` kernel. The first run of `search.ipynb` saves `search_results.csv`; delete it after changing the steps so the search runs again.

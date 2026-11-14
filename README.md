# CV-vehicle-detection

Computer vision course project: detecting moving vehicles in data from a DAS system. A fiber optic cable along Jana Pawła II street works like many vibration sensors in one line: every 5.1 m of the cable is one channel, and every channel measures the strain rate 625 times per second. A vehicle driving along the street shakes the next places of the cable one after another, so in a table of time and position it makes a slanted line, and the slope of this line is its speed.

We use the synthetic data, which has the ground truth, to build and check our methods, and then use them to detect vehicles in the real data. Only classical methods are used (NumPy, OpenCV, Matplotlib, no neural networks). The steps and results are described in `docs/report.pdf`.

## Notebooks

The notebooks are in `notebooks/`, one per iteration:

- `iteration1_methods.ipynb`: data loading, signal statistics and comparison of the first methods (abs, gradient, median normalization, opening) with ROC curves.
- `iteration2_search.ipynb`: search over every combination of steps (frequency filter → abs / grad → channel normalization → gamma + blur → percentile threshold → opening), leave-one-out check and plots of the best chains.
- `iteration3_denoising_real.ipynb`: a cleaner mask for the real data (40-312 Hz band envelope → log ratio to channel median → row median removed → Gaussian blur → hysteresis threshold), checked on the hand-made reference in `annotations/real_reference.json` and on the whole recording. At the end it saves its score images to `images/` (not in git), which `iteration4_detection.ipynb` uses, so run it first.
- `iteration4_detection.ipynb`: detection on the images from iteration 3: Hough transform + region growing find every vehicle with its speed and direction, then strength and length give its type (car, van, truck or tram).

## Data

The data is not in the repository. Put it in the repository root (the notebooks switch to it when run from `notebooks/`):

```
real-world-data/
synthetic-data/
```

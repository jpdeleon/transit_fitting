# Modeling systematics and noise

Suppose the data cover one ground-based transit and the flux falls slowly throughout the night. An airmass trend can mimic part of ingress or egress, especially if the out-of-transit coverage lies mostly on one side. Plot flux and residuals against time, airmass, sky background, image width, and centroid position to identify plausible sources.

## A baseline for each visit

A simple multiplicative model is $m_i=T(t_i)B_i$, where $T$ is the exposure-averaged transit signal and $B_i$ is the baseline for exposure $i$. Start with a constant offset for each visit. A linear extension is $B_i=c_0+c_1(X_i-\bar X)$, where $X_i$ is airmass. Fit the baseline and transit together so uncertainty in the slope contributes to the uncertainty in depth. Flares, spot crossings, scattered light, focus changes, and pointing drift have different signatures. Keep the measured covariates when testing explanations.

## Independent and time-correlated scatter

Independent flux errors enter through their per-exposure variances. An additional fitted scatter term can describe excess white noise. If nearby residuals move together, a Gaussian process (GP) can represent their covariance in time or detector position. Its amplitude controls the size of the fluctuations, and its correlation scale controls how rapidly they change. Check whether the allowed scales could absorb ingress or the whole transit. Injecting a transit into a representative baseline and refitting it measures how much signal a detrending method preserves.

GP implementations include george, celerite2, scikit-learn, and tinygp. More flexible models, including neural networks in PyTorch, TensorFlow, Flax, or Equinox, need the same transit-recovery checks.

## Relating noise to the measurement

| Source | What to look for in a light curve |
| --- | --- |
| Detector read noise | At a given gain and readout mode, each pixel read contributes approximately fixed variance in electrons squared. For equal total exposure time, many short exposures incur more reads than a few long ones. |
| Dark current | Thermal charge accumulates throughout an exposure. For dark rate $d$ electrons per second per pixel and exposure $t$, the mean and Poisson variance per pixel are both numerically $dt$ in their respective units. Subtracting a master dark removes the mean dark signal while its shot noise remains. |
| Atmosphere | Twilight, moonlight, and clouds change sky background or transparency. Atmospheric scintillation is the rapid intensity variation caused by turbulent air. Its contribution depends on airmass, telescope aperture, and exposure duration. [Osborn et al. (2015)](https://arxiv.org/abs/1506.06921) |
| Stellar and instrumental variability | Rotation and evolving spots can create smooth or quasi-periodic variability. Spacecraft pointing, scattered Earth or Moon light, and temperature-driven focus changes can introduce repeatable structure tied to spacecraft motion or image position. |

A white-noise spectrum is flat. Red noise has greater power at lower frequencies. Pink noise has power approximately proportional to $1/f$ over the frequency range of interest. Consequently, averaging $N$ adjacent residuals reduces their standard deviation by $1/\sqrt N$ for independent equal-variance measurements, while correlation can leave much more scatter on the timescale of ingress. Compare the measured scatter at that timescale with realizations from the noise model. [Carter & Winn (2009)](https://arxiv.org/abs/0909.0747)

## Choosing detrending inputs

- **Spitzer and K2 pixel-level decorrelation (PLD).** Normalized pixel intensities supply regressors for position-dependent sensitivity. Deming et al. applied PLD to Spitzer. EVEREST adapts the technique to K2's roll motion. Compare the recovered transit depth across reductions. [Deming et al. (2015)](https://arxiv.org/abs/1411.7404) · [Luger et al. (2018)](https://arxiv.org/abs/1702.05488)
- **Position and ensemble trends.** K2SFF estimates a flux correction from position along the roll path. The trend-filtering algorithm (TFA) of Kovács et al. (2005) and the eigen light curves of Foreman-Mackey et al. (2015) use trends inferred from ensembles of stars. Kepler PDC uses cotrending basis vectors (Smith et al. 2012 and Stumpe et al. 2012, 2014). Compare simultaneous transit fitting with a reduction trained on out-of-transit cadences. [K2 ensemble analysis](https://arxiv.org/abs/1502.04715) · [Kepler data handbook](https://archive.stsci.edu/kepler/manuals/Data_Characteristics.pdf)
- **Other pixels and covariance models.** Causal pixel models use pixels away from the target to predict shared instrumental signals (Wang et al. 2016 and Poleski et al. 2019). The unpopular package applies this approach to TESS. For a probabilistic time-series model, gallifrey uses JAX and sequential Monte Carlo to explore Gaussian-process kernel structures. [Hattori et al. (2022)](https://arxiv.org/abs/2106.15063) · [gallifrey](https://arxiv.org/abs/2505.20394)

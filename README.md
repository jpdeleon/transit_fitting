# Modeling Exoplanet Transit Light Curves

A transit light curve records how much starlight a planet blocks as it crosses its host star. Its depth, shape, and timing carry different information. This book follows a transit observation from the first flux measurement to an estimate of the planet-to-star radius ratio and a prediction for the next transit.

## Part I. From observations to a light curve

1. [Reading a transit light curve](chapters/reading-a-transit.md) — Identify the baseline, ingress, egress, depth, and duration. Decide which measurements can constrain a radius ratio, orbital geometry, or ephemeris.

   Read a transit chord from first to fourth contact. Connect spot crossings and gravity darkening to stellar spin, wavelength-dependent depths to atmospheric scale height and composition, timing variations to companion masses, and eclipses and phase curves to atmospheric circulation.
2. [Choosing and preparing photometry](chapters/photometry.md) — Select surveys, epochs, and passbands. Follow ground-based images through calibration and differential photometry, or choose a TESS light-curve product.

   Compare Kepler quarters (~90 days), K2 campaigns (~80 days), and TESS sectors (~27 days), their target catalogs, discovery samples, and ground-based survey sites. Work through CCD calibration, aperture and PSF photometry, absolute and apparent magnitudes, colors, and flux uncertainties. Compare SAP and PDCSAP with QLP, CDIPS, PATHOS, TGLC, and causal pixel model reductions at the cadence available for each sector.

## Part II. Building the model

3. [Modeling the transit signal](chapters/transit-signal.md) — Build a box model, add ingress and limb darkening, and account for exposure time and contaminating light.

   Compare trapezoidal, Mandel–Agol, series-expansion, numerical, and spherical-harmonic models in batman, PyTransit, ellc, starry, and jaxoplanet. Convert between contaminant-to-target flux ratio and contaminant fraction before correcting a depth.
4. [Orbital and transit geometry](chapters/transit-geometry.md) — Solve Kepler's equation for the mean, eccentric, and true anomalies, orient the orbit and stellar spin with i, ω, Ω, λ, i★, and ψ, then watch impact parameter, limb darkening, eccentricity, and dilution reshape the light curve.
5. [Choosing parameters and priors](chapters/parameters-and-priors.md) — Choose independent orbital parameters, allow for grazing transits, and incorporate measurements of the host star.

   Compare radius and area ratios, impact parameter and inclination, duration and circular density, and quadratic limb-darkening coordinates. Recover eccentricity and periastron angle from their sampled components. Examine grazing and photoeccentric degeneracies, Jeffreys priors, and the stellar-density constraint in allesfitter’s `params.csv` workflow.
6. [Modeling systematics and noise](chapters/systematics.md) — Use observing conditions and detector measurements to model the baseline. Describe the remaining scatter with independent errors or a time-correlated process.

   Separate read noise, dark current, atmospheric scintillation, and correlated stellar variability. Use airmass and centroid trends, compare white, pink, and red noise spectra, and choose among pixel-level decorrelation, self-flat-fielding, ensemble trends, and Gaussian processes.

## Part III. Fitting and interpreting observations

7. [Fitting the light curve](chapters/fitting.md) — Write the likelihood, inspect prior predictions, optimize a fit, and estimate parameter uncertainties with MCMC or nested sampling.

   Select the phase window, inspect flagged data, choose free parameters and external measurements, then check prior draws against the observed cadence. Compare SciPy and JAX optimization, emcee, nested sampling, HMC, NUTS, and variational inference. Locate DiffEvol, JAXopt, Optax, and the integrated fitting frameworks in this workflow.
8. [Checking and reporting a fit](chapters/diagnostics.md) — Diagnose sampling and residuals, compare prior and posterior predictions, and report fitted and derived quantities.

   Use trace and corner plots, prior–posterior overlays, binned residual scatter, and posterior predictive light curves. Compare a fit with published solutions in matching coordinates and propagate uncertainties to derived parameters.
9. [Combining epochs and passbands](chapters/joint-fits.md) — Fit TESS and ground-based observations together to refine an ephemeris, test for wavelength-dependent depths, or measure transit timing variations.

   Use short exposures to resolve ingress and widely separated epochs to constrain period. Fit per-filter radius ratios, propagate shape uncertainty into partial-transit times, and evaluate observed-minus-calculated timing residuals against a linear ephemeris.
10. [The modeling ecosystem](chapters/ecosystem.md) — Map array backends, transit models, optimizers, samplers, GP and neural-network libraries, and integrated frameworks, and see which combinations support gradient-based inference.

## Build the book locally

From the repository root, run the following commands in a Python environment.

```sh
python -m pip install "jupyter-book>=1,<2"
jupyter-book build .
```

Open `_build/html/index.html` to read the book.

## Publish to GitHub Pages

In the repository's **Settings → Pages → Build and deployment**, select **GitHub Actions** as the source. Each push to `main` runs **Publish Jupyter Book**, builds the HTML, and deploys it. To republish the current commit, select **Actions → Publish Jupyter Book → Run workflow**.

Read the [published book](https://jpdeleon.github.io/transit_fitting/).

# Checking and reporting a fit

A small residual scatter answers only one question about the fit. The sampler may still have missed a grazing solution, and a baseline error may have shifted the inferred depth. Check both the parameter exploration and the flux predictions.

## Check the inference

Inspect chain traces across independent runs. For appropriate MCMC runs, examine effective sample sizes and convergence diagnostics such as rank-normalized split R-hat. For HMC, investigate divergent transitions. For nested sampling, use the posterior weights and check stability across runs or live-point counts. A skewed or multimodal posterior can be a genuine consequence of the geometry.

Overlay prior and posterior distributions for radius ratio, impact parameter, and limb darkening. A corner plot shows whether a broad marginal radius uncertainty follows a narrow relationship with impact parameter. Repeat a fit with a scientifically plausible alternative prior to see which conclusions depend on the external information.

## Check the light curve

Plot observations and posterior model draws against time. Below them plot observed minus predicted flux, keeping ingress, egress, and the baseline visible. Look for residual runs of one sign, outliers concentrated in one visit, correlations with airmass, and changes in scatter with time bin size. Compare these patterns with replicated data drawn from the fitted noise model.

Compare the transit-inferred stellar density with an independent estimate, recording the eccentricity assumption and whether that estimate was already used in the fit. Agreement with a measurement used as a constraint has less diagnostic value than agreement with a held-out measurement.

## Report a reproducible result

Give the target, data product, exposure time, filter, timing standard, exclusions, transit model, noise model, and priors. Report a stated central estimate and credible interval, including units and interval probability. Distinguish fitted radius ratio from the planet radius derived using a stellar measurement. For an ephemeris, report the reference epoch, period, and their covariance or joint samples so readers can propagate uncertainty to another epoch.

## Comparing with a published solution

Bring the two fits into common coordinates first. Is the reported depth an area ratio, a diluted flux decrement, or a limb-darkened model depth? Are the radii from the same passband? Propagate both ephemerides to the same epoch using their period–midtime covariance. For independent approximately Gaussian estimates of the same quantity, the difference has uncertainty $\sqrt{\sigma_1^2+\sigma_2^2}$. Fits that reuse the same TESS transits or stellar prior can be correlated. Report these shared inputs when interpreting agreement.

For an independent Gaussian noise model, inspect residuals divided by their modeled standard deviations. Check their histogram for heavy tails and their autocorrelation for structure. For a GP with covariance $K=LL^{\mathsf T}$, compare whitened residuals $L^{-1}(f-m)$ with noise realizations treated in the same way. If using residual-test output from allesfitter, give the test and its assumption alongside the residual plot. An innocuous histogram can coexist with a run of positive residuals through egress that shifts the inferred midtime.

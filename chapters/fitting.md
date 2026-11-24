# Fitting the light curve

For observed fluxes $f_i$, predicted fluxes $m_i(\theta)$, and independent Gaussian errors with variances $v_i$, the log likelihood is

$$
\log L(\theta)=-\frac12\sum_i\left[\frac{(f_i-m_i(\theta))^2}{v_i}+\log(2\pi v_i)\right].
$$

With quoted flux uncertainties $\sigma_i$ and extra scatter $s$, set $v_i=\sigma_i^2+s^2$. Keep the normalization term when fitting $s$. For correlated residuals, use the covariance matrix of the noise model. Bayes' rule then gives $\log p(\theta\mid f)=\log L(\theta)+\log p(\theta)+\mathrm{constant}$.

## An initial depth estimate

Consider a box with known transit times and a known unit baseline. Three independent in-transit measurements are 0.989, 0.991, and 0.990, each with uncertainty 0.003. The maximum-likelihood depth is one minus their mean, or 0.010. Its standard error is $0.003/\sqrt{3}\approx0.0017$. Fitting the baseline from a short stretch of data introduces another source of uncertainty. A joint fit carries that uncertainty into the inferred depth.

## From prior predictions to posterior samples

1. Draw parameters from the prior and simulate flux at the actual observation times. Inspect the implied depths, durations, and baselines. This prior predictive check can reveal a time-unit error or implausible geometry before sampling.
2. Locate plausible solutions. Nelder–Mead and Powell use function evaluations, differential evolution searches a bounded region, and BFGS uses gradients. Try several initial points when distinct geometries fit the same event.
3. Explore parameter uncertainty. *Monte Carlo* means estimating posterior means, intervals, and other integrals by averaging over random draws. In a Markov chain each draw depends only on the previous one, and after convergence the chain visits regions in proportion to their posterior probability. Ensemble MCMC with emcee can expose the radius–impact parameter degeneracy. HMC and NUTS use gradients of the log density. Nested sampling with dynesty, MultiNest, or UltraNest produces weighted posterior samples and an estimate of the evidence for the specified priors.
4. Compute derived quantities at each joint posterior draw. This preserves correlations when propagating uncertainties to absolute radius, duration, or a future midtime.

## Taking one data set through a fit

Suppose the goal is a precise period from TESS and a later ground-based transit.

1. **Select the signal and time window.** Keep the full orbit for a phase curve or secondary eclipse. For primary-transit timing, keep each transit plus enough data on either side to constrain its baseline. A joint radial-velocity fit adds a velocity model and instrument offsets while sharing the orbital elements.
2. **Inspect the measurements.** Compare SAP and PDCSAP around the same events. Check time standards, exposure durations, flags, and gaps. Retain the original flux uncertainties and document each outlier exclusion. Group observations by visit and passband.
3. **Specify the forward model.** Share $P$ and $T_0$ for a linear ephemeris. Fit dilution or constrain it with independent imaging, choose passband-specific limb darkening, and assign each visit a constant, covariate trend, or GP baseline. List fixed values alongside fitted coordinates. Fixing a parameter the data cannot constrain reduces the dimension of the problem, but its uncertainty then drops out of every derived quantity. Free a parameter, with an informative prior if needed, whenever its uncertainty matters for the result.
4. **Predict, optimize, and sample.** Inspect simulated data from the priors, then optimize the chosen likelihood or posterior and examine the transit overlaid on the observations. Evaluate products of many small probabilities in log space to avoid numerical underflow. In MCMC the draws are correlated, so use effective sample size when assessing Monte Carlo precision.
5. **Diagnose and revisit assumptions.** Compare posterior predictions and residuals for each visit. For a plausible alternative baseline or stellar prior, repeat the fit and record how the answer changes. A changed prior defines a different analysis and should appear in the report.

## Finding an implementation

| Task | Examples and distinctions |
| --- | --- |
| Optimize an objective | SciPy provides Nelder–Mead, Powell, BFGS, and differential evolution. PyDE's `DiffEvol` is another differential-evolution implementation. For differentiable objectives, `jax.scipy.optimize.minimize`, JAXopt, and Optax provide optimization tools. JAX can differentiate through a compatible transit model and likelihood. [PyDE](https://github.com/hpparvi/PyDE) · [JAXopt](https://jaxopt.github.io/dev/index.html) · [Optax](https://optax.readthedocs.io/) |
| Draw posterior samples or approximate a posterior | emcee uses ensemble MCMC. dynesty, MultiNest, and UltraNest perform nested sampling. PyMC supports HMC/NUTS and variational approximations through PyTensor (older PyMC releases used Aesara). NumPyro and BlackJAX implement gradient-based samplers in JAX. Variational inference optimizes an approximating distribution, so check the sensitivity of reported uncertainties to its shape. |
| Assemble the full fit | allesfitter and juliet combine physical light-curve models, noise models, and inference. pyaneti and Exo-Striker support joint orbital analyses, and exoplanet supplies components for PyMC models. In any implementation, specify which parameters the photometry and radial velocities share. See [the ecosystem page](ecosystem.md) for a fuller map. [allesfitter](https://github.com/MNGuenther/allesfitter) · [Günther & Daylan (2021)](https://arxiv.org/abs/2003.14371) |

For a broader introduction to Bayesian inference in exoplanet work, see [Parviainen (2018)](https://arxiv.org/abs/1711.03329).

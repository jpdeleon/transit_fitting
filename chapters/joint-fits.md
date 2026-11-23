# Combining epochs and passbands

Suppose TESS observed several transits and LCO obtained a later transit through a different filter. Assign every point a visit and passband. A joint model can share the orbital period, reference midtime, and geometry, while giving each visit its own baseline and noise terms. Use limb darkening appropriate to each filter and dilution appropriate to each aperture.

## Refine an ephemeris

For integer epoch $E$, the linear prediction is $T(E)=T_0+EP$. A later transit can substantially improve the period because a small period error accumulates every orbit. For example, if epochs 0 and 100 fall 300 days apart and each midtime has an independent uncertainty of one minute, the two-point period estimate is 3 days with uncertainty $\sqrt{2}/100$ minutes, about 0.85 seconds. This calculation assumes a known cycle count and a constant period. Short exposures that resolve ingress help measure each midtime, while observations spread over many orbits extend the timing baseline.

## Test for chromatic depths

Fit a common radius ratio first, then allow a separate $k$ in each filter. At each joint draw, compute the difference between two radius ratios and summarize the resulting interval. Shared orbital uncertainties propagate into that difference. Examine changes under alternative limb-darkening and dilution assumptions. A wavelength-dependent signal may come from the atmosphere, unocculted stellar spots, or an unresolved neighbor of a different color.

## Measure transit timing variations

Allow each event its own midtime while sharing shape parameters. Propagate uncertainty in geometry and systematics, particularly when only ingress or egress was observed. Subtract the predicted midtime from each measured midtime and plot these observed-minus-calculated (O−C) values against epoch with uncertainties. Compare the timing series with predictions from a linear ephemeris that account for uncertainty in its fitted period and reference epoch. Individual one-sigma excursions occur frequently even under the linear model. A claim of timing variations requires evaluating the series as a whole.

## Interpreting one-sigma differences

**Chromaticity.** For blue and red passbands, calculate $\Delta k=k_{\rm blue}-k_{\rm red}$ at each joint posterior draw. For example, a central 68% interval of $[-0.001,0.003]$ includes zero but also allows a blue radius ratio larger by 0.002. Report the interval and, if the science requires agreement to within a chosen tolerance $\epsilon$, the posterior fraction with $|\Delta k|<\epsilon$. Comparing the error bars on the separate radii loses the covariance induced by their shared geometry.

**Transit timing.** In a two-stage fit, holding transit shape and baseline fixed while freeing only the midtimes gives uncertainties conditional on those fixed values. A partial transit makes the trade-off apparent: a shift in midtime can move a sloping ingress much like a change in baseline. Fit the events jointly or propagate the shared-parameter posterior through the timing step. An independent Gaussian measurement has a 31.7% chance of landing more than one standard deviation from its true mean. Even a constant-period planet can therefore produce roughly three such points in ten measurements. Fitting the ephemeris introduces additional covariance. Compare the observed timing pattern with simulated observations at the same epochs and errors, refitting the linear ephemeris in each simulation. A coherent pattern across epochs can then motivate a dynamical fit. [Holman & Murray (2005)](https://exoplanets.cfa.harvard.edu/publications/use-transit-timing-detect-terrestrial-mass-extrasolar-planets)

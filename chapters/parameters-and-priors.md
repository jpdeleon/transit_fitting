# Choosing parameters and priors

Begin with a circular orbit. One choice of free parameters is the midtime $T_0$, period $P$, radius ratio $k=R_p/R_\star$, scaled semimajor axis $a/R_\star$, impact parameter $b$, and limb-darkening coefficients. For a circular orbit, $b=(a/R_\star)\cos i$, so the inclination $i$ is already determined. Sampling all three geometric quantities independently would allow inconsistent orbits.

## Choose coordinates for the measurement

| Choice | Consequence for the fit |
| --- | --- |
| $k$ or $k^2$ | A flat prior on area ratio gives more weight to larger radii than a flat prior on radius ratio. |
| $a/R_\star$, stellar density, or duration | Each constrains the transit timescale. Converting between them requires assumptions about the orbit. |
| $b$ or $\cos i$ | Allow $0\leq b<1+k$ for a transit, including grazing configurations. |
| $\sqrt e\cos\omega$, $\sqrt e\sin\omega$ | Their squared sum is eccentricity $e$. For a bound orbit sample inside the unit disk. |
| $u_1,u_2$ or $q_1,q_2$ | Kipping's triangular coordinates map the unit square to physically admissible quadratic limb-darkening coefficients. |

A uniform prior on the logarithm of a positive quantity corresponds to a log-uniform prior on that quantity. Give finite bounds when using it as a proper prior. For grazing geometries, depth and duration can admit a wide range of radius ratios. Inspect the joint prior on $b$ and $k$ as well as their separate bounds. For further treatments see Espinoza (2018), Gilbert et al. (2022), and [Kipping (2013)](https://arxiv.org/abs/1308.0009).

## Include external stellar information

For a circular orbit and negligible planet mass, Kepler's third law gives $\rho_\star=3\pi(a/R_\star)^3/(GP^2)$, where $G$ is the gravitational constant. If $P$ and $a/R_\star$ are sampled, compute density for each draw. An independent density measurement with Gaussian error contributes a term proportional to $-(\rho_{\rm model}-\rho_{\rm measured})^2/(2\sigma_\rho^2)$ to the log posterior. This constrains several transit parameters together. If the orbit is eccentric, its speed at transit also affects the inferred geometry.

Stellar effective temperature, surface gravity, metallicity, and passband can inform limb darkening through atmosphere tables such as Claret's, or tools such as LDTk and ExoCTK. Carry uncertainty in these inputs into the fit. Record whether each quantity was sampled, derived, fixed, or supplied as an independent measurement so the same evidence enters only once.

## Recovering eccentricity from a transit duration

Let $f_s=\sqrt e\sin\omega$ and $f_c=\sqrt e\cos\omega$. The inverse transformation is $e=f_s^2+f_c^2$ and $\omega=\operatorname{atan2}(f_s,f_c)$, with $\omega$ undefined at $e=0$. For example, $(f_s,f_c)=(0.3,-0.4)$ gives $e=0.25$ and $\omega\simeq143$ degrees. Use the argument of periastron of the **planet** in the following expression.

For a nearly edge-on orbit and a transit short compared with the period, the speed across the chord relative to the circular prediction is approximately $g=(1+e\sin\omega)/\sqrt{1-e^2}$. Fitting an eccentric transit under a circular assumption can give a stellar *pseudo-density* $\rho_{\rm circ}\simeq g^3\rho_\star$. A short transit across a known chord can indicate a planet moving quickly near periastron. An independent stellar-density measurement limits the allowable combinations of $e$ and $\omega$, but a grazing chord can also shorten the event. This is the photoeccentric effect. [Dawson & Johnson (2012)](https://arxiv.org/abs/1203.5537) · [Kipping (2010)](https://arxiv.org/abs/1004.3819) · [MacDougall et al. (2023)](https://arxiv.org/abs/2307.07070) · [Eastman (2024)](https://arxiv.org/abs/2309.14410)

## How a prior changes with coordinates

A Gaussian prior can express an external measurement with a standard deviation. A uniform prior gives equal probability to equal intervals within stated bounds. Either can be informative. A Jeffreys prior depends on the likelihood's Fisher information. For a scale parameter it commonly takes the form $p(x)\propto1/x$, which is uniform in $\log x$. For example, a bounded log-uniform prior on period gives equal mass to 1–10 days and 10–100 days. Merely computing in $\log P$ does not select a prior until the sampling density is specified.

For a transit with poorly resolved ingress, a flat prior on circular pseudo-density can favor low impact parameters and shift the inferred radius. Alternatives studied by Gilbert et al. include uniform priors on total duration $T_{14}$, log-uniform priors on $\rho_{\rm circ}$, and log-uniform priors on $a/R_\star$. At fixed $P$, the latter two are equivalent for matched bounds because $\rho_{\rm circ}\propto(a/R_\star)^3$. A flat duration prior induces a different distribution. Examine their prior draws in $(b,k,T_{14})$ and keep an independent stellar-density measurement distinct from these coordinate choices. [Gilbert, MacDougall & Petigura (2022)](https://arxiv.org/abs/2206.03432)

For fixed $k<1$, a conditional uniform draw $b\sim U(0,1+k)$ includes both full and grazing transits. Near $1-k<b<1+k$, the same depth can describe a larger planet crossing less of the disk. Joint sampling of $b$ and $k$, or separately exploring grazing and non-grazing regions with umbrella sampling and recombining them with their weights, helps explore that boundary. Selection for a detectable transit also changes the population distribution of impact parameters. [Espinoza (2018)](https://arxiv.org/abs/1811.04859) · [Gilbert (2022)](https://arxiv.org/abs/2201.08350) · [Kipping & Sandford (2016)](https://arxiv.org/abs/1603.05662)

For quadratic limb darkening, $q_1=(u_1+u_2)^2$ and $q_2=u_1/[2(u_1+u_2)]$ where the denominator is nonzero. Sample the unit square and transform back with $u_1=2\sqrt{q_1}q_2$ and $u_2=\sqrt{q_1}(1-2q_2)$. Compare a fit using these geometric bounds with one informed by atmosphere models in the actual passband. [Kipping (2013)](https://arxiv.org/abs/1308.0009) · [Espinoza & Jordán (2015)](https://arxiv.org/abs/1503.07020)

## Worked example with an external constraint in allesfitter

1. In `params.csv`, mark independent coordinates for fitting and specify their prior distributions and bounds. Each MCMC walker or nested-sampling live point carries values for those coordinates in the order stored in `fitkeys`. For one planet, `b_rr` denotes $R_p/R_\star$ and `b_rsuma` denotes $(R_\star+R_p)/a$. Thus $a/R_\star=(1+\mathtt{b\_rr})/\mathtt{b\_rsuma}$. [Parameter file](https://www.allesfitter.com/setup/parameters)
2. With `use_host_density_prior` enabled and stellar measurements supplied in `stellar_params.csv`, the code evaluates the host density implied by each draw. In the small-companion limit it follows from $P$ and $a/R_\star$. In a joint transit and radial-velocity solution, the companion-mass correction can also involve the velocity semi-amplitude $K$, inclination, and eccentricity. [Settings](https://www.allesfitter.com/setup/settings) · [Density calculation](https://github.com/MNGuenther/allesfitter/blob/master/allesfitter/computer.py)
3. The quantity `host_density` is derived. The function `calculate_external_priors` evaluates the density constraint and its log contribution is added to `lnlike_total`. For a 1.50 ± 0.10 g/cm³ independent Gaussian measurement, a draw predicting 1.30 g/cm³ receives a log penalty of $-2$ relative to a draw predicting 1.50 g/cm³. Each orbital parameter inherits the constraint through the density calculation and its correlations with the other parameters. [Likelihood implementation](https://github.com/MNGuenther/allesfitter/blob/master/allesfitter/computer.py)

Outline
Transit models
Box
Mandel-Agol
BATMAN package
Pytransit package
Starry package
Transit model parameterization
Systematics model
Bayesian statistics: Likelihood, Prior, Posterior
Model optimization 
Parameter estimation/inference
MCMC
Nested Sampling
Tests
Convergence tests
Diagnostic tests on residuals 
Reporting results
Summary statistics
Visualization of prior and posteriors
Other plots: Corner plot
Notes
Autodiff with jax
Limb darkening
Stellar parameters


Transit surveys
Space-based
TESS
CoRoT (Baglin et al. 2006; (Auvergne et al. 2009), 
Kepler (Borucki et al. 2010a) and 
K2 (Howell et al. 2014)
190,000 stars for Kepler , >200,000 stars for K2, >500,000 stars for TESS
Ground-based
wide-field photometric surveys operating from the ground, such as
WASP (Pollacco et al. 2006), 
HAT (Bakos et al. 2004; Bakos et al. 2013), 
KELT (Siverd et al. 2012), 
Qatar (Alsubai et al. 2013), 
XO (McCullough et al. 2005; Crouzet 2017) and 
TrES (Alonso et al. 2004)

Space-based
Kepler (2009-2013)
FOV:  1 patch of sky in constellar Cygnus continuously for 4 years
1 quarter ~ 91 d (4Q=1y)
All target stars have Kepler Input Catalog number (e.g. KIC 12345678)
Discovered thousands of planets (e.g. Kepler-51b, c)
Planet candidates (not verified as real planets) are named Kepler Object of Interest (KOI) 
Data reduced by SPOC & QLP pipeline
K2 (2014-2019)
FOV: 18 patches of sky called “campaigns” along the ecliptic for 5 years
1 campaign ~ 80d; a few overlapping campaigns
Discovered hundreds of planets (e.g. K2-100b)
Data reduced by EVEREST & K2SFF pipeline
TESS (2018-now)
FOV: 90 large patches of sky called “sectors” over 6 years now; almost all-sky except the Galaxy midplane
All target stars have TESS Input Catalog number (e.g. TIC 12345678)
1 sector ~ 26 d; many overlapping sectors
Discovered thousands of planets (e.g. TOI-200b)
Planet candidates are named TESS Object of Interest (TOI)
Data reduced by SPOC & QLP pipeline
CHEOPS

Ground-based
HATNet (Hungarian Automated Telescope Network)
network of small, wide-field telescopes located in Arizona, USA, and Hawaii.
Discovered over 80 exoplanets
KELT (Kilodegree Extremely Little Telescope)
KELT-North in Arizona, USA, and KELT-South in South Africa
Discovered over 20 exoplanets
WASP (Wide Angle Search for Planets) & Super WASP 
consists of two robotic observatories (WASP-North in La Palma, Spain, and WASP-South in South Africa).
discovered over 150 confirmed exoplanets
NGTS (Next-Generation Transit Survey)
array of 12 robotic telescopes located in Paranal Observatory in Chile


MEarth 
MEarth-North at the Fred Lawrence Whipple Observatory in Arizona, and MEarth-South at the Cerro Tololo Inter-American Observatory in Chile
Discovered a few small planets around M dwarfs
TRAPPIST (TRAnsiting Planets and PlanetesImals Small Telescope)
Operated from La Silla Observatory in Chile and Oukaïmeden Observatory in Morocco.
Discovered the TRAPPIST-1 system with seven Earth-sized planets.
QES (Qatar Exoplanet Survey)
Located at the New Mexico Skies Observatory in the USA.
Discovered several hot Jupiters and other exoplanets.
GAPS (Global Architecture of Planetary Systems)
A collaboration of Italian astronomers using telescopes like the Telescopio Nazionale Galileo (TNG) in La Palma.


Pre-requisites (background knowledge)

Measurement & Error in terms of sigma
Model fitted against data; Fitted vs. Derived parameters   
Time-series photometry
Observation
Ephemeris, equatorial, overhead, CCD, saturation, linearity, bad/hot pixels, S/N
Constraints: twilight, target altitude, Moon shine
Calibration
Raw vs. Calibrated frames
dark & flat & sky frames
Reduction
aperture photometry vs. PSF photometry
absolute photometry
Absolute vs apparent magnitude, zero-point
relative photometry
flux, aperture, relative flux vs. normalized flux, comparison stars
Magnitude system
Bandpass: Johnson UBV & SDSS-griz filters (optical)
JHKs (near-infrared)
Color= difference in magnitudes e.g. G_BP-G_RP
Limb darkening
Baseline/ out-of-transit vs. in-transit
Transit: ingress, egress, transit duration, impact parameter, grazing orbit, transit shape 
Systematics
Covariates: airmass (lowest at zenith), sky background (due to Moon or twilight), centroid offset, peak flux
White/random & red/ correlated & pink/ quasi-random noise
Statistical Distributions
Monte Carlo in MCMC
Propagation of uncertainties 
Likelihood
Free vs Fixed parameter
Assumption: Residuals (=Data-Model) is a Normal distribution 
Prior
Uniform/Flat (UP), Normal/Gaussian (NP), Jeffreys prior; bounds e.g. (0,1]
Informative (UP) vs non-informative (e.g. NP)
Posterior= prior*likelihood
Use of natural log to avoid overflow → ln Po = ln Pr + ln L 

Modeling frameworks
 Atomic code
Numpy package
 
Jax package
Transit model
PyTransit, batman, ellc

(numerical/approximate)
Starry


(analytic/exact + gradient)
Jaxoplanet 

(automatic differentiation)
Optimization


Nelder-Mead/Powell algo using Scipy; Differential Evolution algo using DiffEvol
BFGS algo using scipy.optimize 
jax.scipy.optimize, Jaxopt 
Sampling
MCMC using emcee; NS using dynesty, multinest, ultranest package
HMC, NUTS, Variational Inference using Pymc  (uses pytensor, aesara)
Numpyro, blackjax, optax
Systematics model using GP;
using Neural networks
Scikit-learn, george, celerite2


Scikit-learn, TensorFlow, pytorch
tinygp, gallifrey

Flax, equinox 
Wrapper (transit+GP+sampling)
allesfitter, juliet, pyaneti, exostriker
exoplanet 
 

Types of transit data
Single event, single band/filter
CHEOPS, Hubble, Spitzer, LCO
Multi-epoch, single band
Kepler, TESS
Single epoch, multi-band (simultaneous)
MuSCAT, JWST
Multi-epoch, multi-band
DESI?
Note that multi-epoch can be
Continuous e.g. space-based
Gapped e.g. ground-based

What can we learn from a transit observation?
The precise measurement of the transits of an exoplanet offers a host of information about the planet’s properties (Charbonneau et al. 2007; Winn 2008, 2010; Haswell 2010). 
times of transit → planet’s orbital ephemeris
depth of transit →  planet’s radius relative to that of the star (Heller 2019)
shape of the transit, especially the duration of ingress and egress relative to the full transit duration → the orbital impact parameter of the planet → inclination of the orbit relative to the observer (Seager & Mallen-Ornelas 2003). 


Spin-Orbit Misalignment (λ) – via Transit Duration Variations or Spot-crossing Events
Gravity Darkening → Stellar Oblateness and Spin Rate
If the transit depth is seen to vary with wavelength, the presence of spectral features may be used to constrain the chemical composition of the planet’s atmosphere and scale height (Brown 2001; Seager & Deming 2010; Burrows 2014; Crossfield 2015; Madhusudhan 2019)
If the transit times are seen to vary, a dynamical model can constrain the masses of the planet companions, and vice versa (Agol et al. 2005; Holman 2005) 
If the planet is seen in eclipse, its temperature, emission spectrum, and atmospheric circulation pattern can be constrained (Cowan & Fujii 2017; Alonso 2018) 
When combined with radial velocity measurements, the bulk density of a planet can be inferred, yielding constraints on its bulk composition (Udry et al. 2007).

Goals
Measure precise ephemeris & transit duration
Longer baseline is better; short cadence (exp=120s) is better
Measure robust Rp/Rs
TESS has dilution often so follow-up data e.g. LCO with uncontaminated aperture is best
TESS+LCO joint fit (in different filters)  
Measure chromaticity
Allow Rp/Rs to vary in each filter/band
Rp/Rs within 1-sigma is desired i.e. achromatic
Measure TTVs
Use all lightcurves (partial or full transit) observed in different epochs
Fix all best-fit transit+systematics model parameters and free transit midtimes
TTV is significant if N points > 1 sigma (~33%) off the O-C=0 line (linear ephemeris model)

Modeling


Signal: Transit or RV or both
Variability / Systematics
Non-random, deterministic variations 
caused by known (astro-)physical processes 
e.g. stellar activity, flare, atmospheric scintillation effects for ground-based observatories, light from blended objects, or scattered light from the Earth and the moon for space-based observatories, clouds, telescope pointing drift, focus change due to temperature/pressure, etc.
Noise
random (stochastic) variations 
Types
Red noise: Time-correlated noise where low frequencies dominate
e.g. stellar activity (if its property e.g. rotation is not of interest), anything above not explained by covariates
White noise: Completely uncorrelated noise across all frequencies
e.g. Detector read noise (independent on exp time), dark current (dependent on exp time)

Procedure
Define the goal: e.g. Measure precise period
Define data to use (what are available?): e.g. TESS
Full phase (including secondary eclipse?) or around phase=0 + few hours of baseline (primary transit only)
Which TESS pipeline/ exp. time?
Inspect data
Remove outliers if necessary
Choose pipeline e.g. PDCSAP vs SAP
Format data for fitting
Define free and fixed parameters
Free: only sample something the model doesn't already determine from other parameters
Fix: for simplifying/ reducing model complexity
Choose systematics model
Constant offset, Linear, GP?
Define priors
Uniform or Normal?
Use external priors? e.g. stellar density
Check initial fit (aka prior predictive check)
Need to adjust priors?
Check fit results
Well-behaved posteriors? trace and corner plots look Gaussian?
Adjust priors then re-fit if needed

Sampled parameter vs external prior
Directly sampled parameter
It's a dimension of θ (in fitkeys). The sampler carries a coordinate for it; every walker/live-point explores it
Its prior is the params.csv bounds — a genuine marginal prior π(θ_i) on that free coordinate
The likelihood (data) updates it → you get a posterior in that exact coordinate
Requirement: it must be an independent degree of freedom.
External prior
The quantity is not in θ. host_density is recomputed deterministically from sampled params (period, rr, K, incl, …) each step
The "prior" term log N(mu(θ), σ) is added into the total log-prob via the likelihood channel (calculate_external_priors → lnlike_total += lnprior_external). So functionally it behaves like an extra measurement/datum, not a  marginal prior on a free parameter
Its effect propagates backward onto the parameters that feed it — it tightens the joint posterior of period, a/R★ (rsuma/rr), K, incl, breaking degeneracies (the e–ω / a/R★ degeneracy, Winn 2010)
e.g. prior on ρ constrains the P–a/R★–K–incl combination jointly

Consistency checks
Best-fit parameters consistent with literature?
Stellar density derived from transit consistent with bulk stellar density?
Residuals (=data-model) looks flat? (see also stat. test results from allesfitter)
What is the effect of using priors?
Stellar parameters/ Limb darkening coefficients (exoctk)
Density

TESS data
SPOC (exp<=120s, all sectors available)
TESS Science Processing Operations Center
Flux types: 
PDCSAP: corrected for systematics such as CCD effects (e.g., bias, dark current) and cosmic rays (Jenkins et al. 2016)
SAP
QLP (exp>=600s, all sectors available)
CDIPS (exp>=600s, few sectors available)
PATHOS (exp>=600s, few sectors available)
TGLC (any exp, few sectors available)
CPM (any exp)
Hattori+2022

Detrending
methods to remove systematics from light curves
Spitzer
“pixel-level decorrelation” (PLD) Deming et al. 2015
trend-filtering algorithm (TFA) developed by Kovács et al. (2005)
(Charbonneau et al. 2005; Knutson et al. 2008; Ballard et al. 2010; Stevenson et al. 2012)
K2
“Self-flat-fielding” (SFF): Vanderburg & Johnson (2014)
Presearch Data Conditioning (PDC) module (Smith et al. 2012; Stumpe et al. 2012, 2014), 
“eigen light curve” approach taken by Foreman-Mackey et al. (2015), 
EVEREST pipeline (Luger et al. 2016, 2018), 
Causal Pixel Model (CPM)–based pipelines (Wang et al. 2016, 2017; Poleski et al. 2019)
TESS
Unpopular package based on CPM (Hattori et al. 2022)
TESS-Gaia LightCurve (TGLC)

Transit modeling frameworks
Simple boxy/ trapezoidal model
Analytical (closed-form equations)
Mandel & Agol 2002
Analytical transit model for some simple LD models
Python code
Gimenez+2006
uses a series expansion of a transit model for increased model flexibility
Maxted & Gill 2019
model-specific analytical approximations
 Numerical (integration)
Roadrunner: Parviainen 2020
Nelson & Davis 1972
batman: Kreidberg 2015
ellc: Maxted 2016
Exact
Starry package uses spherical harmonics 

Mandel-Agol model
Original parameters: k, (u1,u2), t0, P, a/Rs, inc, e=0, w=0 (see pytransit)
Alternative parameters:
k2, instead of k=Rp/Rs?; See also Heller+2019
(q1,q2), instead of (u1,u2); See Kipping+2013, Espinoza & Jordan 2015 
t0
P (or logP)
Transit duration, instead of a/Rs (see e.g., Winn 2010)
Host star density (log rho; see Gilbert+2022)
cos(inc), instead of inc
b (impact parameter), instead of inclination → b = cos(inc) * (a/Rs)
f_s=sqrt(e) sin (ω) & f_c=sqrt(e) cos (ω), instead of eccentricity & argument of periastron (ω)
w=arctan2(f_s, f_c)
e=f_s2 + f_c2; See also photoeccentric effect (Dawson+2012; Solano-Oropeza+2023)
See Eastman 2024
Contamination or dilution parameter (0<c<1)
1 - (Ftarget / (Ftarget + Fblend))  (Gunther+2021) 
LC_undiluted + C / (1 + C) (Newton+2021)
For more discussion, see McDougall+2023; See also Exoplanet Handbook, 2nd ed. p.202
For general equations, see Winn+2010; Kipping+2010; Exoplanet handbook (Perryman+2018)

Priors
Limb darkening coefficients from Teff, logg, [Fe/H] (Claret)
See also stellar limb darkening profiles using PyLDTk package
For effective sampling of impact parameter and Rp/Rs, see Espinoza 2018
For unbiased priors when directly sampling host star density, see Gilbert+2022b
use log-uniform priors on host stellar density, or
use uniform priors on transit duration, or
use log-uniform priors on a/Rs 
b = U[0,1+Rp/Rs), allowing for grazing transits
For effective sampling grazing orbits, see Gilbert+2022a 
b < 1+Rp/Rs, in order to prevent unphysical transit durations
See other observational biases in Kipping & Sandford 2016
log X is equivalent to placing log-uniform priors on X which facilitates exploration of posterior values over different orders of magnitude








# Reading a transit light curve

Start with a plot of flux against observation time. On either side of a complete transit, the out-of-transit baseline tells us how bright the system appears when the planet is clear of the stellar disk. Dividing by the estimated baseline puts that level at one. A normalized flux of 0.990 corresponds to a loss of 1% of the observed light.

## Depth, duration, and geometry

For a uniformly bright star, an opaque planet entirely in front of the disk blocks a fraction of light equal to the ratio of their projected areas. In the absence of blending, the example depth of 0.010 gives a radius ratio of $R_p/R_\star=\sqrt{0.010}=0.10$. An absolute planet radius also requires a stellar radius. Propagate the uncertainty in both measurements.

Ingress begins at first contact, when the two disks first touch. Egress ends at fourth contact, when they separate. The time between these contacts is the total duration. The duration of ingress relative to the total duration helps constrain the impact parameter, the projected distance between the centers at conjunction in units of stellar radius. In a grazing transit the planet never lies entirely inside the stellar disk, making it harder to infer its size from depth alone. [Winn (2010)](https://arxiv.org/abs/1001.2010)

## Choosing a measurement

| Observable | Physical question | Additional information |
| --- | --- | --- |
| Transit midtimes across epochs | When will the next transit occur? | An unambiguous count of elapsed orbits |
| Depths in several filters | Does the apparent planet radius vary with wavelength? | Passband-specific limb darkening, stellar activity, and blending |
| Departures from a linear ephemeris | Could another body perturb the orbit? | Timing uncertainties and a dynamical model |
| Planet disappearing behind the star | How much light comes from the planet? | Eclipse depths by band and a model of thermal emission and reflection |
| Transit radius and radial velocities | What is the planet's bulk density? | Stellar properties and an orbital solution for the mass |

Transit duration variations, spot crossings, and gravity darkening can also reveal aspects of stellar rotation and spin–orbit geometry. Interpreting these features requires a model of the stellar surface or orbital dynamics. A single asymmetric event can have several explanations.

## Connecting light-curve features to physical properties

- **Stellar shape and spin.** A rapidly rotating star can have an oblate surface and hotter, brighter poles. A planet crossing different stellar latitudes can then produce an asymmetric transit. Modeling that asymmetry with a rotation constraint can inform the stellar spin rate, oblateness, and projected spin–orbit angle $\lambda$. Repeated spot crossings trace the transit chord relative to the rotating surface. A changing duration traces changes in chord or orbital speed, so interpreting it as precession requires a dynamical model. [Barnes (2009)](https://arxiv.org/abs/0909.1752) · [Spot crossings in HAT-P-11](https://arxiv.org/abs/1107.2920)
- **Atmospheric composition.** At wavelengths where the atmosphere is more opaque, starlight is blocked at a larger effective radius. In an isothermal hydrostatic atmosphere the scale height is $H=k_{\rm B}T/(\mu m_{\rm u}g)$, with mean molecular weight $\mu$, atomic mass unit $m_{\rm u}$, temperature $T$, and gravity $g$. One scale height changes the transit depth by about $2R_pH/R_\star^2$ for a thin atmosphere and a uniform stellar disk. Spectral features across many wavelengths help distinguish absorbers. Clouds and the stellar spectrum also affect the measured depths. [Brown (2001)](https://arxiv.org/abs/astro-ph/0101307)
- **Companion masses and bulk composition.** Gravitational interactions can advance or delay transits. Fitting the timing pattern across epochs, ideally for several planets, can constrain companion masses and eccentricities. A mass from timing or radial velocities and a radius from transits give $\rho_p=3M_p/(4\pi R_p^3)$. Compare density with interior models to constrain the range of possible compositions. [Agol et al. (2005)](https://arxiv.org/abs/astro-ph/0412032)
- **Dayside temperature and heat transport.** During secondary eclipse the star hides the planet. The lost flux measures the dayside planet-to-star flux ratio in that band. Infrared eclipse depths can yield a brightness temperature or an emission spectrum. Observations throughout the orbit add the day–night contrast and the phase of peak brightness, which constrain how the atmosphere redistributes heat. [Cowan & Fujii (2017)](https://arxiv.org/abs/1704.07832) · [Alonso (2018)](https://arxiv.org/abs/1803.06204)

For the connection between contact times and orbital geometry, see Seager & Mallén-Ornelas (2003) and Winn (2010). Heller (2019) quantifies how the stellar intensity along the chord changes the relation between depth and radius ratio. [Seager & Mallén-Ornelas (2003)](https://arxiv.org/abs/astro-ph/0206228) · [Winn (2010)](https://arxiv.org/abs/1001.2010) · [Heller (2019)](https://arxiv.org/abs/1901.01730)

For a broader treatment of transits and follow-up, see Charbonneau et al. (2007), Haswell’s *Transiting Exoplanets* (2010), and Perryman’s *The Exoplanet Handbook* (2nd edition, 2018). For the progression from broadband eclipse depths to transmission and emission spectra, see Seager & Deming (2010), Burrows (2014), Crossfield (2015), and Madhusudhan (2019). [Charbonneau et al.](https://arxiv.org/abs/astro-ph/0603376) · [Seager & Deming](https://doi.org/10.1146/annurev-astro-081309-130837) · [Burrows](https://www.nature.com/articles/nature13782) · [Crossfield](https://arxiv.org/abs/1507.03966) · [Madhusudhan](https://arxiv.org/abs/1904.03190)

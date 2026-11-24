# Modeling the transit signal

A box model predicts a constant loss of flux during transit. Its three observable features are the midtime, duration, and depth. It makes a useful first calculation when the period is known. Here time and duration are in days and flux is normalized to one.

```python
import numpy as np

def box_flux(time, period, midtime, duration, depth):
    offset = (np.asarray(time) - midtime + period / 2) % period - period / 2
    return np.where(np.abs(offset) < duration / 2, 1 - depth, 1.0)

time = np.array([-0.10, 0.00, 0.10])
print(box_flux(time, period=3.0, midtime=0.0, duration=0.12, depth=0.01))
# [1.   0.99 1.  ]
```

A trapezoid adds sloping ingress and egress. To infer a physical radius or impact parameter, compute the overlap of the planet and star along an orbit and weight the blocked area by stellar surface brightness.

## Limb darkening and model families

The stellar limb usually appears fainter than the center in optical light. A quadratic limb-darkening law describes the intensity as a function of $\mu$, the cosine of the angle between the local surface normal and the line of sight. Its coefficients $u_1$ and $u_2$ set the curvature of the transit bottom and affect the inferred radius ratio. Mandel and Agol derive flux expressions for a spherical star occulted by a dark planet. [Mandel & Agol (2002)](https://arxiv.org/abs/astro-ph/0210099)

## Exposure time and dilution

Each recorded flux is an average over an exposure. Evaluate the model at sub-exposure times and average before comparing it with the data. Increase the sampling within each exposure until the modeled ingress and egress are stable at the precision of the observations.

If a fraction $c$ of the total baseline flux comes from a constant neighboring source, the observed flux is $F_{\rm obs}(t)=(1-c)F_{\rm target}(t)+c$, where the target model has a unit baseline. A 1% intrinsic depth appears as 0.8% when $c=0.20$. State the dilution convention and check which crowding corrections the light-curve pipeline has already applied.

## Translating between model implementations

| Calculation | Suitable use and example |
| --- | --- |
| Analytic occultation integrals | Mandel & Agol (2002) integrate a circular occultor over a disk with specified limb darkening. A typical interface needs $T_0,P,k,a/R_\star,i,e,\omega$ and coefficients for the chosen intensity law. With $e=0$, the periastron angle is arbitrary. [Mandel & Agol (2002)](https://arxiv.org/abs/astro-ph/0210099) |
| Series and approximations | Giménez (2006) expands the flux deficit in a series. Maxted & Gill (2019) develop a power-2 limb-darkening approximation. Its accuracy depends on planet size and transit geometry. [Giménez (2006)](https://www.aanda.org/articles/aa/pdf/2006/18/aa4445-05.pdf) · [Maxted & Gill (2019)](https://doi.org/10.1051/0004-6361/201834563) |
| Numerical integration | batman combines analytic solutions for supported laws with numerical integration for others (Kreidberg 2015). PyTransit (Parviainen 2015) offers several fast models; its RoadRunner model integrates an arbitrary radial intensity profile (Parviainen 2020). ellc models projected stellar shapes in detached binaries, extending a tradition that includes Nelson & Davis (1972). [batman](https://arxiv.org/abs/1507.08285) · [PyTransit](https://arxiv.org/abs/1504.07433) · [RoadRunner](https://arxiv.org/abs/2009.08500) · [ellc](https://arxiv.org/abs/1603.08484) |
| Surface maps and gradients | starry integrates occultations of maps expressed in spherical harmonics (Luger et al. 2019). jaxoplanet provides models compatible with JAX automatic differentiation. Analytic integration describes how the flux is calculated. Automatic differentiation describes how derivatives of a calculation are obtained. [starry](https://arxiv.org/abs/1810.06559) |

**Converting dilution conventions.** Let $F_b$ and $F_\star$ be the contaminating and target fluxes outside transit in the chosen aperture. Some codes use $C=F_b/F_\star$ (e.g. Newton et al. 2021) and others use $c=F_b/(F_\star+F_b)=C/(1+C)$ (e.g. allesfitter; Günther & Daylan 2021). Perryman's *Exoplanet Handbook* (2nd ed., p. 202) and MacDougall et al. (2023) discuss dilution further. For an undiluted unit-baseline light curve $T(t)$, the first convention gives $[T(t)+C]/(1+C)$. The second gives $1-(1-c)[1-T(t)]$. For a neighbor one quarter as bright as the target, $C=0.25$, $c=0.20$, and either expression gives 0.992 at the bottom of a 1% intrinsic transit. In crowded TESS photometry, check any existing correction and estimate contamination separately for a higher-resolution follow-up aperture. [allesfitter parameter definitions](https://www.allesfitter.com/setup/parameters)

# The modeling ecosystem

A transit fit is assembled from layers: an array library, a transit model, a systematics model, an optimizer, and a sampler. Packages within a layer are often interchangeable, but a choice of array backend constrains everything above it. A sampler that uses gradients, such as HMC or NUTS, needs a transit model and likelihood written in a framework that can differentiate them.

## Array backends

| Backend | What it provides | Consequence for the fit |
| --- | --- | --- |
| [NumPy](https://numpy.org/) | Vectorized arrays on the CPU | Works with any optimizer or sampler that only needs log-probability values. No derivatives. |
| [PyTensor](https://pytensor.readthedocs.io/) | Symbolic graphs compiled to C, with automatic differentiation. Successor to Theano and Aesara. | The backend of PyMC, exoplanet, and starry. Gradients come for free once a model is written in it. |
| [JAX](https://docs.jax.dev/) | NumPy-like arrays with automatic differentiation, just-in-time compilation, and GPU support | Model, likelihood, optimizer, and sampler can all live in one differentiable program. Code must avoid Python-side state and in-place array updates. |

## Packages by layer

In the table below, "Gradients" means the package can supply derivatives of the model or log density to an optimizer or sampler.

| Layer | Package | Backend | Gradients | Notes |
| --- | --- | --- | --- | --- |
| Transit model | [batman](https://lkreidberg.github.io/batman/) | C via NumPy | No | Analytic quadratic and nonlinear limb darkening, numerical integration for other laws. [Kreidberg (2015)](https://arxiv.org/abs/1507.08285) |
| | [PyTransit](https://github.com/hpparvi/PyTransit) | NumPy, Numba, OpenCL | No | Several fast models, including RoadRunner for arbitrary limb-darkening profiles. [Parviainen (2015)](https://arxiv.org/abs/1504.07433) |
| | [ellc](https://github.com/pmaxted/ellc) | Fortran | No | Ellipsoidal stars, eclipsing binaries, spots, and gravity darkening. Used by allesfitter. [Maxted (2016)](https://arxiv.org/abs/1603.08484) |
| | [starry](https://github.com/rodluger/starry) | PyTensor | Yes | Analytic occultations of spherical-harmonic surface maps. [Luger et al. (2019)](https://arxiv.org/abs/1810.06559) |
| | [jaxoplanet](https://github.com/exoplanet-dev/jaxoplanet) | JAX | Yes | Orbits and limb-darkened light curves for JAX workflows. |
| Optimization | [SciPy](https://docs.scipy.org/doc/scipy/reference/optimize.html) | NumPy | Optional | Nelder–Mead and Powell use only function values. BFGS uses gradients, numerical if none are supplied. `differential_evolution` searches a bounded region. |
| | [PyDE](https://github.com/hpparvi/PyDE) | NumPy | No | `DiffEvol` differential evolution, often used to seed MCMC walkers. |
| | [jax.scipy.optimize](https://docs.jax.dev/en/latest/jax.scipy.html#module-jax.scipy.optimize) | JAX | Yes | BFGS minimization of a JAX objective. |
| | [JAXopt](https://jaxopt.github.io/), [Optax](https://optax.readthedocs.io/) | JAX | Yes | Gradient-based optimizers. JAXopt is no longer developed, and much of it has moved into Optax. |
| Sampling | [emcee](https://emcee.readthedocs.io/) | NumPy | No | Affine-invariant ensemble MCMC. Needs only log-probability values. |
| | [dynesty](https://dynesty.readthedocs.io/), [PyMultiNest](https://johannesbuchner.github.io/PyMultiNest/), [UltraNest](https://johannesbuchner.github.io/UltraNest/) | NumPy (MultiNest in Fortran) | No | Nested sampling: weighted posterior samples plus an evidence estimate. |
| | [PyMC](https://www.pymc.io/) | PyTensor | Yes | NUTS (adaptive HMC) and variational inference. |
| | [NumPyro](https://num.pyro.ai/), [BlackJAX](https://blackjax-devs.github.io/blackjax/) | JAX | Yes | NUTS, HMC, and other gradient-based samplers. |
| Gaussian processes | [george](https://george.readthedocs.io/) | NumPy (C++) | No | General kernels, $O(N^3)$ in the number of points. |
| | [celerite2](https://celerite2.readthedocs.io/) | NumPy, PyTensor, JAX | Through PyTensor or JAX | Scalable $O(N)$ GPs for a restricted family of one-dimensional kernels. |
| | [scikit-learn](https://scikit-learn.org/stable/modules/gaussian_process.html) | NumPy | No | General-purpose GP regression with kernel hyperparameter optimization. |
| | [tinygp](https://tinygp.readthedocs.io/) | JAX | Yes | GP building blocks, including celerite-style kernels. |
| | [gallifrey](https://arxiv.org/abs/2505.20394) | JAX | Yes | Sequential Monte Carlo over Gaussian-process kernel structures. |
| Neural networks | [scikit-learn](https://scikit-learn.org/), [PyTorch](https://pytorch.org/), [TensorFlow](https://www.tensorflow.org/) | Own frameworks | PyTorch and TensorFlow: yes | Flexible systematics models. They still need the transit-recovery checks in [Modeling systematics and noise](systematics.md). |
| | [Flax](https://flax.readthedocs.io/), [Equinox](https://docs.kidger.site/equinox/) | JAX | Yes | Networks that can sit inside a JAX likelihood next to a jaxoplanet model. |

## Integrated frameworks

These packages combine a transit model, a noise model, and inference behind one configuration.

| Framework | Components | Typical use |
| --- | --- | --- |
| [allesfitter](https://github.com/MNGuenther/allesfitter) | ellc, emcee or dynesty, celerite GPs | Joint photometry and radial velocities, flares, spots, and phase curves configured through `params.csv` and `settings.csv`. [Günther & Daylan (2021)](https://arxiv.org/abs/2003.14371) |
| [juliet](https://juliet.readthedocs.io/) | batman, radvel, george or celerite, dynesty, MultiNest, or UltraNest | Multi-instrument transits and radial velocities with nested sampling and model comparison. |
| [pyaneti](https://github.com/oscaribv/pyaneti) | Fortran likelihood, ensemble MCMC | Multiband transits and radial velocities, including multi-dimensional GPs for stellar activity. |
| [Exo-Striker](https://github.com/3fon3fonov/exostriker) | GUI wrapping transit, radial-velocity, and N-body models | Interactive exploration, dynamical fits, and TTVs. |
| [exoplanet](https://docs.exoplanet.codes/) | PyMC, starry-style light curves, celerite2 | Building custom gradient-based models from components rather than a fixed configuration. |

## Choosing a stack

A NumPy model with emcee or dynesty is the quickest route for a handful of parameters and an existing model. Once a fit has dozens of parameters, such as per-visit baselines, per-filter limb darkening, and GP hyperparameters, gradient-based samplers usually explore the posterior far more efficiently. The cost is writing every component in one differentiable framework. Whatever the stack, the checks in [Checking and reporting a fit](diagnostics.md) apply unchanged.

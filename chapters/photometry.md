# Choosing and preparing photometry

For a period measurement, look for widely separated transit epochs. For a radius ratio, look for well-resolved ingress and egress, a baseline on both sides, and an aperture with little contaminating flux. For a color comparison, simultaneous observations help separate wavelength dependence from changes between nights.

## Surveys and follow-up

| Mission | What one light curve covers | Identifiers and examples |
| --- | --- | --- |
| CoRoT | Pointed observing runs yielded space-based transit photometry before Kepler. | CoRoT-1 b is an early discovery. See Baglin et al. (2006) and Auvergne et al. (2009). [CoRoT performance](https://arxiv.org/abs/0901.2206) |
| Kepler, 2009–2013 primary mission | A single field toward Cygnus and Lyra for roughly four years. The spacecraft rolled each quarter, about every 90 days, placing a star on a different detector. Check the baseline across quarter boundaries. | KIC identifies a catalog target. KOI identifies a Kepler Object of Interest with a disposition that can change after follow-up. Kepler-51 b and c illustrate how repeated transits constrain planetary masses. [Kepler-51 timing analysis](https://arxiv.org/abs/1401.2885) · [Kepler archive](https://archive.stsci.edu/missions-and-data/kepler) |
| K2, 2014–2018 | Campaigns 0–19 sampled fields near the ecliptic. Many lasted about 80 days, with overlap in some fields. K2SFF and EVEREST offer reductions that address spacecraft roll. | EPIC identifies catalog targets, including the host of K2-100 b. Record every campaign used. [Howell et al. (2014)](https://arxiv.org/abs/1402.5163) · [K2 observing fields](https://archive.stsci.edu/missions-and-data/k2/campaign-fields) |
| TESS, launched 2018 | A typical sector lasts 27.4 days. Overlapping sectors give some targets a much longer baseline. Footprints and gaps depend on sky position and observing cycle. | TIC identifies a catalog target. TOI identifies a TESS Object of Interest. TOI-200 is the host HD 21749, an example where a planet also has a discovery name under the stellar designation. [TESS sector dates](https://heasarc.gsfc.nasa.gov/docs/tess/sector.html) · [Dragomir et al. (2019)](https://arxiv.org/abs/1901.00051) |

**Survey scale.** The Kepler DR25 stellar catalog characterized about 200,000 targets. K2 reached 300,000 standard targets during its extended mission, and TESS selected over 200,000 primary-mission targets for short-cadence photometry. The QLP reduction alone delivered FFI photometry for about 10 million stars from those first two TESS years. The NASA Exoplanet Archive attributes thousands of confirmed discoveries to Kepler and hundreds to K2. TESS passed 1,000 confirmed discoveries in October 2026. Candidate counts, observed targets, and confirmed planets answer three different questions. [Kepler DR25](https://arxiv.org/abs/1609.04128) · [K2 milestone](https://keplergo.github.io/KeplerScienceWebsite/keplers-k2-mission-reaches-300000-standard-targets-and-200-confirmed-planets.html) · [TESS products](https://heasarc.gsfc.nasa.gov/docs/tess/data-products.html) · [Primary-mission QLP sample](https://arxiv.org/abs/2011.06459) · [TESS discovery milestone](https://heasarc.gsfc.nasa.gov/docs/tess/tess-reaches-1000-confirmed-exoplanets.html) · [Archive counts and definitions](https://exoplanetarchive.ipac.caltech.edu/docs/counts_detail.html)

## Ground-based surveys and observing windows

| Program | Sites and observing role |
| --- | --- |
| Hungarian Automated Telescope Network (HATNet) | Small, wide-field telescopes in Arizona and Hawaii. The project's catalog lists 70 discoveries and provides their light curves. HATSouth is a separate southern network. [Operations](https://hatnet.org/operations.html) · [HATNet planet catalog](https://www.hatnet.org/planets/) |
| Kilodegree Extremely Little Telescope (KELT) | KELT-North in Arizona and KELT-South in South Africa surveyed bright stars. The team had discovered 26 planets by 2020. [Survey design](https://arxiv.org/abs/1605.02425) · [Discovery history](https://science.nasa.gov/people/joshua-pepper/) |
| Wide Angle Search for Planets (WASP) | SuperWASP on La Palma and WASP-South at Sutherland used wide-field camera arrays. The project reports over 150 planets. [WASP project](https://wasp-planets.net/about/) |
| Next Generation Transit Survey (NGTS) | Twelve telescopes at Paranal in Chile monitor fields for shallow transits. [Wheatley et al. (2018)](https://arxiv.org/abs/1710.11100) |
| MEarth | The northern array at Mount Hopkins, Arizona, and southern array at Cerro Tololo, Chile, targeted nearby M dwarfs. A small star gives a deeper transit for a given planet radius. [Irwin et al. (2014)](https://arxiv.org/abs/1409.0891) |
| TRAPPIST | Telescopes at La Silla, Chile, and Oukaïmeden, Morocco, provide targeted photometry. The TRAPPIST-1 discovery and follow-up campaign, including Spitzer, established a system of seven terrestrial-size planets. [Gillon et al. (2017)](https://arxiv.org/abs/1703.01424) |
| Qatar Exoplanet Survey (QES), XO, TrES | Wide-field searches for transits, including hot Jupiters. QES used cameras in New Mexico. For XO and TrES see McCullough et al. (2005), Crouzet (2017), and Alonso et al. (2004). [XO survey](https://arxiv.org/abs/astro-ph/0505560) · [QES survey team](https://arxiv.org/abs/1401.1984) |
| Global Architecture of Planetary Systems (GAPS) | An Italian-led radial-velocity program using HARPS-N on the Telescopio Nazionale Galileo, La Palma. Its spectra can supply the mass measurement for a photometrically characterized planet. [GAPS project](https://arxiv.org/abs/1803.11429) |

Space-based observations can cover consecutive orbits with occasional gaps for downlinks or spacecraft operations. A ground-based series usually consists of visits separated by daylight, weather, and seasons. A single visit may contain only one contact. Select a layout that preserves these windows.

| | One passband | Several passbands |
| --- | --- | --- |
| One epoch | A single CHEOPS visit or Las Cumbres Observatory (LCO) light curve | Simultaneous MuSCAT photometry or wavelength bins from a JWST transit |
| Several epochs | Repeated Kepler or TESS transits | Repeated MuSCAT visits or TESS combined with ground-based filters |

Hubble and Spitzer visits can also cover individual events. Keep the original time stamps and gaps even when displaying a phase-folded curve.

## From images to differential flux

Use the predicted ephemeris and equatorial coordinates (right ascension and declination) to check whether the transit and baseline occur above the horizon and outside twilight. A target near the zenith has lower airmass. Moonlight, saturation, detector nonlinearity, and bad pixels can all reduce the precision of a measurement.

Calibrate images with the appropriate bias, dark, and flat frames. Estimate and subtract the sky background. Measure stellar counts through an aperture or by fitting the point spread function. For differential photometry, divide target counts by an ensemble of stable comparison stars measured in the same exposures. Normalize that relative flux to its out-of-transit level. Retain the flux uncertainty, airmass, sky level, centroid, peak counts, and image width for each exposure.

A photometric zero point converts instrumental magnitudes to a calibrated apparent magnitude. A color is a difference in magnitudes, for example Gaia BP minus RP. Record the passband explicitly, whether Johnson UBV, SDSS optical griz, or near-infrared JHKs. A transit model uses the stellar intensity profile appropriate to that passband.

**Magnitudes and measurement error.** An apparent magnitude measures flux at the observer. An absolute magnitude is the apparent magnitude the object would have at 10 parsecs with no extinction. Differential photometry can bypass the absolute zero point. For a same-band magnitude difference $\Delta m$ between target and comparison, their flux ratio is $10^{-0.4\Delta m}$. Record a one-standard-deviation uncertainty $\sigma_F$ in the same units as flux. A single exposure's signal-to-noise ratio is $F/\sigma_F$. The precision of the transit depth also depends on baseline coverage and correlations between exposures.

## Working with TESS products

In the Kepler archive, SAP and PDCSAP are products of the Kepler Science Operations Center pipeline. For TESS, the Science Processing Operations Center (SPOC) also provides SAP and PDCSAP. SAP sums calibrated pixels in an aperture. Presearch Data Conditioning acts on an extracted time series to mitigate common instrumental trends. Pixel calibration, background subtraction, and cosmic-ray treatment are separate processing steps. Plot both flux columns across transit and baseline, and check which crowding correction the product includes. [Kepler data handbook](https://archive.stsci.edu/kepler/manuals/Data_Characteristics.pdf) · [TESS pipeline](https://heasarc.gsfc.nasa.gov/docs/tess/data_pipeline.html)

| TESS product | Sampling and coverage to check |
| --- | --- |
| SPOC and TESS-SPOC | Selected targets have 120-second postage stamps, with 20-second products for a subset from Cycle 3. TESS-SPOC also extracts selected targets from full-frame images (FFIs). FFI cadences were 30 minutes in Cycles 1–2, 10 minutes in Cycles 3–4, and 200 seconds starting in Cycle 5. [NASA product guide](https://heasarc.gsfc.nasa.gov/docs/tess/data-products.html) |
| Quick-Look Pipeline (QLP) | FFI-based light curves. The primary-mission release spans sectors 1–26. Look up the target list, cadence, and processing version for each sector. [QLP release notes](https://archive.stsci.edu/hlsp/qlp) |
| CDIPS | FFI photometry selected for cluster membership or youth. For example, release 6 adds sectors 27–55. Target selection matters even within a released sector. [CDIPS releases](https://archive.stsci.edu/hlsp/cdips) |
| PATHOS | PSF-based and aperture measurements in selected crowded fields. The first release targeted the field of 47 Tuc in sector 1. [PATHOS releases](https://archive.stsci.edu/hlsp/pathos) |
| TESS-Gaia Light Curve (TGLC) | Uses Gaia source positions and brightness information to separate neighbors in TESS FFIs. The archive lists files and target counts by sector. [TGLC archive](https://archive.stsci.edu/hlsp/tglc) |
| Causal pixel model (CPM) | A reduction method that predicts instrumental variation using other pixels. The unpopular implementation works with TESS FFIs and inherits their cadence and gaps. [Hattori et al. (2022)](https://arxiv.org/abs/2106.15063) |

Store time, flux, uncertainty, exposure duration, passband, and visit identifier together. Verify the time scale and any reference offset before combining facilities. Inspect flagged cadences and document exclusions, especially points near ingress or egress where the astrophysical signal itself changes quickly.
